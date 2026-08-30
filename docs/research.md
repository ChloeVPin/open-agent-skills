# Research notes

This document is the central research base for every skill in this repository. Skills cite these findings by name; they do not carry private bibliographies. It separates the source material from the decisions made here.

- Grading rules and provenance requirements: [`docs/evidence-standard.md`](evidence-standard.md).
- Agent-specific failure evidence (MAST, FAROS, "What Breaks When LLMs Code?") was verified and graded in the companion repository's research base: [`ChloeVPin/agents-md: docs/research.md`](https://github.com/ChloeVPin/agents-md/blob/main/docs/research.md).

## Evidence grades

- **Strong**: controlled evaluation or large-scale empirical study with reproducible methodology.
- **Moderate**: multiple real-world incident reports, postmortems, or convergent independent evidence.
- **Inferential**: reasonable design inference from empirical findings, not directly tested as an intervention.
- **Principled**: sound design principle, not empirically proven as a failure-prevention mechanism.

## Source boundary

The sources below are design input for skills, not proof that any skill works. Several are practitioner references (Google SRE, OWASP) whose authority comes from convergent incident experience, not controlled evaluation; where that is the best available evidence, the finding is graded Moderate or Principled, not Strong. Vendor documentation is treated as authoritative for tool behavior only. Every claim imported from secondary summaries was traced to a primary source; summaries were not counted as independent confirmation.

## Findings: foundations and meta-skills

Applies to `epistemic-coding`, `skill-quality-review`, `skill-composition-and-routing`, `contradiction-resolution`, `knowledge-maintenance`, `requirements-to-acceptance`, `implementation-planning`, `architecture-decision-making`, `repository-exploration`.

### F1. Plausible output is not evidence [Strong]

Coding agents produce patches and explanations that look coherent while being incorrect, and these failures propagate silently when outputs pass superficial checks.

- Wang, Pradel & Liu (2025), [Are "Solved Issues" in SWE-bench Really Solved Correctly?](https://arxiv.org/abs/2503.15223): 7.8% of patches counted as correct fail the developer-written test suite; 29.6% of plausible patches induce different behavior than ground truth; reported resolution rates inflate by 6.2 absolute percentage points.
- MAST (Cemri et al., [arXiv:2503.13657](https://arxiv.org/abs/2503.13657)): verification failure modes FM-3.2 (No or Incomplete Verification, 8.20%) and FM-3.3 (Incorrect Verification, 9.10%) occur even in successful multi-agent runs.
- [What Breaks When LLMs Code? (arXiv:2605.30777)](https://arxiv.org/abs/2605.30777): 547 confirmed real-world safety failures mined from GitHub issue trackers; developers routinely accept plausible generated code without understanding downstream impact.

### F2. Instruction misreading is a top agent failure mode [Strong]

FAROS (2026): clustered ~4,000 failed rubric items across 6 coding agents on 100 SWE-bench Pro tasks; the largest failure cluster (247 failures) was agents misreading or over-literally applying task instructions. Stronger models failed more articulately; this is not fixed by scale. Verified via the [agents-md research base](https://github.com/ChloeVPin/agents-md/blob/main/docs/research.md).

### F3. Context quality and relevance matter more than raw length [Moderate, nuanced]

[arXiv:2602.11988](https://arxiv.org/abs/2602.11988) directly evaluates repository-level context files for coding agents; evidence supports "keep guidance relevant, current, and selective" rather than "shorter is always better." Attention dilution and lost-in-the-middle effects penalize disorganized context. This repository applies it by keeping skills focused and moving depth here. Verified via the [agents-md research base](https://github.com/ChloeVPin/agents-md/blob/main/docs/research.md).

### F4. Separating observation from hypothesis is operationalized scientific method [Principled]

The observation/hypothesis/experiment distinction in `epistemic-coding` and `evidence-driven-debugging` is the scientific method applied to software work; it is not itself an evaluated intervention. NIST's [AI Risk Management Framework](https://www.nist.gov/itl/ai-risk-management-framework) independently requires valid, reliable evaluation and explicit uncertainty management, which supports the practice as risk-management alignment (Principled for effectiveness, factual for alignment).

### F5. Information lifecycle needs explicit deprecation signals [Moderate]

[RFC 9745](https://www.rfc-editor.org/rfc/rfc9745.html) (Deprecation HTTP Response Header Field, March 2025, Standards Track) and [RFC 8594](https://www.rfc-editor.org/rfc/rfc8594.html) (Sunset header) define IETF-standard mechanisms for signaling resource lifecycle; RFC 9745 itself notes deprecation signals are hints, not guarantees. The knowledge-maintenance skill's freshness/deprecation model mirrors this structure (Strong that the mechanisms exist and are standardized; Inferential that mirroring them in skill documentation prevents staleness).

### F6. Decision records improve traceability of architectural choices [Principled]

The ADR practice (document context, decision, consequences) is established practitioner methodology; see AWS [Prescriptive Guidance on ADRs](https://docs.aws.amazon.com/prescriptive-guidance/latest/architectural-decision-records/adr-process.html). No controlled study shows ADRs prevent bad architecture; the grade reflects traceability value, not outcome proof.

### F7. Requirement keywords need a defined semantics [Strong for the mechanism]

[RFC 2119](https://www.rfc-editor.org/rfc/rfc2119) defines MUST/SHOULD/MAY semantics used across IETF standards; ambiguity in requirement strength is a documented source of divergent implementation (see F2: agents over-literally applying "do not modify" boilerplate). ISO/IEC/IEEE 29148 makes the same argument for requirements quality (Principled in this repository's context).

### F8. Skill routing benefits from progressive disclosure [Principled]

The [Agent Skills specification](https://agentskills.io/specification) prescribes loading metadata (~100 tokens) for all skills, full instructions on activation, and reference material on demand. This is a format decision by the spec authors, not an evaluated intervention; its effectiveness claim is Inferential.

## Findings: change and diagnosis

Applies to `evidence-driven-debugging`, `behavior-preserving-refactoring`, `dead-code-and-comment-cleanup`, `api-contract-compatibility`, `differential-patch-review`, `repository-change-verification`, `safe-git-workflow`.

### C1. Differential patch comparison exposes incorrect-but-plausible changes [Strong]

Wang, Pradel & Liu (2025) built PatchDiff, which automatically exposes behavioral discrepancies between generated and ground-truth patches; behavioral differences were found in 29.6% of plausible patches, and 46.8% of those stemmed from similar-but-divergent implementations. This is the direct empirical basis for `differential-patch-review` and the candidate/trusted-behavior comparison in `repository-change-verification`.

### C2. Incident discipline: roles, facts, and preserved follow-ups [Moderate]

[Google SRE: Managing Incidents](https://sre.google/sre-book/managing-incidents/): effective incident work uses clear roles, explicit communication, and preserved facts and follow-up actions. Convergent postmortem practice across the industry; no controlled evaluation of the intervention, hence Moderate. `evidence-driven-debugging` and `contradiction-resolution` apply this to agent debugging.

### C3. Refactoring preserves behavior only under external verification [Principled, with empirical support]

Fowler's [Refactoring](https://martinfowler.com/books/refactoring.html) defines behavior-preserving transformations with small steps and test support, a practitioner methodology, not a controlled study. Independent evidence that unverified restructuring is risky comes from F1 (divergent implementations passing tests). The skill's claim "small steps + regression checks preserve behavior" is therefore Principled, strengthened inferentially by C1.

### C4. Dead code and provenance are mechanically detectable, within tool limits [Strong for tool facts]

TypeScript's [`noUnusedLocals`](https://www.typescriptlang.org/tsconfig/noUnusedLocals.html), ESLint's [`no-unused-vars`](https://eslint.org/docs/latest/rules/no-unused-vars), and [`git blame`](https://git-scm.com/docs/git-blame) are documented, deterministic tools (Strong that they report what they report). The Inferential step is that tool-clean removal is safe: the tools cannot see reflection, dynamic dispatch, or cross-repository consumers, which is why the skill requires caller checks before deletion.

### C5. Interface evolution needs declared compatibility semantics [Moderate]

Three convergent sources: [semver](https://semver.org/) defines version-change semantics for compatibility communication (authoritative spec); [RFC 9110](https://www.rfc-editor.org/rfc/rfc9110) defines HTTP semantics that contract changes must respect (authoritative spec); Google's [API design guide](https://cloud.google.com/apis/design) prescribes backward-compatibility practices from operating internet-scale APIs (practitioner consensus). Effectiveness of semver as a breakage-prevention mechanism in ecosystems is studied but mixed; the skill stays at Moderate and requires verifying consumer behavior, not trusting version numbers.

### C6. Git operations have precise, documented recovery semantics [Strong for facts]

[`git reset`](https://git-scm.com/docs/git-reset) documentation defines exactly what moves (ref, index, working tree) and what is recoverable via reflog. `safe-git-workflow`'s rules (commit before destructive operations, prefer revert over rewrite on shared history) are Principled conventions built on these Strong facts.

## Findings: quality and operations

Applies to `regression-test-design`, `test-effectiveness-analysis`, `static-analysis-and-type-safety`, `concurrency-and-shared-state`, `resilience-and-retry-design`, `performance-regression-analysis`, `observability-and-instrumentation`, `accessibility-review`, `internationalization-and-localization`, `release-and-rollback-safety`, `data-migration-safety`, `agent-evaluation-and-benchmarking`.

### Q1. Mutation testing is coupled to real faults [Strong]

Petrovic, Fraser, Ivanković & Just (ICSE 2021), [Long Term Effects of Mutation Testing](https://research.google/pubs/long-term-effects-of-mutation-testing/): analysis of 15 million mutants at Google shows developers using mutation testing write more and better tests, and mutants are coupled with real faults: had mutation testing been applied to the fault-introducing change, it would have flagged a live mutant that could have prevented the bug. Direct basis for `test-effectiveness-analysis` and the boundary-focus advice in `regression-test-design`.

### Q2. Mutation-augmented test writing raises coverage and fault detection [Moderate]

Roman & Mnich (2021), [Test-driven development with mutation testing: an experimental study](https://link.springer.com/article/10.1007/s11219-020-09534-x): controlled experiment (22 students, 8 groups) found TDD+M tests achieved 53.3% vs 33.5% statement coverage and 64.9% vs 37.5% mutation coverage, statistically significant (p<0.0001). Honest limits: small, student-only sample; reported as Moderate, not Strong.

### Q3. Static analysis findings are facts about defect risk, not proof of exploitability [Moderate]

[CWE](https://cwe.mitre.org/) provides the common defect taxonomy; static analyzers map code to it deterministically (Strong for tool facts). The gap between a finding and an exploitable or reachable defect requires triage, supported by F1's evidence that superficial checks miss real failures. The skill's "triage, don't suppress" rule is Principled on top of these facts.

### Q4. Memory-model reasoning requires language-specific happens-before definitions [Strong for the spec]

The [Go Memory Model](https://go.dev/ref/mem) precisely defines when writes are visible to reads and which synchronization primitives establish ordering. `concurrency-and-shared-state` builds on this: the facts are Strong per language model; the general procedure ("identify shared state, find synchronization, check ordering") is Inferential extrapolation to languages with analogous models (C++, Java, Rust).

### Q5. Retry and backoff must be bounded to prevent cascading failure [Moderate]

[Google SRE: Addressing Cascading Failures](https://sre.google/sre-book/addressing-cascading-failures/) and [Handling Overload](https://sre.google/sre-book/handling-overload/): unbounded retries amplify load, cascades propagate through retry feedback loops, and load shedding with bounded retry budgets is the documented mitigation. Convergent practitioner evidence including publicly documented large-scale outages; `resilience-and-retry-design` requires explicit bounds, jitter, and budget accounting.

### Q6. SLOs make performance regressions measurable [Moderate]

[Google SRE: Service Level Objectives](https://sre.google/sre-book/service-level-objectives/): SLOs and error budgets define the reliability/performance contract that regression analysis measures against. Practitioner consensus (Moderate); the skill's requirement for representative workloads and before/after baselines is Principled methodology built on it.

### Q7. Structured telemetry needs semantic conventions and privacy boundaries [Strong for the spec, Moderate for practice]

[OpenTelemetry](https://opentelemetry.io/docs/concepts/observability-primer/) defines the observability data model and [semantic conventions](https://opentelemetry.io/docs/concepts/semantic-conventions/) define attribute naming (authoritative spec: Strong that the conventions exist and are standardized). [Google SRE: Monitoring Distributed Systems](https://sre.google/sre-book/monitoring-distributed-systems/) documents symptom-based alerting over cause-based heuristics (Moderate practitioner consensus). Telemetry can capture sensitive payload data, which requires deliberate exclusion (linked to S5).

### Q8. Accessibility conformance is a checkable standard [Strong for criteria, Moderate for outcome]

[WCAG 2.2](https://www.w3.org/TR/WCAG22/) is a W3C Recommendation with testable success criteria ([WAI overview](https://www.w3.org/WAI/standards-guidelines/wcag/)); the [ARIA Authoring Practices Guide](https://www.w3.org/WAI/ARIA/apg/) documents pattern-level implementation. Strong that conformance is objectively checkable; Moderate that checklists alone catch most real barriers; automated tooling finds a minority of issues, which is why `accessibility-review` requires task-based and assistive-technology review rather than tool output only.

### Q9. Locale handling requires authoritative cultural data, not inference [Strong for the data]

[Unicode TR35](https://www.unicode.org/reports/tr35/) defines the locale data model and [CLDR](https://cldr.unicode.org/) supplies the survey-backed cultural data (calendars, collation, plurals, formats). The rule "never hand-roll locale rules; use the platform's CLDR-backed formatting" rests on Strong standardization facts; the Inferential part is the claim that hand-rolled rules are the common failure source in practice.

### Q10. Staged delivery with automated rollback reduces release risk [Moderate]

[Google SRE: Release Engineering](https://sre.google/sre-book/release-engineering/) documents staged rollouts, canaries, and fast rollback as incident-limiting practice; DORA's [continuous delivery capability](https://dora.dev/capabilities/continuous-delivery/) research associates delivery performance outcomes with deployment automation across large multi-year surveys (Moderate: survey-based and self-reported, but large-scale and replicated across years).

### Q11. Data migration safety follows expand–migrate–contract [Principled, with incident support]

Fowler's [Evolutionary Database Design](https://martinfowler.com/articles/evodb.html) describes incremental schema change with transition periods; [PostgreSQL `ALTER TABLE`](https://www.postgresql.org/docs/current/sql-altertable.html) documents which operations take locks and rewrite tables (Strong vendor facts); [Google SRE: Data Integrity](https://sre.google/sre-book/data-integrity/) frames recovery boundaries. The composite expand/contract procedure is Principled methodology whose value is supported by the documented cost of lock-taking and non-reversible destructive operations.

### Q12. Benchmark results overstate agent capability without differential verification [Strong]

Wang, Pradel & Liu (2025) show benchmark-reported resolution rates inflate by ~6.2 points because plausible-but-incorrect patches pass benchmark tests ([arXiv:2503.15223](https://arxiv.org/abs/2503.15223)); the [SWE-bench paper](https://arxiv.org/abs/2310.06770) (ICLR 2024) notes the limits of test-based validation. `agent-evaluation-and-benchmarking` requires task-level correctness checks beyond harness pass/fail; the anti-Goodhart framing is Principled with this Strong empirical support.

## Findings: security and maintenance

Applies to `secure-coding-review`, `privacy-and-data-handling`, `configuration-and-secrets-safety`, `build-and-ci-integrity`, `dependency-security-audit`, `tool-authorization-audit`, `prompt-injection-resistance`, `contradiction-resolution`, `knowledge-maintenance`.

### S1. Secure-coding review needs a defect taxonomy and prevalence data [Moderate]

[OWASP Top 10](https://owasp.org/www-project-top-ten/) ranks application-security risks from incident and contribution data (Moderate: industry aggregation, not a controlled study); the [Secure Coding Practices Quick Reference](https://owasp.org/www-project-secure-coding-practices-quick-reference-guide/) and [CWE](https://cwe.mitre.org/) provide the checkable defect taxonomy. `secure-coding-review` requires trust-boundary and abuse-case reasoning; its claim that checklists improve review recall is Principled.

### S2. Privacy needs lifecycle-stage controls, not ad-hoc care [Moderate]

[NIST Privacy Framework](https://www.nist.gov/privacy-framework) defines core functions (Identify-P, Govern-P, Control-P, Communicate-P, Protect-P) for managing privacy risk across the data lifecycle. The framework is voluntary guidance (its own words); effectiveness as an intervention is not studied, but its lifecycle coverage is comprehensive and government-published; Moderate for structure, Principled for specific minimization rules.

### S3. Secrets leak through configuration by predictable channels [Moderate]

The [OWASP Secrets Management Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Secrets_Management_Cheat_Sheet.html) and [CI/CD Security Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/CI_CD_Security_Cheat_Sheet.html) document the recurring leak channels (env vars, logs, CI configuration, artifacts) from convergent incident experience. Specific rules (never hardcode, never log secrets, scope credentials narrowly) are Moderate; the claim that any specific vault product prevents leaks is not made.

### S4. Supply-chain attacks exploit the build and dependency graph [Strong for documented incidents; Moderate for defenses]

[SLSA v1.0](https://slsa.dev/spec/v1.0/levels) defines provenance levels addressing documented attack classes; GitHub's [secure use of Actions](https://docs.github.com/en/actions/reference/security/secure-use) documents the specific hardening steps for the dominant CI platform (Strong vendor facts); Spracklen et al. (USENIX Security 2025) demonstrated systematic LLM package hallucination creating supply-chain injection vectors (Strong, controlled measurement; verified via) [agents-md research base](https://github.com/ChloeVPin/agents-md/blob/main/docs/research.md)). `dependency-security-audit` and `build-and-ci-integrity` rest on this combination.

### S5. Privacy risk extends to agent inputs, outputs, and telemetry [Moderate]

[NIST AI 600-1](https://nvlpubs.nist.gov/nistpubs/ai/NIST.AI.600-1.pdf) (Generative AI Profile) enumerates generative-AI-specific risks including data leakage and confabulation, extending the [AI RMF](https://www.nist.gov/itl/ai-risk-management-framework). `privacy-and-data-handling` and `observability-and-instrumentation` apply its risk framing to agent workflows (Moderate: authoritative risk enumeration, not evaluated controls).

### S6. Agent tool authorization is a distinct, under-provened attack surface [Moderate]

The [OWASP AI Agent Security Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/AI_Agent_Security_Cheat_Sheet.html) and [MCP Security Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/MCP_Security_Cheat_Sheet.html) document least-privilege tool scoping, human confirmation for high-impact actions, and confused-deputy risks; NIST's [AI Agent Standards Initiative](https://www.nist.gov/artificial-intelligence/ai-agent-standards-initiative) signals active standardization. The domain is young; grades stay Moderate with explicit freshness review triggers.

### S7. Prompt injection is a demonstrated, unsolved threat class [Strong for the threat; Principled for specific mitigations]

Greshake et al. (DIMVA 2023, [arXiv:2302.12173](https://arxiv.org/abs/2302.12173)) demonstrated indirect prompt injection against real LLM-integrated applications; OWASP lists it as [LLM01](https://genai.owasp.org/llmrisk/llm01-prompt-injection/) and publishes the [Prompt Injection Prevention Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/LLM_Prompt_Injection_Prevention_Cheat_Sheet.html). Strong that the threat is real and empirical; the field has no complete defense, so the skill's specific controls (content/action separation, least-privilege, human confirmation) are Principled mitigations with an explicit "residual risk remains" statement.

### S8. Contradiction handling follows evidence-hierarchy reasoning [Principled]

`contradiction-resolution` applies F4's epistemics plus the source-hierarchy rules in [`evidence-standard.md`](evidence-standard.md). No direct study of contradiction-resolution procedures exists; graded Principled and kept out of Strong claims.

### S9. Knowledge needs explicit freshness and supersession semantics [Moderate]

RFC 9745/RFC 8594 (F5) standardize lifecycle signaling for HTTP; MAST and FAROS (F1, F2) document that stale or misread context drives agent failure. `knowledge-maintenance` combines both (Moderate for the model, Inferential for its effectiveness in skill libraries specifically).

## What the evidence does NOT cover

Stated explicitly, as in the companion repository's research base:

- **No study shows SKILL.md files improve agent outcomes.** Even where the underlying domain evidence is Strong (Q1, C1, S7), the intervention (packaging guidance as a skill) is Inferential at best. Every skill's effectiveness remains unproven until exercised on representative tasks.
- **MAST failure modes not addressed by this library**: context loss (FM-1.4, 28.0%), conversation reset, information withholding, ignored agent input, step repetition. These are multi-agent/continuity problems; a per-task skill library does not solve them. Listed for honesty about scope.
- **Prompt-injection defense is incomplete by consensus.** No cited source claims prevention; the skill's value is harm reduction, not immunity.
- **Practitioner references (SRE, OWASP, DORA, Fowler) are not controlled studies.** Where they are the best available evidence, findings are graded Moderate or Principled, never Strong.
- **Single-experiment domains**: Q2 rests on one small controlled experiment; if a second independent study contradicts it, the affected guidance is revised.

## Reading guide

Each skill's "Validation evidence and provenance" section maps its material claims to the finding IDs above (F*, C*, Q*, S*) and states confidence, freshness triggers, and what would disconfirm it. [`docs/skill-audit.md`](skill-audit.md) records the per-skill evidence review decisions. Changes to a finding here require re-reviewing every skill that cites it.






