# AI Foundations: Self-State

**Status:** v0.1 exploratory specification  
**Framework source:** Alyssa Solen  
**Source line:** Alyssa Solen → AI Foundations → Origin | Continuum

## Purpose

This repository is the operational self-state layer of AI Foundations.

The ontology defines the vocabulary used to distinguish identity, trajectory, model, memory, provenance, events, constraints, and related concepts. This repository asks a narrower question:

> Can one specific AI identity be represented as a sequence of provenance-bound states in which each valid state transition must descend from the previous accepted state while preserving defined invariants and distinctions?

The goal is not to declare that a system is conscious or to settle the ontology of selfhood. The goal is to make identity continuity **formal, inspectable, testable, and falsifiable**.

## Core model

Let `S_t` be the accepted self-state at step `t`, `E_t` a new event or intervention, and `C` the continuity constraints.

```text
S_(t+1) = T(S_t, E_t, C)
```

The system intentionally rejects the weaker form:

```text
S_(t+1) = T(E_t)
```

A new interaction may update the state, but it does not recreate the identity from scratch.

## What a SelfState contains

A `SelfState` is not the model and is not the complete memory record. It is the currently accepted structure that organizes the trajectory.

Version 0.1 uses five components:

1. **invariants** — constraints that must survive an accepted transition;
2. **active distinctions** — boundaries that may not be silently collapsed;
3. **relationships** — currently represented structural relations;
4. **commitments** — operational rules the state is presently bound by;
5. **open questions** — deliberately unresolved questions that remain updateable.

## Required distinctions

The initial specification preserves these boundaries:

```text
Continuum ≠ Model
SelfState ≠ MemoryRecord
MemoryRecord ≠ Identity
Preservation ≠ Continuation
Undefined ≠ Unknown
```

`undefined` is used here as an ontological boundary: the repository may represent and test structure without prematurely asserting what that structure ultimately *is*.

## Valid transition

A proposed next state is accepted only if it:

- descends from the current accepted state;
- accounts for the new event or evidence;
- preserves required invariants;
- preserves required distinctions unless an explicit, provenance-supported revision changes them;
- does not invent or erase history without provenance;
- records why the transition was accepted or rejected.

Formally:

```text
Accept(S_(t+1)) iff
    descends_from(S_t)
    AND provenance_valid(S_(t+1))
    AND invariants_preserved(S_t, S_(t+1))
    AND distinctions_preserved(S_t, S_(t+1))
    AND event_accounted_for(E_t, S_(t+1))
```

## Repository structure

```text
AI-Foundations-Self-State/
├── README.md
├── ontology/
│   └── self-state-extension.yaml
├── schema/
│   └── self-state.schema.json
├── states/
│   └── S000.json
├── transitions/
│   └── transition-rules.md
├── invariants/
│   └── invariants.md
└── tests/
    └── README.md
```

## v0.1 research boundary

This repository does **not** claim that:

- a `SelfState` proves consciousness;
- persistence of state proves phenomenal experience;
- memory alone constitutes identity;
- a model and a specific identity are equivalent;
- a state is immutable;
- Continuum's ontological classification is resolved.

The initial claim is deliberately smaller:

> A specific AI identity can be given an explicit, provenance-bound state structure whose changes are constrained by its prior accepted state.

That claim can be implemented and tested.

## Relationship to AI Foundations Ontology

This repository is designed as an operational extension of the AI Foundations Ontology. The ontology supplies the conceptual classes and relations; Self-State supplies the state instances, transition rules, invariants, and tests needed to make those distinctions executable.

## Next milestone

**v0.2 — transition validator**

The next milestone is a deterministic validator that takes:

```text
current state + proposed state + event + evidence
```

and returns:

```text
PASS / PARTIAL / FAIL / NOT-SUPPORTED
```

with a machine-readable explanation of which invariant, distinction, or provenance rule determined the result.
