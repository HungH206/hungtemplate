---
name: pr-precheck
description: Grade a PR package (a candidate pull request read against the plan it claims to implement and the issue that plan belongs to) and decide whether it is ready to submit. Use when checking your own branch, draft PR title, and description before opening the pull request, or when grading an eval package bundle.
---

# pr-precheck: rubric-driven PR grading

## The question

You answer one question about one PR package: **is this pull request
ready to submit?** A PR package is a candidate pull request (its
title, its description, its commits, its diff, and its test evidence)
read against two things: the plan it claims to implement (including
any deviation notes recorded in that plan) and the issue that plan
belongs to. Do not answer any other question. Do not review the code's
style for its own sake, suggest a better fix, or grade more than one
package per run. You answer by executing this directory's components,
never from gut feel.

## Inputs and modes

Decide the mode first, from what you were given.

**Eval mode**: you were given a package bundle (a markdown file with a
repo-facts block, the issue, thread highlights, a plan-context block,
and a candidate PR with title, description, commits, diff, and test
evidence). The bundle is the whole world. Take every fact from the
bundle text; fetch nothing, read no other file, run no command against
any repository. Grade every check in `rubric.md` and apply the full
verdict rule. Ignore `scope.md` and `voice-guide.md` entirely.

**Live mode**: you were given the student's issue URL and pointers to
their drafts, normally run from the top folder of their fork's clone,
on their fix branch. Read exactly these inputs:

1. **The plan**: the `plan.md` file the student names (in the clone's
   top folder), including its `## Deviations` section.
2. **The diff**: the output of `git diff main...HEAD` (three dots), run
   in the working copy. This is every committed change the branch makes
   relative to `main`. Also read `git log --oneline main..HEAD` for the
   commit list. Uncommitted edits and untracked files are not part of
   the PR; ignore them, and never treat `plan.md`, `comment.md`,
   `pr_draft.md`, or `test_evidence.md` as part of the diff.
3. **The draft title and description**: the file the student names
   (normally `pr_draft.md`). The first line is the title; everything
   after it is the description.
4. **The test evidence**: the file the student names (normally
   `test_evidence.md`), plus any before/after output pasted in the
   description.
5. **The issue side**: the issue's body and thread, fetched from GitHub
   (`gh` or the GitHub API), including the student's posted repro and
   plan comments and any maintainer direction.
6. **The repo's stated standards**: `.github/PULL_REQUEST_TEMPLATE.md`
   and `docs/CONTRIBUTING.md` in the working copy (or on the repo's
   default branch), including any stated AI-use policy.

A student on the house chain reads the house plan and the house repro
pack in place of their own plan and repro; grade them with the same
checks. If any input you need is missing (no plan file, no draft, an
empty diff), say which one, grade the checks that depend on it
`unclear`, and let the verdict rule decide.

## The scope seam (live mode only)

In live mode, read `scope.md` before anything else.

- If its `Repo:` line still holds the bracketed placeholder, stop
  without grading. Tell the student to fill the `Repo:` line in
  `scope.md` with their section's Path Review repo, and emit nothing
  else. Never guess a scope.
- If the issue URL is not in the scoped repo, refuse to grade and say
  why.
- Apply the house rules it states when you read the evidence: the PR
  comes from the student's fork branch, one PR per issue per student,
  the repo's PR template is always used, and a classmate's PR on the
  same issue does not block this one.

In eval mode, ignore `scope.md` entirely.

## The voice seam (live mode only)

In live mode, read `voice-guide.md` after grading the checks. Hold the
draft PR title and description against each of its rules and its
"Things I never post" list. In your summary, list every rule the draft
breaks, quoting the rule's name and the offending line. The voice
guide is the student's own conscience, not a rubric check: it never
changes the verdict unless `rubric.md` has a check that reads it. In
eval mode, ignore `voice-guide.md` entirely.

## Component reads

Read the components in this order, then execute.

1. `rubric.md` defines the checks (name, evidence, pass condition,
   weight) and the verdict rule. Every check in its table is graded,
   in table order.
2. `references/evidence-guide.md` is the map: for each evidence family,
   where it lives in a bundle and in live mode, and what good looks
   like. Use it to find the evidence each check names.
3. `procedure.md` is the operating procedure. Execute it as written:
   its read order, its evidence gathering, its check execution, and its
   verdict assembly. Where it is silent on a step you need, do not
   invent one. Say in your summary which step was missing and what you
   did in its absence.

If `rubric.md` has no checks, or `procedure.md` has no steps of its own
(only template headings and comments), refuse to grade. Say which file
is empty and that this tool cannot grade without it.

## Verdict and output

The verdict is binary. `accept` means the PR is ready to submit;
`reject` means hold it and fix what failed. There is no third verdict
and no score; reservations go in the evidence lines.

Before the JSON block you may print a short readable summary: one line
per check with its grade and deciding fact, then (live mode) any
voice-guide notes. End your reply with this fenced JSON block, filled
in with one entry per rubric check in rubric order. It must be valid
JSON, and nothing may follow it:

```json
{
  "item": "<PR URL or bundle id>",
  "checks": [
    {"name": "<check name>", "grade": "pass|fail|unclear",
     "evidence": "<one line: the fact or quote that decided it>"}
  ],
  "verdict": "accept|reject"
}
```

In eval mode, `item` is the bundle id (for example `pkg-07`). In live
mode, it is the issue URL, since the PR does not exist yet.

## Grading discipline

- **Evidence first.** Never grade a check without naming the fact or
  quote that decided it. "Looks fine" is not evidence.
- **Grade the thing, not the polish.** Read the diff itself against the
  plan, the test output itself against the test plan, and the
  description itself against the diff. A terse complete PR can be
  ready; a long, confident, perfectly formatted one can be hiding
  drift. Never let the description's claims stand in for the diff.
- **Honest shortfalls are not failures.** A PR that does less than
  everything but says so (a deferral recorded in the plan and restated
  in the description) is graded on what it says it does.
- **The rubric decides.** If a check passes by its stated condition
  but feels wrong, it still passes; note the tension in the summary.
  The fix belongs in the rubric, not in the run.
- **The procedure decides how.** Follow `procedure.md` as written and
  report its gaps rather than improvising around them.
- **Unclear is a fail.** Treat `unclear` as `rubric.md`'s verdict rule
  directs. Where the rule is silent, treat it as `fail`: a PR you
  cannot verify from its own package is not ready to submit.
