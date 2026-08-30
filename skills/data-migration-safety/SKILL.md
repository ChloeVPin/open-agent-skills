---
name: data-migration-safety
description: Design and review database, schema, storage, and data migrations for correctness, compatibility, observability, rollback limits, and safe recovery. Use when an AI coding agent changes persisted data, schemas, indexes, formats, or migration scripts.
---

Lifecycle: `draft`

# Data Migration Safety

## Purpose and scope

Change persisted data or its representation without silently losing, corrupting, exposing, or stranding records. This skill covers planning, compatibility, execution, verification, and recovery for data migrations; it does not replace backup policy, data-owner approval, compliance review, or database-specialist review for high-risk systems.

## Triggers and prerequisites

Trigger when adding, removing, renaming, transforming, backfilling, partitioning, reindexing, or changing the meaning of stored data, schema, serialization, or retention. Prerequisites: current schema and data assumptions, data owners, producers and consumers, volume/distribution, availability constraints, backup/restore capability, migration tooling, and a defined success and recovery condition.

## Decision criteria

- “Migration succeeded” requires completeness, correctness, compatibility, and an observable integrity check, not merely a successful command.
- Prefer additive expand/migrate/contract steps when old and new application versions can overlap.
- A rollback of code does not undo a destructive or semantic data change; distinguish rollback, forward recovery, restore, and reconciliation.
- Idempotence, resumability, bounded batches, and checkpointing reduce operational risk but do not prove transformation correctness.

## Procedure

1. Inventory affected tables, files, records, indexes, constraints, producers, consumers, jobs, reports, permissions, retention rules, and external exports. Identify ownership and sensitive fields.
2. Define the transformation contract: source and target invariants, mapping for every source state, treatment of null/empty/duplicate/invalid/orphaned data, precision and ordering, and preservation requirements.
3. Measure the baseline: row counts, checksums or aggregates, distributions, referential relationships, representative samples, query behavior, and current error/backlog rates.
4. Choose a compatibility strategy: expand/migrate/contract, dual read/write, versioned format, maintenance window, or another justified approach. State which old/new versions may coexist and for how long.
5. Design the migration for bounded work, safe locking, transaction scope, retries, checkpoints, rate limits, resumability, idempotence, and failure visibility. Avoid unbounded transactions and silent partial completion.
6. Test on representative and adversarial data: empty and large sets, malformed values, duplicates, missing relations, boundary sizes, interrupted batches, retries, concurrent writes, old/new readers, and rollback or forward-recovery paths.
7. Run a dry run or staged subset when feasible. Compare post-migration counts, invariants, samples, errors, performance, locks, resource use, and application behavior against the baseline.
8. Execute with an owner and pause/abort criteria. Monitor progress and integrity, preserve checkpoints and logs, and stop on unexplained divergence instead of pushing through to completion.
9. Verify every success criterion, including data integrity, consumer compatibility, permissions, indexes/constraints, backups, cleanup, and removal of temporary dual-write or compatibility paths only after the migration boundary is proven.
10. Record the migration version, scope, evidence, residual anomalies, recovery procedure, irreversible actions, owner, and review/deprecation trigger.

## Examples and counterexamples

Good: Add a nullable column, deploy code that writes both representations, backfill in resumable batches with counts and checksums, switch readers after verification, then remove the old field in a later release.

Bad: Rename a column and deploy application code simultaneously, assuming deployment is atomic across all readers and workers.

Good: A data conversion defines how invalid records are quarantined, counted, corrected, and retried rather than silently dropping them.

Bad: Ignore conversion errors so the migration command exits successfully.

## Failure modes and recovery

If data ownership, invariants, or restore capability is unknown, stop and resolve them before modifying persisted state. If a batch partially fails, resume from a verified checkpoint or reconcile it; do not blindly rerun a non-idempotent operation. If integrity checks diverge, pause consumers where authorized, preserve evidence, and choose restore, rollback of compatible code, forward repair, or reconciliation based on the actual mutation. If the migration is irreversible, require stronger approval and a tested recovery plan before execution.

## Validation evidence and provenance
Claims in this skill map to graded findings in [`docs/research.md`](../../docs/research.md):

- Lock and rewrite semantics of schema operations (Q11, Strong vendor fact): PostgreSQL `ALTER TABLE` documents them precisely.
- Expand–migrate–contract pattern (Q11, Principled): Fowler's evolutionary database design; practitioner methodology.
- Recovery boundaries and integrity (Q11, Moderate): Google SRE data integrity chapter.

Source boundary: the sources support the pattern and the cost facts; migration correctness for a specific schema is verified by the procedure, not the sources.

Confidence: medium-high for reversible-change principles; medium for any specific migration until rehearsed on production-like data. Freshness: review when database engines, the research base, or migration tooling change.

Disconfirmation: engine-level features that make destructive operations safely reversible would simplify the expand/contract choreography.
## Related skills and conflicts

Related: `api-contract-compatibility`, `release-and-rollback-safety`, `secure-coding-review`, `performance-regression-analysis`, `observability-and-instrumentation`, `regression-test-design`, and `knowledge-maintenance`. This skill does not authorize destructive data changes, bypassing ownership approval, or calling a migration reversible without testing recovery.
