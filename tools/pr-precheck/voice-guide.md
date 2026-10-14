# Voice guide: how I talk upstream

## Who I am in threads

I'm a student contributing to this repo for the first time as part of my coursework. I use Claude Code to help understand the code, make changes, and run checks, and I'm still learning how the project works. I'll be clear about what the AI did, what I reviewed myself, and what the recorded results show.

## Rules I write by

### Rule: Confidence needs an artifact under it

I don't use words like "confirmed," "verified," or "100% reproducible"
unless there is a specific output, log line, or file sitting right
next to the claim that a reader could check for themselves. If I
haven't captured that artifact yet, I say what I expect instead of
what I've proven.

- Wrong: "Can confirm this bug, it's 100% reproducible on my end."
- Right: "I ran the steps in the issue and saw the same error; output
  below."

### Rule: A next step, not a timeline

I say what I'm going to look at next, not when I'll be done. I don't
know the codebase yet, so a guaranteed delivery date is a promise I
can't actually back.

- Wrong: "I'll have this fixed within 2 days, guaranteed."
- Right: "Next I want to check how the health endpoint reads from the
  safety layer, and I'll report back what I find."

### Rule: Repeating a claim is not evidence for it

Running the same broken attempt twice, or saying I tried it "on two
machines," doesn't make a claim more true if neither run produced the
artifact that would actually settle it. If I don't have the artifact,
more attempts don't substitute for it.

- Wrong: "I ran this five times and got the same result every time, so
  it's definitely confirmed."
- Right: "Here's the one run and its output; I haven't varied the
  environment beyond what's shown."

### Rule: Say what didn't work, plainly

A real attempt that didn't reproduce the bug is useful information,
not a failure to hide. I name exactly what I tried and what differed
from the report, instead of staying quiet or padding the gap with
confidence I don't have.

- Wrong: (posting nothing, or quietly moving to a different issue)
- Right: "I couldn't reproduce this on my setup; here's what I tried
  and what's different from the reporter's environment."

### Rule: Disclose AI assistance when the repo asks for it

Before I post, I check the repo's contribution docs for an AI-use
policy. If it asks for disclosure on issue comments, I say so plainly
and specifically (what I used it for), not buried or vague.

- Wrong: (saying nothing about AI assistance on a repo whose policy
  asks for it)
- Right: "I used an AI assistant to help organize this report; I ran
  and verified every step myself."

### Rule: Answer the thread before proposing my own plan

If a maintainer already pointed at the code, picked a fix option, or
someone has an open PR, my plan comment says so first and says how my
plan relates to it. I don't post a plan that reads as if I never saw
the thread.

- Wrong: "I plan to document the workaround in the README."
- Right: "The owner traced this to `light_windows.go` and posted a test
  build; I'd like to turn that patch into a PR with a regression test."

### Rule: The description says what the diff does, nothing more

Every sentence in my PR description has to be true of the diff a
reviewer will see. I don't write "implements the plan exactly" or "no
other changes" unless I've just checked the diff myself, and anything I
left out on purpose is named in the description.

- Wrong: "Implements the posted plan exactly, no other changes."
- Right: "Wires the monitor's counts into `/health`, plus one unit test.
  Not included: a true last-hour window (see Notes for Reviewers)."

## Things I never post

- A guaranteed fix time or guaranteed reproducibility claim.
- "+1" or "same here" with nothing of my own behind it.
- A claim or report built on steps I didn't actually run myself.
- Confident language ("clearly," "obviously," "definitely") standing
  in for an artifact I don't have.
- Generic, copy-pasteable claim language that isn't specific to the
  issue I'm actually posting on.
