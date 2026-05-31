# IQC Token Audit — Evolution of Findings Across Models

This document tracks how severity ratings and understanding evolved as more rigorous models reviewed the contracts.

## Model Sequence & Character

| Order | Model                  | Style                          | Key Strength                          | Notable Weakness                  |
|-------|------------------------|--------------------------------|---------------------------------------|-----------------------------------|
| 1     | Original (Grok)        | Structured, broad              | First pass coverage                   | Severity inflation                |
| 2     | Grok 4.3 via Hermes    | Dense, exploit-focused         | Good on ERC1363 trap                  | Some duplication                  |
| 3     | Opus 4.8               | Economic/tokenomics lens       | Strong on "zero actual supply reduction" | Less focus on methodology     |
| 4     | Grok 4.3 web           | Governance + trust model       | Best on allocation contracts          | -                                 |
| 5     | Kimi 2.5 (agents)      | Highly rigorous + architectural| Burn permission feasibility gap       | -                                 |
| 6     | Opus 4.7 (default)     | Critical of contradictions     | Strong on internal audit contradictions and severity inflation | - |
| 7     | Opus 4.7 (master prompt) | Extreme methodological skepticism | "Six models, zero verification" — best critique of the entire LLM review process | - |
| 8     | Kimi 2.5 (master prompt) | Strongest skepticism on process value | "Consensus theater", phantom findings, calls for actual code execution over text analysis | Questions whether the whole multi-model exercise has value compared to running Foundry + Slither |

---

## Evolution of Major Findings

### 1. ERC1363 Excess Fund Trap

- **Original**: Critical
- **All subsequent models**: **Critical** (unanimous)
- **Notes**: This finding survived every review with the highest severity. Later models improved the framing (unbounded user loss rather than "griefing").

**Status**: Remains the clearest deployment blocker.

### 2. No Actual Supply Reduction (`transfer(DEAD)` vs `_burn()`)

- **Original / Early**: High or Critical
- **Opus 4.8**: Strongly emphasized as fundamental
- **Kimi 2.5**: Added critical constraint (permission model may block the fix)
- **Opus 4.7**: Acknowledged the issue but questioned how much of the original severity was justified

**Current calibrated view**: High (serious tokenomics problem with architectural complexity in the fix).

### 3. Reentrancy in `stake()`

- **Original / Hermes**: High (listed as blocker in some summaries)
- **Kimi 2.5**: Downgraded
- **Opus 4.7**: **Informational** (correctly noted it is not exploitable with current plain ERC-20)

**Final view**: Informational / Low.

### 4. Inconsistent Burn Paths (Dual Commit Mechanisms)

- **Early models**: High
- **Kimi 2.5 & Opus 4.7**: Medium (observability + incentive problem, not direct fund loss or double-spend)

**Final view**: Medium.

### 5. TokenAllocation `recoverERC20` Risk

- **Early models**: Medium or missed
- **Grok 4.3 web**: Raised strongly
- **Kimi 2.5**: Agreed it was High

**Final view**: High (trust model violation).

### 6. Burn Permission Feasibility Gap

- **Early models**: Not identified
- **Kimi 2.5**: Major new finding
- **Later models**: Recognized as important constraint

**Final view**: High architectural issue.

### 7. DEAD Address + ERC20Votes Governance Risk

- **Most models**: Missed or low
- **Grok 4.3 web**: Raised as High (if Votes is used)

**Final view**: High (conditional on enabling Votes).

### 8. Unbounded `batchTransfer`

- **Early models**: Medium
- **Opus 4.7**: Low (self-harm only)

**Final view**: Low.

---

## Major Shifts in Perspective

**From Broad to Narrow**:
- Early audits raised 8–11 issues at High+ severity.
- Later audits (Kimi 2.5 and Opus 4.7) converged on **1–2 real Critical/High blockers** + several design/credibility problems.

**From "Find Everything" to "Audit the Audit"**:
- Opus 4.7 in particular shifted the conversation by examining the *quality and consistency* of the audit report itself (internal contradictions, missing PoCs, severity inflation, lack of trust model).

**Increasing Focus on Feasibility of Fixes**:
- Early recommendations were relatively optimistic ("just use `_burn()`").
- Kimi 2.5 forced recognition that the recommended fix path has real architectural obstacles.

---

## Latest Input: Two Opus 4.7 Responses (Default + Master Prompt)

The final two responses (both from Opus 4.7) represent the strongest methodological pushback:

- **Default Opus 4.7**: Focused on internal contradictions in the synthesis documents, downgraded C-01 to High, reclassified supply reduction as tokenomics issue (not security Critical), and argued that the "burn permission gap" is likely not real because the Registry can burn from its own balance.
- **Master Prompt Opus 4.7**: Extremely skeptical of the entire exercise. Core argument: "Six models, zero verification." The whole process lacks any actual code execution, fuzzing, or static analysis output. This is the most fundamental critique of the multi-model approach received.

**Net effect on the synthesis**:
- Further downgrading of several High items.
- Addition of "Audit Process Weakness" as a meta-category.
- Stronger emphasis that a defined Trust Model is a prerequisite for credible severity ratings.
- Significant doubt cast on whether the "burn permission feasibility gap" is even a real blocker.

This input has been the most effective at forcing the synthesis toward honesty about its own limitations.

---

*Final evolution after seven model perspectives (including two from Opus 4.7).*

## Latest Input: Kimi 2.5 with Master Prompt

Kimi 2.5's final response (using the dense inter-model prompt) is the most aggressive rejection of the process itself:

- **Core argument**: The entire "multi-model synthesis" is "six correlated text generations with zero ground truth verification." Treating LLM outputs as independent reviews is fundamentally flawed.
- **Further downgrades**:
  - ERC1363 trap → High (severe UX footgun, not Critical).
  - Supply reduction → Medium (tokenomics/disclosure issue, not security).
  - Burn permission gap → Phantom issue (should be removed).
  - Dual commit paths → Low.
- Strongest call yet for the project to stop iterating on LLM reviews and instead run actual code (Foundry tests, Slither, fuzzing).
- Points out that the "consensus theater" and "phantom findings" undermine the credibility of the output more than any individual technical error.

**Net Effect**:
This input solidifies the shift from "find more issues" to "the review process itself has major limitations that must be disclosed." It is currently the strongest argument against over-relying on this style of multi-model audit for high-stakes decisions.

---

*Final evolution after eight model perspectives.*

## Key Lessons from the Review Process

1. **Severity inflation is real** — Multiple independent reviewers applying pressure is an effective correction mechanism.
2. **Methodological critique is as valuable as new findings** — Opus 4.7’s review was extremely useful even though it introduced relatively few new technical issues.
3. **Architectural constraints matter more than individual bugs** — The burn permission problem is more important than most individual code-level findings.
4. **Later models were generally more accurate** — The final calibrated view is significantly more conservative than the original audit.

---

## Current Consensus (Post All Reviews)

**Deployment Blockers**:
- ERC1363 excess trap (Critical)
- Real supply reduction problem + fix feasibility (High, with architectural complexity)

**Important Design/Trust Issues**:
- Allocation contract protection
- Dual commit path inconsistency
- Owner privileges and trust model (still under-specified)

Everything else has been downgraded to Medium or lower.

---

*This evolution shows the value of multi-model review.*