# Raw Model Outputs Archive

This directory preserves the raw responses from the various AI models that participated in the IQC token multi-model security audit research.

## Organization

```
raw-outputs/
├── round-1-initial/         # First independent reviews (no prior synthesis context)
├── round-2-synthesis/       # Responses after seeing initial synthesis documents
├── round-3-refinement/      # Later iterative reviews with more context
├── head-to-head/            # Direct model-to-model debate packages
└── final-rounds/            # Last set of deep critiques (Opus 4.7 + Kimi 2.5)
```

## Purpose

These raw outputs are preserved for:
- Transparency of the research process
- Future analysis of how different models reason about security
- Studying the effects of providing previous model outputs as context
- Documenting the evolution of the multi-model auditing methodology

## Note on Model Identification

Responses are labeled with the exact model + configuration the user reported at the time (e.g., "Opus 4.7 default", "Opus 4.7 with master prompt", "Kimi 2.5 with master prompt").

## Current Status (Maximum Detail Backfill)

As of this update, the following have been archived with detailed summaries and key positions:

**Round 1 - Initial Independent Reviews**
- Fully documented: Grok 4.3 Hermes, Opus 4.8, Grok 4.3 web

**Round 2 - After Initial Synthesis**
- Kimi 2.5 (with agents)

**Final Rounds**
- Two detailed responses from Opus 4.7 (default + master prompt)
- Kimi 2.5 (master prompt) — full response

**Head-to-Head**
- Full debate package between Opus 4.7 and Kimi 2.5 (also available in the `head-to-head/` folder at repo root)

Earlier raw responses will continue to be added as they are extracted with maximum available detail from the conversation history.

See individual round READMEs for more specific status.