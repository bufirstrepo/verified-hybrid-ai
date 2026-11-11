# Sources and relationship to prior work

Sources reviewed during preparation of this proposal, November 10, 2026. External model catalogs, access terms, and protocol documentation can change. These sources support individual design ingredients; none validates this proposed integration as a whole.

## A2A

[A2A Protocol v1.0 announcement](https://a2a-protocol.org/dev/blog/2026/03/12/a2a-protocol-ships-v10-production-ready-standard-for-agent-to-agent-communication/) and [protocol specification](https://a2a-protocol.org/dev/specification/).

Role: interoperability between task-taking agents. The project's claim semantics, authority rules, and verification logic remain application responsibilities. Pin the chosen protocol revision during implementation rather than relying on a moving documentation URL.

## MCP

[Official MCP SDK introduction](https://py.sdk.modelcontextprotocol.io/get-started/first-steps/).

Role: client/server integration for tools, resources, and prompts. Structured tool results do not automatically become exact, trusted, or proven results. C++ client support and adapter choices require an implementation-specific review.

## PCM

Ghosh, B. [PCM: A Proof-Carrying Message Protocol for Verifiable Multi-Agent Communication](https://arxiv.org/abs/2610.39109). Preprint, 2026.

Role: inspiration for typed claims supported by independently attested append-only records. The paper's threat model excludes agent control of the attesting signer and log-writing authority. Its reported evaluation includes false acceptances caused by stale evidence. This proposal therefore explicitly requires freshness and state binding.

The prototype should be called PCM-inspired unless actual implementation conformance is established. Log integrity is not a universal guarantee of semantic truth.

## REASON

Wan, Z., et al. [REASON: Accelerating Probabilistic Logical Reasoning for Scalable Neuro-Symbolic Intelligence](https://arxiv.org/abs/2601.20784). [Full text](https://arxiv.org/html/2601.20784v1).

Role: structured representation and scheduling of symbolic and probabilistic computations. REASON describes a specialized acceleration framework integrated with GPU streaming multiprocessors. It is not simply an off-the-shelf multi-agent scheduler, and its hardware speedups cannot be attributed to this software proposal.

## GURU

[GURU: A Hierarchical Neuro-Symbolic Multi-Agent System for Verification-Aware Automation of Mission-Critical Digital Engineering and Operations](https://arxiv.org/abs/2611.01001). Preprint, 2026.

Role: the proposed division between learned proposals, symbolic constraints, and validated domain-specific decision machinery. This project adapts that separation to a controller-owned acceptance contract. Merely writing the controller in C++ does not make it validated. The relationship here is based on the paper's stated authority split, not an independent reproduction of its results.

## Beam and Nous Portal

[Reflection: Beam is now available](https://reflection.ai/blog/beam-is-available), [Beam model card](https://reflection.ai/beam-model-card), and [Nous Portal integration documentation](https://hermes-agent.nousresearch.com/docs/integrations/nous-portal).

Role: Beam is the proposed execution model, developed by Reflection and previewed as MISSINGNO through Nous Portal. The model works inside a harness connected to tools and compute. Model inference, agent runtime, sandbox, and tool-service access are separate dependencies. Current free access, quotas, and continued availability must be checked at deployment; the architecture must not depend on permanent free service.

## Scope of the literature review

The supplemental [research review](RESEARCH_REVIEW.md) maps SCL, FormalJudge, ATA, patch-correctness evaluation, uncertainty-gated logical reasoning, and zkARc to concrete design additions. It records which sources were inspected and which claims remain unverified. The [control contract](CONTROL_CONTRACT.md) is this project's proposed synthesis, not a claim of implementation or conformance to those systems.

This is a focused source check for the technologies named in the proposal, not a systematic novelty review. Before making research priority claims, compare implemented neuro-symbolic systems, blackboard architectures, runtime assurance, tool-gated agents, and evidence-aware multi-agent systems. Record similarities as well as differences.
