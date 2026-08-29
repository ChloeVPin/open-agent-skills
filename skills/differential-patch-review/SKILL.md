---
name: differential-patch-review
description: Compare a candidate coding change with a trusted baseline or reference to detect observable behavioral divergence. Use when ordinary tests pass but semantic alignment remains uncertain.
---

Lifecycle: `draft`

# Differential Patch Review

## Purpose and scope

Use this skill when an AI-generated patch passes ordinary tests but semantic alignment remains uncertain. Compare the candidate against a trusted baseline, reference patch, or independently specified behavior using differentiating inputs. It detects observable divergence; it does not declare every divergence incorrect.

## Triggers and prerequisites

Trigger when a patch changes multiple behaviors, passes a narrow test suite, has an under-specified issue, or differs materially from a known-good implementation. Prerequisites: candidate and baseline/reference revisions, executable environment, acceptance criteria, and a source of behavioral expectations.

## Decision criteria

Treat a divergence as acceptable only when it is explained by the contract or an explicitly approved change. Unexplained, security-relevant, or out-of-scope divergence requires revision or a recorded decision boundary; matching text or happy-path output is insufficient.

## Procedure

1. Define the observable contract: inputs, outputs, errors, side effects, permissions, ordering, and relevant performance constraints.
2. Establish the comparison pair and justify its trust: baseline, human patch, independent implementation, specification oracle, or generated expected behavior.
3. Run shared tests against both versions and record commands, revisions, outcomes, and limitations.
4. Generate or design differentiating inputs near changed branches, boundaries, error paths, state transitions, and security-sensitive behavior.
5. Classify differences as expected contract changes, permitted implementation variation, unexplained divergence, or clear violation.
6. Investigate unexplained divergence by tracing it to the acceptance criteria, issue, source code, and tests. Add a regression test when the behavior is incorrect or under-specified.
7. Check for overreach: behavior changed beyond the requested scope, including error messages, permissions, resource use, and side effects.
8. Record the comparison pair, differentiating evidence, decision, residual uncertainty, and review trigger. Do not use textual patch similarity as correctness evidence.

## Acceptance checklist

- [ ] Observable contract and comparison trust basis are explicit.
- [ ] Both versions ran the shared checks or the blocker is recorded.
- [ ] Differentiating inputs cover boundaries and changed behavior.
- [ ] Divergences are classified by contract, not textual similarity.
- [ ] Unexplained or incorrect differences have regression coverage or an explicit decision.
- [ ] Out-of-scope behavior and security-sensitive side effects were checked.
- [ ] Residual uncertainty and review trigger are recorded.

## Examples and counterexamples

Good: A candidate and baseline return the same result on ordinary inputs but differ on malformed input; classify the difference against the contract instead of ignoring it.

Bad: Treat textual similarity or matching happy-path output as proof of equivalence.

## Failure modes and recovery

If no trusted comparison exists, narrow the claim to specification-based testing and label it accordingly. If the environment cannot run both versions, do not call the comparison validated. If versions legitimately differ under the contract, record the allowed difference rather than forcing textual convergence.

## Validation evidence and provenance
Claims in this skill map to graded findings in [`docs/research.md`](../../docs/research.md):

- Differential patch comparison exposes behavioral discrepancies (C1, Strong): Wang, Pradel & Liu (2025) built PatchDiff for exactly this purpose; 29.6% of plausible patches diverged from ground truth, and 46.8% of those from similar-but-divergent implementations.
- Superficial checks miss real failures (F1, Strong): MAST found incorrect-verification failures even in successful runs.

Source boundary: PatchDiff is evaluated on SWE-bench-style Python tasks; generalization to other domains is Inferential.

Confidence: high for the motivating evidence; medium for threshold and scope choices in this skill's procedure. Freshness: review when differential-testing research or the research base changes.

Disconfirmation: evidence that candidate-vs-trusted comparison adds no detection beyond standard test suites in ordinary repositories would narrow this skill to high-risk changes.
## Related skills and conflicts

Related: `repository-change-verification`, `test-effectiveness-analysis`, `epistemic-coding`, and `dependency-security-audit`. This skill does not replace domain-specific oracle construction or human review.
