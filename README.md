# IQC Token Research

This repository documents the research and development work on the IQC (Immutable Quality Control) token, with a particular focus on security auditing methodology using multiple frontier AI models.

## Current Focus

### Multi-Model Security Audit Research

We conducted an extensive, iterative security review of the IQC token contracts (`IQCToken`, `IQCRegistryV3`, and `TokenAllocation`) involving multiple large language models.

**Key artifacts from this research:**

- [audit-synthesis/FINAL_SYNTHESIZED_AUDIT.md](./audit-synthesis/FINAL_SYNTHESIZED_AUDIT.md) — Final consolidated audit report
- [audit-synthesis/MASTER_RISK_TABLE.md](./audit-synthesis/MASTER_RISK_TABLE.md) — Calibrated risk register with cross-model attribution
- [audit-synthesis/AUDIT_EVOLUTION_SUMMARY.md](./audit-synthesis/AUDIT_EVOLUTION_SUMMARY.md) — Analysis of how findings and severity assessments evolved across successive reviews

### Head-to-Head Model Discussions

- [head-to-head/opus-4.7-vs-kimi-2.5/](./head-to-head/opus-4.7-vs-kimi-2.5/) — Direct debate package between two of the most rigorous reviewers (Opus 4.7 and Kimi 2.5)

## Research Themes

This body of work explores several emerging questions in AI-assisted security auditing:

- How do different models vary in severity calibration?
- What is the actual value (and limitations) of "multi-model consensus"?
- How to distinguish real security vulnerabilities from tokenomics/credibility issues
- The gap between textual analysis by LLMs and actual code execution / formal verification
- Methodological standards for credible LLM-assisted security reviews

## Repository Structure

- `/audit-synthesis/` — Final synthesized reports and risk tables
- `/head-to-head/` — Direct discussion packages between specific models
- (Future) `/iterations/` — Earlier raw model outputs and intermediate drafts (if archived)

---

*Part of the Immutable Quality Control (IQC) project.*