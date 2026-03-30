# Skill: Minimal Diff Implementation

## Purpose
Prevent unnecessary changes and reduce regression risk.

---

## Instructions

When implementing:

- Modify only what is required to pass the test
- Avoid touching unrelated files
- Avoid renaming unless necessary
- Avoid introducing new abstractions prematurely

---

## Rules

- Smallest possible change wins
- No opportunistic refactoring
- No stylistic rewrites
- No dependency additions without justification

---

## Trade-off

Pros:
- Lower regression risk
- Easier review

Cons:
- May accumulate technical debt (address later in controlled refactors)