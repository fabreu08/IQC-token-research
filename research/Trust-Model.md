# IQC Token System — Trust Model (Team Review Draft v2)

**Status**: Team Review Draft  
**Version**: 0.2  
**Date**: June 2026  
**Reviewers**: Synthesized from multi-model audit process + internal review

**Purpose**: This document defines the trust assumptions, privileged roles, and threat model for the IQC token system. It is a required prerequisite for credible security analysis, severity calibration, and any mainnet deployment decision.

---

## 1. System Overview

The IQC system consists of:

- **IQCToken**: Core ERC-20 with ERC20Votes, ERC20Permit, ERC2771 support, Multicall, and ERC20Burnable + on-chain burn tracking.
- **IQCRegistryV3**: Handles staking, slashing, and QC data commitments (traditional + ERC1363 path).
- **7 TokenAllocation contracts**: Purpose-bound escrows holding 99% of supply.

All contracts use `Ownable2Step` (or `Ownable`).

---

## 2. Current Post-Deployment Ownership (As of Genesis)

| Contract                  | Current Owner          | Key Privileged Functions                              | Notes |
|---------------------------|------------------------|-------------------------------------------------------|-------|
| **IQCToken**              | Deploying EOA          | `mint()`, `lockMintingForever()`, `setTrustedForwarder()` | Minting can still occur until `lockMintingForever()` is called |
| **IQCRegistryV3**         | Deploying EOA          | `slash()`                                             | Slashing authority is currently unlimited and immediate |
| **TokenAllocation (x7)**  | Deploying EOA          | `release()`, `recoverERC20()`                         | Each has its own `Ownable` instance |

**Important**: At launch, a single EOA controls all privileged functions across the entire system.

---

## 3. Threat Model

### 3.1 Adversary Classes (in descending order of concern)

| Adversary                        | Likelihood | Impact | Notes |
|----------------------------------|------------|--------|-------|
| **Compromised or Malicious Deploying EOA** | High (pre-hardening) | Very High | Can mint, slash arbitrarily, and interfere with allocations |
| **Compromised Multisig**         | Medium     | High   | Becomes the dominant risk after ownership transfer |
| **External Attackers (bugs)**     | Medium     | Medium-High | Depends on remaining code issues (ERC1363 receiver, etc.) |
| **Governance / Slashing Abuse**  | Medium     | High   | Economic censorship of participants |

### 3.2 Key Risks While Single EOA Controls Everything

- Unlimited minting until `lockMintingForever()` is executed.
- Arbitrary slashing of any staker.
- Ability to extract or redirect funds from purpose-bound allocations.
- Control over the ERC2771 trusted forwarder.

---

## 4. Recommended Trust Model (Target State)

### Phase 1: Immediate (Pre-Mainnet)

1. Close the pre-lock mint window (make locking atomic).
2. Transfer ownership to a secure multisig (3-of-5 or 4-of-7 recommended).
3. Add a timelock (48–72h minimum) on privileged actions.
4. Strengthen protection on the allocation contracts.

### Phase 2: Steady State

- All privileged actions go through a timelocked multisig.
- Clear published governance for slashing and other sensitive actions.
- Consider renouncing `IQCToken` ownership after minting is permanently locked.

---

## 5. Impact on Audit Findings

| Finding                                      | Severity (Single EOA) | Severity (Timelocked Multisig) | Notes |
|----------------------------------------------|-----------------------|--------------------------------|-------|
| Allocation `recoverERC20` abuse              | High                  | Low                            | Extremely sensitive to trust model |
| Pre-lock mint window                         | High                  | Low–Medium                     | Can be closed in code |
| Arbitrary slashing                           | High                  | Medium                         | Depends on governance process |
| Trusted forwarder control                    | Medium                | Low                            | - |
| ERC1363 excess trap                          | High (UX)             | High (UX)                      | Not owner-dependent |

**Key Point**: Several findings that were rated High are only High because of the current weak (single EOA) trust model.

---

## 6. Open Questions Requiring Team Decisions

1. What is the long-term ownership/governance structure?
2. Should slashing authority move to a separate governance process?
3. Should the allocation contracts remain owner-controlled long-term?
4. Is there a plan to renounce `IQCToken` ownership after locking?
5. What are the emergency / key loss recovery procedures?

---

## 7. Recommendations

**Do not deploy to mainnet** until:

- This Trust Model (or successor) is published and stable.
- Ownership is transferred to a secure, documented, timelocked multisig.
- The pre-lock mint window is closed.
- Allocation contract protections are strengthened.

---

*This is a living document. Update it whenever ownership, roles, or threat assumptions change.*

**Version 0.2 – Team Review Draft**