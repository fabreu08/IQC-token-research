# IQC Token System — Trust Model (v0.4)

**Status**: Draft (Incorporating latest Kimi 2.5 review)  
**Version**: 0.4  
**Date**: June 2026

**Purpose**: This document defines the trust assumptions, roles, and threat model for the IQC token system. It is the canonical reference for security analysis and deployment decisions.

---

## 1. Executive Summary

The IQC system is currently controlled by a single EOA at genesis. This creates several high-severity risks that must be mitigated before mainnet.

**Key Hardening Requirements (Pre-Mainnet):**

- Transfer ownership of all contracts to a secure, diverse, timelocked multisig.
- Close the pre-lock mint window atomically.
- Implement real vesting with cliff on allocation contracts.
- Move slashing toward governed processes over time.
- Define and publish clear emergency procedures (or explicitly accept there are none).

Until the Decision Record below is completed with specific choices, this document remains a draft and cannot serve as the final canonical trust model.

---

## 2. Current Ownership at Genesis

| Contract                  | Owner (at launch)      | Key Privileges                                      | Risk Level (Current) |
|---------------------------|------------------------|-----------------------------------------------------|----------------------|
| IQCToken                  | Deploying EOA          | `mint()`, `lockMintingForever()`, `setTrustedForwarder()` | Critical (pre-lock) |
| IQCRegistryV3             | Deploying EOA          | `slash()`                                           | High |
| TokenAllocation (7x)      | Deploying EOA          | `release()`, `recoverERC20()`                       | High (for 99% supply) |

**Single point of failure**: One EOA controls minting, slashing, and release of nearly all supply.

---

## 3. Adversary Analysis

### 3.1 Adversary Matrix

| Adversary                              | Likelihood                  | Impact     | Risk     | Priority |
|----------------------------------------|-----------------------------|------------|----------|----------|
| Compromised / Malicious Deploying EOA  | High (single key)           | Critical   | Critical | P0 |
| Compromised Future Multisig            | Medium*                     | Critical   | High     | P1 |
| External Attacker (via contract bugs)  | Medium                      | High       | High     | P1 |
| Governance / Slashing Abuse            | Medium                      | High       | High     | P1 |

*Likelihood for future multisig assumes:
- Minimum 4-of-7 threshold
- Signers from at least 3 distinct organizations
- At least 2 geographic regions
- Hardware security module / air-gapped signing

Lower diversity or weaker operational security increases this likelihood to **High**.

### 3.2 Cross-Contract Risk (New in v0.4)

A single compromised owner can interact with both the Registry and Allocation contracts in coordinated ways:

- Slash a user's stake in the Registry, then immediately release their allocation tokens (or vice versa).
- Front-run a legitimate slashing event by releasing allocation tokens to the target first.
- Use `recoverERC20` on allocations while simultaneously slashing in the Registry.

**Mitigation Direction**: Consider adding timelocks or governance requirements on slashing during allocation release windows, or explicit coordination rules.

---

## 4. Recommended Hardening Path (v0.4)

### Phase 1 — Pre-Mainnet (Mandatory)

1. **Atomic Mint Lock**
   - Call `lockMintingForever()` in the same transaction as deployment (or constructor).

2. **Transfer to Secure Multisig + Timelock**
   - Recommended concrete setup: 4-of-7 multisig with 48-hour minimum timelock.
   - Signers must include diversity (team + external + community recommended).

3. **Strengthen Allocation Contracts**
   - Implement linear vesting + cliff (already partially done in code).
   - Restrict or remove `recoverERC20` ability on the IQC token.

4. **Emergency Pause**
   - Add pausable functionality to Registry (already implemented).

### Phase 2 — Post-Mainnet

- Move slashing to a governed (ideally on-chain) process.
- Add social recovery / guardian mechanisms or explicitly document that none exist.
- Consider renouncing `IQCToken` ownership after minting is locked.

---

## 5. What the Owner Cannot Do (Invariants)

Regardless of who holds the keys:

- Owner cannot un-burn tokens.
- Owner cannot reduce `totalSupply` below the amount actually burned via `_burn()`.
- Owner cannot change the 7 allocation contract addresses after deployment.
- Owner cannot slash without emitting an on-chain event.

---

## 6. Emergency Procedures

**Current Status**: No formal social recovery or "break glass" mechanism is defined.

**Decision Required**: The team must either:
- Implement a documented social recovery / guardian process, **or**
- Explicitly state in this document and all communications: "Loss or compromise of the controlling multisig is catastrophic with no recovery path."

---

## 7. Decision Record (Required Before v1.0)

This section must be completed with concrete decisions before this document can be treated as the canonical trust model.

**Decision 1: Multisig Configuration**
- Implementation: ____________________
- Signers (names/entities): 
  1. ____________________
  2. ____________________
  ...
- Threshold: ____ of ____
- Timelock delay: ____ hours

**Decision 2: Slashing Governance**
- [ ] Remains with timelocked multisig
- [ ] Moves to separate on-chain process
- Details: ____________________

**Decision 3: Allocation Contract Long-term Model**
- [ ] Keep owner-controlled with vesting + restricted recover
- [ ] Move to more autonomous model over time
- Details: ____________________

**Decision 4: Emergency Procedures**
- [ ] No recovery mechanism (catastrophic loss)
- [ ] Social recovery / guardians (details below)
- Details: ____________________

**Decision 5: Renounce IQCToken Ownership?**
- [ ] Yes, after `lockMintingForever()`
- [ ] No

---

## 8. Impact on Audit Findings

| Finding                                      | Single EOA Owner     | Timelocked Multisig | Notes |
|----------------------------------------------|----------------------|---------------------|-------|
| Allocation `recoverERC20` / immediate release| Critical             | Medium              | Highest sensitivity to trust model |
| Pre-lock mint window                         | Critical             | Informational (if closed) | Should be eliminated in code |
| Arbitrary slashing                           | High                 | Medium              | Depends on governance |
| Trusted forwarder control                    | Medium               | Low                 | Currently disabled |
| ERC1363 excess trap                          | High (UX)            | High (UX)           | Owner-independent |

---

## 9. Recommendations

**Do not treat this document as final or publish v1.0 until** the Decision Record above is completed with specific, named decisions.

The Trust Model process has successfully surfaced the real root risks. The remaining work is execution and documentation of concrete choices.

---

*Version 0.4 — Incorporates feedback from Kimi 2.5 v0.3 review and prior Opus 4.7 input.*

**Next Step**: Team completes the Decision Record → Publish v1.0 → Reference in all future audits and deployment communications.