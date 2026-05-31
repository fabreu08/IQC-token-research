# IQC Token — Master Risk Table (Multi-Model Synthesis)

**Date**: Current  
**Models Included**:
- Original Audit (Grok)
- Grok 4.3 via Hermes
- Opus 4.8 (strong supply reduction focus)
- Grok 4.3 web (Governance + Allocation focus)
- Kimi 2.5 with agents (most rigorous technical review)
- Opus 4.7 (default) — critical of contradictions and inflation
- Opus 4.7 (master prompt) — strongest methodological critique + verification skepticism
- Kimi 2.5 (master prompt) — extreme skepticism on multi-model process value and "consensus theater"

**Purpose**: Consolidated, calibrated view of risks with model attribution and adjusted severity.

---

## Critical Risks

| ID | Risk | Consensus Severity | Models Raising It | Key Insight | Fix Priority | Notes / Architectural Constraints |
|----|------|--------------------|-------------------|-------------|--------------|-----------------------------------|
| **C-01** | ERC1363 Receiver — Excess Fund Trap | **High** (Kimi 2.5 + Opus 4.7 argue against Critical) | Most | Severe UX footgun allowing permanent loss on over-send. Self-inflicted (requires deliberate `transferAndCall` misuse). One-line fix. Later models push back on "Critical" classification. | P0 | High-severity deployment blocker, but framing as Critical is now actively debated. |

---

## High Risks

| ID | Risk | Consensus Severity | Models Raising It | Key Insight | Fix Priority | Notes / Architectural Constraints |
|----|------|--------------------|-------------------|-------------|--------------|-----------------------------------|
| **H-01** | TokenAllocation `recoverERC20` can drain IQC | **High** (trust model dependent) | Grok 4.3 web, Kimi 2.5 | Owner can drain purpose-bound allocations. Severity depends entirely on who controls Ownable2Step. | P1 | **Requires defined trust model** before final severity can be assigned (Opus 4.7). Kimi 2.5 suggests Medium if worst-case not assumed. |
| **H-02** | Dual Commit Path Inconsistency | **Medium** | Opus 4.8, Grok 4.3 web, Kimi | ERC1363 path bypasses stake requirement. Invariant violation. Kimi 2.5 downgraded to Low. | P1 | Better framed as incentive misalignment than security vulnerability. |
| **H-03** | Burn Permission Feasibility Gap | **Remediation Constraint** (not a finding) | Kimi 2.5 (Opus 4.7 agrees) | Likely not a real blocker — Registry can call `burn()` on its own balance. Kimi 2.5 calls it a phantom issue. | - | Remove from risk table. |

---

## Medium Risks

| ID | Risk | Consensus Severity | Models Raising It | Key Insight | Fix Priority | Notes |
|----|------|--------------------|-------------------|-------------|--------------|-------|
| **M-01** | Broken / Misleading `totalBurned()` | **Low-Medium** | All | View function is inaccurate. Observability issue. Opus 4.7 downgraded. | P2 | Damages credibility more than creates loss. |
| **M-02** | Reentrancy surface (especially via `onTransferReceived`) | **Medium** (debated) | Kimi, Opus 4.7 | More relevant through the ERC1363 callback than through `stake()`. | P1 | Opus 4.7 notes this was under-weighted. |
| **M-03** | Unbounded `batchTransfer` | **Low** | Opus 4.7 strongest | Self-harm only. Not a cross-user security issue. | P2 | UX bound recommended. |
| **M-04** | Pre-lock Mint Window | **Medium** | Multiple | Real trust window between deploy and `lockMintingForever()`. | P1 | Make atomic. |
| **M-05** | Registry constructor token validation | **Informational** | Hermes, Kimi, Opus 4.7 | Operational/configuration risk under owner control. | P2 | Downgraded by Opus 4.7. |

---

## Audit Process & Meta Issues (New Category)

| Issue | Severity | Key Insight |
|-------|----------|-------------|
| No code execution or tool usage across entire review process | **Meta-Critical for audit credibility** | Six+ models produced text analysis with zero Foundry tests, zero Slither, zero fuzzing. "Six models, zero verification" (Opus 4.7 + Kimi 2.5). |
| Undefined Trust Model | **Foundational Gap** | Multiple findings cannot have final severity assigned without knowing who controls Ownable2Step. |
| "Multi-model consensus" framing | High (process issue) | Correlated LLM outputs treated as independent verification. Later models downgraded earlier ones, but this is prompt-dependent, not truth convergence (Kimi 2.5). |
| Severity inflation & category confusion | High | Documents still disagree on counts. Several items kept at higher severity despite later analysis showing they are not real blockers. |

---

## Low / Informational

| ID | Risk | Consensus Severity | Notes |
|----|------|--------------------|-------|
| **L-01** | Manual ERC2771 implementation | Low | Not present on current GitHub `main`. Only relevant for the advanced local version. |
| **L-02** | Missing access control / spam protection on `commitQCPacket` | Low | Public function with only stake requirement. |
| **L-03** | `COMMIT_FEE` is immutable forever | Low | Design choice with long-term consequences. |
| **L-04** | Use of `DEAD_ADDRESS` as burn mechanism | Low | Non-standard. Real burns via `_burn()` are preferred. |

---

## Key Cross-Model Insights & Disagreements

- **Burn Mechanism**: Strong consensus that `transfer(DEAD)` is a serious design flaw. Kimi 2.5 added the critical constraint that simply switching to `_burn()` is non-trivial due to permissions.
- **Severity Discipline**: Kimi 2.5 is the most conservative. Earlier models (especially B) were more aggressive on classifying accounting issues as High/Critical.
- **Governance Surface**: Only Grok 4.3 web deeply analyzed ERC20Votes + DEAD address interactions.
- **Allocation Trust**: Grok 4.3 web was the strongest on the `recoverERC20` risk to purpose-bound allocations.
- **Methodology & Rigor**: Opus 4.7 provided the strongest critique of the audit process itself (internal contradictions, severity inflation, lack of PoCs, missing trust model).
- **Reentrancy**: Severity is model-dependent based on assumptions about future token hooks.

---

## Recommended P0 Priorities (Before Any Mainnet Deployment)

1. **Fix ERC1363 receiver** — Require exact fee or implement safe refund (strongly prefer exact).
2. **Solve the burn mechanism properly** — Decide on real `_burn()` path + required permission model (this is now a first-class design decision).
3. **Protect TokenAllocation contracts** — Prevent `recoverERC20` from draining IQC.
4. **Make `lockMintingForever()` atomic** with deployment (or use a constructor flag).
5. **Add `recoverERC20` (with proper guards)** to `IQCRegistryV3`.

---

## Notes

- This table reflects **calibrated severity** after five independent reviews rather than any single model's view.
- Several "High" issues from early audits were downgraded once the distinction between **fund loss** and **broken promises / poor observability** was applied.
- The burn permission gap identified by Kimi 2.5 is currently the most important open architectural question.

**Next Action Recommendation**: Resolve the burn permission model before writing code fixes. All other P0 items depend on this decision.

---

*Generated from multi-model audit synthesis.*