# PHASE_SHIFTS_CLOSE_AND_SPLIT_INHERIT_BY_REFERENCE

**Org:** `RULES-OF`
**Domain:** `github-issue-lifecycle`
**Type:** Atomic rule
**Status:** Active

---

## The Rule

> When a thread crosses a real boundary — a scope change, or sheer size —
> close it as an archival record and open a successor that points back.
> The successor inherits by reference, not copy.

---

## Classes and objects in play

- **Phase boundary** — a scope change (design → execution, investigation →
  fix, audit → remediation) *or* a size threshold where the comment count
  itself becomes the obstacle (in practice, once a thread runs 40-80+
  comments, no agent can afford to read it in full before acting on it).
- **Archival record** — a closed issue, still fully readable, whose scope is
  now finished and bounded. Not deleted, not hidden — just no longer the
  live surface.
- **Successor** — a new issue scoped to the next phase, whose body links
  back to the archival record instead of restating its content.

## Semantics

Two failure modes, both wrong in opposite directions:

1. **Keep patching the same thread forever.** The body update rule
   ([`ISSUE_BODIES_CARRY_CURRENT_TRUTH_NOT_HISTORY`](./ISSUE_BODIES_CARRY_CURRENT_TRUTH_NOT_HISTORY.md))
   keeps the body honest, but doesn't bound the thread's *size*. A design
   discussion that finished pages ago, still open because execution notes
   kept landing in the same place, becomes unreadable — design history and
   execution status bolted onto one thread whose original scope closed long
   ago.
2. **Silently rewrite the body and orphan the history.** Applying rule 1
   carelessly: the body now says something true, but nothing links forward
   to where the *real* current work actually is, and nothing links backward
   to how the decision was reached. An agent landing on the old thread via a
   stale link or search hit finds a body describing a different job than the
   one the thread's history shows it doing.

The fix is neither: **close, with a plain evidence-backed reason, and open a
new issue for the next phase — cross-linked in both directions.**

## Why this ordering

This is the same shape as an archive/patch split in a stable-identity path
hierarchy: the closed issue becomes a complete record, addressable by its own
identity, and the successor references it rather than duplicating its
content. Copying loses the ability to trace back to *why*; referencing keeps
provenance intact while still giving the next phase a clean, boundable
surface to work in.

## Corollaries

- The trigger is either kind of boundary — scope *or* size — not just scope.
  A thread that never changed scope but crossed ~40+ comments still qualifies.
- "Cross-linked in both directions" is not optional politeness — an agent
  told "go work issue N" with no idea a predecessor or successor exists will
  either redo finished work or miss where the real state moved to.
- A registry that would itself need this treatment (accumulating one record
  per event, forever, with no natural close point) is a sign it was never a
  GitHub issue's job to begin with — see
  [`ENDURING_SUBSTRATES_CANNOT_BE_WRAPPED_BY_VOLATILE_ONES`](./ENDURING_SUBSTRATES_CANNOT_BE_WRAPPED_BY_VOLATILE_ONES.md).
  This rule is for threads that *can* reach a real close; a thing designed to
  never close needs a different substrate entirely, not a bigger version of
  this rule.

## Relationships (typed)

| Rule | Relationship |
|---|---|
| `ISSUE_BODIES_CARRY_CURRENT_TRUTH_NOT_HISTORY` | **Predecessor rule** — this rule picks up exactly where that one's own limit is reached (thread too large to keep patching a body against) |
| `ENDURING_SUBSTRATES_CANNOT_BE_WRAPPED_BY_VOLATILE_ONES` | **Boundary condition supplied by** — tells you when a thread's growth pattern means it should never have been a single-thread substrate at all, rather than periodically split |
| `RULES-OF/STABILITY_WRAPS_VOLATILITY` *(cross-domain)* | **Same underlying shape, different application** — "the successor inherits by reference, not copy" restates that repo's own corollary almost verbatim, applied to issue threads instead of filesystem paths |

---

*Rule formalized: 2026-08-03, extracted from a project-local operating-rules
file where it had been independently restated after recurring — generalized
here with all project/account specifics removed.*
