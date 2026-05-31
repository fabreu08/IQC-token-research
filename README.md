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
├── raw-outputs/                        # Raw model responses (archived)
│   └── README.md
└── research/                           # Higher-level methodology papers
    └── (in progress)
```

## Status

This is an active research project. The current focus is on synthesizing lessons from the multi-model audit process and developing better frameworks for AI-assisted security auditing.

---

*Part of the Immutable Quality Control (IQC) project.*