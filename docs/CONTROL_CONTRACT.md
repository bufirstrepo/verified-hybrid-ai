# Control and evidence contract v0.2

Compact implementation contract for Milestones 1–2. Proposed behavior; not implemented or validated. This contract supplements [the architecture](ARCHITECTURE.md).

## Constraints and authority

**Hard constraints** are mandatory predicates under a pinned policy version. A failed or unknown required check blocks the action or commit. **Soft constraints** rank otherwise eligible candidates and never compensate for a hard failure. Responses to hard failure are bounded revision, rejection, or recorded escalation.

The controller cannot waive a hard constraint during acceptance. A separately authorized policy change creates a new version, records the authorizer and rationale, and triggers revalidation. Non-overridable invariants stay mandatory. A changed-policy result cannot be reported as passing the original policy.

## Minimal claim record

| Field | Type and invariant |
| --- | --- |
| `claim_id`, `task_id`, `schema_version` | Strings; unique claim identity within a task; supported schema required |
| `kind`, `predicate_id`, `arguments` | Allowlisted claim type, versioned predicate identifier, schema-typed arguments; no executable free text |
| `scope` | Typed object binding tenant, task, run, and relevant resources |
| `assumptions`, `derived_from` | Lists of claim IDs; ancestry must resolve and be acyclic; unsupported premises remain hypotheses |
| `state_version`, `policy_hash` | Nonnegative integer and content digest; bind the evaluation context |
| `artifact_hashes`, `evidence_refs` | Typed digest and record-reference lists; evidence must match these exact artifacts and this claim |
| `confidence` | Tagged `none`, `score`, or `calibrated_probability`; calibrated values require calibration-version metadata |
| `status` | `PROPOSED`, `SUPPORTED`, `REFUTED`, or `UNRESOLVED`; only the trusted verifier assigns the last three |

The message envelope adds authenticated sender identity and delivery/replay metadata. Repeated evidence does not become independent corroboration. Confidence never upgrades evidence status. `SUPPORTED` means the declared predicate is established under recorded assumptions, not universal truth.

## Evidence boundary and receipt

Assume models and agent processes may lie, replay records, or propose unauthorized actions. They cannot access recorder signing keys, modify protected logs, or directly invoke gated tools. The trusted base still includes the OS/isolation layer, executor instrumentation, checker, gate, and controller.

Use an authenticated hash chain with externally retained checkpoints for the prototype; verification requires the relevant chain segment. A Merkle implementation can later provide compact inclusion proofs. Neither scheme alone proves freshness, completeness, accurate observations, or absence of log forks.

Each acceptance receipt binds the claim, exact artifact digest, before/after state versions, policy hash, required predicates and verdicts, checker version, evidence records, checkpoint, and grant chain. An independent checker must validate the complete required predicate set and evidence binding, not merely a controller signature. Historical receipts remain historical; current use requires a freshness check.

## Bounded progress and escalation

Fix revision, time, token, and tool budgets before execution. Exhaustion yields `UNRESOLVED` with a reason; contradiction, missing evidence, or stale evidence can trigger abstention and escalation. `REJECTED` means a known failure; `CANCELED` is distinct. No success is inferred from silence or timeout.

A handoff includes the task, conflicting proposals, failed/unknown constraints, evidence, artifact/state/policy identities, budget use, and the decision requested. Human responses are authenticated and scoped; resumption revalidates against current state.

## Evaluation firewall

Freeze and hash the grading contract, grader environment, and private assessment fixtures before evaluation. Expose task requirements and development checks, but isolate held-out grading assets from every engine. Candidate acceptance is recorded before independent grading. Final grades cannot feed repair on the same held-out trial. Agent-authored tests cannot replace the frozen grader. Report false acceptance both per accepted result and per independently incorrect result, with explicit counts and denominators.
