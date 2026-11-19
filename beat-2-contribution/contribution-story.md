# Contribution: Shotgrid: Add env var to configure project types for users

Path: `beat-2-contribution/contribution-story.md`

This file is Assignment 5, and it is graded at the path above; a copy kept anywhere else in
the repository is not read. **Keep every section header below under its exact name** — the
format criteria grade exactly that. Your course repo link is the whole submission: no tool
files and no evidence uploads are part of Assignment 5.

**Contribution Number:** [1]
**Student:** Hung Hoang
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

**Unit 6: Installed the voice guard, wrote my Upstream rules, and wrote a repo memory file (2026-11-18)**

I merged my three voice guides into one, added four rules for claims, questions and pings, and installed the hook that checks every `gh issue/pr comment` before it posts. Its first real run stopped the claim I had already posted, because "open a PR within a few days" breaks my own rule against dates, and a second draft then broke my one-question rule, so I rewrote it twice until it passed. I also swapped in the scope-feasible check and difficulty note in my rubric, and wrote a 29-line `CLAUDE.local.md` for the dna repo that is kept out of my PR with `.git/info/exclude`. What fell short: the posted claim still has the old wording, and re-grading dna#254 changed nothing, so I have not yet tested the scope patch on an issue with an open edge.

**Unit 7: Installed the ship-gate and wrote a three-check manifest for dna (2026-11-18)**

My manifest lists black, isort and the backend tests, taken from CONTRIBUTING.md and the repo's formatting workflow, with each command written for a fresh shell (`.venv/bin/black`) and Docker's `make test` in the excluded table. The hook test gave two stops, one from the voice guard and one from the gate. On my branch the gate passed, but I ran it after I had already opened PR #292, so it checked finished work, and it cannot see the DCO sign-off, which I checked by hand. I also ran plan-check, which rejected my first plan for having no repro evidence, no expected test output and no risks, and I fixed all three.

**Unit 8: Installed the PR monitor and wrote my playbook from dna's real review times (2026-11-18)**

My playbook came from six recent merged PRs by outside contributors: a maintainer usually replied within 0 to 1 days and the slowest took 6, so I nudge on day 8. I ran the monitor once on PR #292; it drafted a wait note, which was right, because nothing in the thread needed an answer, so I posted nothing. It did mislabel the state as FEEDBACK-IN because of the EasyCLA bot's comment, and it found a hole in my playbook: a nudge tags the last maintainer to comment and none had commented, so I rewrote that rule to name the maintainer from the issue thread. The PR is under a day old, so I have not tested the reply or nudge drafts on a real review.

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
reject on #254 was correct, and I am overriding it knowingly. In Unit 6 I replaced my scope check with the staff `scope-feasible` row plus a difficulty note, and re-graded #254: it is still a reject on Unclaimed (open PR #262), and the difficulty note says **straightforward**, because the issue names the function and gives a code sketch. The patch changed nothing for this issue, so I have not tested it on one with an open edge.

**Toolkit**

- Tool: issue-scout (my search strategy of three `gh search issues` queries and nine ranking
  filters, plus my Unit 1 rubric), run as a scan and as a direct grade of #254
- What it produced: a scan of 292 results graded down to five (three accepted, two rejected), and
  a reject on #254 for the open PR #262
- Difficulties: a false reject on Gulliver#38 from timeline cross-references; two issues from one
  owner in the top five; no repo-age signal
- What changed: patched the Unclaimed check in `rubric.md`, changed the diversity filter to one
  issue per owner, and in Unit 6 replaced the scope check with `scope-feasible` and added the
  difficulty note; all three are logged in `scan-notes.md`

---

## Understanding the Issue

**Problem Description**

DNA shows a user every ShotGrid project they belong to. A studio cannot limit which kinds of projects (by ShotGrid's project type, `sg_type`) are available inside DNA, and the issue asks for an environment variable that does that.

**Expected Behavior**

When `PROJECT_ALLOWED_TYPES` is set (for example `Feature,Commercial`), `get_projects_for_user` returns only the user's projects whose `sg_type` is in that list. When it is unset, nothing changes.

**Current Behavior**

`get_projects_for_user` filters projects only by the user, so every project the user belongs to comes back, whatever its type, and there is no setting to change that.

**Affected Components**

`ShotgridProvider.get_projects_for_user` in `backend/src/dna/prodtrack_providers/shotgrid.py`, its tests in `backend/tests/test_shotgrid_provider.py`, and the environment-variable table in `QUICKSTART.md`. The base class, the mock provider and the API route in `main.py` are not touched.

**Toolkit**

- Tool: Claude Code, reading the repo with my `CLAUDE.local.md` notes (none of my graded skills were used for this section)
- What it produced: the file and function to change and the other places that mention it (base class, mock provider, route)
- Difficulties: none met
- What changed: nothing

---

## Reproduction Process

**Environment Setup**

macOS 26.3.1 (Apple silicon), Python 3.11.1, pytest 7.4.3, shotgun_api3 3.9.2, repo at upstream `main` commit `ac6f610` (2026-11-18). I cloned my fork `HungH206/dna_hung`, made a venv in `backend/` (`python3 -m venv .venv`) and installed `backend/requirements.txt` plus `pytest pytest-asyncio pytest-cov`. The challenge was that the repo's `make test` runs through Docker, which I avoid, so I ran pytest directly with `PYTHONPATH=src`; the ShotGrid client is mocked in the existing tests, so no ShotGrid site or credentials were needed. My disk filled up partway through and I had to free space before I could run anything.

**Steps to Reproduce**

1. Check out `ac6f610`, create the venv and install the requirements (above).
2. Read `ShotgridProvider.get_projects_for_user` in `backend/src/dna/prodtrack_providers/shotgrid.py`: the project query is `self._sg.find("Project", filters=[["users", "is", user]], ...)`, with no `sg_type` filter and no `PROJECT_ALLOWED_TYPES` anywhere.
3. Keep that original `shotgrid.py`, add the five tests from my fork's commit `2569688` (`TestShotgridProviderProjectAllowedTypes` in `backend/tests/test_shotgrid_provider.py`), and from `backend/` run `PYTHONPATH=src python -m pytest tests/test_shotgrid_provider.py -q --no-cov -k ProjectAllowedTypes`.
4. Observed: with `PROJECT_ALLOWED_TYPES=Feature` set, the filters passed to `find` are only the `users` filter, so no project-type restriction is applied.

**Reproduction Evidence**

- **Commit showing reproduction:** https://github.com/HungH206/dna_hung/commit/2569688 (the tests and the fix are in one commit; the failure is reproduced by running those tests against the original `shotgrid.py`)
- **Screenshots/logs:**

```
tests/test_shotgrid_provider.py ..FF.                                    [100%]
E   AssertionError: assert ['sg_type', 'in', ['Feature']] in [['users', 'is', {'email': 'artist@example.com', 'id': 1, 'type': 'HumanUser'}]]
E   AssertionError: assert ['sg_type', 'in', ['Feature', 'Commercial', 'Episodic']] in [['users', 'is', {'email': 'artist@example.com', 'id': 1, 'type': 'HumanUser'}]]
================== 2 failed, 3 passed, 27 deselected in 0.14s ==================
```

- **My findings:** the two tests that need the type filter fail on the original code, and the three that describe behavior that already works (no filter configured, blank value, user filter kept) pass. So the filter the issue asks for is missing today. This shows what query the code builds, not what a live ShotGrid server returns for it, because I did not test against a real site.

**Toolkit**

- Tool: repro-check, with the wild scope, against my claim draft and my repro report
- What it produced: a reject on the first run, because my claim said "open a PR within a few days" and that promises a timeline; the repro report passed all its checks
- Difficulties: the issue is a feature request, so there is no crash to reproduce, and "failure signature" had to mean "the missing filter"; the posted claim already contained the timeline sentence before the guard existed
- What changed: I rewrote the claim to name a first step and a report-back instead of a date, and it now passes; the original wording is still in my posted comment

---

## Solution Approach

**Analysis**

`get_projects_for_user` in `shotgrid.py` finds the user by email, then queries `Project` with only `filters=[["users", "is", user]]`. There is no `sg_type` filter and nothing in the file reads a project-type setting, so every project the user belongs to is returned. The route in `main.py` just calls the provider, so the gap is in the provider alone. My reproduction agrees: against the original code the two type tests fail with only the users filter in the query, and the three tests for behavior that already works pass.

**Proposed Solution**

Add an optional `PROJECT_ALLOWED_TYPES` environment variable, a comma-separated list of ShotGrid `sg_type` values. A small helper `_get_allowed_project_types()` reads it at call time, trims whitespace and drops empty entries. When the list is not empty, `get_projects_for_user` appends `["sg_type", "in", allowed_types]` to its filters; unset or blank leaves behavior unchanged. Document the variable in `QUICKSTART.md`. Not changing: the mock provider, the base class, the API route, or any validation of which type names exist.

**Implementation Plan**

1. Add tests in `test_shotgrid_provider.py` for unset, blank, one type, several types with spaces, and the users filter still applying; run them on the original code and confirm the type tests fail.
2. Add `_get_allowed_project_types()` and use it in `get_projects_for_user`.
3. Add one `PROJECT_ALLOWED_TYPES` row to the `QUICKSTART.md` environment table.
4. Run black and isort checks, then the full backend suite; expect every existing test and the five new ones to pass, with coverage at or above 90%.

Test plan: re-run the step-1 command after the fix and expect `5 passed` (the two that failed now pass, the three controls unchanged). Risks: the tests mock the `Shotgun` client, so I have not checked against a live ShotGrid site; PR #262 targets the same issue and the maintainers decide which to prefer.

No deviations from this plan so far.

**Toolkit**

- Tool: plan-check (wild scope), on `plan.md` and `plan-comment.md`
- What it produced: a reject on my first drafts and an accept on the revised ones
- Difficulties: the first drafts quoted no repro evidence, had no test plan with an expected output, and listed no risks; the procedure also expects the repro evidence in a posted comment, which this unit says not to post, so I quoted it inside the plan
- What changed: I added a repro-evidence block, a test plan that expects `5 passed`, and a risks section, and rewrote the plan comment to name #262 first

---

## Testing Strategy

The reproduction command is `cd backend && PYTHONPATH=src .venv/bin/python -m pytest tests/test_shotgrid_provider.py -q --no-cov -k ProjectAllowedTypes`.

- **Before:** against the original `shotgrid.py`, 2 failed and 3 passed; the failures show only the users filter in the query: `assert ['sg_type', 'in', ['Feature']] in [['users', 'is', {...}]]`.
- **After:** on the branch, the same command gives `5 passed`.
- **Repo checks (from my ship manifest):** `black --check src/ tests/` and `isort --check-only src/ tests/` report no changes, and the full backend suite gives `860 passed` with `92%` coverage (the project requires 90% or higher).

The tests mock the `Shotgun` client. I did not test against a live ShotGrid site.

**Toolkit**

- Tool: ship manifest and the ship-gate (it runs the manifest's three checks), plus `test_evidence.md`
- What it produced: before and after output for the reproduction and the three check results, saved in `test_evidence.md`
- Difficulties: the repo's `make test` uses Docker, so I wrote the equivalent venv command in the manifest and listed `make test` in its excluded table with the reason
- What changed: I wrote each check the way it runs in a fresh shell (`.venv/bin/black`, not `black`)

---

## Implementation Notes

**2026-11-18**

Cloned the repo, made a venv, and ran the existing ShotGrid tests (27 passed). Read `get_projects_for_user` and confirmed it filters only by user. Wrote five tests first; two failed on the original code, as planned.

**2026-11-18**

Added `_get_allowed_project_types()` and the `sg_type` filter, then a `QUICKSTART.md` row. black wanted to reformat my first version of `shotgrid.py`, so I ran black. Full suite: 860 passed, 92% coverage. I committed with `git commit -s` for the DCO check.

**2026-11-18**

Pushed to my fork and opened PR #292. EasyCLA first failed because I had not signed the CLA; after signing, DCO and EasyCLA both passed. Order matters here: I opened the PR before installing the ship-gate and before running plan-check, so both ran on the finished branch, not ahead of it.

**Toolkit**

- Tool: Claude Code, with `CLAUDE.local.md` and the voice guard
- What it produced: the tests-first fix, and the commands that kept black, isort and DCO sign-off right
- Difficulties: my disk filled up mid-work and stopped my commands until I freed space; I also let a timeline sentence ("within a few days") into my claim before the guard existed
- What changed: I cleared space, and I rewrote the claim to a first step with a report-back instead of a date

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

- 2026-11-18: none yet. EasyCLA and DCO checks pass, and the only comment on the PR is the EasyCLA bot's.
- 2026-11-18: ran my PR monitor; it drafted a wait note, and I posted nothing. My playbook says to wait 7 quiet days before one nudge.

**Status:** Awaiting review

**Toolkit**

- Tool: the ship-gate (my ship manifest, plus my pr-precheck from Unit 4 on the branch, plan, test evidence and description), and the voice guard on the PR text
- What it produced: PASS on my branch: the three manifest checks (black, isort, backend tests) were ok, and pr-precheck passed all six of its checks
- Difficulties: I opened PR #292 before I installed the gate, so the PASS came after the PR was already open, not before it; the gate also cannot see the DCO sign-off, so I checked `git log -1` by hand and found `Signed-off-by`; `make test` needs Docker, so it sits in my manifest's excluded table
- What changed: nothing in the PR, because it passed; next time I would install the gate and run plan-check before pushing

---

---

## Learnings and Reflections

**Technical Skills Gained**

I learned to test a function that calls an outside service by mocking its client, and to read an environment variable at call time so a test can patch it. I also learned this project's contribution mechanics: an EasyCLA signature, a DCO `Signed-off-by` on every commit with `git commit -s`, Black and isort formatting, and a 90% coverage floor. And I learned to read a GitHub timeline, because a PR can claim an issue without showing up on its page.

**Challenges Overcome**

The hardest part was working on an issue that already had an open PR (#262): I asked on the issue first, said so in my PR description, and kept my claim to a first step instead of a date after my voice guard flagged it. My scout also falsely rejected Gulliver#38, which I fixed by narrowing what counts as a claiming PR. And my disk filled up mid-work, which stopped my commands until I freed space.

**What I'd Do Differently Next Time**

I would run plan-check and the ship-gate before opening the PR, not after, and I would install the voice guard before posting my claim so the timeline sentence never went up. I would also give the maintainer longer than a few hours to answer my question on the issue before opening a competing PR.

---

## Resources Used

- dna's CONTRIBUTING.md (CLA, DCO, Black and isort, the 90% coverage rule): https://github.com/AcademySoftwareFoundation/dna/blob/main/CONTRIBUTING.md
- The issue and its sketch of the filter: https://github.com/AcademySoftwareFoundation/dna/issues/254
- PR #262 and the maintainer's comments on it, which showed me the doc row to add and that its branch had stalled: https://github.com/AcademySoftwareFoundation/dna/pull/262
- The repo's backend formatting workflow (`.github/workflows/backend-formatting.yml`), which told me CI runs black and isort, not ruff
