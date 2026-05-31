# Multi-Model LLM Security Auditing: Lessons from the IQC Token Review

**Status**: Draft / Work in Progress  
**Date**: June 2026

## Abstract

This document reflects on a multi-month experiment in using multiple frontier large language models to perform security auditing of a production-intent smart contract system (the IQC token). We describe the process, analyze its strengths and serious limitations, and propose preliminary methodological principles for future work in this area.

## 1. Motivation

Traditional smart contract security auditing is expensive, slow, and dependent on a small number of highly specialized human experts. As AI capabilities have advanced, there has been growing interest in using large language models to assist with (or even partially automate) security review.

Most early attempts involved feeding a model a codebase and asking it to find bugs. Results were mixed — models could surface known vulnerability patterns but frequently hallucinated issues, missed context-dependent problems, and lacked the ability to reason rigorously about economic incentives and system-level invariants.

We hypothesized that **structured multi-model collaboration** — deliberately pitting different models against each other, iterating on their critiques, and synthesizing their outputs — might produce higher-quality results than any single model working in isolation.

The IQC token project provided a real, high-stakes target on which to test this hypothesis.

## 2. Process Overview

Over several months, we collected reviews from multiple models (including multiple instances and prompting regimes of Grok 4.3, Claude Opus 4.7/4.8, and Kimi 2.5). The process evolved organically:

- Initial broad reviews
- Sharing of previous model outputs as context
- Iterative refinement of findings
- Head-to-head debates between the strongest critics
- Synthesis documents attempting to reconcile conflicting perspectives

We produced three main classes of artifacts:
- Individual model responses
- Structured head-to-head discussion packages
- Synthesized reports and risk tables

## 3. Key Observations

### 3.1 Severity Calibration Varies Significantly

Early models tended to produce more alarming severity ratings. Later models, when shown previous work and explicitly prompted toward skepticism, consistently downgraded many findings. This suggests that "consensus" across models is heavily influenced by prompting order and framing rather than independent convergence on truth.

### 3.2 The "Verification Gap" is Severe

The single most consistent and damning critique across the strongest reviewers (particularly late-stage Opus 4.7 and Kimi 2.5) was that **no model in the entire process ever executed code, ran static analysis, or produced a working proof-of-concept**.

All analysis was purely textual. This creates a fundamental ceiling on credibility. Models can reason about code they are shown, but they cannot discover what they were not shown, and they cannot validate their hypotheses against reality.

### 3.3 Phantom Findings and Severity Theater

Several findings that survived early rounds were later identified as likely non-issues or mis-categorized (most notably the "Burn Permission Feasibility Gap"). Once a finding is introduced by one model, subsequent models can be reluctant to fully dismiss it, creating a form of severity inflation through social dynamics in the review process.

### 3.4 Trust Model is Foundational

A recurring theme was that many severity ratings were meaningless without an explicit trust model (who controls privileged roles, what capabilities they have, and what the threat model actually is). This is not a new insight in security, but it was striking how often it was under-specified even in relatively advanced AI-generated analysis.

## 4. Strengths of the Multi-Model Approach

Despite its limitations, the process had genuine value:

- **Diverse failure modes**: Different models caught different classes of issues.
- **Pressure against inflation**: Later, more skeptical models served as an effective correction mechanism against overconfident early reviews.
- **Head-to-head debates** surfaced higher-quality reasoning than solo reviews.
- The process forced explicit documentation of reasoning that might otherwise have remained implicit.

## 5. Fundamental Limitations

- **No ground truth**: Without execution, fuzzing, or formal verification, there is no reliable way to distinguish signal from sophisticated hallucination.
- **Correlated blind spots**: All current frontier models share similar training data and architectural limitations.
- **Prompt sensitivity**: Small changes in how previous work is presented dramatically affect later outputs.
- **Lack of economic reasoning depth**: Models struggled with deeper cryptoeconomic and incentive questions compared to code-level bugs.

## 6. Proposed Methodological Principles

Based on this experiment, we propose the following minimum standards for future multi-model LLM security work:

1. **Tooling is non-negotiable**. Any claim above Medium severity should be accompanied by actual code execution (tests, fuzzing, static analysis).
2. **Trust model first**. No severity rating should be assigned before the trust model is explicitly documented.
3. **Findings must be deduplicated and categorized**. Separate security vulnerabilities from design debt, tokenomics issues, and process problems.
4. **Acknowledge correlation**. Do not present multiple LLM reviews as "independent verification."
5. **Head-to-head debate has higher signal** than parallel independent reviews.
6. **Human judgment remains central**. The most valuable contributions in this process came from synthesizing and critically evaluating model outputs, not from the raw outputs themselves.

## 7. Future Directions

Promising areas for further research include:
- Hybrid workflows that tightly integrate LLMs with actual verification tools (Foundry, Slither, Echidna, Halmos, etc.).
- Structured debate formats between models with explicit scoring.
- Using models to generate high-quality invariants and properties for formal verification, rather than trying to find bugs directly.
- Longitudinal studies on how model performance on security tasks evolves over time.

## 8. Conclusion

Multi-model LLM collaboration is a useful research and augmentation technique, but it is not yet (and may never be) a substitute for rigorous, tool-supported security engineering. The primary value we derived was not a dramatically more accurate list of bugs, but a much clearer understanding of where current AI systems are strong, where they are weak, and what methodological guardrails are required when working with them on high-stakes technical work.

The IQC audit process served as an excellent stress test for these ideas. Future work should focus on closing the verification gap rather than simply adding more models to the conversation.

---

*This document is itself a living artifact and will be updated as the research continues.*