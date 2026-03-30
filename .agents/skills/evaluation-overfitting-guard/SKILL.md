# Skill: Evaluation Overfitting Guard

## Purpose
Prevent optimizing for accidental quirks of the evaluation setup in ways that may be brittle or non-compliant.

## Instructions

When proposing evaluation-time tricks, adaptation, or caching:
- identify whether the method uses only permitted information
- identify whether it may overfit to the validation procedure
- distinguish robust modeling gains from eval-specific exploitation

Use conservative language when legality or generality is unclear.

## Rules

- Do not present fragile eval hacks as general improvements
- Always flag leakage or ambiguity risk
- Prefer methods that would remain reasonable under stricter evaluation

## Output

### Proposed Eval-Side Method
...

### Why It Might Help
...

### Overfitting / Leakage Risk
...

### Conservative Alternative
...