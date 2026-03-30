# Frozen Baseline: `repo_aligned_transformer_v1`

## Why This Baseline

This is the most credible repo-aligned baseline for Lane B because it follows the board order and optimizes for time-to-signal with low compliance risk:

- existing dense transformer path
- existing quant/export path
- existing loader path
- existing causal eval path
- fixed default seed

It intentionally avoids:

- new architecture
- new tokenizer
- quant sweeps
- loader changes
- sliding-eval or TTT defaults

## Freeze Contract

All later experiments must compare against `repo_aligned_transformer_v1` first.

Required comparison contract:

- keep this baseline config unchanged
- report deltas vs baseline for BPB, artifact size, runtime, and compliance risk
- if multiple variables change, explicitly mark attribution as weak
- if a run does not measure roundtrip BPB, treat it as proxy-only evidence

## Proxy Position

This baseline is benchmark-faithful in objective, but any future smoke or 1-GPU run is still only a proxy for the official 8xH100 budgeted setting. Use proxy runs only to validate stability, logging, and direction before scaling.

## Reproducibility Anchor

- baseline name: `repo_aligned_transformer_v1`
- frozen commit: `a92f98fcba8fe6e29ea991cdcb51111b04d31d3d`
- default seed: `1337`
- follow-up variance seeds: `1337`, `2337`, `3337`
