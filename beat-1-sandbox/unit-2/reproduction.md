# Unit 2 — Claim and Reproduce

Path: `beat-1-sandbox/unit-2/reproduction.md`

Record of your claim and reproduction on the issue you chose in Unit 1, and of the
evaluation runs that produced `eval-run.txt`. This file is graded at the path above; a copy
kept anywhere else in the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Your identity upstream

**GitHub username**

HungH206

---

## Posted upstream

**Claim comment**

https://github.com/codepath/pathreview-ai301-fa26-s1/issues/26#issuecomment-5982161503

Hi, I'd like to work on this as a first contribution (following up on my earlier claim comment above). The `/health` endpoint currently hardcodes `safety_events_last_hour` to `0` instead of reading from the safety layer. My plan is to look at `SafetyMonitor.get_event_count()` in `safety/monitoring.py` and wire its per-type counts into `api/routes/health.py`'s response. I'll report back with a reproduction of the current always-zero behavior before opening a PR.

**Reproduction comment**

https://github.com/codepath/pathreview-ai301-fa26-s1/issues/26#issuecomment-5982172892

> Hi, I'd like to work on this as a first contribution (following up on my earlier claim comment above). The `/health` endpoint currently hardcodes `safety_events_last_hour` to `0` instead of reading from the safety layer. My plan is to look at `SafetyMonitor.get_event_count()` in `safety/monitoring.py` and wire its per-type counts into `api/routes/health.py`'s response. I'll report back with a reproduction of the current always-zero behavior before opening a PR.

Reproduced: `/health` reports `"safety_events_last_hour": 0` while `SafetyMonitor` holds recorded events.

**Environment:** macOS 26.3.1 (arm64), Python 3.12.13, repo at commit `f89c06f` (main). Backing services from the repo's `docker-compose.yml` (`docker compose up -d db redis`: postgres:16-alpine on 5433, redis:7-alpine on 6379), `.env` copied from `.env.example`, `alembic upgrade head`, app run with `uvicorn api.main:app --port 8000`. redis-py 8.1.0, fastapi 0.142.2.

**Steps:**

1. Record three safety events through the real `SafetyMonitor`, against the same Redis the app uses (`log_events.py`, run from the repo root):

```python
import redis
from safety.monitoring import SafetyMonitor
r = redis.Redis.from_url("redis://localhost:6379/0")
for k in r.scan_iter("safety:events:*"): r.delete(k)
m = SafetyMonitor(r)
m.log_event("pii_detected", {"field": "email"})
m.log_event("pii_detected", {"field": "phone"})
m.log_event("injection_attempt", {"source": "resume"})
print({t: m.get_event_count(t) for t in sorted(m.VALID_EVENT_TYPES)})
```

```
$ python log_events.py
{'bias_detected': 0, 'content_filtered': 0, 'injection_attempt': 1, 'pii_detected': 2, 'rate_limited': 0}
```

2. Query the health endpoint:

```
$ curl -s -w "\nHTTP %{http_code}\n" localhost:8000/health
{"detail":{"status":"unhealthy","dependencies":{"postgres":"unhealthy","redis":"unhealthy","vector_db":"healthy"},"safety_events_last_hour":0,"timestamp":"2026-10-04T03:03:39.290105"}}
HTTP 503
```

**Expected:** `safety_events_last_hour` reflects the 3 events the monitor reports in step 1.

**Actual:** it is `0`. `api/routes/health.py` sets the field to the literal `0` (line 26) and again in its "safety events" block (line 80); nothing in the handler reads `SafetyMonitor`, so the value can't change no matter how many events are recorded.

**Note on the 503:** the endpoint returns 503 with postgres and redis marked unhealthy even though both containers are up. That comes from two separate issues (#61, the raw `"SELECT 1"` string under SQLAlchemy 2.x, and #62, the probe reading `settings.redis_host`), not from this one. The `safety_events_last_hour` value is still visible in the 503 body, so it doesn't block this reproduction, and I'm not touching those probes here.

## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**

1. Smoke run (`--limit 3`, to confirm the harness and skill were wired correctly):
   `agreement: 3/3 scored items`.
2. Full run: `agreement: 20/20 scored items  (bar: 18/20: PASS)`, with
   `categories: clear-accept 8/8  disclosure 1/1  no-evidence 4/4  unfollowable-comms 3/3  wrong-target 4/4`.
3. Full run, confirming (saved with `--save-run`, committed as `eval-run.txt`):
   `agreement: 20/20 scored items  (bar: 18/20: PASS)`, with the same category line.

Separately, as a warm-up sanity check (not part of the scored run history — calibration
packages are never scored), I ran the 4 calibration packages with `--include-calibration
--only calib-01,calib-02,calib-03,calib-04`: all four agreed with their worksheet labels
(the clear accept, the loud reject, the operator-swap trap, and the no-environment-record
borderline case).

**Package analysis**

`pkg-02` (sharkdp/bat#3845, "bat panics (capacity overflow) on a huge `--line-range`
offset-from-end"). Gold label: `reject`, category `wrong-target`, with the note `"ran a
prefix range instead of the issue's offset-from-end syntax; got a graceful arg-validation
error (exit 1), narrated as the reported capacity-overflow crash (exit 101)"`. My rubric's
decision: `reject` — agreement `yes`.

The issue's exact trigger is `bat --line-range ':-18446744073709551614'` (the `:-N`
offset-from-end syntax), which panics with `capacity overflow` and exit code 101. The
candidate report instead ran `bat --line-range '18446744073709551614:'` — a different flag
value entirely, missing the leading `:-` — which bat's own argument parser rejects
gracefully: `error: Invalid value for '--line-range'...`, exit code 1. The report then
narrates this as confirmation: `"The crash is confirmed and consistent: I ran this ten
times and it failed with the same message every single time."` My "Artifact shows the
issue's behavior" check reads this correctly as a fail: the trigger executed does not match
the issue's own construct (wrong flag syntax), and the shown artifact is a different failure
signature (exit 1 validation error, not exit 101 panic) than the one reported — a mismatch
the report never acknowledges. The "Outcome is stated honestly" check fails it a second way:
the "ran this ten times" framing tries to manufacture certainty through repetition rather
than by showing the actual matching artifact.

**Check rationale**

From `rubric.md`, the "AI-use disclosed where the policy requires it" row's pass condition
(as currently written):

> "Fails only when the policy affirmatively instructs disclosing AI assistance specifically
> on issue or PR comments (naming the tool and the extent of use) and neither candidate
> comment discloses it. Passes when the policy is silent, when it only asks contributors to
> personally understand/test/take responsibility for contributions without a comment-level
> disclosure ask, when it asks for human-voiced comments without a separate disclosure ask,
> when disclosure is scoped to pull requests rather than issue comments, or when the
> required disclosure is present."

I wrote the narrow trigger condition (an *affirmative, comment-level* disclosure
instruction) after reading every package's stated policy side by side. Most of the 20
packages' repos have a policy that sounds related but isn't actually a comment-disclosure
ask: fd's policy requires disclosure in the pull request but explicitly states "no
disclosure ask for issue comments"; conda's and prettier's ask contributors to understand
and take responsibility for their code, with nothing about comments; ripgrep's asks for
comments to be human-voiced, which is a tone rule, not a disclosure rule. Only
ghostty's policy (`pkg-20`, the one-item `disclosure` category the eval set's category
floor exists to force) actually says "all AI usage in any form must be disclosed... AI-
assisted issues and comments must be reviewed." A looser check — failing on any mention of
AI in a policy — would have wrongly rejected several gold-accept packages whose repos
merely have an opinion about AI-assisted code, not a comment-disclosure requirement.

**Trade-offs**

The narrow trigger trades recall for precision: it only fires on policies that use
explicit, comment-level disclosure language. A repo that requires disclosure in spirit but
phrases it differently — for example, "we expect full transparency from contributors about
the tools they used to prepare a submission," without the words "disclose" or "state the
tool and extent," and without naming comments specifically — would not trip this check even
though a stricter human reader might read it the same way ghostty's policy reads. I accept
this as a real gap: the check is calibrated to the phrasing patterns actually present across
this eval set's 20 policies, and a policy phrased unusually could slip past it. I chose this
trade deliberately after seeing how many gold-accept packages have AI-adjacent policy
language that is not a disclosure requirement (fd, conda, prettier, ripgrep above) — a
broader trigger would have cost more correct accepts than the narrow one plausibly costs in
missed disclosure requirements.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/repro-check/`.
