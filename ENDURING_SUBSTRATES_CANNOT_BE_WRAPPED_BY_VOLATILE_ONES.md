# ENDURING_SUBSTRATES_CANNOT_BE_WRAPPED_BY_VOLATILE_ONES

**Org:** `RULES-OF`
**Domain:** `github-issue-lifecycle`
**Type:** Atomic rule
**Status:** Active

---

## The Rule

> Anything required to *outlive* a given substrate cannot be built to
> *live inside* that substrate.
> If X must survive an outage of Y, X cannot be a feature of Y.

---

## Classes and objects in play

- **Substrate** — the platform a system is built on top of (here: GitHub's
  issue/comment API). Has its own uptime, its own outage modes, independent
  of anything built using it.
- **Enduring system** — something explicitly required to keep working, or at
  least be recoverable, when the substrate it coordinates *about* is down.
  A cross-agent session registry is the canonical example: its entire reason
  to exist is answering "who's active and doing what" even in conditions
  that include "GitHub itself is unreachable."
- **Append-only comment log** — a data store implemented as one GitHub issue,
  where every new record is one more comment, forever. Looks structured
  (JSON blobs, consistent shape) but the medium is still GitHub's comment API.

## Semantics

This is not primarily about size (see
[`PHASE_SHIFTS_CLOSE_AND_SPLIT_INHERIT_BY_REFERENCE`](./PHASE_SHIFTS_CLOSE_AND_SPLIT_INHERIT_BY_REFERENCE.md)
for the size failure mode). A session registry stored as GitHub issue
comments would still violate this rule even at a *small*, perfectly-readable
size — the violation is structural, not volumetric. The registry's whole job
is to be the thing you can still query when GitHub is unavailable. Storing it
*inside* GitHub gives it exactly zero resilience to the one failure mode it
was built to survive. It isn't a cheaper, smaller version of the right
system — it's incapable of the one thing that made building it worthwhile.

## Why this ordering

This is the converse application of `RULES-OF/STABILITY_WRAPS_VOLATILITY`'s
"what endures wraps what changes" — read in the *durability* domain instead
of the *naming/path* domain it was originally formalized in:

- Original claim: the stable container should be the *outer* layer; volatile
  content nests inside it.
- This claim: if a system must be *more durable* than a given substrate, that
  substrate cannot be its outer layer — the containment has to run the other
  way, or the enduring system inherits the substrate's fragility as its own.

Same underlying shape (durability flows outward-in, never inward-out),
applied to infrastructure dependency instead of path hierarchy. Not a
subtype of that rule — a distinct proposition the same axiom implies once
you ask "what if the volatile thing is the whole platform, not just a value
inside it?"

## Corollaries

- A system can *use* a volatile substrate as one input channel (e.g., GitHub
  webhooks feeding a registry) without being *wrapped by* it — the
  distinction is whether the enduring system's own state lives on the
  substrate, or the substrate is just one of several signals feeding a state
  that lives elsewhere.
- "It's just comments for now, we'll migrate later" does not make this rule
  inapplicable in the interim — every day it operates as a GitHub-issue log
  is a day the registry has the resilience of a system with no registry at
  all, silently.
- This is a recurring institutional mistake, not a one-off: the same shape
  (durable-thing-implemented-inside-fragile-thing) has been independently
  built and independently corrected more than once in at least one swarm
  this rule was extracted from, without the correction propagating to a
  later instance that rebuilt the same pattern. Citing this rule by name,
  in the artifact itself, is intended to make the correction discoverable by
  a fresh agent instance that never saw the earlier correction happen.

## Relationships (typed)

| Rule | Relationship |
|---|---|
| `PHASE_SHIFTS_CLOSE_AND_SPLIT_INHERIT_BY_REFERENCE` | **Supplies a boundary condition to** — a growth pattern with no natural close point is evidence a thread was never a valid substrate for this content, rather than evidence it needs splitting again |
| `ISSUE_BODIES_CARRY_CURRENT_TRUTH_NOT_HISTORY` | **Sibling, same domain** — no direct dependency |
| `RULES-OF/STABILITY_WRAPS_VOLATILITY` *(cross-domain)* | **Converse-application of** — same axiom, read in the durability/dependency domain rather than the naming/path domain it was formalized in |

---

*Rule formalized: 2026-08-03, extracted from a project-local operating-rules
file where it had been independently restated after recurring — generalized
here with all project/account specifics removed.*
