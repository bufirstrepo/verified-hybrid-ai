# Community launch copy

These drafts are ready to adapt for a GitHub discussion or AI developer community. Add the final repository URL when publishing; this package has not been posted anywhere.

## Repository description

A verification-centered AI architecture: C++ orchestration, three cooperating reasoning engines, evidence-backed state updates, and Beam-powered agent execution.

## Suggested discussion title

Design feedback wanted: three cooperating reasoning engines under a verifiable C++ controller

## Main pitch

I’m developing an AI architecture that brings neural reasoning, deterministic constraint checking, and symbolic-probabilistic inference into one cooperating system.

A C++ controller owns the plan and accepted state. The three reasoning engines exchange intermediate results, constraints, and revised hypotheses. When a task requires code execution or other tool use, the controller delegates it to an external agent runtime powered by Beam. Tool actions are checked before execution, and the results return with evidence tied to the actual task and artifacts.

The part I want to explore is the interaction between these layers: can continuous feedback improve the quality of proposals while explicit acceptance rules keep the system's state consistent? Can the system distinguish a plausible answer from a result it has enough evidence to accept?

The design uses A2A for agent communication, MCP for tool and service integration, and PCM-inspired evidence handling. It draws on GURU's authority split and REASON's treatment of structured reasoning workloads, while keeping the first prototype independent of specialized hardware.

This is currently an architecture proposal. The repository includes the execution contracts, trust boundaries, prototype roadmap, and an evaluation plan. I’m starting with sandboxed code repair and comparing the full design against Beam alone and simpler checker-based variants under matched budgets.

The latest revision makes each acceptance auditable through a receipt bound to the policy, state, artifacts, and evidence. It also specifies narrowing delegation, explicit abstention, bounded escalation, and a frozen independent grader so the system cannot improve its apparent performance by rewriting its own success criteria.

I’m looking for collaborators and critical feedback on the shared representation, fusion algorithm, evidence verification, and benchmark design. In particular, I want to understand where the third reasoning engine adds measurable value—and where a simpler system would be better.

The architecture concept is mine; the models, protocols, and research it builds on are credited in the repository. I want to turn it into something the community can inspect, implement, and test.

— Dino Silvestri

## Short version

I’m designing a hybrid AI system with a C++ controller, three communicating reasoning engines, and an external Beam-powered execution runtime. Proposals pass explicit constraints; accepted results require evidence tied to actual execution. The architecture and prototype evaluation plan are ready for review. I’m looking for feedback on fusion, verification, and whether the full system outperforms simpler baselines.

## Suggested GitHub topics

`neuro-symbolic-ai`, `multi-agent-systems`, `agent-orchestration`, `verification`, `probabilistic-inference`, `cpp`, `mcp`, `a2a`

## Suggested first discussion questions

1. What is the smallest useful representation that all three engines can share?
2. Which evidence claims should the first prototype support?
3. What would convince you that fusion adds value beyond propose-and-check?
4. Which failure case should be demonstrated before anyone trusts a successful demo?
