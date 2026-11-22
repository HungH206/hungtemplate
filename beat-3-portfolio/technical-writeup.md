# My Open Source Contribution to Academy Software Foundation

## Overview

  In this contribution, I have used my Issue scout to hunt for an open source issue, and I had an easy first beginner problem from Academy Software Foundation to work on Dailies Notes Assistant (DNA). The issue I had with selecting my first open source problem is that in the open source project Gulliver, issue 38 was claimed by someone, and there were two open pull requests. I found out that there was for another issue #37. I rewrote the check that the scout has to count a pull request only if it's closing the issues or naming its number. The challenges that I had were prior implementations of the issue while selecting this open source issue contribution issue for this part. Then I had to pick the dna#254 anyway and work with Claude to understand what type of work can be done, how it ended is that my issue would not be merged and would be closed because the maintainer has merged the prior contributions for the same issue. So that this project may maintain as a good introduction to open source, I'm still happy with the contribution. 

## What happened

 The objectives of this issue are to add optional project-allowed-type environment variables, so the studios of the Note Assistant can control the project types that label short grid accessible within the system.

I had made a test file called shortgrid_provider.py, as well as fixed the shortgrid provider to add in the line of `get projects for user` to read `project allowed types` filter and add into the filter after it's set to fill in `project query` alongside the existing `users` filter, so it would have the behavior remain unchanged. It can allow more project types that are being allowed to use and accessible in DNA to be operated. I then change the Python path command to `pytest` for running any backend test, with all 860 tests passed and 92% coverage. The threshold for this project would need 90%, and all the source tests have been passed. After a few days, my contribution was closed because of prior implementation was more preferred. I ended up closed on this contribution, but it was good to learn about this new technology. 

More on this project: https://github.com/AcademySoftwareFoundation/dna/pull/292.

### 1. The rubric I wrote, and its first version of "Unclaimed"

```
Unclaimed (my Unit 1 rubric, commit 2373e60, 2026-09-23):
"No assignee is set, no linked PR is in the `open` state ... and any claim comment in the thread is either more than 90 days old ... or has no active linked PR behind it"
```

There are many rubrics that I wrote, but the main 4 things I'm looking for:
1. My assignee is set to no one being assigned for the project.
2. No PR will be linked in the open state.
3. No claim comment is more than 90 days old.
4. No active linked PR behind it.

### 2. The scout's first real scan, and the miss

```
scan-notes.md, 2026-11-04:
"Gulliver#38 was a FALSE reject: PRs #41 and #42 both close #37 ("gulliver_lint tool description says GV001-GV012"), not #38; the timeline cross-reference to #38 was noise."
(BrowserForge-MCP#17 was a correct reject: PR #23 closes #17.)
```

The scout has checked for beginner-friendly issues, and one of the issues I selected was graded based on checklists:
- There's no one being assigned.
- There's no linked PR in open state.
- Any claim comment in the thread was not old (was 90 days older or had no active linked PR behind it).
It's scanned, and one checklist item asks whether someone already works. It was the unclaimed check. The thing I did not see it miss is that someone has already been working on it. I still want to claim that issue and work. On its first real search, the scout wrongly told me Gulliver#38 was taken. It had counted pull requests that only mentioned that issue in passing, when they were really about a different issue.

### 3. The change I made

```
Before: "no linked PR is in the `open` state"
After:  "A PR counts as linked to THIS issue only if it closes it ... or its title or body names this issue's number; a timeline 'cross-referenced' event alone, from a PR whose text names a different issue, is not a claim on this one"
```
There was a change that I have to make. Instead of "no linked PR is in the open state," I rewrote the PR requirement. I got that by checking on hand, and I rewrote it so that it counts a pull request that is closed on the issues and named its id number. 

### 4. Choosing an issue my scout rejected

```
Five new tests, run against the ORIGINAL code:
tests/test_shotgrid_provider.py ..FF.
E   AssertionError: assert ['sg_type', 'in', ['Feature']] in [['users', 'is', {'email': 'artist@example.com', ...}]]
2 failed, 3 passed, 27 deselected

Same command after the fix: 5 passed, 27 deselected
Full suite: 860 passed, 92% coverage (project requires 90%)
```
For my issue (dna#254),  I selected because it's an open source issue that is active. My scouts rejected because there's an open PR that would likely be linked on closing this issue. 

I chose to claim it anyway to complete the project because the fix is very small and the other pull requests are stuck. I got it myself for the first contribution because the maintainer was more active, and it is not over 90 days or has no assignees to be set. I want to write the test, and without my change, none of this addition towards the issue would have worked. It all passed on my implementation and PR #292 was opened.

### 5. How it ended

On Saturday, November 21st, PR #262 got merged and my PR #292 closed for the contribution since it was already picked up by someone else. The scout's warning about #262 was right, and that no reviewer ever commented, so there was nothing to change.

## Limitations and what's next

There are two setbacks:
1. The unclaimed patch was never rerun against the unit 1 eval set.
2. I just reran the patch scout on the Gulliver #38, and it still rejects for a real reason: a PR got merged for one assignee, so the original false reject case cannot be replayed. I still don't have proof the patch fixed it.

Because I never re-ran my patch, the next thing I will do is to test on a new issue, so that I know whether scout no longer counts a patch it as a claim.

> Maintainer jspada200, closing PR #292 on 2026-11-21: "Sorry but this was already picked up by someone else"