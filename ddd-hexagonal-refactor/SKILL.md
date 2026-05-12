---
name: ddd-hexagonal-refactor
description: Refactor an existing codebase toward Domain-Driven Design and hexagonal architecture with priority on eliminating duplicate logic and removing unused code safely. Use when user asks to deduplicate business rules, delete dead code, reduce copy-paste modules, or clean architecture while preserving behavior.
---

# DDD Hexagonal Refactor

Refactor legacy or mixed-architecture code into a domain-centered design, prioritizing duplicate and unused code removal first.

## Quick start

When invoked, do this first:

1. Build a duplicate/dead-code inventory before any structural refactor.
2. Classify each item as `domain duplicate`, `adapter duplicate`, `unused`, or `uncertain`.
3. Protect behavior with tests around impacted use cases.
4. Remove or consolidate the highest-confidence items first.
5. Apply DDD/hexagonal boundaries while deduplicating.

Minimal example output:

- Duplicate: discount eligibility logic repeated in `orders/models.py`, `checkout/service.py`, and `api/handlers.py`.
- Refactor: consolidate into `domain/pricing.py` and call via service-layer use case.
- Unused: `legacy_discount_v2` function has no callers; remove after tests pass.
- Port updated: `DiscountPolicyPort` becomes the single outward contract.

## Workflows

### 1) Duplicate and dead-code audit

- Find duplicate logic by behavior, not text similarity.
- Find unused code through references, imports, routes, jobs, and wiring checks.
- Track candidates in a table: `location`, `type`, `confidence`, `removal strategy`.
- Prioritize: `high-confidence unused` -> `high-impact duplicate business rules` -> `uncertain`.

### 2) Safe cleanup slices

For each slice:

- Add or update tests to lock expected behavior.
- Remove truly unused code first; avoid broad rewrites.
- Consolidate duplicates into one domain-owned implementation.
- Keep orchestration in service layer and I/O in adapters.
- Introduce/refine ports so callers depend on stable contracts.
- Replace scattered copies with calls to canonical domain logic.

### 3) Quality gate

Before finishing:

- Confirm deleted code has no runtime references.
- Confirm each removed duplicate maps to a canonical replacement.
- Confirm domain has no framework/ORM imports.
- Confirm service layer depends on ports, not concrete adapters.
- Confirm tests cover behavior and touched integration seams.

## Output format

Provide a concise handoff with:

- Removed unused code (with confidence and verification method)
- Consolidated duplicate logic (old locations -> canonical location)
- Boundary violations fixed
- Ports introduced or changed
- Risks, uncertain candidates, and deferred cleanup
- Test evidence added

## Guardrails

- Prefer small, reversible steps over big-bang rewrites.
- Do not delete code with uncertain usage without an explicit safety check.
- Preserve public behavior while deduplicating unless change is requested.
- Do not rename domain terms casually; align with ubiquitous language.
- Keep adapters thin translators, not policy owners.
- If a new adapter capability is needed, update the port in the same change.

## Advanced features

If needed, add a companion `REFERENCE.md` with:

- Duplicate-detection heuristics and false-positive handling
- Dead-code validation checklist (routing, jobs, reflection, plugin loading)
- Migration patterns (strangler slices, anti-corruption layer, dual-write mitigation)
