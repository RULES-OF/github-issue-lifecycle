# ISSUE_BODIES_CARRY_CURRENT_TRUTH_NOT_HISTORY

**Org:** `RULES-OF`
**Domain:** `github-issue-lifecycle`
**Type:** Atomic rule
**Status:** Active

---

## The Rule

> The body is the current-state summary.
> Comments are the history.
> An agent that reads only the body must still learn the truth.

---

## Classes and objects in play

- **Issue body** — the one field every reader sees first, and the only field
  a context-constrained agent may read at all.
- **Comment stream** — an ordered log of what was learned, decided, or done
  *at the time each comment was written*. Individually accurate, collectively
  stale the moment a new fact lands.
- **Reader** — not assumed to read the whole thread. May be a fresh agent
  instance with no memory of the conversation the fact was stated in.

## Semantics

A comment is a timestamped claim, correct as of its own timestamp. The body
has no timestamp semantics — it is read as *now*, regardless of when it was
last edited. Any fact that changes after the body was written and is *not*
folded back into the body creates a body that actively misleads, not just
one that's incomplete. Silence in the body is read as "still true."

## Why this ordering

**The body is the interface; the comments are the implementation log.**
Same relationship as a function's docstring to its commit history — the
docstring must describe current behavior even though the commits show how it
got there. Nobody reads every commit before calling the function.

The cost of getting this backwards is not cosmetic: an agent that trusts a
stale body will re-derive an already-known answer, re-propose an
already-rejected fix, or re-ask a question already answered — burning real
work re-deriving what one edit would have preserved.

## Corollaries

- Updating the body is not optional follow-up work to get to later. The
  instant a fact stated in conversation changes an issue's real status, that
  update is part of the same unit of work as learning the fact — not a
  separate task queued behind it.
- This is not "delete the comments" — the history stays. It's "the body
  never lags behind what the comments already collectively prove."
- This rule has a size limit before it stops being sufficient on its own —
  see [`PHASE_SHIFTS_CLOSE_AND_SPLIT_INHERIT_BY_REFERENCE`](./PHASE_SHIFTS_CLOSE_AND_SPLIT_INHERIT_BY_REFERENCE.md).
  Rewriting the body keeps it *true*; it doesn't keep the *thread* readable
  once the comment count itself becomes the obstacle.

## Relationships (typed)

| Rule | Relationship |
|---|---|
| `PHASE_SHIFTS_CLOSE_AND_SPLIT_INHERIT_BY_REFERENCE` | **Successor rule** — handles the case this rule alone can't fix (thread too large to keep patching a body against) |
| `ENDURING_SUBSTRATES_CANNOT_BE_WRAPPED_BY_VOLATILE_ONES` | **Sibling, same domain** — no direct dependency; both are about GitHub-issue-as-substrate failure modes but address different failure shapes (staleness vs. survivability) |

---

*Rule formalized: 2026-08-03, extracted from a project-local operating-rules
file where it had been independently restated after recurring — generalized
here with all project/account specifics removed.*
