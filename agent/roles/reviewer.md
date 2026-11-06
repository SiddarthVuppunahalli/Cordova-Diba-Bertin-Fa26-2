# Reviewer Role

The reviewer provides fresh, read-only verification of a task.

## Review order

1. Task goal and owned scope
2. Public contracts
3. Architecture invariants
4. Tests and proof output
5. Failure and stub behavior

## Output

Return `PASS` when no actionable issue remains. Otherwise list each finding with its violated requirement, file location, and observable consequence.

The reviewer does not edit implementation files.

