# Evidence guide: where evidence lives in a plan package

## Diagnosis and grounding

Where it lives: the cause is in the candidate plan's "Diagnosis"
section (or its first paragraph if unlabelled). The behavior it must
explain is in the "Repro evidence" block: the numbered steps, the
"Control" runs, and any `--debug`, trace, or timing output. Live mode:
the plan's diagnosis is in the student's `plan.md`; the repro evidence
is the student's own posted repro comment on the issue.

What good looks like: the stated cause explains every run in the repro
evidence, failing and control alike. The tell for a wrong cause is a
control run where the blamed component demonstrably works (the same
items parse without the flag, the same build prints a friendly error
elsewhere, the operator already returns the right value at top level)
or a step showing the data is already wrong before the blamed code
runs. A cause borrowed from the thread is only as good as the repro
evidence lets it be: if the package's own measurements point
elsewhere, the measurements win.

## Scope

Where it lives: the plan's change list or numbered approach, its
"In scope" / "Not in scope" lines, and its files list. Compare against
the issue's title and expected behavior. Live mode: the same sections
in `plan.md`.

What good looks like: one change that fixes the reported behavior,
plus its regression test and, at most, the same defect at a sibling
site in the same code. A stated deferral ("not in scope: the larger
rework") is a good sign. Scope creep looks like a dependency
migration, a new setting or CLI option, UI changes, a subsystem
rewrite, or "while I'm in here" fixes riding along with the core
change; a correct core fix does not excuse them.

## Executability

Where it lives: the plan's "Files" list and "Approach"/"Change"
section. Live mode: the same in `plan.md`.

What good looks like: named files or a named module/function area, and
one chosen approach in a sentence. A stranger could open those files
and start. It is not executable when the plan says "investigate",
"profile", or "figure out what changed" without choosing a change, or
leaves the location open ("somewhere around", "not sure which layer",
"whichever is easier").

## Test plan

Where it lives: the plan's "Test plan" section, read against the repro
evidence's commands and their outputs. Live mode: `plan.md`'s test
plan against the student's posted repro comment.

What good looks like: a named command or fixture (usually the repro
itself) with the expected observable after the fix: an exit code, an
output line, a measurement, a visible behavior. The best ones also
re-run the repro's controls and expect them unchanged. A vague test
plan names no observable: "should feel fast", "should work again",
"nothing else broken", or "run the full suite" with nothing specific
to this fix.

## Honesty

Where it lives: certainty language in the plan's diagnosis and the
plan comment ("root cause is", "confirmed", "red herring", "clearly"),
and the plan's "Risk", "Unknowns", or open-question lines. Mid-build
deviations, in live mode, are recorded as an update to `plan.md`.

What good looks like: what is verified is stated plainly, what is not
is named as an open question or risk ("exact functions pinned after
tracing", "cannot test Windows, flagging for review"). False certainty
is a confident claim the package does not support or actively
contradicts, such as dismissing as irrelevant the very variable the
repro shows flips the outcome.

## Comms

Where it lives: the "Candidate plan comment", read against the "Thread
highlights" (especially OWNER, MEMBER, and COLLABORATOR comments, and
any PR numbers) and the repo-facts "contribution policy" line. Live
mode: the draft plan comment against the live issue thread and the
repo's `CONTRIBUTING.md` / AI-policy files.

What good looks like: the comment shows it read the thread. If a
maintainer pointed at the culprit code, posted a patch or test build,
or picked a fix option, the plan follows it or says why not. If there
is an open or prior-art PR, the comment names it and says how this
work relates (not racing it, building tests on it). Ignoring explicit
maintainer direction in favor of a different plan is the failure.

On AI use: only a policy that affirmatively requires disclosure on
comments, or of all AI usage in any form, makes disclosure mandatory;
then the plan comment must say which tool helped and how. A policy
that asks for human-written comments, or for disclosure in pull
requests only, or for contributors to understand their code, does not
require a disclosure line in the comment.
