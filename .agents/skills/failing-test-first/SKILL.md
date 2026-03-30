# Skill: Failing Test First

## Purpose
Ensure every change is driven by a failing test.

---

## Instructions

Before writing any implementation:

1. Identify the behavior to change
2. Write a test that:
   - Reproduces the bug OR
   - Encodes the desired behavior
3. Confirm:
   - The test FAILS
   - It fails for the correct reason

Only then proceed to implementation.

---

## Rules

- No implementation without a failing test
- Avoid vague or weak assertions
- Tests must be specific and deterministic

---

## Anti-Patterns

- Writing tests after implementation
- Writing tests that always pass
- Testing implementation details instead of behavior