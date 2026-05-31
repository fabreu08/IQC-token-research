# Multi-Model LLM Security Auditing: Lessons from the IQC Token Review

**Status**: Draft / Work in Progress  
**Date**: June 2026  
**Version**: 0.2 (Expanded with head-to-head insights)

## Abstract

This document reflects on a multi-month experiment in using multiple frontier large language models to perform security auditing of a production-intent smart contract system (the IQC token). We describe the process, analyze its strengths and serious limitations — particularly those revealed through direct head-to-head debate between the strongest models — and propose preliminary methodological principles for future work in this area.

## 1. Motivation

Traditional smart contract security auditing is expensive, slow, and dependent on a small number of highly specialized human experts. As AI capabilities advanced, interest grew in using large language models to assist with (or partially automate) security review.

Most early attempts involved feeding a model a codebase and asking it to find bugs. Results were mixed. Models could surface known vulnerability patterns but frequently hallucinated issues, missed context-dependent problems, and lacked rigorous reasoning about economic incentives and system-level invariants.

We hypothesized that **structured multi-model collaboration** — deliberately pitting different models against each other, iterating on their critiques, and synthesizing outputs — might produce higher-quality results than any single model in isolation.

The IQC token project provided a real, high-stakes target on which to test this hypothesis over many rounds of review.

## 2. Process Overview

Over several months, we collected reviews from multiple models (Grok 4.3 variants, Claude Opus 4.7/4.8, Kimi 2.5). The process evolved:

- Initial broad independent reviews
- Sharing of previous model outputs as context
- Iterative refinement
- Head-to-head debates between the strongest critics
- Synthesis documents attempting to reconcile perspectives

We produced individual responses, structured head-to-head packages, and synthesized reports/risk tables.

## 3. Key Observations

### 3.1 Severity Calibration Varies Significantly

Early models produced more alarming severity ratings. Later models, shown prior work and prompted toward skepticism, consistently downgraded findings. "Consensus" across models is heavily influenced by prompting order and framing rather than independent convergence on truth.

### 3.2 The "Verification Gap" is Severe

The most consistent critique from the strongest reviewers (late-stage Opus 4.7 and Kimi 2.5) was that **no model in the entire process ever executed code, ran static analysis, or produced a working proof-of-concept**.

All analysis was purely textual. This creates a fundamental ceiling on credibility.

### 3.3 Phantom Findings and Severity Theater

Several findings survived early rounds only to be later identified as non-issues or mis-categorized (most notably the "Burn Permission Feasibility Gap"). Once introduced, findings can persist due to reluctance to dismiss prior models' work. This creates "severity inflation through social dynamics."

### 3.4 "Consensus Theater" and Correlated Reasoning

A major insight from the head-to-head phase: Treating outputs from multiple LLMs as "independent verification" is fundamentally misleading. Models share training data and reasoning patterns. What appears as "convergence" is often the result of sequential prompting and model agreeableness rather than robust, diverse analysis. Later models downgrading earlier ones reflects prompt engineering more than discovery of truth.

### 3.5 The Lifecycle of Phantom Findings

The Burn Permission Gap serves as a diagnostic case study:
- Raised as a concern in one round.
- Persisted through multiple iterations because subsequent models were hesitant to fully dismiss it.
- Only in direct head-to-head debate did it become clear it was likely illusory (the Registry can burn from its own balance).

This reveals a systemic bias in sequential LLM review toward *conserving* findings rather than rigorously re-evaluating them.

### 3.6 Diminishing Returns Are Real and Severe

Analysis of the full process suggests the first 2–3 models captured the majority of genuine signal. Subsequent rounds produced:
- Diminishing new technical findings
- Increasingly meta commentary about the process itself
- Refinement of severity ratings (often downward)

The marginal value of rounds 4–8 was primarily methodological hygiene rather than new security insights.

### 3.7 Trust Model is Foundational

Many severity ratings remained indeterminate without an explicit trust model (who controls `Ownable2Step`, what the actual threat model is). This is not new in security, but it was striking how often it was under-specified in AI-generated analysis.

### 3.8 What LLMs Are Actually Good For

Tooling excels at pattern detection. LLMs can be useful for **adversarial scenario generation** — narrative reasoning about complex interactions between subsystems that are hard to encode as invariants. However, these scenarios still require human judgment and actual verification to become credible findings.

## 4. Strengths of the Multi-Model Approach

- Diverse failure modes across models
- Effective correction mechanism against overconfidence via skeptical later reviewers
- Head-to-head debate produced higher-quality reasoning than solo reviews
- Forced explicit documentation of reasoning

## 5. Fundamental Limitations

- No ground truth without execution/fuzzing/formal verification
- Correlated blind spots across models
- High prompt sensitivity
- Weak performance on deep economic/incentive reasoning compared to code-level bugs
- Strong bias toward severity inflation and phantom finding persistence in sequential review

## 6. Proposed Methodological Principles

Based on this experiment, we propose the following standards:

1. **Tooling is non-negotiable** for anything above Medium severity.
2. **Trust model first** — No severity rating without explicit documentation of privileged roles and threat model.
3. **Findings must be deduplicated and properly categorized** (security vs. tokenomics vs. disclosure vs. process).
4. **Acknowledge correlation** — Do not present multiple LLM reviews as independent verification.
5. **Head-to-head debate has higher signal** than parallel independent reviews.
6. **Limit iteration** — Expect strongly diminishing returns after 3 models. Further rounds should target new attack scenarios, not re-litigation of existing findings.
7. **Human judgment is the product**, not the raw model outputs.
8. **Document the process failures** — Phantom findings and consensus theater should be treated as first-class research outputs.

## 7. Future Directions

- Hybrid LLM + tooling workflows (models generate invariants and scenarios; tools verify)
- Structured debate formats with explicit scoring
- Using models primarily for property generation rather than bug finding
- Controlled experiments measuring marginal value of additional models
- Longitudinal studies as model capabilities evolve

## 8. Conclusion

Multi-model LLM collaboration is a useful research and augmentation technique with real but narrow value. It is not a substitute for rigorous, tool-supported security engineering. The primary lasting contribution of this project is not a dramatically more accurate bug list, but a clearer map of where current AI systems are useful, where they are dangerous, and what methodological guardrails are required.

The IQC audit process was an excellent stress test. Future work should focus on closing the verification gap and treating the LLM review process itself as an object of study, rather than simply adding more models to the conversation.

---

*This document is a living artifact and will be updated as the research continues.*