# Evidence standard

This document is the rulebook for every skill in this repository. A `SKILL.md` may only make claims that this standard supports. Reviewers (human or agent) reject, revise, or downgrade skills that do not pass it. The standard is adapted from the verification-first approach used in the companion repository [`ChloeVPin/agents-md`](https://github.com/ChloeVPin/agents-md), whose research base established the evidence-grade taxonomy.

## Why grades exist

A skill that sounds authoritative is not authoritative. Agents follow written instructions literally, so unsupported guidance becomes misapplied guidance. Every material claim in a skill must therefore carry a grade that tells the reader how much the claim is actually proven, and provenance that lets the reader verify it independently.

## Evidence grades

Every material claim in a skill (a decision criterion, a procedure step that rules out alternatives, a failure-mode assertion) is graded as one of:

- **Strong**: supported by controlled evaluation or large-scale empirical study with reproducible methodology.
- **Moderate**: supported by multiple real-world incident reports, postmortems, or convergent evidence from independent sources.
- **Inferential**: reasonable design inference from empirical findings, but not directly tested as an intervention in this skill's context.
- **Principled**: a sound design principle, not empirically proven as a failure-prevention mechanism.

Rules for assigning grades:

1. A claim is only as strong as its **single weakest** source. Ten weak citations do not make a Strong claim.
2. Copied summaries are **not independent**. Trace repeated claims to their origin; citing three pages that all paraphrase one paper is one source, not three.
3. Vendor documentation is authoritative for **what a tool does**, not for **whether using it this way prevents failures**. "The docs say X exists" is a fact about X, not evidence that the skill's procedure around X works.
4. Absence of a grade is not allowed. If a claim cannot be graded honestly, it is rewritten as speculation or removed.

## Provenance requirements

Each skill's **Validation evidence and provenance** section must contain:

1. **Claim → source → grade mapping.** Each material claim links to a specific source recorded in [`docs/research.md`](research.md) with its grade. No anonymous attributions such as "the governing research emphasizes…"; every claim names what supports it.
2. **Source boundary statement.** An acknowledgment that the sources are design input, not proof that the skill works in every repository or agent.
3. **Confidence statement.** High confidence is reserved for graded, sourced claims; medium or low is stated explicitly for procedural judgment calls.
4. **Freshness trigger.** The condition under which the skill's evidence must be re-reviewed (source update, tooling change, new failure evidence).
5. **Disconfirmation note.** At least one statement of what evidence would weaken or overturn the skill's guidance.

## Facts versus advice

Skills must separate:

- **Facts**: directly verifiable in a source or repository ("HTTP 429 means too many requests", per RFC 9110). Facts need a primary citation.
- **Advice**: recommended actions ("bound retries with exponential backoff"). Advice needs either a Moderate/Strong evidence base or an explicit Principled label with its tradeoffs stated.
- **Speculation**: untested hypotheses. Speculation must remain visibly uncertain and must never appear in a procedure step as if settled.

## Source quality hierarchy

When sources conflict, prefer in this order:

1. Standards and specifications (RFCs, W3C, ISO, ECMA, Unicode UTR/CLDR).
2. Primary empirical research (peer-reviewed papers with reproducible methodology).
3. Large-scale incident evidence (public postmortems, vulnerability databases, curated failure taxonomies).
4. Practitioner references by named, accountable organizations (Google SRE books, OWASP Cheat Sheet Series, DORA).
5. Vendor documentation for tool behavior only.
6. Individual opinion pieces: acceptable only as context, never as support for a material claim.

## Lifecycle promotion

A skill may move from `draft` to a higher lifecycle only when:

- All material claims are graded and sourced under this standard,
- The skill has been adversarially reviewed against counterexamples and neighboring skills,
- The procedure has survived at least one representative real task without structural revision forced by failure.

Skills whose domains lack strong evidence (agent-specific evaluation, prompt-injection defense) remain `draft` with an explicit statement of what evidence would promote them. Draft is an honest status, not a failure.

## Enforcement

The repository's validation workflow (`.github/workflows/validate.yml`) checks mechanically: frontmatter conformance to the [Agent Skills specification](https://agentskills.io/specification), name-to-directory match, required sections, link validity, and that every source cited in a skill appears in [`docs/research.md`](research.md). Grades, source independence, and disconfirmation notes are reviewed by the `skill-quality-review` skill and human review; they cannot be fully automated and are not pretended to be.

## Repository conventions

Two formatting rules are mandatory for all Markdown and configuration files in this repository, enforced by the validation workflow as pipeline checks (preferences for clarity and toolchain compatibility, not evidence-backed failure-prevention mechanisms):

1. **No em-dashes (U+2014).** Do not use the em-dash character anywhere. Use colons for labels introducing definitions or lists, commas for appositives, semicolons for independent clauses, and parentheses for true parentheticals. This avoids ambiguity in machine-consumed text and matches the direct style of the companion repository.
2. **Use the 9-section skill anatomy** (`Purpose and scope`, `Triggers and prerequisites`, `Decision criteria`, `Procedure`, `Examples and counterexamples`, `Failure modes and recovery`, `Validation evidence and provenance`, `Related skills and conflicts`, plus any domain-specific section such as `Acceptance checklist`) in every `SKILL.md`.

## Limits

This standard improves the honesty and traceability of the skills. It does not prove that any skill produces correct agent behavior in a given repository, and it does not replace tests, security review, or human judgment. An instruction file (including a well-sourced one) is an artifact to be verified, not a guarantee.
