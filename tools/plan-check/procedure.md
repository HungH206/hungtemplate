# Procedure: how this skill grades a plan package

## Read order

1. Read the issue first: its title, body, stated trigger, and expected
   behavior. Write down in one line what behavior is broken.
2. Read the thread highlights next. Write down every comment from an
   OWNER, MEMBER, or COLLABORATOR that gives direction (names the
   culprit code, posts a patch or test build, picks a fix option,
   rules something out), and every PR number mentioned for this issue
   with its state if given.
3. Read the repro evidence before the plan. List each step and each
   control run as a pair: "what was varied" and "what happened". Mark
   which runs fail and which pass. This list is what the diagnosis
   will be checked against, so it has to exist before you read the
   plan's diagnosis; otherwise the plan's confident wording sets the
   frame.
4. Read the repo-facts block. Note the contribution policy line word
   for word.
5. Read the candidate plan: diagnosis, change list, scope lines,
   files, approach, test plan, risks.
6. Read the candidate plan comment last, as a maintainer on the thread
   would.

## Evidence gathering

For each check, pull exactly this before grading it (eval mode: from
the bundle only; live mode: the issue and thread from GitHub, the repro
evidence from the student's posted repro comment, the plan and comment
from the drafts):

1. Diagnosis fits the repro evidence: the plan's stated cause (quote
   it), and the run/control list from Read order step 3.
2. Scope is one bounded change: every distinct change the plan
   proposes, as a numbered list, and the issue's one-line broken
   behavior from Read order step 1.
3. A stranger could start executing it: the files or areas named, and
   the approach sentence. Note any hedge words ("somewhere", "not
   sure", "whichever", "maybe", "investigate", "profile").
4. Test plan names an observable outcome: the test plan text, quoted,
   and the repro command(s) it re-runs if any.
5. Unknowns stated, not dressed as certainty: every certainty phrase
   in the plan and comment ("root cause is", "confirmed", "red
   herring", "clearly") and the risk/unknowns lines.
6. Comment engages the thread's direction: the maintainer-direction
   list and PR list from Read order step 2, and whatever the plan and
   comment say about each item.
7. AI-use disclosed where the policy requires it: the policy line from
   Read order step 4, and any sentence in the plan comment about AI
   use.

## Check execution

1. Run the checks in rubric order, one at a time.
2. For each check, compare the gathered evidence to that check's pass
   condition only. Do not let a different check's failure decide this
   one; grade each independently.
3. Diagnosis: go through the run/control list one pair at a time and
   ask "does any part of the evidence contradict the stated cause?"
   A contradiction is a fail: the blamed part demonstrably works in a
   run where the bug still shows up, or the damage is already visible
   before the blamed code runs. A control the plan addresses with an
   explanation that the evidence does not contradict is not a fail,
   even if the explanation is unproven.
4. Scope: for each numbered change, ask "does the issue's broken
   behavior require this?" Regression tests for the fix, the same
   defect's sibling site in the same code, and notes for the PR count
   as required. Any change that is a migration, upgrade, new option or
   setting, UI rework, redesign, or a separate fix is extra; one extra
   change is a fail.
5. Executability: fail only if a deciding choice (where the change
   goes, or which approach) is left open. Exact function names pinned
   later are fine if the area and approach are fixed.
6. Test plan: pass only if you can name the observable result that
   would show success (an output, exit code, number, or visible
   behavior on a stated command or fixture).
7. Unknowns: fail only if a certainty phrase contradicts the run list
   or asserts something the package does not show.
8. Thread direction: if the maintainer-direction and PR lists are both
   empty, pass. Otherwise check each item is followed or addressed.
9. Disclosure: if the policy line does not affirmatively require
   disclosure on comments or on all AI use, pass without reading
   further. Otherwise pass only if the plan comment discloses.
10. If the evidence a check needs is genuinely absent from the package
    (not just hard to find), grade it `unclear` and say what is
    missing.
11. Write one evidence line per check: the quote or fact that decided
    it.

## Verdict assembly

1. Apply the rubric's verdict rule: accept only if every required check
   is `pass`.
2. Treat every `unclear` as `fail` when applying the rule.
3. If the verdict is reject, the summary names the first failing check
   in rubric order and quotes its evidence line.
4. In live mode, after the verdict, hold the plan comment against
   `voice-guide.md` and list any rule it breaks; this never changes
   the verdict.
5. Emit the JSON block with all seven checks in rubric order, last in
   the output.
