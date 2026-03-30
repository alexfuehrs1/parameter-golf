# Skill: Benchmark Fidelity

## Purpose
Ensure all work is aligned with the real challenge objective rather than vague local proxies.

## Instructions

Before implementing or proposing an experiment:

1. Identify the exact target:
   - BPB on FineWeb validation
   - artifact size
   - 10-minute wall-clock budget
   - submission legality

2. Distinguish clearly between:
   - true target metric
   - proxy metric
   - engineering convenience metric

3. For every proxy, state:
   - why it is expected to correlate with BPB
   - where correlation may break

4. If local evaluation differs from the official setup, explicitly label it as proxy-only.

## Rules

- Never confuse training loss with final leaderboard value.
- Never assume a proxy improvement will transfer.
- Always mention what is and is not benchmark-faithful.
- Prefer improvements that can be explained in terms of final BPB, artifact size, or runtime.

## Output Format

### Benchmark Target
...

### Proxy Used
...

### Why It Should Correlate
...

### Fidelity Risks
...