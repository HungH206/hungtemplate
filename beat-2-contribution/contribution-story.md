# Contribution: [Issue Title]

Path: `beat-2-contribution/contribution-story.md`

This file is Assignment 5, and it is graded at the path above; a copy kept anywhere else in
the repository is not read. **Keep every section header below under its exact name** — the
format criteria grade exactly that. Your course repo link is the whole submission: no tool
files and no evidence uploads are part of Assignment 5.

**Contribution Number:** [1]
**Student:** [Student name]
**GitHub Username:** HungH206
**Issue:** https://github.com/AcademySoftwareFoundation/dna/issues/254
**Pull Request:** https://github.com/AcademySoftwareFoundation/dna/pull/292

One repository, built across units 5 to 8, telling the whole story of your real
contribution. Fill it as you work, not at the end — the sections below ask for evidence that
is far easier to write down as it happens than to reconstruct in the last weekend.

Contribution Number stays `[1]` unless you make more than one contribution this term; it
just numbers them.

---

## Toolkit

Every story section below, from Why I Chose This Issue through Pull
Request, ends with a Toolkit block: which tool did that phase's work
and how you used it, what it produced, what difficulties or
shortcomings it presented, and what you changed or would change
because of them. Together the blocks are the thread the rubric's
Toolkit points read, and they are the raw material for your unit-9
portfolio, so keep them current as you work rather than
reconstructing them later.

A Toolkit block looks like this (copy it into each story section and
fill it there):

```text
**Toolkit**

- Tool: [which tool did this phase's work, and how you used it]
- What it produced: [described in a sentence or a short excerpt]
- Difficulties: [what fought you or fell short, or "none met"]
- What changed: [the patch, revision, or workaround, what you would
  update, or "nothing"]
```

This section also keeps one short dated entry per week of tool work,
starting below: what you built or changed that week, what broke, and
what you changed because of it. The blocks tell the story where it
happened; these entries are the toolkit's own timeline.

All four entries are graded. Keep every heading below, and fill each
one in the week it belongs to.

**Unit 5: Grew my issue-select skill into issue-scout and wrote its search strategy (2026-11-04)**

I turned my Unit 1 rubric into a scout. It runs three `gh search issues` queries that come from my
fit profile (Python and TypeScript API/endpoint work, with Docker issues filtered out), then ranks
the results and grades the top five. My first scan graded five issues: it accepted three and
rejected two. When I checked the rejects by hand, Gulliver#38 turned out to be a false reject. My
Unclaimed check had counted two open PRs that were only cross-referenced in the timeline, but both
of them actually close #37. I tightened Unclaimed so a PR only counts if it closes the issue or
names its number. I also changed one ranking filter from one issue per repo to one issue per owner,
because one owner had taken two of the five slots with issues from two different repos.

**Unit 6: [one line on the tool work]**

[2-4 sentences: the voice-guard, the repo-memory file. What broke or
fell short, and what changed because of it. "Nothing broke" counts when
it is true - say how you know.]

**Unit 7: [one line on the tool work]**

[2-4 sentences: the ship-gate and your gate manifest. What it produced
on your real branch, what fought you, what changed because of it.]

**Unit 8: [one line on the tool work]**

[2-4 sentences: the PR-monitor and your playbook. What it read, what it
drafted, what you did with the draft.]

---

## Why I Chose This Issue

I picked [AcademySoftwareFoundation/dna#254](https://github.com/AcademySoftwareFoundation/dna/issues/254),
"Shotgrid: Add env var to configure project types for users", from the Issue Selection site. I set
the filters from my fit profile (Python, backend/API work). I also ran my own scout, whose scan
surfaced three accepted issues in other repos, but #254 was not one of them, so I graded it by
direct URL. My scout **rejected** it: Maintainer alive, Repo in use, Scope, Policy and the
good-first-issue label all passed, but Unclaimed failed because open PR #262 by another contributor
closes #254, and the maintainer had replied "Looks good" on it. I chose it anyway because the fix
is small and bounded (add an `sg_type` filter to `get_projects_for_user` from an environment
variable, with tests), it matches the Python API work I want practice with, and #262 has stalled:
on 2026-11-18 the maintainer wrote that "something got a little messed up on this branch." I am not
claiming over the author. My claim comment asks whether #262 is still being worked on and offers to
take a smaller piece or the whole fix if it was dropped:
https://github.com/AcademySoftwareFoundation/dna/issues/254#issuecomment-6994478920 (2026-11-18).
If #262 merges first, I will pick another issue.

On the tool: on a real search it fell short in one way I fixed and one I only noted. It rejected
Gulliver#38 on two PRs that only cross-referenced it (both close #37), so I rewrote the Unclaimed
check to count a PR only if it closes the issue or names its number. It also cannot see that a
repo is two days old, since repo age is not in the search fields, so I judge that by hand. Its
reject on #254 was correct, and I am overriding it knowingly.

**Toolkit**

- Tool: issue-scout (my search strategy of three `gh search issues` queries and nine ranking
  filters, plus my Unit 1 rubric), run as a scan and as a direct grade of #254
- What it produced: a scan of 292 results graded down to five (three accepted, two rejected), and
  a reject on #254 for the open PR #262
- Difficulties: a false reject on Gulliver#38 from timeline cross-references; two issues from one
  owner in the top five; no repo-age signal
- What changed: patched the Unclaimed check in `rubric.md`, changed the diversity filter to one
  issue per owner, and logged both in `scan-notes.md`

---

## Understanding the Issue

**Problem Description**

[In your own words, what is broken or missing?]

**Expected Behavior**

[What should happen?]

**Current Behavior**

[What actually happens?]

**Affected Components**

[Which parts of the codebase are involved?]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Reproduction Process

**Environment Setup**

[What a stranger would need to stand this up: your operating system,
the relevant versions (language, runtime, package manager, the project
itself), and how you set the project up. Then the challenges you faced
and how you solved them, or that none came up.]

**Steps to Reproduce**

1. [Step 1]
2. [Step 2]
3. [Observed result]

**Reproduction Evidence**

- **Commit showing reproduction:** [link to commit in your fork]
- **Screenshots/logs:** [if applicable]
- **My findings:** [what you discovered during reproduction; an
  honest, evidenced cannot-reproduce that led you to re-pick or ask
  the maintainer belongs here and costs you nothing]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Solution Approach

**Analysis**

[Your analysis of the root cause: what is causing the issue, traced
to the files and functions involved.]

**Proposed Solution**

[High-level description of your fix approach, and what you are
deliberately NOT changing.]

**Implementation Plan**

1. [Modify file X to do Y]
2. [Add or change Z]
3. [Update tests]

[Record deviations here as they happen: what changed mid-build and
why. A recorded deviation is honest work; silent drift is not.]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Testing Strategy

[What observable checks prove the fix: your reproduction steps re-run
against the built change (before and after), and the repo's own test
suite or checks with the outcome shown. "Tests pass" with nothing
observable named is not a strategy.]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Implementation Notes

[A running log, added to as you work. Date each entry. What you
built, decisions you made when you made them, wrong turns included.]

**[Date]**

[Entry]

**[Date]**

[Entry]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Pull Request

**PR Link:** https://github.com/AcademySoftwareFoundation/dna/pull/292

**PR Description:** Adds an optional `PROJECT_ALLOWED_TYPES` environment variable (comma-separated
ShotGrid `sg_type` values). `ShotgridProvider.get_projects_for_user` adds an `sg_type` "in" filter
to its `Project` query when the variable is set, and behaves as before when it is unset or blank.
Five new tests cover unset, blank, single type, multiple types and the existing user filter, and
the variable is documented in `QUICKSTART.md`. The description says openly that PR #262 targets the
same issue and offers to close mine in favor of it.

**Maintainer Feedback:**

- 2026-11-18: none yet. EasyCLA and DCO checks pass.

**Status:** Awaiting review

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Learnings and Reflections

**Technical Skills Gained**

[What you learned technically, named concretely.]

**Challenges Overcome**

[What was hard and how you solved it; point at the real events, the
Toolkit blocks above are full of them.]

**What I'd Do Differently Next Time**

[Reflection on your process.]

---

## Resources Used

[What actually helped, not a reading list. List things this story
already shows you using: the doc you followed to set the environment
up, the thread that explained the bug, the person who answered you.
One real entry beats ten plausible ones.]

- [Link to documentation that helped]
- [Discussion, issue, or post that helped]
