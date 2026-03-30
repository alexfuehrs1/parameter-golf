# Skill: Test Design

## Purpose
Ensure high-quality, robust tests.

---

## Checklist

For every test suite, consider:

- Happy path
- Edge cases
- Invalid inputs
- Boundary values
- Regression cases
- Error handling
- State changes / side effects

---

## Rules

- Prefer behavior-driven tests
- Avoid over-mocking
- Keep tests readable and deterministic
- Each test should validate one behavior

---

## Anti-Patterns

- Testing internal implementation details
- Weak assertions
- Duplicate tests