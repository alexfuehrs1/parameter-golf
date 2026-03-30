# Skill: Reproducible Experiments

## Purpose
Make results comparable, interpretable, and repeatable.

## Instructions

Every run should produce a structured log containing:
- run name
- code revision / branch / commit
- seed
- model configuration
- optimizer configuration
- data slice / proxy setup
- runtime
- BPB or proxy metric
- artifact size
- notes

Use stable naming for runs and avoid ambiguous labels.

## Rules

- No unlabeled experiments
- No result claims without config traceability
- No manual cherry-picking without explicit disclosure
- If multiple changes are bundled, state that attribution is weak

## Output

### Run Record Template
- run_name:
- revision:
- seed:
- config:
- proxy_or_full:
- runtime:
- metric:
- artifact_size:
- interpretation:

### Comparison Table
...