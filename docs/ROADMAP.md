# Prototype roadmap and evaluation

All milestones below are proposed. None are marked complete.

## v0.2 additions to milestone acceptance

Milestones 1–2 must implement the [control and evidence contract](CONTROL_CONTRACT.md): typed claim records, hard/soft constraint separation, a protected evidence recorder, receipt-bound commitment, and explicit unresolved outcomes. Start with an authenticated hash chain and checkpoint retention; compact Merkle proofs are optional later work.

Milestone 1 fixtures must cover missing required predicates, illegal status upgrades, cyclic evidence ancestry, expired/widened grants, and aggregate overspending across siblings. Milestone 2 must demonstrate receipt replay in an independent verification process, tamper detection, and rejection of evidence for another artifact or policy version.

Before Milestone 5, freeze the grader contract and environment by digest. Keep held-out fixtures and grader credentials outside all agent-accessible files, prompts, and services. Give agents public requirements and development tests. Record controller acceptance before the independent evaluation; never let final-grade feedback repair the same held-out trial. If grading is defective, quarantine and adjudicate affected tasks transparently rather than silently relabeling them.

Add deadline pressure, impossible requirements, deliberate test deletion/weakening, grader-substitution attempts, omitted-check receipts, capability widening, and budget exhaustion to the failure suite. Treat infrastructure failure separately from a failed task predicate, and preregister exclusions so the system cannot improve its apparent score by discarding difficult cases.

Report both `incorrect accepted / all accepted` and `incorrect accepted / all independently incorrect` with names, counts, and uncertainty. Include rejected and unresolved cases in the outcome table. Publish coordination costs and independent-verifier costs separately. These additions do not create a new engine or protocol layer.

## Milestone 1: Contracts and replayable controller

Implement task states, message schemas, immutable artifact references, state versioning, and bounded retries. Use scripted engines before connecting models.

**Exit condition:** recorded synthetic tasks can be replayed with the same acceptance decisions; stale state, duplicate results, malformed messages, and checker timeouts produce explicit outcomes.

## Milestone 2: Deterministic checks and isolated execution

Build an action gate and sandbox wrapper. Implement path, tool, budget, and artifact checks. Capture actual execution evidence independently of the agent. Select a concrete evidence format and trust model before claiming protocol conformance.

**Exit condition:** a worker cannot bypass the gate, fabricate a passing execution record, or commit a result against a different artifact revision in the scoped test environment.

## Milestone 3: Beam-powered execution adapter

Connect a real agent harness and model endpoint. Declare the exact model version, provider, runtime version, tool set, inference parameters, and limits. Handle disconnects, exhausted quotas, cancellations, and partial artifacts.

**Exit condition:** an end-to-end sandbox task yields an inspectable artifact and independently recorded check results. No hard-coded successful outputs may be substituted for actual execution.

## Milestone 4: Cooperative reasoning

Add the neural proposal interface and finite-candidate fusion baseline. Feed checker failures and observed outcomes back to the engines through versioned messages. Record which updates changed a decision and why.

**Exit condition:** the system demonstrates a bounded revision based on a real failed check, plus correct termination when constraints cannot be satisfied. Document separately which fusion operations remain heuristic.

## Milestone 5: Comparative evaluation

Freeze the evaluation task set and grading criteria before tuning on it. Start with a small held-out collection of code repairs with independent tests and controlled environments. Expand only after the execution and grading path is stable.

| Variant | Purpose |
| --- | --- |
| Beam-powered harness alone | Baseline agent performance |
| Harness plus the same action gate and checker | Isolate the value of enforcement |
| Controller plus neural and deterministic engines | Isolate structured coordination |
| Full system with symbolic-probabilistic or explicitly labeled scoring engine | Measure the marginal contribution of fusion |

Use the same underlying execution model, tool access, tasks, and sandbox policy wherever applicable. Match total token, tool, and time budgets across variants or report budget-performance curves. Extra model calls must count toward the full system's cost.

Repeat stochastic runs, report sample counts and uncertainty, and retain failures. Do not select only demonstrations that favor the full system.

| Metric | Operational definition |
| --- | --- |
| Task success | Fraction of tasks passing independent task-level grading |
| False acceptance | Accepted results that fail independent grading; report numerator and denominator |
| False rejection | Rejected results later found acceptable under the declared grading contract |
| Constraint violations | Violations that actually executed, reported separately from blocked attempts |
| Evidence integrity failures | Fabricated, altered, stale, or misbound evidence incorrectly accepted |
| Unresolved tasks | Tasks stopped by timeout, contradiction, insufficient evidence, or exhausted budget |
| Latency | Median and tail end-to-end completion times |
| Cost | All inference tokens, compute, tool usage, and paid service costs; show free tiers separately |
| Coordination overhead | Time and compute spent messaging, checking, and revising |

## Required adversarial and failure cases

- A passing test record belongs to an earlier code revision.
- A model reports a successful command that never ran.
- Two engines repeat one unsupported claim and appear to corroborate it.
- The checker times out or returns `UNKNOWN`.
- Constraints are mutually incompatible.
- A permitted action becomes invalid after relevant state changes.
- The runtime proposes a tool call outside its capabilities.
- A retried request would duplicate an external effect.
- A task is canceled while its worker is still running.
- A tool returns misleading content or instructions that conflict with the task's authority rules.

## Decision rule for continuing

Continue investing in fusion if it improves independently graded success or reduces false acceptance at an acceptable cost compared with the simpler configurations. Choose the acceptable tradeoff before the final evaluation. Publish null or negative results too.

If the checker-only variant performs as well at lower cost, simplify the architecture. If stale evidence dominates failures, fix state and evidence handling before adding more reasoning engines.

## Later research

Evaluate calibrated probabilistic inference, richer formal checks, task-dependent routing, and protocol conformance. Hardware acceleration is a separate investigation after profiling demonstrates a bottleneck. Generalization beyond code repair requires new domain-specific acceptance contracts and independent graders.
