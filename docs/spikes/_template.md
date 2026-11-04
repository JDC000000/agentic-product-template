# Spike S<N>: <short name>

> Template for a Gate 0 spike. Copy to `docs/spikes/S<N>.md` and fill in.
> A spike is a time-boxed investigation whose output is a decision, not a feature.
> TODO: review once the-wip and 24-hours-of-power are compared.

## Spike

<!-- What question are we answering? Frame it as a question, not as a task.
     Good: "Can we separate stems at <3× real-time on a single Fly A100?"
     Bad:  "Build a stem separator." -->

## Success criterion

<!-- The ONE measurable outcome that decides the spike. Write the number in,
     not "fast enough". -->

## Date

<!-- When was this spike run? YYYY-MM-DD. -->

## Status

<!-- One of: open, in-progress, done, abandoned. -->

## What exercised

<!-- What did we actually run / build / test? Include commit SHAs, commands,
     and the environment (local Docker, Fly machine, CI, etc.) so another
     agent can repro. -->

## Measured results

<!-- Numbers only. No "it felt fast". Attach raw timing / cost / token /
     error-rate output; link out to logs or fixture artefacts. -->

## Honest caveat

<!-- The thing that would make this measurement wrong: a cache hit we didn't
     notice, a fixture that's easier than real inputs, a budget that moves
     under load. Say it in one short paragraph. A spike without a caveat is
     a spike that didn't learn anything. -->

## Decision

<!-- One of:
     - go      — the criterion was met; ship the design the spike exercised.
     - pivot   — the criterion was missed but the shape of a working design
                 is now clear; name it and open the follow-up spike.
     - stop    — abandon this path; the budget doesn't exist here.
     Record WHO decided and WHEN, not just what. -->
