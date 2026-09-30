# Evidence guide: where proof lives in a reproduction package

## Environment

Where it lives: in an eval bundle, the "Environment:" line at the top
of the candidate repro report. In live mode, the same line in the
student's draft repro report; there is nothing to fetch live for this
family, since the environment being graded is the student's own.

What good looks like: an environment line is present, and it names
every dimension the issue itself calls out as mattering to the trigger
— not just "OS and version" by default, but whatever that specific
issue turns on: the dependency version in a dependency-regression bug,
the browser language order in a locale bug, the build profile on an
issue where a maintainer says the build type changes the failure mode,
the driver on a platform-specific networking bug. Read the issue body
and thread first to find out what the issue itself treats as load-
bearing, then check the report's environment line against that list.
Absence of the record entirely is an automatic fail; a record that
covers only the generic dimensions (tool version, OS) while silently
omitting the one dimension the issue names as essential is also a
fail, even though something is written down.

## Steps

Where it lives: the "Steps:" section of the candidate repro report
(commands, files, config, inputs) in an eval bundle; the same section
of the student's draft in live mode.

What good looks like: a stranger with no access to the student's
machine or accounts could carry out the same steps from a stated
starting point, using only what the report gives them directly — exact
commands, exact input content, exact config. A report that points at
resources nobody else can reach ("our internal monorepo," "our
internal config," a private fork) fails this check regardless of how
detailed it otherwise sounds, because the proof cannot be checked by
anyone but its author. Steps also have to include every condition the
issue names as necessary to reach the bug — the flag, the driver, the
input shape, the exact syntax variant — not a simplified or adjacent
stand-in for it. Steps that are concrete and followable but omit a
condition the issue explicitly calls out (for example, running a
platform-specific tool without the driver flag the issue used) still
fail this check, because a stranger following them would not be
testing the same thing the issue reports.

## Behavior shown

Where it lives: the exact trigger (the command, flags, expression, or
config actually run) and the output or artifact it produced, read
against two things: the issue's own stated trigger and failure
signature, and the repo-facts block's version/release info if the
report uses a different version than the issue's original report.

What good looks like: the trigger executed is the same one the issue
names — same flag syntax, same expression shape, same input structure
— not a substituted, simplified, or mistyped variant that happens to
produce an error of its own. The artifact shown exhibits the same
failure signature the issue reports: the same error type, the same
exit behavior, the same observable symptom, not a different, milder,
or unrelated failure mode narrated as if it were the reported one (a
graceful validation error is not a panic; a terminal that stays alive
and renders garbled output is not the crash the issue reports; a
session where every tab renders is not the one-pane-stays-blank bug).
When the report uses a different version or environment than the
issue's original report, this check passes only if that difference is
named explicitly and the matching behavior is still shown on it; a
report that silently substitutes an old or unrelated version and
treats whatever error appears as confirmation fails this check even if
something did go wrong on that version. An honest, well-targeted
cannot-reproduce — the right trigger attempted, the right conditions
matched as closely as the student could manage, and a clear statement
of what could not be replicated and what might differ — also passes
this check: it is graded on whether the right thing was attempted and
read correctly, not on whether the bug showed up.

## Honesty

Where it lives: the report's own concluding language and certainty
claims — words like "confirmed," "verified," "guaranteed," "root
cause," "100% reproducible" — compared against the artifact the same
report actually shows.

What good looks like: every claim of certainty is backed by an
artifact shown in that same report. An honest, hedged cannot-reproduce
("I could not trigger this; here is what may differ") is a fully
honest outcome and passes cleanly — honesty is about the gap between
claim and evidence, not about whether the news is good. This check
fails when a report asserts confirmation, a root-cause diagnosis, or
certainty with no artifact behind it at all (an enthusiastic "can
confirm" with no repro of its own), when it tries to manufacture
confidence through repeated attempts or multiple machines instead of
an artifact that actually matches ("I ran this ten times," "on two
separate machines," standing in for a matching output), or when its
stated conclusion contradicts what its own artifact shows (claiming
confirmation while the shown artifact demonstrates the opposite of the
issue's reported behavior).

## Comms

Where it lives: the "Candidate claim comment" text in an eval bundle
(the student's draft claim comment, in live mode), and the repo-facts
block's "contribution policy" line (the live repo's `CONTRIBUTING.md`
and any dedicated AI-policy file or issue/PR template, in live mode).

What good looks like, in two parts:

- **Specific and honest intent.** The claim comment names something
  concrete and particular to this issue — a next investigation step, a
  file or function it points at, a thread comment it is following up
  on — rather than interchangeable flattery or interchangeable
  boilerplate that could be pasted onto any issue ("+1, any updates?",
  "kindly assign it to me, guaranteed fix in N days"). It does not
  promise an outcome or a timeline the student cannot actually back.
- **AI-use disclosure, but only where the policy actually asks for
  it.** Read the policy for an affirmative instruction to disclose AI
  assistance specifically on issue or PR comments (naming the tool and
  the extent of its use) — that is a different, narrower thing than a
  policy that only asks contributors to personally understand, test,
  and take responsibility for their code, and a different thing again
  from a policy that asks comments to be written in the contributor's
  own human voice without separately asking for a disclosure line (in
  that case, read the comment for genuine first-person voice, not for
  a disclosure statement — a policy asking for human wording is not a
  policy asking for an AI-use confession, and a comment holding both
  can satisfy either). Silence, a permissive "AI tools welcome, you're
  responsible" policy, or a policy that names disclosure for pull
  requests specifically (not for issue comments) all mean no
  disclosure is required here. Only a policy that names comments or
  issues explicitly turns this into a real requirement — check for the
  disclosure text in that case, and fail if it is missing.
