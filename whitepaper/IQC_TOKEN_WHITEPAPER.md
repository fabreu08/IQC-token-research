> **SHELVED (2026-10-17). Do not cite.** Immutable QC follows a no-patented-work rule (see `docs/patent-guardrails.md` in `fabreu08/immutableqc-website`). That rule takes the blockchain path off the table: no token, no on-chain registry, no network anchoring and no smart contracts. This document is kept unchanged below as a historical research record. It is not a current plan, recommendation or product description.

# IQC Token White Paper

**Immutable Quality Control (IQC) — Optional Token Infrastructure**  
Version: 0.1 (research draft)  
Date: 2026-09-08  
Network intent: Base (Sepolia for test)  
Status: Contracts exist in `fabreu08/iqc-token`; **not deployed** to Base mainnet (addresses still placeholders)

---

## 0. Disclaimer

This document is **not** an offer to sell securities or any other financial instrument. It is **not** legal, tax, or investment advice. Nothing herein is a promise of profit, listing, liquidity, or token price.

Smart contracts in `fabreu08/iqc-token` have been subject to **multi-model AI-assisted review** archived in `fabreu08/IQC-token-research/audit-synthesis/`. They have **not** been audited by an independent professional security firm unless a later revision of this paper says otherwise. AI-only review is not a substitute for a firm audit.

Laboratory customers may use Immutable QC software **without** acquiring or using IQC tokens. Token utility described below applies only where registry/token integration is enabled and used.

Forward-looking statements (roadmap items labeled FUTURE or INFERRED) may change or be abandoned.

---

## 1. Problem

Laboratories generate measurement data that regulators, sponsors, and partners treat as evidence. Integrity failures are rarely “the sensor lied” in isolation; they are gaps in **who measured what, when, on which instrument, and whether the record was altered afterward**.

Immutable QC addresses that product problem as **laboratory middleware**: sign measurements at the instrument, hash-chain an audit trail, and publish Merkle roots for verification. Product surfaces: [immutableqc.com](https://immutableqc.com) and Open Alpha [dashboard](https://immutableqc.com/dashboard/).

The token is not the product. The product is the integrity workflow.

---

## 2. Why a token at all

### Steelman (case for)

1. **Spam resistance:** On-chain QC commitments that cost a burned fee make flooding the registry expensive (`COMMIT_FEE = 1 IQC` in `IQCRegistryV3`).
2. **Atomic pay + commit:** ERC-1363 `transferAndCall` lets an instrument or gateway attach payload data to the fee transfer in one transaction (DESIGN.md).
3. **Transparent supply escrow:** Large allocations sit in purpose-labeled `TokenAllocation` contracts rather than opaque wallets alone.
4. **Governance readiness:** ERC20Votes checkpoints enable future voting without a token migration (capability ≠ live DAO).
5. **Sound-money constraints:** Design docs reject fee-on-transfer and core-token blacklist/freeze (MONEY_CHARACTERISTICS_ANALYSIS.md).

### Case against

1. **Product works without IQC:** Open Alpha does not require a public ticker for labs to evaluate the middleware.
2. **Securities and communications risk:** A public float invites Howey-style scrutiny if marketed with profit expectation (not legal advice).
3. **Liquidity and unlock risk:** Illiquid markets plus large escrow releases damage users and the lab brand.
4. **Audit gap:** AI multi-model synthesis ≠ firm audit.
5. **Memecoin residual:** If the only demand is speculation around a lab logo, the honest label is **memecoin** — not “oracle money.”

**Position of this paper:** Token is **optional infrastructure**. Prefer WAIT on public listing until real lab registry use and disclosure quality catch up (see `launch/LAUNCH_STRATEGY.md`).

---

## 3. Why Base

- EVM tooling matches the existing Hardhat/OpenZeppelin stack in `iqc-token`.
- Low fees matter for repeated instrument/gateway transactions if meta-tx (ERC-2771) and Permit paths are used.
- Coinbase-affiliated L2 brings mainstream on-ramps; that is distribution infrastructure, **not** an endorsement of IQC by Coinbase or Base.
- Base’s no-code launchers (Zora, Clanker, Flaunch, Mint Club per Base docs) are **not** the recommended distribution channel for this token (reputational mismatch with lab buyers). See launch strategy.

---

## 4. Token design

| Parameter | Value | Notes |
| --- | --- | --- |
| Name / ticker | IQC | README |
| Standard | ERC-20 | + extensions below |
| Chain | Base; Sepolia for test | Not mainnet-deployed as of this draft |
| Max supply | **1,000,000,000** IQC | Fixed at genesis mint |
| Decimals | 18 | Standard ERC-20 |
| Mint lock | `lockMintingForever()` | Ignition module calls lock during deploy path |
| Genesis | **10,000,000** (1%) to `immutableqc.base.eth` | Direct, not in TokenAllocation |
| Escrowed | **990,000,000** (99%) across 7 `TokenAllocation` | Purpose strings on-chain |
| Extensions | Permit, Votes, ERC1363, ERC2771 (mutable forwarder), Ownable2Step, Multicall, burn / `totalBurned` | DESIGN / MONEY docs |
| Explicit non-features | No fee-on-transfer; no core blacklist/freeze | MONEY_CHARACTERISTICS_ANALYSIS.md |

Supply numbers match `fabreu08/iqc-token` README and DEPLOYMENT_GUIDE as fetched 2026-09-08. If a future `main` differs, prefer on-chain state and update this table — do not invent a second supply.

---

## 5. Utility — LIVE / DESIGNED / FUTURE

Labels:

- **LIVE** — implemented in current contracts on `iqc-token` main (still may be undeployed).
- **DESIGNED** — specified in DESIGN.md / MONEY docs as intended behavior.
- **FUTURE** — open questions or non-implemented product surfaces.

| Mechanism | Status | Detail |
| --- | --- | --- |
| Burn 1 IQC per `commitQCPacket` (stake path) | **LIVE** | `IQCRegistryV3._commit` requires stake ≥ fee, then `burn` |
| ERC1363 exact-fee pay + commit | **LIVE** | `onTransferReceived` / `onIQCDataReceived` require `value == COMMIT_FEE`, then burn |
| Stake / unstake | **LIVE** | Registry holds stake balances |
| Owner slash → burn | **LIVE** | Privileged; Trust Model expects eventual governed process |
| Pause registry | **LIVE** | Owner pause/unpause |
| Permit / meta-tx / multicall / batch | **LIVE** (token) | UX/money-characteristic features |
| Votes checkpoints / delegation | **LIVE** (token capability) | Not a shipped governance UI/DAO |
| Variable fees / marketplace / pay-per-query | **FUTURE** | DESIGN.md open questions |
| Mandatory burn for all product commits off-registry | **FUTURE / product choice** | Product Merkle path can exist without token |
| Live DAO parameter control | **FUTURE** | Votes enable; nothing forces it |

No vapor: if it is not in the contracts or clearly labeled FUTURE, it is not claimed as live utility.

---

## 6. Tokenomics table and unlock/vest

### Supply split (specified)

| Allocation | Amount (IQC) | % | On-chain / docs description |
| --- | --- | --- | --- |
| Genesis to `immutableqc.base.eth` | 10,000,000 | 1% | Genesis allocation to deployer |
| Community & Staking Rewards | 250,000,000 | 25% | Community & Staking Rewards |
| Liquidity & Market Making | 200,000,000 | 20% | Liquidity & Market Making |
| Team & Advisors | 150,000,000 | 15% | Team & Advisors |
| Treasury & Operations | 150,000,000 | 15% | Treasury & Operations |
| Ecosystem & Partnerships | 100,000,000 | 10% | Ecosystem & Partnerships |
| Early Contributors & Incentives | 100,000,000 | 10% | Early Contributors & Incentives |
| Reserved for Future Use | 40,000,000 | 4% | Reserved for Future Use |
| **Total** | **1,000,000,000** | **100%** | |

### Unlock / vest

`TokenAllocation` **supports** linear vesting with cliff in Solidity. The Ignition module fetched 2026-09-08 **does not pass** `vestingDuration` / `cliffDuration` (constructor arity mismatch — see `issues/TOKENALLOCATION_CONSTRUCTOR_MISMATCH.md`).

Therefore unlock dates are **unspecified on-chain descriptions / deploy-module parameters**, not a calendar you can chart honestly. This paper deliberately **does not** include a fake unlock pie chart.

Deflation side-channel: burns from commits and slashes reduce `totalSupply` when `burn` is used (registry paths). Magnitude depends on real usage — unknown pre-adoption.

---

## 7. Registry / Merkle commitments vs the token

Keep three layers distinct:

1. **Product integrity path** — instrument signatures, hash-chained audit trail, Merkle roots published for verification (marketing/product). Can operate for labs without a public token.
2. **On-chain registry (`IQCRegistryV3`)** — stake, slash, emit `QCPacketCommitted` with instrumentId/dataHash, burn fees. Couples to IQC when that deployment is used.
3. **Token (`IQCToken`)** — transferable ERC-20 with extensions; supply escrowed in allocation contracts.

Conflating “we publish Merkle roots” with “you must buy IQC” is a category error. Merkle publication is a product integrity mechanism; IQC burn is an optional economic spam brake on a specific registry deployment.

---

## 8. Governance

What Votes **actually** enables today:

- Checkpointed voting power and delegation at the token layer (OpenZeppelin ERC20Votes pattern).

What it does **not** imply:

- A live DAO, on-chain proposal module, or community control of slash/pause/fees.
- Automatic transfer of `Ownable` rights from deployer to token holders.

Steady-state Trust Model language in contract comments expects powerful roles (slash, pause, allocation `release`) to move toward timelocked multisig / governed process — that is **operational intent**, not a shipped governor.

---

## 9. Risks

| Risk | Description |
| --- | --- |
| Contract | Bugs in token, registry, or allocation logic; AI audit ≠ firm audit; privileged owner functions |
| Liquidity | Thin or unlocked LP; inability to exit; POL mismanagement |
| Unlock / vest | Unspecified deploy parameters → sudden circulating supply shocks |
| Regulatory | Public token distributions may be securities offerings in some jurisdictions — obtain counsel; this paper is not advice |
| Product-without-token | Correct outcome for many labs; implies weak organic demand for a float |
| Memecoin residual | Speculative trading detached from lab usage damages trust with regulated buyers |
| Centralization | Owner slash/pause/release until renounced or governed |
| Bridge / chain | Base and bridge assumptions; no multi-chain claim in this version |

---

## 10. Roadmap (checkable)

Not a price roadmap.

| Step | Exit criterion |
| --- | --- |
| Sepolia integration | Token + registry tests green; sample commits observable |
| Fix allocation deploy params | Ignition matches Solidity ABI; vest/cliff published per bucket |
| First lab commit | Named design-partner lab performs registry commits in a real workflow (testnet OK) |
| Firm audit path | Vendor engaged or report published |
| Lock mint (on deploy) | `lockMintingForever` executed as part of deployment path |
| Counsel review | Written advice on any public distribution/LP plan |
| Liquidity (only if GO) | Aerodrome (or Uniswap v3) POL seeded from Liquidity bucket; locker address + duration published |

No “Q4 moon.” No guaranteed exchange listings.

---

## 11. References

1. [DESIGN.md](https://github.com/fabreu08/iqc-token/blob/main/DESIGN.md)
2. [MONEY_CHARACTERISTICS_ANALYSIS.md](https://github.com/fabreu08/iqc-token/blob/main/MONEY_CHARACTERISTICS_ANALYSIS.md)
3. [DEPLOYMENT_GUIDE.md](https://github.com/fabreu08/iqc-token/blob/main/DEPLOYMENT_GUIDE.md)
4. [iqc-token README](https://github.com/fabreu08/iqc-token/blob/main/README.md)
5. Contracts: `IQCToken.sol`, `IQCRegistryV3.sol`, `TokenAllocation.sol`
6. This repository: `audit-synthesis/` (FINAL_SYNTHESIZED_AUDIT.md, MASTER_RISK_TABLE.md, AUDIT_EVOLUTION_SUMMARY.md)
7. `launch/LAUNCH_STRATEGY.md` (companion decision memo)
8. Product: https://immutableqc.com

---

*End of white paper v0.1*
