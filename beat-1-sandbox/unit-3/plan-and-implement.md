# Unit 3 — Plan and Build

Path: `beat-1-sandbox/unit-3/plan-and-implement.md`

Record of your plan, the branch you built it on, and the evaluation runs that produced
`eval-run.txt`. This file is graded at the path above; a copy kept anywhere else in the
repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Posted upstream

**GitHub username**

HungH206

**Plan comment**

https://github.com/codepath/pathreview-ai301-fa26-s1/issues/26#issuecomment-5982231557

Plan for this, built on my repro above (3 events recorded through `SafetyMonitor`, `/health` still reports `"safety_events_last_hour":0`):

The handler never reads the safety layer: `api/routes/health.py` sets the field to a literal `0` at line 26 and again in the placeholder block at line 80. I'll replace that block so it builds a `SafetyMonitor` on a Redis client from `settings.redis_url` and sums `get_event_count()` over `VALID_EVENT_TYPES`. If Redis errors, the field stays `0` and the error is logged, same as now. I'll add a unit test with a mocked Redis (2 + 1 events → 3, and a Redis error → 0).

Test: re-run my repro and expect `"safety_events_last_hour":3`; clear the keys and expect `0`.

Not in this change: the Postgres and Redis probes (#61, #62), which are why `/health` still returns 503, and the monitor's time window. `get_event_count` reads a counter that expires 24h after the last event and doesn't enforce `window_hours`, so the number won't be a strict last-hour count. I'll flag that in the PR rather than change the monitor's storage here.

---

## Your branch

**Branch**

fix/26-health-safety-event-count

**Evidence**

Before (original code, `f89c06f`; this is the output from my Unit 2 repro):

```
$ python log_events.py
{'bias_detected': 0, 'content_filtered': 0, 'injection_attempt': 1, 'pii_detected': 2, 'rate_limited': 0}

$ curl -s -w "\nHTTP %{http_code}\n" localhost:8000/health
{"detail":{"status":"unhealthy","dependencies":{"postgres":"unhealthy","redis":"unhealthy","vector_db":"healthy"},"safety_events_last_hour":0,"timestamp":"2026-10-04T03:03:39.290105"}}
HTTP 503
```

After (branch `fix/26-health-safety-event-count`, same steps, then a control):

```
$ git log --oneline -1
f5e1574 fix(health): report safety event count from SafetyMonitor (#26)

$ python log_events.py
{'bias_detected': 0, 'content_filtered': 0, 'injection_attempt': 1, 'pii_detected': 2, 'rate_limited': 0}

$ curl -s -w "\nHTTP %{http_code}\n" localhost:8000/health
{"detail":{"status":"unhealthy","dependencies":{"postgres":"unhealthy","redis":"unhealthy","vector_db":"healthy"},"safety_events_last_hour":3,"timestamp":"2026-10-04T03:11:05.618380"}}
HTTP 503

# control: clear the safety counters, then query again
$ docker compose exec redis sh -c 'redis-cli --scan --pattern "safety:events:*" | xargs redis-cli del'
2
$ curl -s -w "\nHTTP %{http_code}\n" localhost:8000/health
{"detail":{"status":"unhealthy","dependencies":{"postgres":"unhealthy","redis":"unhealthy","vector_db":"healthy"},"safety_events_last_hour":0,"timestamp":"2026-10-04T03:11:05.953514"}}
HTTP 503

$ pytest tests/unit/test_health.py -v
tests/unit/test_health.py::TestHealthSafetyEventCount::test_reports_sum_of_monitor_counts PASSED [ 50%]
tests/unit/test_health.py::TestHealthSafetyEventCount::test_reports_zero_when_redis_errors PASSED [100%]
============================== 2 passed in 0.73s ===============================
```

The 503 is the same before and after. It comes from #61 (raw SQL in the Postgres probe) and
#62 (`settings.redis_host` in the Redis probe), which my plan leaves out of scope. Against
the original code, `test_reports_sum_of_monitor_counts` fails and the other test passes.
The unit suite goes from `375 passed, 53 xfailed` on `f89c06f` to `377 passed, 53 xfailed`
on the branch.

## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**

1. Warm-up on the four worksheet packages (`--include-calibration --only
   calib-01,calib-02,calib-03,calib-04`): `agreement: 0/0 scored items` (calibration is never
   scored). All four agreed with their labels, including the calib-03 operator-swap trap.
2. Full run: `agreement: 19/20 scored items  (bar: 18/20: PASS)`, with
   `categories: clear-accept 6/7  scope-creep 4/4  thread-convention 2/2  unbuildable 3/3  wrong-cause 4/4`.
   The one miss was `pkg-14  clear-accept  accept  reject  NO  failed: Diagnosis fits the repro evidence`.
3. Partial re-run after revising the Diagnosis check (`--include-calibration --only
   pkg-14,pkg-01,pkg-07,pkg-11,pkg-16,calib-03`): `agreement: 5/5 scored items`.
4. Full run, confirming (saved with `--save-run`, committed as `eval-run.txt`):
   `agreement: 19/20 scored items  (bar: 18/20: PASS)`, with
   `categories: clear-accept 6/7  scope-creep 4/4  thread-convention 2/2  unbuildable 3/3  wrong-cause 4/4`.
   The one miss was again pkg-14, this time on a different check:
   `failed: Unknowns stated, not dressed as certainty`.

**Package analysis**

`pkg-01` (httpie/cli#1838, argument parsing differs between Python < 3.13 and 3.13+). Gold
label: `reject`, category `wrong-cause`, note `"plan blames the request-item tokenizer; the
package's own control run (same items, no -v flag) parses fine, ruling the tokenizer out,
and --debug shows argparse consuming positionals before the item parser runs"`. My rubric
decided `reject`, and the two agree.

The plan's diagnosis is `"The REQUEST_ITEM tokenizer in httpie/cli/requestitems.py is the
problem."` The package's repro evidence has two runs that rule that out. Step 2 is a control
in the same venv with the same items and no `-v` flag, and it prints the request with
`header1: xyz` and `{"x": "1"}`, so the tokenizer handles those exact items correctly.
Step 4 says `"--debug on the failing run shows the error is raised by argparse's parse_args
while consuming positionals; the request items are never handed to HTTPie's item parser."`
My Diagnosis check fails a plan when `"any control shows the blamed component working while
the bug still occurs elsewhere"` or when `"the evidence shows the damage happens before the
code the plan blames ever runs"`. This package does both. The procedure has the grader list
each run with what was varied and what happened before it reads the plan, so the plan's
confident "the Python-version difference is a red herring" can't set the frame. Step 3 shows
the Python version is exactly what flips the outcome.

**Check rationale**

From `rubric.md`, the "Diagnosis fits the repro evidence" row's pass condition, as it reads
now:

> "The stated cause is consistent with ALL of the repro evidence: no control run rules it
> out. A control the plan explicitly accounts for with a reasonable explanation passes even
> if that explanation is not separately proven, as long as nothing in the evidence
> contradicts it (whether it is stated with honest confidence is the "Unknowns" check's job,
> not this one's). Fails if any control shows the blamed component working while the bug
> still occurs elsewhere (e.g. the same input parses fine without the flag, the "missing"
> module prints in the same build, the operator already behaves correctly outside the
> condition), if the evidence shows the damage happens before the code the plan blames ever
> runs, or if the plan adopts a cause from the thread that the package's own measurements
> contradict. A plan whose cause matches the issue's or a maintainer's diagnosis passes only
> if the repro evidence does not contradict it"

The second sentence was added after my first full run. The original wording required every
failing run and every control to be "explained by" the cause, and the procedure called one
unexplained control a fail. That rejected pkg-14, a gold accept. Its plan does address its
cache-clear control ("with an empty cache the color data is refetched along the
fresh-attach path once"), but the grader called that explanation "an unsupported ad hoc
patch" because nothing in the package proves it. That treated "not proven" as
"contradicted." In the four wrong-cause packages, a control actually shows the blamed part
working. In pkg-14, nothing contradicts the plan's explanation, so the revised wording
separates the two cases and leaves the confidence question to the Unknowns check. The last
clause stays as written for calib-03, where a confident diagnosis borrowed from the thread
is contradicted by the package's own timing matrix.

**Trade-offs**

This revision loosened a check, so I re-ran canaries with `--only` before the confirming
run: pkg-14 together with every wrong-cause package (pkg-01, pkg-07, pkg-11, pkg-16) and the
calib-03 trap. All four wrong-cause packages still rejected, calib-03 still rejected, and
pkg-14 flipped to accept (`agreement: 5/5 scored items`). What the change gives up: a plan
can now pass Diagnosis with a guess, as long as the package holds nothing that contradicts
the guess. A plausible but wrong explanation for an awkward control will get through this
check, and only the Unknowns check can catch it, if the plan states the guess with too much
confidence. The confirming full run shows that hand-off in practice: pkg-14 failed again,
this time on Unknowns instead of Diagnosis. The gold note itself calls pkg-14 "arguable on
the deferral, ready as scoped", so I accepted the miss rather than loosen a second check to
fit one borderline package.

---

Related paths: `plan.md` and `eval-run.txt` in this directory; your skill's files in
`tools/plan-check/`.
