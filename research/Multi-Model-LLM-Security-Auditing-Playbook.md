# Multi-Model LLM Security Auditing Playbook

**A Practical Guide Based on the IQC Token Audit Experiment (2026)**

**Version**: 0.1 (Initial Draft)  
**Status**: Work in Progress

---

## 1. Introduction

This playbook distills lessons from a multi-month, multi-model security audit of the IQC token smart contract system. The goal was not only to audit the contracts, but to rigorously test the emerging practice of using multiple frontier large language models for deep technical security review.

Over eight perspectives (including repeated rounds with Opus 4.7 and Kimi 2.5), we learned a great deal about what works, what creates "theater," and what is required to produce credible output.

This document is intended as a practical reference for future teams attempting similar work.

---

## 2. Core Principles

### 2.1 Correlation Is Not Independence

The single most important lesson:

**Multiple LLMs do not equal independent verification.**

All current frontier models share heavily overlapping training data and similar reasoning architectures. What looks like "consensus" is often just correlated guessing shaped by the order and framing of prompts.

**Rule**: Never present "X models reviewed this" as evidence of robustness. At best, it provides diverse angles. At worst, it creates false confidence.

### 2.2 Tooling Is Non-Negotiable Above Medium Severity

No claim above Medium severity should be considered credible without actual code execution (tests, fuzzing, static analysis, or formal verification).

Text-only analysis has a hard ceiling. The strongest reviewers in this experiment (Opus 4.7 and Kimi 2.5) repeatedly and correctly emphasized this point.

### 2.3 Trust Model First

You cannot assign meaningful severity to owner-privileged functions without a clear trust model.

Many early findings in this audit became speculative the moment the undefined ownership was examined. This was one of the most repeated and valid criticisms.

### 2.4 Head-to-Head Debate Has Higher Signal Than Parallel Reviews

Direct debate between strong, skeptical models produced better reasoning than independent parallel reviews. It forces models to defend positions and exposes weak arguments.

### 2.5 Diminishing Returns Are Dramatic

In this experiment:
- The first 2–3 models captured the majority of genuine technical signal.
- Rounds 4–8 produced mostly refinement of severity ratings (often downward) and meta-commentary on the process itself.

**Practical rule of thumb**: Plan for 3 high-quality models. Anything beyond that should be narrowly scoped (e.g., "generate new attack scenarios we haven't considered yet").

---

## 3. Common Anti-Patterns ("Theater")

### 3.1 Consensus Theater
Treating sequential LLM outputs as independent validation. Later models inherit framing and bias from earlier ones.

### 3.2 Phantom Finding Persistence
A finding raised in an early round survives multiple iterations because subsequent models are reluctant to fully dismiss prior work. The Burn Permission Gap was the clearest example in this project.

### 3.3 Severity Inflation via Sunk Cost
Keeping a finding at high severity because "it was high in the first review" rather than re-evaluating it on current evidence.

### 3.4 Documentation Theater
Writing long, structured reports with tables and sections that create an appearance of rigor while lacking actual verification (PoCs, tool output, trust model).

---

## 4. Recommended Workflow

### Phase 1: Setup (Do This First)
1. Explicitly document the **Trust Model** (who controls what, threat assumptions).
2. Define **Invariants** the system must maintain.
3. Run baseline tooling (Slither, Mythril, basic Foundry tests).

### Phase 2: Initial Exploration (2–3 Models Max)
- Give models the code + Trust Model + invariants.
- Ask for findings + attack scenarios.
- Do **not** show them each other's work yet.

### Phase 3: Head-to-Head Pressure Testing
- Identify the 1–2 strongest (most skeptical + technically deep) responses.
- Put them in direct dialogue with specific points of tension.
- This is where the highest-quality reasoning usually emerges.

### Phase 4: Synthesis + Verification
- Human synthesizes the strongest points.
- **Every High+ finding must have a PoC or tool confirmation.**
- Produce the final report with clear scope, trust model, and verification status.

### Phase 5: Diminishing Returns Check
If you're on round 5+ and mostly re-arguing severity or methodology rather than finding new issues, stop. Move to tooling and human review.

---

## 5. Minimum Standards for Credible Output

Any output claiming to be a security review should include:

1. **Explicit Trust Model** (not optional).
2. **Scope statement** (what was reviewed and what was not).
3. **Invariants** the system is claimed to maintain.
4. **Tool output summary** (Slither, fuzzing, etc.).
5. **PoCs** for every Critical and High finding (Foundry preferred).
6. **Severity justification framework** (Impact × Likelihood is recommended).
7. **Clear distinction** between security vulnerabilities, tokenomics issues, and process problems.

If any of these are missing, the output should be labeled as "exploratory analysis" rather than "security audit."

---

## 6. When LLMs Add the Most Value

LLMs are currently best at:
- Generating novel attack scenarios and "what if" chains across subsystems.
- Identifying mismatches between stated goals and code behavior (tokenomics vs implementation).
- Surfacing UX and interface risks that static tools miss.
- Acting as adversarial red-team partners in structured debate.

They are currently weak at:
- Reliable vulnerability detection without heavy human guidance.
- Deep economic/incentive game analysis.
- Anything requiring actual execution or formal verification.

---

## 7. Templates & Artifacts

This repository contains several reusable artifacts developed during the IQC project:

- `Trust-Model.md` + Decision Record Template
- `Trust-Model-Review-Notes.md` (example of internal review)
- Head-to-head discussion packages (example format)
- `AUDIT_EVOLUTION_SUMMARY.md` (example of tracking how findings changed across rounds)

---

## 8. Future Directions

Promising areas for 2026–2027:

- Tight LLM + formal verification loops (models propose invariants, tools check them).
- Structured multi-model debate platforms with scoring.
- Longitudinal studies measuring how model performance on security tasks changes over time.
- Hybrid human + multi-model workflows with clear handoff points.

---

## 9. Final Principle

**The output of a multi-model LLM security review is not the list of findings.**

The real output is a **sharpened understanding** of the system, its risks, and the limitations of the review process itself — combined with a clear list of issues that have been *verified* through execution or formal methods.

Everything else is research and augmentation.

---

*This playbook is a living document based on the IQC token multi-model audit experiment. It will be updated as more data is gathered.*