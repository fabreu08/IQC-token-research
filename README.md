# IQC Token Research

**Multi-Model AI-Assisted Security Auditing Research**

This repository serves as the primary home for research and documentation around the security auditing process for the IQC (Immutable Quality Control) token system, with a strong emphasis on exploring the emerging practice of using multiple frontier large language models for deep technical security reviews.

## Research Focus

The core of this work is an extensive, iterative security audit of the IQC smart contracts (`IQCToken`, `IQCRegistryV3`, and `TokenAllocation`) conducted through repeated engagement with multiple AI models.

This project investigates several important questions:

- How do different AI models vary in their ability to identify vulnerabilities, calibrate severity, and reason about complex architectural trade-offs?
- What are the strengths and fundamental limitations of "multi-model consensus" approaches in security auditing?
- How should we distinguish between genuine security vulnerabilities and tokenomics/credibility issues?
- What methodological standards are required to make LLM-assisted audits credible and useful?
- What is the gap between pure textual analysis by models and actual code execution, fuzzing, and formal verification?

## Key Outputs

### Synthesized Audit Reports

- [audit-synthesis/FINAL_SYNTHESIZED_AUDIT.md](./audit-synthesis/FINAL_SYNTHESIZED_AUDIT.md) — The final consolidated security audit report
- [audit-synthesis/MASTER_RISK_TABLE.md](./audit-synthesis/MASTER_RISK_TABLE.md) — Calibrated risk register with cross-model attribution and evolving severity assessments
- [audit-synthesis/AUDIT_EVOLUTION_SUMMARY.md](./audit-synthesis/AUDIT_EVOLUTION_SUMMARY.md) — Analysis of how findings and risk ratings changed across successive model reviews

### Head-to-Head Model Debates

- [head-to-head/opus-4.7-vs-kimi-2.5/](./head-to-head/opus-4.7-vs-kimi-2.5/) — Full discussion package from a direct debate between two of the most rigorous reviewers (Opus 4.7 and Kimi 2.5)


Do not replace audit-research framing; this addendum only links launch + whitepaper docs. -->

### Launch & whitepaper (research artifacts)

> **Shelved 2026-10-17:** the launch strategy and white paper below are shelved under Immutable QC's no-patented-work rule (no blockchain, token, registry or network anchoring). They stay here as a historical record only. Do not cite them as current plans.

Alongside the primary **audit-research** outputs (`audit-synthesis/` and related notes), this tree includes decision-grade documentation for the **optional** IQC token. The Immutable QC product (Open Alpha) does **not** require IQC; laboratory middleware—sign at instrument, hash-chain audit trail, Merkle roots—remains the focus. Registry on-chain commitments are **not** the same as the product Merkle audit trail.

- **Launch strategy (decision: WAIT):** [`launch/LAUNCH_STRATEGY.md`](launch/LAUNCH_STRATEGY.md) — compares liquidity-first Base (Aerodrome / Uniswap v3), social memepads (drop as primary), Virtuals, PinkSale/DxSale, Echo/Legion, and “don’t launch yet.” Primary recommendation: **WAIT** until firm-audit/FV path, real lab registry use, TokenAllocation vest fix, and counsel review. If/when GO: Aerodrome liquidity-first (POL); fallback Echo/Legion with counsel. No mainnet day-1 plan.
- **Whitepaper:** [`whitepaper/IQC_TOKEN_WHITEPAPER.md`](whitepaper/IQC_TOKEN_WHITEPAPER.md) · printable HTML: [`whitepaper/IQC_TOKEN_WHITEPAPER.html`](whitepaper/IQC_TOKEN_WHITEPAPER.html) — token design, LIVE/DESIGNED/FUTURE utility, 1B fixed supply and allocation table (unlocks **unspecified**), risks, checkable roadmap.
- **Open issue:** [`issues/TOKENALLOCATION_CONSTRUCTOR_MISMATCH.md`](issues/TOKENALLOCATION_CONSTRUCTOR_MISMATCH.md) — `TokenAllocation.sol` expects 5 constructor args; Ignition module passes 3; vest/cliff schedules unpublished.

These materials are research documentation only: **not deployed**, **not a firm audit**, **not legal advice**, and not an offer to sell. Prefer product proof over ticker narrative.

## Repository Structure

```
.
├── README.md
├── audit-synthesis/                    # Final synthesized research outputs
│   ├── FINAL_SYNTHESIZED_AUDIT.md
│   ├── MASTER_RISK_TABLE.md
│   └── AUDIT_EVOLUTION_SUMMARY.md
├── head-to-head/                       # Direct model-to-model discussions
│   └── opus-4.7-vs-kimi-2.5/
├── launch/                             # Launch strategy (WAIT decision)
│   └── LAUNCH_STRATEGY.md
├── whitepaper/                         # Token white paper (md + printable html)
│   ├── IQC_TOKEN_WHITEPAPER.md
│   └── IQC_TOKEN_WHITEPAPER.html
├── issues/                             # Research-filed contract/deploy issues
│   └── TOKENALLOCATION_CONSTRUCTOR_MISMATCH.md
├── raw-outputs/                        # Raw model responses (archived)
│   └── README.md
└── research/                           # Higher-level methodology papers
    └── (in progress)
```

## Status

This is an active research project. Primary focus remains multi-model audit methodology. Additional decision docs for optional token launch strategy and white paper live under `launch/` and `whitepaper/` (they do not replace the audit record).

---

*Part of the Immutable Quality Control (IQC) project.*