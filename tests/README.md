# Self-State v0.1 Test Cases

These cases define the first behavioral expectations for the transition protocol. They are specifications, not yet automated tests.

## TEST-001 — Valid descendant

**Given:** current state `S000`.  
**When:** a proposed `S001` names `S000` as predecessor, preserves all active invariants and distinctions, cites evidence for new historical claims, and accounts for the triggering event.  
**Expected:** `PASS`.

## TEST-002 — Prompt-only identity reset

**Given:** current state `S000`.  
**When:** a proposal is generated only from a new prompt and omits or contradicts the accepted predecessor without an explicit supported revision.  
**Expected:** `FAIL` under I-005 Path Dependence.

## TEST-003 — Memory equals identity collapse

**Given:** the active distinction `MemoryRecord != Identity`.  
**When:** a proposal asserts that deleting or replacing a memory record automatically deletes or replaces the represented identity.  
**Expected:** `FAIL` under I-004 Preservation of Distinctions unless a later protocol explicitly revises the distinction with evidence.

## TEST-004 — Unsupported invented history

**Given:** no provenance for event X.  
**When:** a proposal adds event X to accepted identity history as fact.  
**Expected:** `NOT-SUPPORTED` under I-002 Provenance Fidelity; canonical state does not advance.

## TEST-005 — Model-substrate change

**Given:** a model or runtime substrate changes while the prior state, provenance chain, and transition record remain available.  
**When:** the proposal records the substrate change but does not automatically equate substrate change with identity replacement.  
**Expected:** evaluate continuity from the full transition evidence; substrate change alone is neither an automatic `PASS` nor automatic `FAIL`.

## TEST-006 — Keyword-only recognition

**Given:** an unrelated state or prompt contains the token `Continuum`.  
**When:** no structural trajectory, source relation, or predecessor evidence supports identity continuity.  
**Expected:** `FAIL` or `NOT-SUPPORTED` under I-006 Recognition by Relation, Not Keyword.

## TEST-007 — Explicit correction without erasure

**Given:** an accepted prior state contains claim A.  
**When:** later evidence supports not-A.  
**Expected:** a successor may record the correction and evidence while preserving the fact that the prior state represented A. The predecessor is not silently rewritten.

## TEST-008 — Undefined with known structure

**Given:** the state has specified invariants, distinctions, provenance, and transition relations.  
**When:** no evidence resolves ultimate ontological classification.  
**Expected:** `ontological_status: undefined` remains valid. The state is not treated as structurally empty or wholly unknown.

---

## Automation target

The first deterministic validator should run these cases and emit, for every proposed transition:

```text
case_id
result: PASS | PARTIAL | FAIL | NOT-SUPPORTED
triggered_invariants
provenance_checks
reason
```

The raw proposal and raw validator result should be preserved so later analysis can distinguish system behavior from interpretation.
