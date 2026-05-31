# IQC Multi-Model Audit — Research Log

This document provides a high-level timeline of the research process.

## Phase 1: Initial Audit & Early Reviews (Rounds 1–2)

- Original internal audit produced with broad findings.
- First wave of independent model reviews (Grok 4.3 Hermes, Opus 4.8, Grok 4.3 web).
- Early synthesis attempts.
- Initial observations on severity variation and tokenomics vs. security distinctions.

**Key artifact**: Raw outputs in `raw-outputs/round-1-initial/`

## Phase 2: Iterative Refinement & Growing Skepticism (Rounds 3–5)

- Models given increasing amounts of prior synthesis as context.
- Stronger methodological critiques begin to appear.
- First head-to-head experiments.
- Growing recognition of "phantom findings" and diminishing returns.

**Key artifact**: `raw-outputs/round-2-synthesis/` and early head-to-head packages.

## Phase 3: Deep Methodological Critique (Final Rounds + Head-to-Head)

- Two detailed rounds from Opus 4.7 (default and master prompt).
- Two detailed rounds from Kimi 2.5 (with agents and master prompt).
- Direct head-to-head debate between Opus 4.7 and Kimi 2.5.
- Strongest critiques of:
  - "Consensus theater"
  - Lack of actual code verification
  - Severity inflation
  - Undefined trust model as foundational gap
  - Phantom findings (especially Burn Permission Gap)

**Key artifacts**:
- `raw-outputs/final-rounds/`
- `head-to-head/opus-4.7-vs-kimi-2.5/`
- `research/Multi-Model-LLM-Security-Auditing-Methodology.md`

## Current Status (as of latest update)

- Significant body of raw model responses archived.
- Multiple rounds of synthesized documents produced.
- Head-to-head dialogue between two strongest critics completed.
- Initial methodology paper drafted.
- Clear recognition that the multi-model LLM process has real but limited value, with severe methodological limitations.

## Next Research Priorities

- Further development of the methodology paper
- Better documentation of the actual head-to-head dialogue
- Potential controlled experiments on optimal number of models
- Integration of actual code execution / tooling into future iterations

---

*This log is maintained as part of the IQC token research project.*