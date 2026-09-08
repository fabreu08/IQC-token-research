# IQC Token — Launch Strategy

**Status:** Decision draft for `fabreu08/IQC-token-research`  
**Date:** 2026-09-08  
**Scope:** Whether / how / when to launch a public ERC-20 (`IQC`) on Base.  
**Not:** A deployment runbook, a fundraising pitch, or legal advice.

---

## One-page decision

| Field | Recommendation |
| --- | --- |
| **Decision** | **WAIT** |
| **Public ticker now?** | No |
| **Primary venue (if/when GO)** | Liquidity-first on Base: seed **Aerodrome** (fallback Uniswap v3) from the on-chain Liquidity & Market Making allocation — **not** a memepad |
| **Fallback raise venue** | Echo Sonar or Legion-class KYC curated sale (counsel required) — **not** PinkSale / Clanker / Zora / Virtuals |
| **Chain until trigger** | Stay undeployed / Base **Sepolia** for registry + token tests |
| **Mainnet deploy day 1?** | No |

**One-liner:** WAIT — do not list a public IQC ticker until a real lab uses the registry in a live workflow, vest/deploy params are fixed, and distribution has counsel review; then liquidity-first on Aerodrome, not a social launchpad.

### GO triggers (all preferred; (b)+(c) minimum)

1. Firm (or equivalent formal) security audit path started — AI multi-model audit alone is not enough for public float.
2. ≥1 real laboratory using registry commits in an actual QC workflow (testnet acceptable as proof-of-use).
3. `TokenAllocation` deploy module matches contract constructor; vesting/cliff parameters published.
4. Counsel review of any public distribution / liquidity plan (US Howey-style exposure).

### Explicit non-recommendations

Do **not** stealth-launch, wash-trade, hire paid shills, or promise listings. Do **not** treat Clanker / Zora / Virtuals / PinkSale as the primary path for a lab-integrity company.

---

## Product constraint (non-negotiable)

Immutable QC is laboratory middleware: sign measurements at the instrument, hash-chain an audit trail, publish Merkle roots ([immutableqc.com](https://immutableqc.com); Open Alpha [dashboard](https://immutableqc.com/dashboard/)).

The token is **optional infrastructure** around that product. Labs can use the product with **zero IQC** today. If a public float exists only as a ticker with a lab logo and no forced demand, the residual case is a **memecoin** — name it that.

---

## Verified token facts (from `fabreu08/iqc-token` main)

| Fact | Value | Source |
| --- | --- | --- |
| Ticker | IQC | README |
| Chain | Base (Sepolia for test) | README |
| Supply | Fixed **1,000,000,000** | README / DESIGN |
| Genesis | **1% (10M)** to `immutableqc.base.eth` | DEPLOYMENT_GUIDE |
| Escrow | **99%** across **7** `TokenAllocation` contracts | README |
| Features | ERC-20, Permit, Votes, ERC1363, ERC2771, Ownable2Step, Multicall, burn / `totalBurned` | DESIGN / MONEY_CHARACTERISTICS |
| Commit fee | **1 IQC** burned per QC commit (`COMMIT_FEE = 1e18`) | `IQCRegistryV3.sol` |
| Deployed? | **No** — addresses still placeholders | README |
| Firm audit? | **No** — AI multi-model synthesis in this research repo only | `audit-synthesis/` |

Money-design stance ([MONEY_CHARACTERISTICS_ANALYSIS.md](https://github.com/fabreu08/iqc-token/blob/main/MONEY_CHARACTERISTICS_ANALYSIS.md)): favor sound-money properties; **reject** fee-on-transfer and core-token blacklist/freeze.

---

## Option comparison (Base, verified ~2025–2026)

### A. Liquidity-first (own contracts → Aerodrome / Uniswap v3)

| Dimension | Assessment |
| --- | --- |
| Audience | Traders + protocol users who already hold Base assets; not Farcaster meme flow |
| Typical raise | None required — you seed LP from treasury/liquidity bucket |
| KYC / geo | None at DEX layer; your legal exposure is still issuer/distributor-side |
| Fees | DEX swap fees only; LP lock via third-party locker (Team Finance / UNCX class) — verify current Base support at time of use |
| Lock / vest norms | Common: 6–24 months LP lock; team vest with cliff. **INFERRED** recommendation for IQC: 12–24 months protocol-owned LP lock |
| Reputational fit (lab / regulated-data) | **Best** among public options — you control contract, disclosures, and liquidity narrative |
| What fails | Thin LP → volatility; large allocation unlocks without published vest → dump risk; launching before product use → memecoin perception |

**Primary path if/when GO.**

### B. Community / raise launchpads on Base

Sources dated for verification:

- Base docs — creator launchers: Zora, Clanker, Flaunch, Mint Club ([docs.base.org launch-token](https://docs.base.org/get-started/launch-token), fetched 2026-09-08).
- TrustSwap “Best Launchpads on Base in 2026” (updated **August 2026**): [trustswap.com/base/best-launchpads](https://trustswap.com/base/best-launchpads) — fees/mechanics table for Clanker, Zora, Flaunch, Mint Club, Virtuals, PinkSale, DxSale.
- Aerodrome as dominant Base DEX liquidity hub — CoinGecko Base ecosystem coverage (historical TVL leadership; re-check TVL at launch time).
- Echo / Legion — KYC + jurisdiction gates (Legion commonly excludes US/UK persons on token sales; Echo/Sonar supports founder-configured KYC/accreditation). Guides: web3wagmi Echo/Legion explainers; Legion CryptoSlate product page (2026).

| Venue | Audience | Raise / model | KYC / geo | Fee snapshot (Aug 2026 TrustSwap unless noted) | Lab reputational fit | Verdict for IQC |
| --- | --- | --- | --- | --- | --- | --- |
| **Clanker** | Farcaster social / meme | Instant deploy → Uniswap v4 pool | Permissionless | Creator sets 1–3% pool fee; Clanker takes **20% of LP fees** | Poor — meme/social brand | Drop as primary |
| **Zora** | Creator / content coins | Post → coin; Base App distribution | Permissionless | Share of ~1% trading fee | Poor — creator-coin frame | Drop as primary |
| **Flaunch** | Meme with economic knobs | Fair-launch window, v4 | Permissionless | 0.1% of set market cap + revenue split | Poor | Drop as primary |
| **Mint Club** | Bonding-curve families | No-code curve | Permissionless | ~0.0007 ETH create | Poor | Drop as primary |
| **Virtuals** | AI agent tokens | Bonding curve → Uniswap (~42k VIRTUAL graduation) | Permissionless | 1% trading fee (70% creator) | Poor — AI-agent lane | Drop as primary |
| **PinkSale / DxSale** | Presale retail | Hard/soft cap presale + lock tools | Optional KYC on some sales | PinkSale: **0.2 ETH + 5% of ETH raised** | Weak — category scam history | Avoid as primary |
| **Fjord** | Permissionless LBP traders | Liquidity bootstrapping pool | None | Gas + swap / curve slippage | Neutral-weak — open price discovery, no curation | Optional mechanic, not brand fit |
| **Echo (Sonar)** | Angels / screened buyers | Founder-hosted compliant sale infra | KYC / accreditation configurable | Platform / deal terms | **Better** if raising under counsel | Fallback raise |
| **Legion** | Merit/reputation ICO buyers | Curated token sale | Full KYC + PoA; US/UK often excluded | No launchpad-token stake required | **Better** if raising under counsel | Fallback raise |

Dead or irrelevant brands for this memo: do not build strategy around defunct BSC-era pads unless they show active Base volume at decision time. Prefer re-checking TrustSwap / Base docs tables in the week before any GO.

### C. Do not launch a public token yet

| Dimension | Assessment |
| --- | --- |
| Audience | Labs buying software; internal spam-resistance if registry is used privately |
| Raise | $0 from token; fund via software revenue / equity |
| KYC / geo | Ordinary commercial SaaS / software contracting |
| Reputational fit | **Highest** for a lab-integrity company pre–product-market proof |
| What fails | If you need crypto-native liquidity or on-chain fee burns at scale without a float — but that need is unproven until labs commit |

**This is the correct posture until GO triggers fire.**

---

## Answers to required questions

### 1. Launch now, after real lab use, or never?

**After real lab use (WAIT until then).**  
“Never as a public ticker” remains open if utility stays forever optional and demand is zero — then keep IQC as an internal coupon or leave undeployed. Do not launch **now**.

### 2. If launch: pad/venue and chain?

- **Venue:** None until triggers. Then **liquidity-first Aerodrome** (Uniswap v3 backup).  
- **Raise fallback:** Echo Sonar or Legion-class — not memepads.  
- **Chain:** Sepolia / undeployed until triggers; **Base mainnet** only after (b)+(c) at minimum.

### 3. Liquidity: who seeds, lock, 99% unlock risk?

- **Who seeds:** Protocol — from **Liquidity & Market Making (200M / 20%)** under multisig (transfer allocation ownership first).  
- **Lock:** **INFERRED** 12–24 months LP lock; publish locker address and unlock date. Prefer protocol-owned liquidity (POL) over anonymous public LP that can rug perception.  
- **If 99% unlocks:** Vesting parameters are **not** safely specified in the current Ignition module (see issue doc). Sudden owner `release()` of large buckets after cliff/vest completes (or if vest were zero) can overwhelm LP. Treat unspecified unlock as a **blocker** for GO.

### 4. Distribution vs the 7 TokenAllocation buckets

| Bucket | Amount | % | On-chain purpose (stated) | Mapping note |
| --- | --- | --- | --- | --- |
| Genesis (EOA) | 10M | 1% | Deployer `immutableqc.base.eth` | Operating / bootstrap — not an allocation contract |
| Community & Staking Rewards | 250M | 25% | Community & staking rewards | Purpose stated; **release schedule unspecified** in deploy module |
| Liquidity & Market Making | 200M | 20% | DEX liquidity & MM | Seed LP here if/when GO; schedule unspecified |
| Team & Advisors | 150M | 15% | Team & advisors | Purpose stated; vest/cliff must be set explicitly before GO |
| Treasury & Operations | 150M | 15% | Ops / development / marketing | Purpose stated; schedule unspecified |
| Ecosystem & Partnerships | 100M | 10% | Grants / integrations / partnerships | Purpose stated; do not invent partner names |
| Early Contributors & Incentives | 100M | 10% | Early supporters | Purpose stated; schedule unspecified |
| Reserved for Future Use | 40M | 4% | Future decisions / governance | Explicitly open-ended — leave undefined rather than fake “ecosystem” story |

**Undefined is allowed.** Inventing fake sub-buckets is not.

### 5. Legal / regulatory (not legal advice)

Public marketing of a tradable token with expectations of profit from others’ efforts can implicate **US Howey-style** securities analysis and non-US regimes (e.g. MiCA for EU offers). Labs buying **software** is not the same as the public buying a **ticker**.

This document is **not legal advice**, **not an offer to sell securities**, and **not a promise of profit**. Before any public distribution or liquidity event: engage qualified counsel; do not use this memo as an evasion plan.

### 6. Honest utility test

- **Product without token:** Live Open Alpha dashboard — labs can evaluate integrity workflows with **zero IQC**.  
- **Token with product:** Burns / stake / ERC1363 paths exist in contracts (`IQCRegistryV3`) but are only meaningful when labs actually commit.  
- **Token without product demand:** Ticker + lab logo = **memecoin residual case**.

### 7. Prohibited tactics

No stealth launch theater, wash trading, paid shills, or “guaranteed listing” claims.

---

## Recommended path summary

| Phase | Action |
| --- | --- |
| Now | **WAIT**. Keep research + Sepolia experiments. Do not mainnet deploy for listing. |
| Primary (post-trigger) | Deploy own `IQCToken` + allocations on Base → lock mint → seed **Aerodrome** POL from Liquidity bucket → long LP lock → disclose unlock schedules |
| Fallback (if capital raise needed) | Echo / Legion-class KYC sale under counsel; still avoid memepad brand contamination |
| Don't | Public retail ticker with no lab usage and AI-only audit |

---

## 30 / 60 / 90 day plan (no mainnet deploy on day 1)

### Days 0–30

- Keep token **undeployed** on mainnet; use Sepolia for registry + token integration tests.
- Fix `TokenAllocation` Ignition constructor mismatch; write explicit vesting/cliff per bucket (even if “0 cliff / N year linear”).
- Document Trust Model ownership path (multisig / timelock) as a checklist, not a live deploy.
- Continue product Open Alpha; pursue **one design-partner lab** for testnet commits.
- No launchpad applications; no LP seeding on mainnet.

### Days 31–60

- If a lab is testing: instrument commit metrics (commits/day, burn rate, failure modes).
- Commission or schedule **firm** audit / formal verification scope (budget + vendor shortlist).
- Draft counsel questions memo (distribution, LP, communications) — still not an offer.
- Optionally: dry-run liquidity sizing spreadsheet from Liquidity bucket (POL vs public) — paper only.

### Days 61–90

- Re-evaluate WAIT vs GO against triggers (a)–(d).
- If still missing lab usage or vest clarity → **extend WAIT**.
- If triggers met → publish unlock schedule, audit status, and a liquidity-first Base mainnet plan (Aerodrome first). Still no memepad.

---

## References

1. `fabreu08/iqc-token` — README, DESIGN.md, DEPLOYMENT_GUIDE.md, MONEY_CHARACTERISTICS_ANALYSIS.md, `contracts/IQCToken.sol`, `IQCRegistryV3.sol`, `TokenAllocation.sol`, `ignition/modules/IQCTokenWithAllocations.ts` (fetched 2026-09-08).
2. Base docs — [Launch a token](https://docs.base.org/get-started/launch-token) (fetched 2026-09-08).
3. TrustSwap — [Best Launchpads on Base in 2026](https://trustswap.com/base/best-launchpads) (updated August 2026; fee table cited above).
4. CoinGecko — Base native projects / Aerodrome as liquidity hub (re-verify TVL at decision time).
5. Echo Sonar compliance overview — [docs.echo.xyz](https://docs.echo.xyz/sonar/planning/compliance-overview).
6. Legion — CryptoSlate launchpad profile / KYC notes (2026 reviews).
7. This repo — `audit-synthesis/` (AI multi-model only).
8. Product — [immutableqc.com](https://immutableqc.com).

---

*Immutable QC product first. Token second. WAIT until the product earns the float.*
