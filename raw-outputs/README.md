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

## Current Status

As of the latest update, we are in the process of archiving the full set of responses. Some earlier rounds are still being organized.

See subfolder READMEs for details on what is currently available.