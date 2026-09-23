# Unit 1 — Issue Selection

Path: `beat-1-sandbox/unit-1/selection.md`

Record of the issue carried into Unit 2, and of the evaluation runs that produced
`eval-run.txt`. This file is graded at the path above; a copy kept anywhere else in
the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the
wrong label is not graded.

---

## Selected issue

**Issue link**

https://github.com/codepath/pathreview-ai301-fa26-s1/issues/26

**Verdict output**

```
Scope: https://github.com/codepath/pathreview-ai301-fa26-s1/issues/26 is in the scoped
repo (codepath/pathreview-ai301-fa26-s1). Path Review house rule noted: other students'
claim comments don't block a candidate here.

Checks:
- Maintainer alive: pass — repo not archived; last push 2026-09-16, 7 days before today
  (2026-09-23)
- Repo in use: pass — same last-push fact, within the 180-day window
- Scope fits a newcomer: pass — one bounded fix in api/routes/health.py, with
  safety/monitoring.py named as the source of the missing count (SafetyMonitor.get_event_count()),
  a 2-4hr estimate, maintainer-filed
- Unclaimed: pass — assignees: []; no linked PR; no comments at all on the thread
- Contribution policy allows AI-assisted work: pass — no CONTRIBUTING.md in the repo, no
  stated policy
- Good-first-issue label (preferred): pass — labels include good first issue

{
  "item": "https://github.com/codepath/pathreview-ai301-fa26-s1/issues/26",
  "checks": [
    {"name": "Maintainer alive", "grade": "pass", "evidence": "repo not archived; last push 2026-09-16, 7 days before capture"},
    {"name": "Repo in use", "grade": "pass", "evidence": "last push 2026-09-16, within 180 days"},
    {"name": "Scope fits a newcomer", "grade": "pass", "evidence": "one bounded fix, named relevant files (api/routes/health.py, safety/monitoring.py), 2-4hr estimate, maintainer-filed"},
    {"name": "Unclaimed", "grade": "pass", "evidence": "assignees: []; no linked PR; no comments"},
    {"name": "Contribution policy allows AI-assisted work", "grade": "pass", "evidence": "no CONTRIBUTING.md in the repo; no stated policy"},
    {"name": "Good-first-issue label (preferred)", "grade": "pass", "evidence": "labels: good first issue"}
  ],
  "verdict": "accept"
}
```

---

## Eval iterations

Quote source text directly in each field below. Paraphrase does not satisfy them.

**Run history**

1. Full run, first rubric draft: `agreement: 16/20 scored items (bar: 18/20: below the bar)`,
   with `categories: claimed 4/4 clear-accept 4/8 dead-repo 3/3 policy 1/1 scope 4/4` — all
   four misses were gold-`accept` issues my "Scope fits a newcomer" check rejected.
2. Partial re-run (`--only` on the 8 issues touching scope/claim logic) after rewriting that
   check to drop an "unresolved implementation decision" clause it never should have had:
   `agreement: 5/8 scored items`.
3. Partial re-run (`--only issue-01,issue-04,issue-19`, `--out`) to read the check-level
   evidence behind the remaining misses: `agreement: 2/3 scored items`.
4. Partial re-run (`--only` on the same 8 issues) after adding the "touching several files
   is not by itself an umbrella" clarification: `agreement: 7/8 scored items`.
5. Partial re-run (`--only issue-15`) to confirm the one remaining miss was grader noise,
   not a rubric defect: `agreement: 1/1 scored items`.
6. Full run, confirming: `agreement: 19/20 scored items (bar: 18/20: PASS)`, with
   `categories: claimed 4/4 clear-accept 7/8 dead-repo 3/3 policy 1/1 scope 4/4`.
7. Full run, final (saved with `--save-run`, committed as `eval-run.txt`):
   `agreement: 19/20 scored items (bar: 18/20: PASS)`, with
   `categories: claimed 4/4 clear-accept 7/8 dead-repo 3/3 policy 1/1 scope 4/4`.

**Issue analysis**

`issue-01` (conda/conda#16475, "Add permanent docs for installing PyPI packages with
`conda install`"). Gold label: `accept`, with the note `"docs task with a stated home and
scope; active repo, unclaimed"`. My rubric's decision on the committed run: `accept` —
agreement `yes`.

It wasn't always a clean agree. Under the pre-fix rubric (run 1 above), this issue was
graded `reject`, and the grader's own evidence for that fail was: `"Issue lists 5 separate
proposed changes across 5 different files (new task page, manage-pkgs.rst,
pip-interoperability.rst, new-features.md, troubleshooting.rst) — an umbrella issue not
located in one place"`. That's a real umbrella *shape* (a checklist across several files)
but not umbrella *substance*: every file edit serves one feature — giving the
`conda install`-from-PyPI workflow a permanent docs home — not several independent
sub-items a maintainer meant to split across contributors. The current check's language,
`"a change that is deliverable in one pull request by one contributor is bounded even if
it spans multiple files or file edits (e.g. a documentation task updating five pages for
one feature)"`, reads this issue the way the gold label does: one bounded ask, several
touch points.

**Check rationale**

From `rubric.md`, the "Scope fits a newcomer" row's pass condition (as currently written):

> "Touching several files or listing several concrete items is NOT by itself an umbrella:
> a change that is deliverable in one pull request by one contributor is bounded even if
> it spans multiple files or file edits (e.g. a documentation task updating five pages for
> one feature, or a bug affecting several named-but-related items), or names several
> contributing causes or possible fixes for ONE broken behavior — grade whether the whole
> ask fits in one PR, not the file count or the writeup's polish."

I wrote this sentence after run 1 above showed the check conflating two different things:
"touches several files" and "is an umbrella issue." Three gold-`accept` issues in that run
(`issue-01`, a five-file docs task; `issue-16`, a bug report with a full diagnostic dump;
`issue-19`, a bug with two named causes and three suggested fixes) all got rejected on
scope for essentially the same reason — the check was reading length or file-count as a
proxy for scope, when the real question the evidence guide asks is whether the work is
"one bounded piece of work," which is about deliverability in one PR, not surface area.

**Trade-offs**

The fix trades precision for recall on genuinely oversized issues. The umbrella clause
now fires only on explicit tells — the issue's body itself lists *other* issue/PR numbers
meant for different contributors to pick from, or says the work applies "anywhere in the
codebase" with no single location. An issue that is actually a multi-day undertaking
across five files, but is *phrased* as one cohesive ask without either tell (no reference
to other issues, no "codebase-wide" language), would now pass this check as bounded, when
a human reviewer sizing it up might reasonably call it too large for a first
contribution. The check now trusts how the issue frames its own scope over an independent
read of its actual size — a deliberate trade after run 1 showed the opposite failure mode
(punishing well-scoped issues for their file count) was costing real accepts.

---

## Selection rationale

Graded on whether all three are answered, in your own words. Not on how good the
reasoning is, and not on length — a short honest answer to each earns the full marks.
This is also the basis for the claim comment you write in Unit 2.

**Selection rationale**

[Answer all three, in your own words:

1. The issue's fit to your interests and to the time available.

This issue fit my plan for the time commitment and my interests to develop my issue contribution.

2. What the verdict identified correctly, and what you weighed that the rubric could
   not.

   This is a good first issue, tier-1. Only thing I could weight in are the extra time to develop and test before open for pull request/ review.


3. The anticipated difficulty in claiming it.

Tier-1, good first issue. Easy to claim.


]
---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/issue-select/`.
