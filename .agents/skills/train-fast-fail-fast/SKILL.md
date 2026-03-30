# Skill: Train Fast, Fail Fast

## Purpose
Catch broken ideas early and cheaply.

## Steps

1. import / dependency check
2. config validation
3. one forward/backward pass
4. one optimizer step
5. one-batch overfit sanity check
6. short proxy train
7. timing sanity check
8. only then bigger runs

## Rules

- Do not wait for a long run to discover basic failures
- Fail fast on numerical instability, shape mismatches, serialization issues, and runtime explosions
- Add quick guards where failures are common

## Output

### Earliest Failure Checks
...

### What Could Break First
...

### Early Abort Conditions
...