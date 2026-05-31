# Raw Model Outputs

This directory is intended to archive the raw responses from the various AI models that participated in the IQC token security audit research.

## Purpose

The raw outputs are valuable for:
- Understanding how different models approached the same problems
- Studying the evolution of analysis as more context (previous model responses, synthesis documents) was provided
- Research into prompt engineering and multi-model collaboration techniques for technical security work
- Transparency and reproducibility of the research process

## Organization

We plan to organize responses as follows:

```
raw-outputs/
├── round-1/                    # Initial independent reviews
│   ├── grok-4.3-hermes.md
│   ├── opus-4.8.md
│   └── ...
├── round-2/                    # Responses after seeing initial synthesis
│   └── ...
├── head-to-head/               # Direct model-to-model exchanges
│   └── ...
└── synthesis-iterations/       # Intermediate synthesis versions
```

## Current Status

As of the latest update, the raw individual model responses are not yet fully archived here. The focus has been on producing high-quality synthesized documents and structured head-to-head discussions.

Raw responses will be added over time as they are cleaned and organized.

## Note on Model Versions

Many responses came from specific model configurations (e.g., "Opus 4.7 with master prompt", "Kimi 2.5 with agents", "Grok 4.3 web"). When archiving raw outputs, we preserve the exact model identifier provided by the user at the time of the response.

---

*This directory supports the broader research into multi-model AI-assisted security auditing.*