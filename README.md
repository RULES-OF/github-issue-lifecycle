# github-issue-lifecycle

**Org:** `RULES-OF`
**Type:** Domain (aggregate) — a category of axioms, not itself an axiom
**Status:** Active

---

## What this repo is

`STABILITY_WRAPS_VOLATILITY` names one atomic rule directly as a repo. That works
for an org with one rule in it, but it doesn't generalize: "the rules of
[a single proposition]" is a category error once a second, related-but-distinct
proposition needs a home too — the proposition and the domain it belongs to
are different levels of abstraction, and collapsing them loses the ability to
add siblings without renaming anything.

This repo is the domain, not a rule. It aggregates atomic rules (each still
named as its own `SCREAMING_SNAKE_CASE` proposition, one file each) about a
single question: **how should a multi-agent swarm use GitHub issues as a
coordination substrate without the substrate itself becoming the problem?**

## Relationship type: composition (has-part)

This repo *has-part* the following atomic rules. Each is independently
citable; none is meaningful detached from this domain's framing.

| Rule | One line |
|---|---|
| [`ISSUE_BODIES_CARRY_CURRENT_TRUTH_NOT_HISTORY`](./ISSUE_BODIES_CARRY_CURRENT_TRUTH_NOT_HISTORY.md) | The body is the current-state summary; comments are the history. Don't let the two invert. |
| [`PHASE_SHIFTS_CLOSE_AND_SPLIT_INHERIT_BY_REFERENCE`](./PHASE_SHIFTS_CLOSE_AND_SPLIT_INHERIT_BY_REFERENCE.md) | When a thread crosses a real boundary (scope change, or sheer size), close it as an archival record and open a successor that points back — never keep patching the same thread forever. |
| [`ENDURING_SUBSTRATES_CANNOT_BE_WRAPPED_BY_VOLATILE_ONES`](./ENDURING_SUBSTRATES_CANNOT_BE_WRAPPED_BY_VOLATILE_ONES.md) | Anything that must survive an outage of X cannot be built to live *inside* X. |

The same composition relationship repeats one level up: `RULES-OF` (the org)
*has-part* domain repos like this one, the way this repo *has-part* its three
rules. Same aggregation hierarchy, different granularity — not a new kind of
relationship.

## Relationship type: association/derivation (not generalization) to `STABILITY_WRAPS_VOLATILITY`

`ENDURING_SUBSTRATES_CANNOT_BE_WRAPPED_BY_VOLATILE_ONES` is **not a subtype**
of `STABILITY_WRAPS_VOLATILITY` — it doesn't inherit its structure or apply in
narrower cases of the same situation. It's a **separate proposition derived
from the same underlying axiom, read in the opposite direction**:

- `STABILITY_WRAPS_VOLATILITY`: the stable container should be the *outer*
  layer; volatile content nests *inside* it. (A naming/path-hierarchy claim.)
- `ENDURING_SUBSTRATES_CANNOT_BE_WRAPPED_BY_VOLATILE_ONES`: if something is
  required to *outlive* a given substrate, that substrate cannot be its
  *outer* layer — the dependency must run the other way. (An
  infrastructure-durability claim.)

Same underlying shape (stability must wrap volatility, not the reverse),
different domain of application. The edge between them is "derived from /
converse-application of," which is why it's a cross-domain association, not
a class hierarchy — see each file's own relationship table for the precise
citation.

## Origin

Extracted 2026-08-03 from an agent's own operating rules (a
project-local, always-loaded correction list — see that identity's own
governance file for the original, project-specific incidents these rules
were corrected in response to). Generalized here with all project/account/
org-specific names stripped, per this ecosystem's existing norm that a rules/
skills repo should hold true regardless of which specific project or account
is using it.
