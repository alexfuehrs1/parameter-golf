# Skill: Experiment Documentation and Analysis

## Purpose

Ensure that every change, experiment, and result is:
- clearly documented
- causally analyzed
- comparable to previous runs
- useful for future decision-making

This is not just logging.
This is structured reasoning about what actually worked and why.

---

## When to Use

Use this skill whenever:
- a model, training setup, tokenizer, or evaluation changes
- a new experiment is run
- results differ from expectations
- a regression or improvement is observed
- a hypothesis is tested

---

## Core Principles

1. No undocumented experiments
2. No results without interpretation
3. No conclusions without uncertainty estimates
4. Separate signal from noise
5. Prefer causal reasoning over correlation

---

## Required Analysis Steps

For every experiment or change:

### 1. Change Description
- What exactly changed?
- Be precise (architecture, hyperparameters, tokenizer, eval, etc.)

### 2. Hypothesis
- What was expected to happen?
- Why (mechanism-level explanation)?

### 3. Experimental Setup
- Full run or proxy?
- Dataset slice / context length
- Training duration
- Model size / parameters
- Compression / quantization

### 4. Result
- Metric (BPB or proxy)
- Artifact size
- Runtime
- Any anomalies

### 5. Comparison to Baseline
- What changed vs previous best run?
- Absolute difference
- Relative difference

### 6. Causal Analysis
- Is the improvement likely real?
- Could it be noise?
- Could multiple changes interact?
- Is attribution clean?

### 7. Unexpected Effects
- What did NOT behave as expected?
- Any instability?
- Any side effects?

### 8. Strategic Interpretation
- Should this direction be:
  - expanded
  - refined
  - abandoned
- Why?

### 9. Next Action
- Next experiment to run
- What variable to isolate next

---

## Output Format

### Experiment Summary
- name:
- revision:
- type: (proxy / full)
- main change:

### Hypothesis
...

### Setup
...

### Result
- metric:
- artifact_size:
- runtime:

### Comparison
...

### Causal Analysis
...

### Unexpected Observations
...

### Strategic Decision
...

### Next Experiment
...

---

## Rules

- Never just report metrics without interpretation
- Never claim success without comparing to a baseline
- Never attribute causality if multiple variables changed (unless stated)
- Always include uncertainty if applicable
- Always propose a next step

---

## Anti-Patterns

- "BPB improved slightly" without context
- "Seems better" without numbers
- Multiple simultaneous changes without attribution
- No baseline comparison
- No follow-up plan

---

## Optional Extensions

If available, also track:
- seeds and variance across runs
- confidence intervals
- rolling leaderboard of best configs

---

## Goal

Turn experimentation into a structured, compounding learning process.

Each experiment should increase:
- understanding of the search space
- confidence in promising directions
- speed of future iteration