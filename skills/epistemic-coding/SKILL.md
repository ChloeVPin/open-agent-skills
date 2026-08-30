---
name: epistemic-coding
description: Apply an evidence-first philosophy while investigating, changing, or explaining a software repository. Use when correctness, uncertainty, scope, or the evidence behind a coding decision matters.
---

Lifecycle: `draft`

# Evidence-First Coding

## Purpose and scope

Produce repository changes that are justified by evidence, explicit about uncertainty, and proportionate to the request. This skill governs investigation and decision-making; it does not replace project-specific instructions, tests, security review, or human approval.

## Triggers and prerequisites

Use when a task involves a bug, design choice, refactor, new dependency, incomplete specification, or a claim that code is correct. Before acting, identify the repository instructions, the files and callers in scope, the requested outcome, and the available ways to verify it.

## Decision criteria

Make the smallest reversible change that explains the strongest evidence and satisfies the observable outcome. Ask when interpretations differ materially; otherwise keep claims proportional to evidence and preserve unresolved uncertainty.

## Knowledge labels

Label material claims as facts, observations, heuristics, conventions, recommendations, hypotheses, or speculation. Facts and observations need direct provenance; heuristics and conventions need scope and limits; recommendations need tradeoffs; hypotheses and speculation must remain visibly uncertain until evidence supports them.

## Procedure

1. Translate the request into an observable outcome. If two interpretations would produce materially different changes, ask before editing.
2. Inspect the relevant code, its callers, tests, and repository instructions. Separate facts observed in the repository from assumptions, conventions, recommendations, hypotheses, and speculation.
3. State the smallest change that could satisfy the outcome. Prefer reversible decisions when evidence is incomplete; do not add features for imagined future needs.
4. Seek disconfirming evidence: ask, “What would make this conclusion wrong?” Check boundary inputs, failure paths, security effects, unrelated behavior, and the strongest plausible alternative explanation.
5. Implement only the justified change. Preserve unrelated behavior and record material tradeoffs rather than presenting one context-dependent choice as universal.
6. Verify the outcome with the narrowest relevant reproduction or test, then run broader checks when the change warrants them. Read the actual results.
7. Report what is proven, what remains uncertain, and the next review trigger. Never call a change correct merely because it looks plausible or a test happens to pass.

## Examples and counterexamples

Good: “The caller expects a 404 for a missing record; the current code raises an uncaught exception. I reproduced it, added the missing-case regression test, and changed only the boundary handling.”

Bad: “This pattern is standard, so I refactored the module.” Popularity is not evidence that the pattern fits this repository or request.

Good: “The test suite passes, but it does not exercise authorization failure; confidence is limited until that boundary is checked.”

Bad: “All tests pass, therefore the feature is correct.” Green tests are evidence with coverage limits, not a proof of every behavior.

## Failure modes and recovery

If the evidence is contradictory, preserve the competing explanations and use the smallest discriminating test or source check. If evidence is insufficient, narrow the claim and mark the decision uncertain. If verification cannot run, record the exact limitation and do not claim completion. If the requested change conflicts with repository instructions or a protected rule, surface the conflict before editing.

## Validation evidence and provenance
Claims in this skill map to graded findings in [`docs/research.md`](../../docs/research.md):

- Distinguishing knowledge types and seeking disconfirming evidence (F4, Principled): the operationalized scientific method; not itself an evaluated intervention.
- Motivating evidence (F1, Strong): plausible-but-wrong agent output passing superficial checks; sources: Wang et al. 2025, MAST verification failure modes, mined real-world agent failures.
- Uncertainty management alignment (NIST AI RMF): factual alignment with risk-management guidance, not an effectiveness study.

Source boundary: these sources are design input, not proof that this skill improves outcomes in a given repository (see `docs/evidence-standard.md` limits).

Confidence: high for the principles as stated; medium for any domain conclusion reached by applying them. Freshness: re-review when any cited finding in `docs/research.md` changes.

Disconfirmation: a controlled study showing evidence-first procedures add no error reduction, or evidence that they slow correct work without preventing failure, would weaken this skill's guidance.
## Related skills and conflicts

Related: `repository-change-verification`, `contradiction-resolution`, `test-effectiveness-analysis`, `differential-patch-review`, `dependency-security-audit`, and `knowledge-maintenance`. This skill does not authorize ignoring project instructions, skipping required verification, or weakening constitutional rules to finish faster.
