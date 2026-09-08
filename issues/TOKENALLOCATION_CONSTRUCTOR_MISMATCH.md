# Issue: TokenAllocation constructor / Ignition module argument mismatch

**ID:** TOKENALLOCATION_CONSTRUCTOR_MISMATCH  
**Severity:** High (blocks correct vest publication; unlock schedules unspecified)  
**Status:** Open — documented from fabreu08/iqc-token research  
**Affects:** `TokenAllocation.sol`, `ignition/modules/IQCTokenWithAllocations.ts`  
**Date recorded:** 2026-09-07  

---

## Summary

`TokenAllocation.sol` defines a constructor that requires **five** arguments:

1. `token`
2. `purpose` (`ALLOCATION_PURPOSE`)
3. `totalAllocated`
4. `vestingDuration`
5. `cliffDuration`

The Hardhat Ignition module `ignition/modules/IQCTokenWithAllocations.ts` passes only **three** arguments:

1. `token`
2. `purpose`
3. `amount`

**Consequence:** Vesting duration and cliff duration are **not supplied** by the current deploy module. Linear vesting + cliff behavior encoded in the contract therefore has **no published schedule** from this wiring. Treat all non-genesis allocation unlocks as **UNSPECIFIED**.

---

## Verified contract behavior (TokenAllocation.sol)

| Behavior | Detail | Confidence |
|----------|--------|------------|
| Pattern | Ownable escrow of allocated IQC | Verified |
| Purpose tag | `ALLOCATION_PURPOSE` | Verified |
| Release model | Linear vesting + cliff; `release()` | Verified |
| Recovery | **Cannot** recover allocated IQC | Verified |
| Constructor arity | **5** args (token, purpose, totalAllocated, vestingDuration, cliffDuration) | Verified |

---

## Verified deploy module behavior

| Behavior | Detail | Confidence |
|----------|--------|------------|
| Module | `ignition/modules/IQCTokenWithAllocations.ts` | Verified |
| Args passed | **3** (token, purpose, amount) | Verified |
| Vest / cliff args | **Omitted** | Verified |

This is a **contradiction** between Solidity constructor signature and Ignition call site—not an inference about intended business schedules.

---

## Allocation amounts (context; from DEPLOYMENT_GUIDE)

Genesis **10M (1%)** → `immutableqc.base.eth` is outside the seven escrow contracts. The **99%** remainder is intended for seven `TokenAllocation` instances:

| Purpose | Amount | % | Unlock in current wiring |
|---------|--------|---|--------------------------|
| Community & Staking Rewards | 250,000,000 | 25% | **Unspecified** |
| Liquidity & Market Making | 200,000,000 | 20% | **Unspecified** |
| Team & Advisors | 150,000,000 | 15% | **Unspecified** |
| Treasury & Operations | 150,000,000 | 15% | **Unspecified** |
| Ecosystem & Partnerships | 100,000,000 | 10% | **Unspecified** |
| Early Contributors & Incentives | 100,000,000 | 10% | **Unspecified** |
| Reserved for Future Use | 40,000,000 | 4% | **Unspecified** |

Total supply remains **1,000,000,000** IQC (fixed). Amounts above are Verified from DEPLOYMENT_GUIDE; **schedules are not**.

---

## Risk

1. **Deploy failure or incorrect parameterization** if the module is run against the 5-arg constructor without updates (compile/runtime mismatch depending on tooling).  
2. **Disclosure failure:** external readers cannot evaluate unlock cliffs for **99%** of supply.  
3. **Launch blocker:** Launch strategy GO trigger (c) requires TokenAllocation deploy/vest **fixed and published**.  
4. **Unlock cliff risk:** Until fixed, assume adverse interpretation (immediate or opaque unlock) for diligence.

**INFERRED (not a code claim):** Recommend publishing per-bucket `vestingDuration` and `cliffDuration` (and any TGE unlock %) before any mainnet allocation deploy; align Ignition module to the 5-arg constructor.

---

## Recommended fix path (documentation only — no deploy here)

1. Update `IQCTokenWithAllocations.ts` to pass `vestingDuration` and `cliffDuration` (and keep `totalAllocated` / amount consistent with DEPLOYMENT_GUIDE).  
2. Publish a vest table (per purpose) in repo docs and whitepaper §6.  
3. Add a test that asserts constructor arity and stored vest/cliff immutables.  
4. Do **not** mainnet-deploy until (1)–(3) and counsel/audit triggers are addressed.

---

## Non-goals / out of scope for this issue note

- Choosing specific vest months (product/legal decision).  
- Deploying to Sepolia or Base.  
- Changing MONEY_CHARACTERISTICS or ERC feature set.  
- Conflating registry commits with the product Merkle audit trail.

---

## References

- `TokenAllocation.sol` — constructor and `release()` semantics  
- `ignition/modules/IQCTokenWithAllocations.ts` — 3-arg call site  
- `DEPLOYMENT_GUIDE` — bucket amounts  
- [`../launch/LAUNCH_STRATEGY.md`](../launch/LAUNCH_STRATEGY.md) — WAIT / GO trigger (c)  
- [`../whitepaper/IQC_TOKEN_WHITEPAPER.md`](../whitepaper/IQC_TOKEN_WHITEPAPER.md) — §4, §6  

---

*No secrets. No deploy. Research documentation only.*
