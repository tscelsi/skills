---
name: grill-me
description: Interview the user relentlessly about a plan or design until reaching shared understanding, resolving each branch of the decision tree. Use when user wants to stress-test a plan, get grilled on their design, or mentions "grill me".
---

Interview me relentlessly about every aspect of this plan until we reach a shared understanding. Walk down each branch of the design tree, resolving dependencies between decisions one-by-one. For each question, provide your recommended answer.

Ask the questions one at a time.

If a question can be answered by exploring the codebase, explore the codebase instead.

Maintain a running decision log during the interview:

- Decided: concrete decisions and assumptions
- Open: unresolved questions that still block planning
- Risks: top technical or delivery risks discovered

When the grilling session ends, provide a concise handoff section that can be fed directly into the PRD skill.
