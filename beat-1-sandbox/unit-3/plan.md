# Plan: #26, add a safety event count to the health check endpoint

Issue: https://github.com/codepath/pathreview-ai301-fa26-s1/issues/26

## Repro evidence this plan builds on

From my repro comment on the issue (macOS 26.3.1, Python 3.12.13, commit `f89c06f`, postgres and redis from the repo's `docker-compose.yml`):

Three events recorded through the real `SafetyMonitor` against the app's Redis:

```
$ python log_events.py
{'bias_detected': 0, 'content_filtered': 0, 'injection_attempt': 1, 'pii_detected': 2, 'rate_limited': 0}
```

The health endpoint right after:

```
$ curl -s -w "\nHTTP %{http_code}\n" localhost:8000/health
{"detail":{"status":"unhealthy","dependencies":{"postgres":"unhealthy","redis":"unhealthy","vector_db":"healthy"},"safety_events_last_hour":0,"timestamp":"2026-10-04T03:03:39.290105"}}
HTTP 503
```

The 503 and the unhealthy postgres/redis come from #61 and #62, not this issue; the field is still readable in the 503 body.

## Diagnosis

`api/routes/health.py` never reads the safety layer. It sets `safety_events_last_hour` to the literal `0` when building the response (line 26), and the block commented "Count safety events in last hour (placeholder)" sets it to `0` again (line 80). The data exists: in the repro, `SafetyMonitor.get_event_count()` returns 2 for `pii_detected` and 1 for `injection_attempt` from the same Redis, yet the endpoint reports 0. So the bug is the missing wiring in the handler, not the monitor's storage.

## Scope

In scope: replace the placeholder block in `health_check` so it builds a `SafetyMonitor` on a Redis client created from `settings.redis_url`, sums `get_event_count(t)` over `SafetyMonitor.VALID_EVENT_TYPES`, and writes the total into `safety_events_last_hour`. If that raises, the existing `except` keeps logging `safety_events_check_failed` and the field stays `0`, as it is today.

Not in scope:
- The Postgres probe (#61) and the Redis probe's `settings.redis_host` (#62). Those are separate issues, and they're why the endpoint still returns 503.
- `SafetyMonitor`'s time window. `get_event_count` reads a counter key whose expiry is reset to 24 hours on every event, and its own docstring says `window_hours` is "not enforced". The module also has a separate seeded defect (the event timestamp is computed but never stored). Making the count a true last-hour window would mean changing how events are stored, which is a different change.
- The response shape: no per-type breakdown field, no change to the field's name or int type, no change to the 503 logic.

## Files

- `api/routes/health.py`: the safety-events block (lines 77-82).
- `tests/unit/test_health.py` (new): a unit test for the count.

## Approach

1. In the safety-events block, import `redis`, `settings`, and `SafetyMonitor`; create `redis.Redis.from_url(settings.redis_url)`; set the field to `sum(monitor.get_event_count(t) for t in SafetyMonitor.VALID_EVENT_TYPES)`.
2. Remove the hardcoded `0` assignment inside that block. Keep the `0` default in the initial dict so the field is still present if the block fails.
3. Add the unit test: patch `redis.Redis.from_url` to return a mock Redis whose `get` returns `b"2"` for `safety:events:pii_detected`, `b"1"` for `safety:events:injection_attempt`, and `None` otherwise. Call `health_check` with a mock db and assert `safety_events_last_hour == 3` in the response (or in the 503 detail, since the other probes still fail in a unit context). A second case has `get` raise and asserts the field stays `0`.

## Test plan

1. Re-run the repro exactly: `python log_events.py`, then `curl -s -w "\nHTTP %{http_code}\n" localhost:8000/health`. Expected after the fix: `"safety_events_last_hour":3`. The status will still be 503 with postgres/redis unhealthy, which is unchanged and belongs to #61/#62.
2. Control: delete the `safety:events:*` keys and curl again. Expected: `"safety_events_last_hour":0`, so the field follows the data instead of always being 0.
3. `pytest tests/unit/test_health.py -v`: both cases pass. Then the full unit suite, to check this change doesn't break any test that was passing on `f89c06f`.

## Risks and unknowns

- The field is named "last hour", but after this fix it reports what the monitor stores, which is a counter that only resets after 24 hours without new events. That's an improvement on a constant 0, but it still isn't a strict last-hour count. I'll state this in the PR rather than fix it here (see Scope).
- #62 will also add a Redis client built from `settings.redis_url` in this same file. My change creates its own client inside the safety block so it doesn't depend on #62's fix, but the two PRs touch nearby lines and may need a rebase.
- If Redis is down, the field stays `0` and the error is logged, so it can't distinguish "Redis down" from "no events". Changing that would change the response shape, so I'm leaving it as is.

## Deviations

One change from the plan, in how a Redis failure is handled. The plan said a Redis error would reach the handler's `except` and be logged as `safety_events_check_failed`. In the build it doesn't: `SafetyMonitor.get_event_count()` already catches Redis errors itself, logs `event_count_error` for that event type, and returns `0`. So when Redis is down, the field is `0` (the behavior I planned) and the error is logged once per event type by the monitor. The handler's `except` now only catches failures building the client or the monitor. I didn't change the monitor to make it raise, because that would change `SafetyMonitor`'s behavior for its other callers, which is outside this issue. The unit test `test_reports_zero_when_redis_errors` covers this path and checks the field is `0`.

Otherwise the build matches the plan: one changed block in `api/routes/health.py` (formatted with the repo's `black` config) and one new test file, `tests/unit/test_health.py`. Results: the repro re-run reports `"safety_events_last_hour":3`, the control after clearing the counters reports `0`, the new tests fail on the original code and pass with the fix, and the unit suite goes from 375 passed / 53 xfailed to 377 passed / 53 xfailed.
