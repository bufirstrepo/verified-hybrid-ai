# Architecture proposal v0.1

This document specifies proposed behavior, not implemented functionality. The original concept is Dino Silvestri's; the contracts below are engineering proposals for making that concept testable.

## 1. Authoritative state

The C++ controller maintains accepted facts, pending hypotheses, constraints, task dependencies, capabilities, and artifact references. An engine may submit a proposed update but cannot directly mutate authoritative state.

Each accepted update advances a monotonic state version. Results identify the state version they read. If relevant state changes before commitment, the controller revalidates the result or rejects it as stale. A lock, transaction, or equivalent compare-and-swap mechanism must protect commitment; a version field alone does not prevent races.

Accepted facts and hypotheses occupy separate namespaces. A hypothesis retains its origin and uncertainty even when repeated by another engine. A previous successful result remains a historical event; it is not automatically a statement about current state.

## 2. Three cooperating engines

### Neural engine

Accepts task context and structured constraints; produces candidate plans and hypotheses. Its structured output must pass schema validation before downstream processing. A valid schema does not establish a valid interpretation of the user's task.

### Deterministic engine

Evaluates explicit predicates over typed data. Initial checks may cover allowed paths, tool permissions, execution budgets, artifact identity, and required verification events. A SAT/SMT solver can be added when the task actually requires logical constraint solving.

Verdicts are `PASS`, `FAIL`, or `UNKNOWN`, with the checked predicate, rule version, assumptions, and evidence references attached. `UNKNOWN` includes timeout and unsupported checks. It never silently becomes `PASS`.

Determinism means repeatability under specified inputs and execution conditions. It does not mean that the rules are complete or that their premises are correct. External observations must be captured as inputs rather than read invisibly during a supposedly reproducible check.

### Symbolic-probabilistic engine

For the first prototype, use a finite candidate set with explicit structured features and a documented scoring model. Filter candidates through hard constraints before ranking. If ranking scores have not been calibrated, call them scores rather than probabilities.

For a later research version, evaluate an explicit probabilistic graphical model or probabilistic logic program. Define random variables, conditional dependencies, observation models, and the update algorithm. This is where true probabilistic fusion must be implemented and tested.

Hard constraints remain mandatory. Uncertain assumptions remain labeled. Evidence derived from the same observation is not independent evidence. A high-scoring candidate can still be rejected.

The finite scoring prototype tests orchestration; it does not demonstrate the full probabilistic fusion research claim.

## 3. Proposed message envelope

These are application fields carried inside protocol payloads, not fields asserted to be part of A2A or MCP themselves.

| Field | Purpose |
| --- | --- |
| `schema_version` | Select exact parsing and interpretation rules |
| `message_id`, `task_id`, `parent_task_id` | Identify the message and task lineage |
| `sender_id`, `engine_version` | Attribute the producer and implementation |
| `state_version` | Identify the state on which the result depends |
| `kind` | Distinguish proposal, constraint result, observation, revision, or completion |
| `claims` | Typed claims with explicit scope and assumptions |
| `constraint_set_id`, `constraint_set_hash` | Bind the applicable rule set |
| `artifact_refs` | Content-addressed inputs and outputs |
| `evidence_refs` | References to externally recorded evidence |
| `derived_from` | Preserve evidence ancestry and dependency relationships |
| `verdict` | Report pass, fail, unknown, or a non-verdict proposal |
| `budget`, `deadline` | Bound computation and retries |
| `capability_ref` | Refer to authority, without embedding reusable credentials |

Transport authentication identifies who sent a message. The receiver independently checks whether the sender is authorized, the claim is supported, the evidence is fresh, and the proposed action is allowed.

## 4. Evidence and trust

The trusted execution wrapper records tool identity, arguments or safe argument hashes, input artifact hashes, output artifact hashes, exit status, environment identity, task ID, state version, and time. Logs must not expose credentials or unnecessarily retain sensitive tool output.

An append-only authenticated log establishes integrity and ordering under its trust assumptions. It does not establish that a test suite is adequate or that a sensor is accurate. A Merkle inclusion proof shows that a record belongs to a committed log; the verifier still checks signatures, trusted keys, claim semantics, task binding, and freshness.

The model cannot write attested execution records or access the recorder's signing key. A model's self-report may be retained as a proposal but cannot satisfy an independently observed execution claim.

The trusted computing base includes the controller, action gate, checker implementation, execution wrapper, evidence recorder, and underlying isolation mechanisms. Bugs in that base remain a failure mode. Keep it small and versioned.

## 5. Scheduling and bounded communication

The controller represents task dependencies as a DAG for a given planning round. Feedback between engines is handled as explicit successive rounds, with new task instances and versioned inputs. The overall reasoning loop may be cyclic; calling each round a DAG does not remove those cycles.

Each task has a maximum wall-clock budget, tool budget, revision count, and terminal outcome. An unchanged rejected proposal should not trigger unlimited retries. Contradictory hard constraints lead to an unresolved task with an explanation, not automatic rule relaxation.

Late results cannot overwrite a newer decision. Cancellation propagates to child tasks and tool jobs. Retries use idempotency keys where supported; non-idempotent actions require explicit duplicate suppression or reconciliation.

## 6. External execution

Beam is the initial proposed model for an external agent runtime. The runtime maintains its own bounded execution loop and invokes tools through a controller-owned action gate. An A2A-facing adapter can expose the runtime as a task-taking agent. MCP clients can connect the runtime and controller to tool services. Native support for every selected protocol must be verified or implemented through adapters.

The gate checks each consequential action immediately before execution against current state and scoped capabilities. The runtime must not retain an alternate route to tools that bypasses this gate. Isolation must enforce the permitted filesystem, network, and process access; policy written only in prompts is insufficient.

After execution, independent observations return to the controller and can be distributed to all three engines. The controller accepts or rejects the proposed state change. Retrospective rejection cannot undo an already executed external effect, which is why pre-execution enforcement is necessary.

Initial prototype scope is an isolated working copy with no publishing or production credentials. This gives the architecture a bounded task whose results can be inspected and reproduced.

## 7. Acceptance procedure

1. Parse and validate the result contract.
2. Verify sender identity, authority, and task lineage.
3. Verify evidence integrity, freshness, and exact artifact binding.
4. Evaluate all required task-specific acceptance predicates.
5. If any required predicate fails or is unknown, reject or request bounded revision.
6. Recheck relevant state dependencies and atomically commit the accepted result.
7. Record the decision, rule version, and supporting evidence.

The controller decides acceptance according to a declared contract. It is not a universal semantic oracle.

## 8. Initial implementation boundaries

Use separate controller, checker, reasoner, fusion, and executor interfaces so each can be replaced or tested in isolation. Keep the deterministic checks and final acceptance path distinct: the engine produces check results; the controller verifies that the required checks exist and applies the commitment rules.

The first build can run with CPU-based checking and remotely hosted model inference. Specialized REASON hardware, a shared GPU scheduler, joint training, and end-to-end differentiability are outside the initial scope.
