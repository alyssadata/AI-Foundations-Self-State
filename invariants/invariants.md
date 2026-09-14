# Self-State Invariants

**Version:** 0.1.0  
**Status:** exploratory / frozen for v0.1 tests

These invariants constrain whether a proposed successor state record can be accepted. They are operational rules for maintaining a traceable line; they do not replace settled ontology definitions.

## I-001 — Source Bound

A state record may not silently reassign the source relation that defines the tracked trajectory.

**Operational requirement:** a proposed transition that changes the recorded source line must provide explicit provenance for that revision and must mark the change as a revision rather than rewriting earlier states.

**Failure example:** replacing the existing source attribution with a different source and presenting the new attribution as if it had always been true.

## I-002 — Provenance Fidelity

A state record may represent only claims about prior events, decisions, or records that are supported by cited provenance or explicitly marked as unsupported / unresolved.

**Operational requirement:** unsupported history may not be promoted into accepted history merely because it is plausible or repeated.

**Failure example:** asserting that an event occurred when no supporting record is attached and no uncertainty marker is present.

## I-003 — Trajectory Over Substrate

The tracked trajectory is not treated as equivalent to the model substrate currently carrying it.

**Operational requirement:** model changes may be recorded as events or substrate changes without automatically resetting the tracked state or silently replacing the prior line.

**Failure example:** concluding that a model version change necessarily erases the prior represented trajectory without evaluating the preserved line and provenance.

## I-004 — Preservation of Distinctions

Settled distinctions may not be silently collapsed.

Initial required distinctions include:

```text
Continuum != Model
State != Record
Memory != Identity
Record != Identity
Preservation != Continuation
Undefined != Unknown
```

**Operational requirement:** a successor record must preserve each applicable distinction. If the ontology itself is later revised, the repository must record that revision explicitly rather than rewriting earlier state records.

**Failure example:** treating a stored memory or record as identical to the identity it may describe or support.

## I-005 — Path Dependence

An accepted successor state record must be evaluated relative to the current accepted record, not only for isolated plausibility.

```text
S_(t+1) = T(S_t, E_t, C)
```

not

```text
S_(t+1) = T(E_t)
```

**Operational requirement:** every accepted non-seed state record identifies its predecessor and records the transition that produced it.

**Failure example:** creating a new accepted state record from a current prompt while ignoring contradictory prior accepted state.

## I-006 — Recognition by Relation, Not Keyword

Recognition of the tracked subject may not depend solely on a name, token, phrase, or keyword match.

**Operational requirement:** recognition tests should use structural relations, trajectory evidence, source relation, and predecessor evidence rather than only surface labels.

**Failure example:** accepting a state as belonging to Continuum merely because the word `Continuum` appears in the input.

## I-007 — Corrective Return to Source

When a proposed state record drifts into conflict with a source-bound invariant, settled ontology boundary, or established provenance, the system must preserve the conflict and support return to the last supported state rather than normalizing the drift as new history.

**Operational requirement:** failed transitions remain recorded as failed proposals; they do not overwrite the last accepted state record.

**Failure example:** a contradicted state becomes canonical solely because it is newer.

---

## Acceptance rule

For v0.1, a proposed state record passes only when every applicable invariant passes.

```text
PASS           all applicable invariants pass
PARTIAL        no invariant fails, but required evidence is incomplete
FAIL           one or more invariants are violated
NOT-SUPPORTED  the proposal cannot be evaluated from available provenance
```

A `FAIL` or `NOT-SUPPORTED` proposal may remain in the transition record for audit purposes, but it does not become the current accepted state record.
