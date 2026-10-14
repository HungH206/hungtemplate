# Procedure: how this tool grades a PR package

## Read order

1. Read the issue (eval: the "Issue" section; live: the issue on
   GitHub). Write down in one line the behavior that is broken.
2. Read the thread (eval: "Thread highlights"; live: the issue's
   comments). Write down any maintainer direction and any requirement a
   maintainer stated for the fix.
3. Read the plan before the PR (eval: "Plan context"; live: `plan.md`).
   Write down four lists: (a) the changes the plan promises, (b) the
   files it names, (c) everything its not-in-scope or "known limit"
   lines exclude or defer, (d) every repro, failure mode, and suite run
   its test plan names, with the expected-after for each. Then read its
   deviation notes and update the four lists with what they add, drop,
   or defer. These lists are the yardstick; build them before reading
   any of the PR so the description's wording cannot set the frame.
4. Read the diff next (eval: "Diff"; live: `git diff main...HEAD`), then
   the commit list (eval: "Commits"; live: `git log --oneline
   main..HEAD`).
5. Read the test evidence (eval: "Test evidence"; live:
   `test_evidence.md` and any output pasted in the description).
6. Read the repo's stated standards (eval: the repo-facts "pull
   requests" and "contribution policy" lines; live:
   `.github/PULL_REQUEST_TEMPLATE.md` and `docs/CONTRIBUTING.md`).
7. Read the title and description last (eval: "Title" and
   "Description"; live: `pr_draft.md`), as a reviewer would.

## Evidence gathering

For each check, gather exactly this before grading it:

1. Diff stays inside the plan: a list of every hunk as "file: what it
   changes", side by side with the plan's lists (a), (b), and (c).
2. Plan delivered as the description claims: the plan's list (a), side
   by side with the hunk list; and every sentence in the description
   that claims what the PR does, does not do, or matches ("exactly as
   planned", "no changes beyond", "now documents").
3. Test evidence shows the fix on the plan's repro: the plan's list
   (d), side by side with each command or scenario in the test
   evidence, its before output, and its after output.
4. Diff is free of debris: every line in the diff that is not part of
   the fix or its tests, quoted with its file; and the commit messages.
5. Repo's PR template and asks are honored: every required section,
   line, checklist item, and diff artifact the template or contributing
   guide states, as a list, each marked present or missing in the
   description or diff.
6. AI use disclosed where required: the policy line quoted, and any
   sentence in the description about AI use. In live mode, note that
   the course always requires the disclosure.

## Check execution

1. Grade the checks in rubric order, one at a time, each against its
   own pass condition only. A failure in one check never decides
   another.
2. Diff stays inside the plan: go through the hunk list one hunk at a
   time and ask "is this the planned fix, its tests, or covered by a
   deviation note?" One hunk that is none of these is a fail. Do not
   accept the description's word for what a hunk does; read the hunk.
3. Plan delivered as the description claims: for each item in list
   (a), find the hunk that delivers it. If none does, look for a
   deferral of that item in the plan's deviation notes or the
   description; missing with no disclosure is a fail. Then check each
   claim sentence against the hunk list; one false claim is a fail.
4. Test evidence: split list (d) into repros of the bug (each failure
   mode the plan reproduces) and secondary checks (smoke checks,
   controls expected unchanged, suite or check runs). For each repro of
   the bug, find a before and an after in the evidence that runs it
   through the changed code; the after must show the plan's
   expected-after. For each secondary check, find its outcome stated.
   One named repro of the bug with no before/after, evidence that only
   runs a control or an unchanged path, or "passes"/"works" with no
   output shown for the repro, is a fail. A secondary check with only
   its outcome stated passes.
5. Debris: if the line list from Evidence gathering step 4 has any
   entry that is debug output, commented-out code, a dead or unused
   function, a stray TODO, or unrelated formatting or import churn, it
   is a fail.
6. Template and asks: if any required item on the list is missing, it
   is a fail. A section counts as present if its content is there under
   any heading. If the repo states no template, check only the
   contributing guide's stated asks.
7. Disclosure: in eval mode, if the policy does not require disclosure,
   pass without reading further; otherwise pass only if the description
   discloses. In live mode, pass only if the description discloses in
   the student's own words.
8. If the evidence a check needs is genuinely absent from the package
   (not just hard to find), grade it `unclear` and name what is
   missing.
9. Write one evidence line per check: the hunk, quote, or fact that
   decided it.

## Verdict assembly

1. Apply the rubric's verdict rule: accept only if every required
   check is `pass`; any `fail` or `unclear` makes it reject.
2. If the verdict is reject, the summary names each failing check and
   quotes its deciding evidence, first failing check first.
3. In live mode, after the verdict, hold the title and description
   against `voice-guide.md` and list any broken rule; this never
   changes the verdict.
4. Emit the JSON block with all six checks in rubric order, as the
   last thing in the reply.
