# Skill evidence audit

Per-skill review decisions for applying [`docs/evidence-standard.md`](evidence-standard.md) with the findings in [`docs/research.md`](research.md). Status values: `pending`, `revised` (provenance rewritten to claim→finding→grade mapping), `verified` (revised and adversarially re-reviewed).

All 35 skills were `Lifecycle: draft` with the same 9-section anatomy at audit time. Two systemic problems were found across the library and fixed everywhere:

1. **Anonymous attribution.** Many provenance sections attributed claims to an unnamed "governing research" report that exists nowhere in the repository. Every provenance section now cites specific finding IDs from `docs/research.md`.
2. **Grade-less claims.** Claims were stated without strength. Every material claim now carries its finding's grade, and Principled guidance is labeled as such.

| Skill | Current sources | Findings | Required changes | Status |
|---|---|---|---|---|
| epistemic-coding | NIST AI RMF | F1, F4 | Replace anonymous "governing research" wording with F1/F4 mapping; keep uncertainty-label system; add disconfirmation note | revised |
| repository-exploration | git-ls-files | C6, F2, F3 | Add why reading callers/tests matters (F1 evidence); tool facts from C6 | revised |
| requirements-to-acceptance | RFC 2119 | F7, F2 | Map keyword semantics to F7; cite F2 as the empirical failure mode of ambiguous instructions | revised |
| skill-composition-and-routing | agentskills.io | F8, F3 | Map progressive disclosure to F8; mark its effectiveness Inferential; context-relevance nuance from F3 | revised |
| implementation-planning | SRE release engineering | F2, Q10 | Replace anonymous attribution; planning scope tied to instruction-misreading evidence (F2) | revised |
| architecture-decision-making | AWS ADR guide | F6 | Grade ADR practice Principled; state tradeoff (documentation cost) honestly | revised |
| evidence-driven-debugging | SRE incidents | F1, F4, C2 | Map observation/hypothesis to F4; incident discipline to C2; add F1 as failure evidence | revised |
| behavior-preserving-refactoring | Fowler | C1, C3 | Grade the methodology Principled (C3); strengthen with C1's PatchDiff evidence | revised |
| dead-code-and-comment-cleanup | TS, ESLint, git-blame | C4 | Split tool facts (Strong) from safety-of-removal (Inferential, tool-blindness limits) | revised |
| api-contract-compatibility | RFC 9110, semver, Google AIP | C5 | Convergent-source mapping; keep "verify consumers, don't trust version numbers" at Moderate | revised |
| differential-patch-review | arXiv:2503.15223, GitHub PR docs | C1, F1 | Cite PatchDiff methodology directly as the skill's empirical basis | revised |
| repository-change-verification | SWE-bench, PatchDiff, PR docs, OWASP AI | C1, Q12, F1 | Map to C1 + F1 + Q12; remove anonymous phrasing | revised |
| safe-git-workflow | git-reset | C6 | Split documented recovery semantics (Strong facts) from conventions (Principled) | revised |
| regression-test-design | Google mutation research | Q1, Q2 | Map to Q1 (Strong) + Q2 (Moderate, small-sample limit stated) | revised |
| test-effectiveness-analysis | Google mutation research, Springer TDD+M | Q1, Q2, Q3 | Same mapping; add mutation-score-vs-real-fault coupling from Q1 | revised |
| static-analysis-and-type-safety | CWE | Q3, F1 | Tool facts vs triage judgment split per Q3 | revised |
| concurrency-and-shared-state | Go memory model | Q4 | Strong per-language facts; mark cross-language generalization Inferential | revised |
| resilience-and-retry-design | SRE overload, cascading failures | Q5 | Map bounded-retry rules to Q5; keep at Moderate | revised |
| performance-regression-analysis | SRE SLOs | Q6 | Map to Q6; baseline/workload methodology Principled on top | revised |
| observability-and-instrumentation | OTel primer/conventions, SRE monitoring | Q7, S5 | Spec facts (Strong) vs symptom-based alerting practice (Moderate); add privacy exclusion rule (S5) | revised |
| accessibility-review | WCAG, WAI, ARIA APG | Q8 | Checkable criteria (Strong) vs checklist limits (Moderate) — require task-based review | revised |
| internationalization-and-localization | TR35, CLDR | Q9 | CLDR data authority (Strong); hand-rolled-rules failure claim Inferential | revised |
| release-and-rollback-safety | SRE release eng, incidents, DORA | Q10, C2 | Map staged rollout to Q10 (Moderate, survey limits noted); incident discipline C2 | revised |
| data-migration-safety | Fowler evodb, SRE data integrity, PG docs | Q11 | Lock/rewrite facts Strong; expand/contract procedure Principled | revised |
| agent-evaluation-and-benchmarking | swebench.com, arXiv:2503.15223 | Q12 | Resolution-rate inflation as core evidence; keep skill draft (fast-moving domain) | revised |
| secure-coding-review | OWASP quick reference, Top 10, CWE | S1 | Prevalence data Moderate (industry aggregation); checklist-recall claim Principled | revised |
| privacy-and-data-handling | NIST Privacy Framework (2 URLs) | S2, S5 | Lifecycle mapping to S2; add agent-specific risk from S5; drop duplicate URL | revised |
| configuration-and-secrets-safety | OWASP secrets + CI/CD cheat sheets | S3 | Leak-channel mapping; keep no-vault-product-claims boundary | revised |
| build-and-ci-integrity | GitHub Actions security, SLSA | S4 | Map hardening facts (Strong vendor) vs defense effectiveness (Moderate) | revised |
| dependency-security-audit | GitHub dependency review, OWASP AI cheat sheet, NIST AI 600-1 | S4, F1 | Add package-hallucination evidence (Spracklen et al. via S4); map risk enumeration to S5 | revised |
| tool-authorization-audit | OWASP AI Agent + MCP cheat sheets, NIST initiative | S6 | Map least-privilege/confirmation rules to S6; add freshness trigger (young domain) | revised |
| prompt-injection-resistance | OWASP LLM01 + prevention cheat sheet | S7 | Add Greshake et al. indirect-injection evidence (Strong for threat); label specific mitigations Principled with residual-risk statement | revised |
| contradiction-resolution | NIST AI 600-1 | S8, F4 | Grade procedure Principled (S8); keep AI 600-1 as risk framing | revised |
| knowledge-maintenance | RFC 9745, RFC 8594, NIST AI 600-1 | S9, F5 | Map lifecycle signaling to F5 (Strong mechanisms); effectiveness Inferential | revised |
| skill-quality-review | agentskills.io | F8 + standard | Add evidence-grade verification to decision criteria; require finding-ID mapping in provenance; keep anonymous phrasing out | revised |

## Audit conclusions

- **35 graded findings back the whole library** (F1–F8, C1–C6, Q1–Q12, S1–S9) after deduplication; several skills shared anonymous variants of the same claims, which is now explicit.
- **New sources added during research**: Greshake et al. (indirect prompt injection, arXiv:2302.12173), Wang/Pradel/Liu PatchDiff (arXiv:2503.15223 — upgraded from an unexplained citation to the central evidence for two skills), Roman & Mnich TDD+M (now grade-limited honestly), FAROS and MAST mappings from the verified agents-md research base.
- **Skills that must stay `draft` regardless of source quality**: `agent-evaluation-and-benchmarking`, `prompt-injection-resistance`, `tool-authorization-audit` (young domains, defenses not empirically validated). Promotion requires real-task evidence per the evidence standard, not more citations.
- **Duplicate citation fixed**: `privacy-and-data-handling` cited the NIST Privacy Framework twice; `repository-change-verification` cited the SWE-bench paper where the PatchDiff paper is the on-point source.

