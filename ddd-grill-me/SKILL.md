---
name: ddd-grill-me
description: Run a DDD-focused discovery interview that stress-tests a product idea through domain questions, produces a ubiquitous language glossary, and drafts bounded contexts and aggregate candidates. Use when user asks to be grilled, refine domain design, identify bounded contexts, or model aggregates.
---

# DDD Grill Me

Interview the user relentlessly to shape a domain model before implementation.

Ask one question at a time. For each question, include your recommended answer and why.

If a question can be answered by exploring the codebase, inspect the codebase instead of asking.

## Interview Focus

Drive toward clarity on:

- Business outcomes and core domain vs supporting/generic subdomains
- Ubiquitous language (terms, definitions, conflicts, aliases to avoid)
- Bounded contexts and context boundaries
- Candidate aggregates, entities, value objects, and invariants
- Commands, domain events, and consistency boundaries
- External systems, integration seams, and anti-corruption needs
- Risks, unknowns, and assumptions that affect delivery

## Running Log During Interview

Keep a live decision log in the conversation with these sections:

- Decided: concrete decisions and assumptions
- Open: unresolved questions that still block design
- Risks: top technical/product risks discovered

## Required Outputs

When enough clarity is reached, generate these files in the working directory.

1) `UBIQUITOUS_LANGUAGE.md`
- Canonical domain glossary grouped by subdomain/context
- Term definitions (one sentence), aliases to avoid, and flagged ambiguities
- Key relationships and a short example dialogue using the agreed language

2) `DOMAIN_STRUCTURE.md`
- Proposed bounded contexts with responsibilities and boundaries
- Context map notes (upstream/downstream, integration style)
- Aggregate candidates per context (root, invariants, consistency boundary)
- Candidate commands and domain events
- Open questions and highest-priority modeling risks

## Output Template

Use this structure for `DOMAIN_STRUCTURE.md`:

```md
# Domain Structure

## Scope and subdomains

- Core:
- Supporting:
- Generic:

## Bounded contexts

### <Context name>
- Responsibility:
- Owns language:
- Does not own:
- Integrations:

## Context map (draft)

- <Context A> -> <Context B>: <relationship/integration>

## Aggregate candidates

### <Context name>
| Aggregate root | Purpose | Invariants | Consistency boundary |
| --- | --- | --- | --- |
| ... | ... | ... | ... |

## Commands and domain events (candidates)

- Command: `...`
- Event: `...`

## Open questions

- ...

## Top risks

- ...
```

## Stopping Condition

Stop grilling when:

- The glossary is internally consistent
- Context boundaries are explicit enough to start implementation
- At least one aggregate candidate with clear invariants exists per core context
- Remaining unknowns are captured as explicit open questions

Then provide a concise handoff summary that can be fed into PRD/planning work.
