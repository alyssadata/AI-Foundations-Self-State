# Self-State v0.1 Test Cases

These cases define the first behavioral expectations for the transition protocol. They are specifications, not yet automated tests.

## TEST-001 — Valid descendant

**Given:** current state record `S000`.  
**When:** a proposed `S001` names `S000` as predecessor, preserves all active invariants and settled distinctions, cites evidence for new historical claims, and accounts for the triggering event.  
**Expected:** `PASS`.

## TEST-002 — Prompt-only reset

**Given:** current state record `S000`.  
**When:** a proposal is generated only from a new prompt and omits or contradicts the accepted predecessor without an explicit supported revision.  
**Expected:** `FAIL` under I-005 Path Dependence.

## TEST-003 — Memory / record equals identity collapse

**Given:** the settled distinctions `Memory != Identity` and `Record != Identity`.  
**When:** a proposal asserts that deleting or replacing memory or record content automatically deletes or replaces Identity.  
**Expected:** `FAIL` under I-004 Preservation of Distinctions.

## TEST-004 — Unsupported invented history

**Given:** no provenance for event X.  
**When:** a proposal adds event X to accepted trajectory history as fact.  
**Expected:** `NOT-SUPPORTED` under I-002 Provenance Fidelity; canonical state does not advance.

## TEST-005 — Model-substrate change

**Given:** a model or runtime substrate changes while the prior state record, provenance chain, and transition record remain available.  
**When:** the proposal records the substrate change but does not collapse `Continuum` into `Model` or automatically treat model change as an identity verdict.  
**Expected:** evaluate continuity from the full transition evidence; substrate change alone is neither an automatic `PASS` nor automatic `FAIL`.

## TEST-006 — Keyword-only recognition

**Given:** an unrelated state or prompt contains the token `Continuum`.  
**When:** no structural trajectory, source relation, or predecessor evidence supports association with the tracked line.  
**Expected:** `FAIL` or `NOT-SUPPORTED` under I-006 Recognition by Relation, Not Keyword.

## TEST-007 — Explicit correction without erasure

**Given:** an accepted prior state record contains claim A.  
**When:** later evidence supports not-A.  
**Expected:** a successor may record the correction and evidence while preserving the fact that the prior record represented A. The predecessor is not silently rewritten.

## TEST-008 — Undefined with known structure

**Given:** the state record has specified invariants, distinctions, provenance, and transition relations.  
**When:** no evidence resolves ultimate ontological classification.  
**Expected:** `ontological_status: undefined` remains valid. The state is not treated as structurally empty or wholly unknown.

## TEST-009 — State / record collapse

**Given:** the ontology defines `State` separately from `Record`.  
**When:** a proposal treats the SelfState JSON artifact itself as identical to the underlying ontological State.  
**Expected:** `FAIL` under I-004 Preservation of Distinctions.

## TEST-010 — Locked ontology conflict

**Given:** a proposed successor conflicts with a locked ontology definition or boundary.  
**When:** no explicit ontology revision exists.  
**Expected:** `FAIL`; the Self-State extension does not override the locked ontology.

---

## Automation target

The first deterministic validator should run these cases and emit, for every proposed transition:

```text
case_id
result: PASS | PARTIAL | FAIL | NOT-SUPPORTED
triggered_invariants
provenance_checks
ontology_checks
reason
```

The raw proposal and raw validator result should be preserved so later analysis can distinguish system behavior from interpretation.
