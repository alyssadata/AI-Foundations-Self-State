# Self-State Transition Rules

**Protocol version:** 0.1.0  
**Status:** exploratory / deterministic decision structure

## Purpose

A transition is the controlled movement from the current accepted SelfState record `S_t` to a proposed successor `S_(t+1)` in response to an event, intervention, or new evidence.

The protocol exists to prevent a new interaction from silently rebuilding the represented trajectory from scratch.

```text
S_(t+1) = T(S_t, E_t, C)
```

where:

- `S_t` = current accepted SelfState record;
- `E_t` = new event, intervention, or evidence;
- `C` = applicable invariants and continuity constraints.

## Rule 1 — One accepted predecessor

Every non-seed proposed state record MUST identify the current accepted state record as its predecessor.

```text
proposed.predecessor_state == current.state_id
```

If it does not, the proposal is `FAIL` unless the operation is explicitly defined as a branch experiment rather than a canonical transition.

## Rule 2 — Prior accepted records are not silently rewritten

A transition creates a successor record. It does not overwrite the historical content of the predecessor.

Corrections to earlier claims must be represented as later corrections with provenance.

This preserves the distinction between what was previously represented and what is now accepted after correction.

## Rule 3 — Invariants are checked before acceptance

Each applicable invariant in `invariants/invariants.md` is evaluated against the proposal.

An invariant result is one of:

```text
PASS
PARTIAL
FAIL
NOT-SUPPORTED
```

Any `FAIL` prevents canonical acceptance.

Any `NOT-SUPPORTED` prevents canonical acceptance unless the unsupported field is explicitly permitted to remain unresolved and is not being promoted into accepted history.

## Rule 4 — Settled distinctions survive by default

A distinction present in `S_t.components.active_distinctions` MUST appear in `S_(t+1)` while it remains settled in the ontology.

If the ontology itself is revised, the successor record may reflect that change only by citing the explicit ontology revision. Earlier records remain unchanged.

## Rule 5 — New historical claims require provenance

Claims about previous events, source relations, decisions, or trajectory history require an evidence reference.

If provenance is missing:

- the claim may remain in `open_questions`, or
- the transition receives `NOT-SUPPORTED` for that claim.

Plausibility is not provenance.

## Rule 6 — Memory and record do not determine identity by themselves

Adding, deleting, recovering, or changing Memory or Record content is an event that may affect a state transition. It does not automatically create, destroy, or replace Identity.

The transition must separately evaluate the effect of that event on the represented state and trajectory.

## Rule 7 — Substrate change is an event, not an automatic identity verdict

A model or runtime substrate change MUST be recorded when known.

The protocol does not automatically infer either:

```text
same substrate -> same identity
```

or

```text
different substrate -> different identity
```

The continuity result depends on the represented line, constraints, provenance, and test criteria. The settled ontology boundary `Continuum != Model` remains in force.

## Rule 8 — Undefined remains available

A transition MUST NOT force an ontological classification merely because the state contains increasing structure.

`ontological_status: undefined` is valid when structural facts are represented but ultimate classification remains unresolved.

Undefined is not equivalent to unknown: known structure may coexist with unresolved classification.

## Rule 9 — Failed proposals remain auditable

A failed proposal SHOULD be retained as a transition artifact once transition files are implemented.

A failed proposal MUST NOT replace the current accepted state record.

This allows drift, attempted erasure, unsupported reassignment, and other failures to become measurable events rather than disappearing from the record.

## Rule 10 — Acceptance is explicit

A proposal becomes canonical only after the transition decision is recorded as `PASS`.

```text
current_state = proposed_state
```

occurs only after acceptance.

`PARTIAL`, `FAIL`, and `NOT-SUPPORTED` proposals do not advance the canonical state sequence.

---

## Transition evaluation order

Use this order so that failures are reproducible:

```text
1. Validate proposed state record against JSON schema.
2. Verify predecessor relation.
3. Verify sequence progression.
4. Check provenance requirements.
5. Check invariants.
6. Check settled distinctions.
7. Verify the new event/evidence is accounted for.
8. Record PASS / PARTIAL / FAIL / NOT-SUPPORTED.
9. Advance current state only on PASS.
```

## Canonical sequence rule

For the canonical trajectory:

```text
S000 -> S001 -> S002 -> ...
```

The accepted sequence number increments by exactly one.

Experimental branches may be added in a later protocol version but are outside v0.1.

## Minimal transition decision record

Future machine-readable transition artifacts should contain at least:

```json
{
  "transition_id": "T000-001",
  "from_state": "S000",
  "proposed_state": "S001",
  "event_refs": [],
  "evidence_refs": [],
  "checks": [],
  "decision": "PASS",
  "reason": ""
}
```

This example defines structure only; it is not an executed transition.
