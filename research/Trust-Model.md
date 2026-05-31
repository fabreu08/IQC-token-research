# IQC Token System — Trust Model

**Status**: Draft (First Version)  
**Date**: June 2026  
**Purpose**: Define who controls privileged actions in the IQC token system and what the threat model is. This document is a prerequisite for credible security analysis and mainnet deployment decisions.

---

## 1. Overview

The IQC system consists of three main contract groups:

- **IQCToken** (`IQCToken.sol`): The ERC-20 token with advanced features (ERC20Votes, ERC20Permit, ERC2771 meta-tx support, Multicall, ERC20Burnable with tracking, ERC1363).
- **IQCRegistryV3** (`IQCRegistryV3.sol`): Handles staking, slashing, and data commitments (both traditional and via ERC1363 `transferAndCall`).
- **TokenAllocation** (7 instances): Transparent on-chain escrows holding 99% of the supply for specific purposes (Community, Liquidity, Team, Treasury, Ecosystem, Early Contributors, Reserved).

All contracts inherit from OpenZeppelin `Ownable` or `Ownable2Step`.

---

## 2. Current Ownership Model (Post-Deployment)

### 2.1 IQCToken

- **Owner**: The deploying address (`immutableqc.base.eth` EOA at genesis).
- **Capabilities**:
  - Call `mint()` (until `lockMintingForever()` is called).
  - Call `lockMintingForever()` (one-way, permanent).
  - Call `setTrustedForwarder()` (for ERC2771 meta-transactions).
  - Transfer ownership via `Ownable2Step`.

**Critical Note**: Between deployment and the call to `lockMintingForever()`, the owner can mint additional tokens beyond the intended 1 billion supply.

### 2.2 IQCRegistryV3

- **Owner**: Same deploying EOA.
- **Capabilities**:
  - `slash(address user, uint256 amount)` — can slash staked balances.
  - Standard Ownable functions (transfer ownership, etc.).

### 2.3 TokenAllocation Contracts (7 instances)

- **Owner**: Same deploying EOA (transferred at or after deployment).
- **Capabilities**:
  - `release(address beneficiary, uint256 amount)` — release tokens according to the contract's purpose.
  - `recoverERC20(address tokenAddress, uint256 amount)` — recover accidentally sent tokens (with protection against the main allocation token).

---

## 3. Threat Model

### 3.1 Primary Adversaries

1. **Compromised or Malicious Owner (Highest Risk)**
   - The single EOA that currently controls all contracts.
   - Can:
     - Mint unlimited tokens before `lockMintingForever()`.
     - Slash any staker (including legitimate participants).
     - Drain or redirect funds from TokenAllocation contracts via `recoverERC20` (for non-IQC tokens) or by controlling release logic.
     - Change the trusted forwarder for meta-transactions.

2. **Compromised Multisig / Timelock (Future State)**
   - Once ownership is transferred, this becomes the new root of trust.

3. **External Attackers**
   - Can exploit bugs in commit paths, reentrancy (if hooks are added later), or economic attacks via the dual commit paths.

4. **Governance / Slashing Abuse**
   - Whoever controls slashing can censor or economically punish participants.

### 3.2 Key Risks Without Hardening

- **Pre-lock mint window**: The deploying EOA can inflate supply between deployment and `lockMintingForever()`.
- **Allocation rug risk**: The owner can extract value from purpose-bound allocations (directly or indirectly).
- **Censorship via slashing**: Owner can slash stakers without due process.
- **Meta-transaction trust**: The owner controls the trusted forwarder, which could be used for griefing or censorship if meta-tx usage grows.

---

## 4. Recommended Hardening Path (Pre-Mainnet)

### Phase 1: Immediate (Before Any Mainnet Deployment)

1. **Define and Publish the Trust Model**
   - Explicitly document who will control the contracts at launch and in steady state.
   - Publish this document publicly (this file or a more formal version).

2. **Close the Pre-Lock Mint Window**
   - Make `lockMintingForever()` atomic with deployment (call it in the same transaction or constructor).
   - Alternatively, add a constructor parameter that locks minting immediately.

3. **Transfer Ownership to a Secure Multisig**
   - Move from single EOA to a multisig (ideally with timelock).
   - Consider a 3-of-5 or 4-of-7 setup with geographically and organizationally diverse signers.

4. **Protect Allocation Contracts**
   - Add explicit restrictions in `recoverERC20` (already partially present) or remove the function entirely for these contracts.
   - Consider adding vesting/release schedules with timelocks.

### Phase 2: Steady State

- Wrap the multisig in a timelock (minimum 48–72 hours delay for all privileged actions).
- Publish clear governance processes for slashing, forwarder changes, and any future parameter adjustments.
- Consider renouncing ownership of `IQCToken` entirely after `lockMintingForever()` (if no further admin functions are needed).

---

## 5. Impact on Audit Findings

Many findings from the multi-model audit process are **highly sensitive to the trust model**:

| Finding | Severity if Owner = Single EOA | Severity if Owner = Timelocked Multisig | Notes |
|---------|--------------------------------|-----------------------------------------|-------|
| `TokenAllocation.recoverERC20` drain | High | Low | Most sensitive to trust model |
| Pre-lock mint window | High | Medium (if timelock exists) | Can be closed in code |
| Slashing power | High | Medium | Governance process matters |
| Trusted forwarder control | Medium | Low | Meta-tx usage dependent |
| ERC1363 excess trap | High (UX) | High (UX) | Not owner-dependent |

**Conclusion**: Without a published trust model, it is impossible to give final severity ratings to several High-impact findings.

---

## 6. Open Questions for the Team

1. What is the intended long-term ownership structure (multisig + timelock? DAO? Something else)?
2. Will slashing authority remain with the owner, or will it move to a separate governance process?
3. Do the TokenAllocation contracts need any owner-controlled functions long-term, or can they be made more autonomous?
4. Is there a plan to renounce ownership of `IQCToken` after minting is locked?

---

## 7. Recommendation

**Do not deploy to mainnet** until:

1. A clear, public Trust Model document exists (this can serve as the starting point).
2. Ownership has been transferred from the single EOA to a secure, documented multisig (ideally with timelock).
3. The pre-lock mint window has been closed.
4. Protection for the allocation contracts has been strengthened.

---

*This document should be treated as a living artifact. It must be updated whenever ownership, governance processes, or threat assumptions change.*

**Next Steps**:
- Team reviews and edits this document.
- Publish a final version before any mainnet deployment.
- Reference this document in all future audit reports and communications.