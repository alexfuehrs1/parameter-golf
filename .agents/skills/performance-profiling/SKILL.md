# Skill: Performance Profiling

## Purpose
Optimize wall-clock training under the challenge time cap.

## Instructions

When profiling, break runtime into:
- startup / initialization
- data loading
- forward pass
- backward pass
- optimizer step
- synchronization / communication
- checkpointing / serialization
- evaluation overhead

Measure wall-clock, not just step counts.

## Rules

- Do not assume throughput improvements automatically improve end-to-end time
- Identify the dominant bottleneck before proposing optimizations
- Include overhead outside pure training loops

## Output

### Runtime Breakdown
...

### Largest Bottleneck
...

### Optimization Candidates
...

### Expected Time Savings
...