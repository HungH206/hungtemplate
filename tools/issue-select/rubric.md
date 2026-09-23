# Rubric: is this a good first issue?

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| Maintainer alive | The repo's `archived` flag; "last push to any branch"; the "maintainer first-response sample" (live mode: the repo front page's archived banner and newest commit date; the first owner/member/collaborator reply on a handful of recently-updated issues) | Repo is not archived, AND at least one of: (a) the last push to any branch is within 90 days of the capture/current date, or (b) at least one entry in the maintainer first-response sample shows a numeric response time from an Owner/Member/Collaborator ("no maintainer comment in thread" does not count) | required |
| Repo in use | "last push to any branch", "latest release", `archived` flag (live mode: the same front-page fields, plus the Releases box) | Repo is not archived, AND at least one of: (a) last push within 180 days of the capture/current date, or (b) latest release within 365 days of the capture/current date | required |
| Scope fits a newcomer | The issue body and its comment thread | Fails only when one of these is true: (a) the issue is a tracking issue whose body is itself a list of OTHER issue/PR numbers or independently-assignable items meant for different contributors to split up (e.g. "anyone could try their hand" at picking one), or the work is open-ended/codebase-wide with no single location ("anywhere in the codebase where it makes sense"); (b) the thread shows an unresolved design debate that no maintainer has settled; (c) a maintainer states outright that the fix touches core/deep internals; (d) it is a pure usage/support question, not a change request; (e) it has sat open for years with abandoned (closed, unmerged) attempts behind it. Touching several files or listing several concrete items is NOT by itself an umbrella: a change that is deliverable in one pull request by one contributor is bounded even if it spans multiple files or file edits (e.g. a documentation task updating five pages for one feature, or a bug affecting several named-but-related items), or names several contributing causes or possible fixes for ONE broken behavior — grade whether the whole ask fits in one PR, not the file count or the writeup's polish. For a feature/enhancement request specifically (not a bug report), also fail if the reporter leaves part of the design genuinely undetermined (e.g. an unresolved asset, a "not sure yet"/"TBD", "alternatives none identified") and no maintainer has settled it in the thread | required |
| Unclaimed | "this issue: assignees" and "linked PRs" lines; claim comments in the thread ("I'll take this" / "working on this" and whether a maintainer answered) | No assignee is set, no linked PR is in the `open` state (a `closed`, non-merged PR is a stale/abandoned attempt and does not fail this check), and any claim comment in the thread is either more than 90 days old relative to the capture/current date, or has no active linked PR behind it | required |
| Contribution policy allows AI-assisted work | The "contribution policy" line (`CONTRIBUTING.md`, a dedicated AI-policy file, or issue/PR templates) | Fails only when the policy is an outright ban with no carve-out for human-reviewed or assistive AI use (e.g. "we do not accept AI-generated code or documentation," stated with no exception). Passes when the policy allows AI use under conditions (disclosure, personal understanding, testing, human review) even if it rejects unreviewed or fully-AI-generated submissions, and passes when there is no stated policy | required |
| Good-first-issue label | The issue's labels | Issue carries a "good first issue" label or clear equivalent | preferred |

## Verdict rule

Accept only if all five required checks pass. Reject if any required check
fails. Treat `unclear` on a required check as a fail: a first issue whose
liveness, scope, claim status, or policy cannot actually be verified from
the evidence is not one to take on faith. The preferred check never
changes the verdict; use it only to rank issues the required checks
already accepted, alongside the fit profile in `scope.md`.
