# Evidence guide: where evidence lives in a PR package

## Plan fidelity (harness category: silent-drift)

Where it lives: in an eval bundle, the plan is the "Plan context"
block: its scope statement, the files it names, its "not in scope" or
"known limit" lines, and any deviation note. What actually changed is
the "Diff" block, file by file and hunk by hunk. The description's
claims about fidelity ("implements the posted plan exactly", "no
changes beyond the fix", "now documents the limitation") are in
"Description". In live mode, the plan is `plan.md` in the clone's top
folder, with its `## Deviations` section; the diff is `git diff
main...HEAD`; the description is `pr_draft.md`.

What good looks like: every hunk in the diff is the planned fix, its
tests, or something a deviation note covers, and everything the plan
promised is in the diff or explicitly deferred. Silent drift runs in
both directions. More than the plan: a new option, flag, or setting, a
renaming pass, a rewrite of a neighboring function, or a file the plan
never names, with no deviation note. Less than the plan: a promised
piece (a docs update, a second fix site) absent from the diff while the
description says it is there. The description never settles this: a
claim of "exactly as planned" over a diff with an extra hunk is drift,
and the extra hunk is what decides it. An honest deferral, recorded in
the plan and restated in the description, is not drift.

## Test evidence (harness category: not-tested)

Where it lives: in an eval bundle, the "Test evidence" section, read
against the plan's test plan in "Plan context" (which names the repro
and the expected-after) and the repro evidence quoted there. In live
mode, `test_evidence.md` next to `plan.md`, plus any before/after
pasted into `pr_draft.md`'s Testing section, read against `plan.md`'s
test plan.

What good looks like: the exact repro the plan named, re-run through
the code the fix changes, with the before (the broken output) and the
after (the plan's expected-after) both shown as real output. Every
repro or failure mode the test plan names gets its own before/after.
When the test plan names a suite or the repo's checks, their outcome
is stated (a count or a pass/fail line). Not tested looks like "tested
locally, works now", "cargo test passes" with the repro never shown,
evidence that runs only the control or a neighboring path that never
broke, or one of two named failure modes silently skipped.

## Diff quality (harness category: unreviewable)

Where it lives: in an eval bundle, the "Diff" block and the "Commits"
list. In live mode, `git diff main...HEAD` and `git log --oneline
main..HEAD`.

What good looks like: a reviewer can see the fix and its test and
nothing else. Debris tells: a print or `eprintln!`/`console.log` left
in, a commented-out line or an earlier attempt kept as comments, a
function nobody calls (often behind `allow(dead_code)` or named
`_unused`), a TODO on a line the fix doesn't touch, and churn hunks
that only re-indent, re-order imports, or re-wrap unchanged lines.
Commit messages like "wip", "fmt", or "misc cleanups while debugging"
usually point at debris in the diff. A comment that explains the fix is
not debris.

## Standards and comms (harness category: standards-wall)

Where it lives: in an eval bundle, the repo-facts "pull requests" line
(the template's required sections, checklist, issue-link line, and
any required changelog or whatsnew entry) and the "contribution
policy" line (including any AI-use policy), read against the
"Description" and, for required file artifacts, the "Diff". In live
mode, `.github/PULL_REQUEST_TEMPLATE.md` and `docs/CONTRIBUTING.md` in
the working copy, read against `pr_draft.md`.

What good looks like: every section the template asks for is there
with real content (under any heading), the issue link line names the
real issue number, required checklist items are answered truthfully,
and a required changelog or whatsnew entry is in the diff, not just
promised. If the policy requires disclosing AI use on PRs or "in any
form", the description says, in the author's own words, how AI was
used. A standards wall looks like a required checklist skipped
entirely, "Closes #xxxx" left as a placeholder, a required changelog
entry missing from the diff, or no disclosure under a policy that
demands one. Path Review (live mode) always gets its template filled
and always gets an AI-use note in "Notes for Reviewers", because the
course requires it.
