# Review of supplemental research

Prepared November 10, 2026. This is a targeted check of the sources supporting the proposed additions, not verification of every URL in the supplied search-results list. No external experiment was reproduced. Paper-reported performance is not project performance.

## Source-to-design mapping

| Source checked | What the inspected source supports | How it affects this proposal |
| --- | --- | --- |
| [SCL, v6 abstract](https://arxiv.org/abs/2511.17673v6) | Separates symbolic regulation of reasoning from deterministic runtime control; reports no policy violations in its experiments | Make hard enforcement and soft ranking explicit; do not claim universal zero violations |
| [PCM, v1, sections 12–13](https://arxiv.org/html/2610.39109v1) | Reports 10 false accepts out of 712 false messages, all involving stale evidence; its plain-A2A comparator performs no content checks | Retain independent records, freshness checks, and narrowed grants; the reported rate is specific to that benchmark |
| [FormalJudge, v1, method and limitations](https://arxiv.org/html/2602.11136v1) | Uses generated formal specifications and Dafny/Z3 checking; explicitly identifies intent-decomposition and semantic-extraction failures | Treat generated rules as candidates; formal proof remains conditional on the formalization and premises |
| [ATA, abstract](https://arxiv.org/abs/2510.16381) | Separates knowledge preparation from online processing, with human review of the symbolic knowledge base | Separate rule activation from task execution; human review does not remove all possible errors |
| [Patch-correctness evidence framework, abstract](https://arxiv.org/abs/2611.10837) | Distinguishes search-visible tests from final assessment evidence and discusses oracle dependence | Isolate held-out grading and report uncertain judgments and cost |
| [Prove What You Can, abstract](https://arxiv.org/abs/2611.10319) | Combines proposed proofs, local validation, and uncertainty-triggered repair in logical inference tasks | Motivate evaluating repair and abstention; it does not establish calibrated confidence for this architecture |
| [zkARc, abstract](https://arxiv.org/abs/2611.10031) | Describes proof receipts for specified formalization and verification computations under stated assumptions | Useful future comparison for receipts; zero-knowledge proving is outside the minimal prototype |

## Corrections to the supplied synthesis

**Hard constraints need policy governance, not an unrestricted override.** A controller that can silently waive any blocking check defeats the proposed authority split. Versioned policy changes must have separate authorization, preserve non-overridable invariants, and trigger revalidation. This is an engineering judgment for this project.

**Hash chains and Merkle proofs are different constructions.** A chain can provide tamper evidence against a trusted retained checkpoint, but checking an old record generally requires the connecting chain segment. Compact Merkle inclusion proofs require the corresponding tree structure. Neither alone prevents an operator from presenting different histories without checkpoint consistency or witnessing assumptions.

**Formal proof cannot repair a wrong interpretation by itself.** FormalJudge's limitations explicitly acknowledge the natural-language-to-formal-representation gap. A solver can prove a proposition about an inadequate specification. The proposed activation process therefore retains provenance, baseline preservation, satisfiability checks, and review of unresolved intent.

**Independent grading must not become another repair signal.** Freeze both contract and implementation/environment identities, protect private fixtures, and retain a held-out final assessment. Public task requirements remain visible. Feedback from a final assessment invalidates its held-out role for further search on that trial.

**Trace cards need checkable contents.** A human-readable explanation or signed controller statement is insufficient. The receipt must support predicate checking or proof-certificate verification and reveal omissions from the mandatory check set.

## Limits of this source check

NetFlow-DSR was identified through an arXiv search result for [2611.07040](https://arxiv.org/abs/2611.07040), but direct abstract and HTML retrieval failed. The specific trace-card/witness claim has not been verified here and is not presented as established support. Receipts are included as an explicit project design choice.

Direct retrieval also failed for [2610.39547](https://arxiv.org/abs/2610.39547), [2610.31895](https://arxiv.org/abs/2610.31895), and [2610.33509](https://arxiv.org/abs/2610.33509). No findings from those papers are attributed in this update. Retrieval failure is not evidence that a paper is invalid.

The supplied [SPRING-5 link](https://arxiv.org/html/2611.10040v1) concerns efficiency in tool-documentation repair. It should not be cited as direct evidence of agents weakening tests under pressure. This review did not establish the broad claim that agents alter tests under mild pressure; pressure and test-tampering cases are included as proposed adversarial tests without an asserted prevalence.

Other links from the supplied bibliography remain outside this targeted review. Aggregator summaries and search snippets are discovery aids, not substitutes for inspecting the source behind a specific claim.

## Adoption order

1. Milestones 1–2: claim contract, hard/soft partition, protected evidence, receipts, bounded outcomes, and protected evaluation design.
2. Before delegated execution: narrowed grants with shared budget accounting.
3. With the fusion prototype: abstention and explicit score semantics; calibration only when probabilities are actually produced.
4. Later research: specification compilation with authorized rule activation, richer proof certificates, and domain-specific human handoffs.

No new engine or protocol is introduced by this revision.
