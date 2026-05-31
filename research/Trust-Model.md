# IQC Token System — Trust Model (v0.3)

**Status**: Draft (Incorporating external reviews)  
**Version**: 0.3  
**Date**: June 2026

**Purpose**: This document defines the trust assumptions and threat model for the IQC token system. It is a required prerequisite for credible security analysis and mainnet deployment.

---

## 1. System Overview

- **IQCToken**: ERC-20 with advanced features (ERC20Votes, Permit, ERC2771, Multicall, Burnable + tracking, ERC1363).
- **IQCRegistryV3**: Staking, slashing, and data commitments.
- **7 TokenAllocation contracts**: Purpose-bound escrows.

All use `Ownable2Step`.

---

## 2. Current Ownership (Genesis)

| Contract             | Owner (at launch)     | Key Privileges                          | Critical Window / Risk |
|----------------------|-----------------------|-----------------------------------------|------------------------|
| IQCToken             | Deploying EOA         | `mint()`, `lockMintingForever()`, `setTrustedForwarder()` | Pre-lock minting |
| IQCRegistryV3        | Deploying EOA         | `slash()`                               | Arbitrary slashing |
| TokenAllocation (x7) | Deploying EOA         | `release()`, `recoverERC20()`           | Immediate full release + recovery of non-IQC tokens |

**Current state**: Single EOA (`immutableqc.base.eth`) is the root of trust for the entire system.

---

## 3. Threat Model

### 3.1 Adversary Matrix

| Adversary                        | Likelihood     | Impact     | Overall Risk | Mitigation Priority |
|----------------------------------|----------------|------------|--------------|---------------------|
| Compromised / Malicious Deploying EOA | High (pre-hardening) | Critical | **Critical** | P0 |
| Compromised Future Multisig      | Medium         | Critical   | **High**     | P1 |
| External Attacker (via bugs)     | Medium         | High       | **High**     | Requires separate analysis |
| Governance / Slashing Abuse      | Medium         | High       | **High**     | P1 |

### 3.2 Primary Risks (Current State)

- Unlimited pre-lock minting.
- Arbitrary slashing.
- Ability to release 99% of supply immediately from allocations.
- Ability to recover non-IQC tokens from allocations.
- Control of the ERC2771 trusted forwarder.

---

## 4. Recommended Hardening Path

### Phase 1 — Pre-Mainnet (Non-Negotiable)

1. **Close the pre-lock mint window** — Call `lockMintingForever()` atomically with deployment.
2. **Transfer ownership** of all contracts to a secure multisig **before any mainnet activity**.
3. **Add a timelock** (minimum 48–72 hours) on all privileged actions.
4. **Strengthen Allocation Contracts** — Add vesting/timelocks to `release()` and restrict `recoverERC20` on the IQC token.

### Phase 2 — Steady State

- All owner actions go through a timelocked multisig.
- Published governance process for slashing decisions.
- Consider renouncing `IQCToken` ownership after minting is permanently locked.

---

## 5. Impact on Audit Findings

| Finding                                      | Severity (Single EOA) | Severity (Timelocked Multisig) | Notes |
|----------------------------------------------|-----------------------|--------------------------------|-------|
| Allocation `recoverERC20` / release abuse    | **Critical**          | **Medium**                     | One of the highest-impact findings |
| Pre-lock mint window                         | **Critical**          | **Low** (if closed)            | Can and should be eliminated in code |
| Arbitrary slashing                           | **High**              | **Medium**                     | Depends on governance process |
| Trusted forwarder control                    | Medium                | Low                            | - |
| ERC1363 excess trap                          | High (UX)             | High (UX)                      | Owner-independent |

---

## 6. Open Questions (Blockers)

The following decisions **must** be made and documented before this document can be considered the canonical reference:

1. **Long-term ownership structure** — What is the target (specific multisig + timelock? On-chain governance?)?
2. **Slashing governance** — Owner-only, timelocked, or separate on-chain process?
3. **Allocation contract philosophy** — Long-term owner control or increasing autonomy?
4. **Emergency / key loss procedures** — What is the recovery plan (social recovery, guardians, etc.)?
5. **Signer requirements** — Geographic/organizational diversity, hardware security expectations, rotation policy?

**Until these are answered, this document remains incomplete.**

---

## 7. What the Owner Cannot Do (Invariants)

Regardless of who controls the contracts, the following are structurally impossible:

- Owner cannot un-burn tokens (burn is irreversible).
- Owner cannot reduce `totalSupply` below the amount that has been genuinely burned.
- Owner cannot change the addresses of the 7 allocation contracts after deployment.
- Owner cannot slash without emitting an on-chain event.

---

## 8. Emergency Procedures

**Current state (as of this draft)**: There is no documented emergency or social recovery procedure. Loss or compromise of the controlling multisig would be catastrophic with no clear recovery path.

This must be addressed before mainnet.

---

## 9. Recommendation

**Do not deploy to mainnet** until:

- A finalized version of this Trust Model is published.
- Ownership is transferred to a secure, diverse, timelocked multisig.
- The pre-lock mint window is closed.
- Allocation contracts have stronger protections.
- Emergency procedures are documented.

---

*This is a living document. It must be updated whenever ownership, roles, or threat assumptions change.*

**Version 0.3** — Incorporates feedback from Kimi 2.5 and Grok reviews on v0.2.