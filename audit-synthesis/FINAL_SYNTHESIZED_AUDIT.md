# IQC Token — Final Synthesized Security Audit Report

**Date**: June 2026  
**Scope**: IQCToken.sol + IQCRegistryV3.sol + TokenAllocation.sol (Base mainnet readiness)  
**Models Consulted**:
- Original internal audit
- Grok 4.3 via Hermes
- Opus 4.8
- Grok 4.3 web
- Kimi 2.5 (with agents)
- Opus 4.7 (agent)

**Primary Documents**:
- This report
- [MASTER_RISK_TABLE.md](./MASTER_RISK_TABLE.md)
- [SECURITY_AUDIT_CONSOLIDATED.md](./SECURITY_AUDIT_CONSOLIDATED.md)

---

## Executive Summary

After five independent external reviews, the consensus verdict is **NO_GO** for mainnet deployment.

However, the reasons have narrowed significantly over successive reviews:

**Primary Blocker**
- ERC1363 receiver allows permanent loss of user funds on over-send (unanimous Critical/High across models; Opus 4.7 argues it is a severe self-inflicted UX footgun rather than a classic Critical systemic vulnerability).

**Core Tokenomics vs Implementation Gap**
- "Burns" do not reduce supply. Later models (especially Opus 4.7) argue this is primarily a **tokenomics disclosure / marketing accuracy** issue rather than a pure security Critical.

**Important Clarification from Opus 4.7**
- The "Burn Permission Feasibility Gap" may not be a real blocker. Since the Registry holds the tokens after `transferAndCall` or as staked balance, it can likely call `ERC20Burnable.burn()` on its own balance without special permissions. This significantly reduces the perceived architectural constraint.

Most other findings raised in early reviews were progressively downgraded by more rigorous later reviewers (particularly Kimi 2.5 and Opus 4.7) as either:
- Observability/credibility problems rather than direct security vulnerabilities, or
- Not exploitable under the current contract design.

**Final calibrated view** (post Kimi 2.5 master prompt):
- 0 clear Critical findings under the strictest readings (Kimi 2.5 and Opus 4.7 both push back on the "Critical" label for the ERC1363 trap, calling it a severe but self-inflicted UX issue).
- The dominant issues are now framed as:
  - One clear High-severity UX vulnerability (ERC1363 trap).
  - Tokenomics credibility gap (fake burns presented as real).
  - Undefined trust model (foundational prerequisite for credible severity ratings).
  - Process weakness (the multi-model LLM review produced correlated text analysis with no code execution, fuzzing, or static analysis — the strongest meta-critique from Opus 4.7 and Kimi 2.5).

The synthesis has been pushed significantly toward honesty about its own limitations. The original audit's severity distribution has been substantially corrected downward.

---

## Calibrated Risk Summary

### Critical

| Risk | Severity | Rationale | Primary Models |
|------|----------|-----------|----------------|
| ERC1363 Excess Fund Trap | **Critical** | Users can permanently lose arbitrary amounts of IQC by sending >1 IQC via `transferAndCall`. No refund path and no `recoverERC20`. | All models |

### High

| Risk | Severity | Rationale | Primary Models |
|------|----------|-----------|----------------|
| No Actual Supply Reduction | High | All commit/slash fees use `transfer(DEAD)` instead of `_burn()`. `totalSupply()` is permanently fixed at 1e9. | Opus 4.8, Grok 4.3 web, Kimi 2.5 |
| Burn Permission Feasibility Gap | High | Recommended fix (real burns) is architecturally constrained. Registry is not owner and may lack burn rights. | Kimi 2.5 |
| TokenAllocation `recoverERC20` Can Drain IQC | High | Owner can extract IQC from purpose-bound allocation contracts, defeating the transparency model. | Grok 4.3 web, Kimi 2.5 |

### Medium

| Risk | Severity | Notes |
|------|----------|-------|
| Dual Commit Path Inconsistency | Medium | ERC1363 path allows commits without requiring stake. Observability and incentive misalignment. |
| Misleading `totalBurned()` | Medium | Currently inaccurate or zero. Credibility issue. |
| Reentrancy surface in `onTransferReceived` | Medium | More relevant than the stake path (raised by Opus 4.7). |

### Lower / Informational

- Unbounded `batchTransfer` (mostly self-harm)
- Pre-lock mint window (operational)
- Constructor token address validation (operational)
- Reentrancy in `stake()` (not exploitable with current plain ERC-20)

---

## Key Insights Across Reviews

**Evolution of Understanding**:

1. **Early models** (Hermes, original) raised many issues at High/Critical severity.
2. **Mid models** (Opus 4.8, Grok 4.3 web) added important economic and governance angles (especially allocation trust and DEAD voting power).
3. **Later models** (Kimi 2.5, Opus 4.7) applied strong methodological and severity discipline:
   - Distinguished fund loss from broken promises.
   - Identified internal contradictions in the original audit.
   - Highlighted the non-trivial permission problem with the recommended burn fix.
   - Criticized lack of PoCs, missing trust model, and duplicated findings.

**Most Valuable Contributions**:
- **Kimi 2.5**: Burn permission architecture gap.
- **Opus 4.7**: Structural contradictions in the audit itself + excellent methodological critique.
- **Grok 4.3 web**: Strongest analysis of allocation contract trust model.
- **Opus 4.8**: Clear emphasis that supply is not actually reduced.

---

## Recommended Path Forward

**P0 (Must fix before mainnet)**:
1. Resolve the ERC1363 receiver (strongly prefer exact fee requirement).
2. Make an explicit decision on the burn mechanism + implement the required permission model (or accept that burns remain fake).
3. Protect `TokenAllocation` contracts from having their IQC drained via `recoverERC20`.
4. Close the pre-lock mint window (make `lockMintingForever()` atomic with deployment).

**P1**:
- Add `ReentrancyGuard` on relevant paths.
- Add `recoverERC20` (guarded) to the Registry.
- Unify or clearly document the two commit paths.
- Bound batch operations.

**Strong Recommendations from Reviewers**:
- Include actual Foundry/Hardhat PoCs for any remaining Critical or High findings.
- Clearly document the trust model (who controls `Ownable2Step`?).
- Decide and document the intended behavior of "burns" vs supply reduction.

---

## Final Verdict

**NO_GO** is the correct conclusion.

However, the justification is narrower and more precise than the original audit suggested:

- **One unambiguous Critical issue** (ERC1363 fund trap) is sufficient on its own to block deployment.
- The supply reduction problem is real but partly a tokenomics design question rather than a pure security bug.
- Several other findings were inflated and should be treated as design debt or credibility risks rather than deployment blockers.

A cleaned-up version of this audit (with PoCs, consistent severity, and resolved architectural questions) would be credible.

---

*This document represents the final synthesis after six independent analyses.*