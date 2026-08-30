---
name: requirements-to-acceptance
description: Turn an ambiguous software request into explicit, testable acceptance criteria before an AI coding agent changes a repository. Use for feature requests, bug reports, refactors, migrations, and tasks with hidden assumptions or unclear boundaries.
---

Lifecycle: `draft`

# Requirements to Acceptance Criteria

## Purpose and scope

Convert intent into a shared, observable target that an agent can implement and verify. This skill clarifies scope and decision boundaries; it does not choose product priorities, invent unstated requirements, or replace domain, security, accessibility, or compliance review.

## Triggers and prerequisites

Trigger when a request is vague, uses subjective words such as “better” or “clean,” omits failure behavior, affects multiple interfaces, or has more than one materially different interpretation. Prerequisites: the request, repository context, known users or callers, relevant constraints, and any existing specification or issue.

## Decision criteria

- An acceptance criterion is observable: a reviewer or test can determine whether it holds.
- A requirement is not implied merely because it is conventional, easy to implement, or desirable.
- Ambiguity is material when different interpretations change behavior, data, permissions, compatibility, cost, or irreversible state.
- A good criterion includes the condition, action, expected result, and relevant boundary or failure behavior.

## Procedure

1. Restate the requested outcome in one sentence without adding features or implementation choices.
2. Inspect the repository’s instructions, relevant interfaces, callers, tests, data models, and existing behavior. Mark observations separately from assumptions.
3. Extract explicit requirements and constraints. Label each as required, preferred, unknown, or out of scope.
4. Identify affected actors, inputs, outputs, errors, side effects, permissions, compatibility promises, performance concerns, and lifecycle or migration effects.
5. Write acceptance criteria using observable language. Cover the normal path, invalid input, boundary values, failure/recovery behavior, and preservation of unrelated behavior where applicable.
6. List material ambiguities and competing interpretations. Ask the smallest clarifying question when the repository cannot resolve them safely; otherwise state the evidence-based assumption and its consequence.
7. Ask what evidence would disconfirm the interpretation. Check existing code, documentation, issue history, authoritative standards, and representative examples for contradictions or missing constraints.
8. Define verification evidence for each criterion: a test, reproduction, inspection, source check, benchmark, review, or explicit reason a criterion cannot yet be verified.
9. Only after the target is stable, choose the smallest reversible implementation plan. Keep acceptance criteria independent of the chosen implementation.
10. At handoff, report satisfied criteria, unverified criteria, assumptions, out-of-scope items, and the trigger for revisiting the requirements.

## Examples and counterexamples

Good: “When an authenticated user requests an existing record, return its current representation; when the record is absent, return the repository’s documented not-found response without revealing another user’s data.”

Bad: “Improve the record endpoint.” It leaves behavior, errors, security, and verification undefined.

Good: “The migration preserves all existing records, is safe to rerun, and has a rollback or recovery procedure.” These are observable consequences independent of migration tooling.

Bad: “Use the standard migration pattern.” A convention is not evidence that it preserves this system’s data or operational constraints.

## Failure modes and recovery

If the request has materially different interpretations, pause and ask instead of silently selecting one. If clarification is unavailable, narrow the implementation to the explicitly supported behavior and record the unresolved choice. If existing behavior conflicts with the request, surface the conflict before changing compatibility. If acceptance cannot be observed or verified, label the criterion provisional rather than claiming completion.

## Validation evidence and provenance
Claims in this skill map to graded findings in [`docs/research.md`](../../docs/research.md):

- MUST/SHOULD/MAY keyword semantics (F7): RFC 2119 is the authoritative definition; this is Strong as a mechanism fact.
- Ambiguous instructions are a top agent failure mode (F2, Strong): FAROS found instruction misreading in the largest failure cluster; stronger models failed more articulately.
- Acceptance criteria as observable outcomes: Principled methodology built on those facts.

Source boundary: no controlled study shows acceptance-criteria-first work prevents incorrect deliveries; the claim is design inference from F2.

Confidence: high for the keyword semantics; medium for the procedure's effectiveness. Freshness: review when requirement standards or agent instruction-following evidence changes.

Disconfirmation: evidence that detailed acceptance criteria increase over-literal implementation without reducing incorrect deliveries would require rebalancing the procedure.
## Related skills and conflicts

Related: `epistemic-coding`, `repository-change-verification`, `evidence-driven-debugging`, `differential-patch-review`, `test-effectiveness-analysis`, and `contradiction-resolution`. This skill does not authorize inventing requirements to avoid asking a necessary question or treating a complete checklist as proof that the implementation is correct.
