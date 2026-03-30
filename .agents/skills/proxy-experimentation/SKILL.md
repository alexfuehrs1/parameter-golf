# Skill: Proxy Experimentation

## Purpose
Reduce wasted compute by validating ideas cheaply before expensive runs.

## Process

Use this escalation ladder:

1. static review
2. import / compile check
3. one-batch sanity run
4. tiny dataset / short-context proxy run
5. reduced-width or reduced-depth proxy run
6. single-device timing sanity check
7. only then consider a full expensive run

## Instructions

For each proposed experiment:
- define the cheapest version that could falsify the idea
- state what success on the proxy would mean
- state what proxy failure would imply
- avoid full runs unless the proxy result is informative

## Rules

- No expensive run without a proxy plan
- No “just try it” on full budget
- Every large run must have a hypothesis and stopping condition

## Output

### Cheapest Valid Proxy
...

### Expected Signal
...

### Go / No-Go Criterion
...

### Escalation Path
...