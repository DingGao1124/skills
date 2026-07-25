# Code Quality Refactoring

Use for Level 1 work. Preserve system features, business rules, public interface semantics, user-visible behavior, and data formats.

## First pass: analyze only

Do not edit code. Run or identify tests, build, type checking, and static analysis to establish the baseline. Inspect implementation, callers, tests, types, and repository conventions.

Output:

1. module responsibilities;
2. duplicated code;
3. overlong functions or components;
4. complex conditional branches;
5. type and error-handling problems;
6. high-risk modification areas;
7. existing behavior coverage and missing characterization tests;
8. a small-step refactoring plan.

Record hidden contracts such as errors, event order, mutation, identity, serialization, rendering, accessibility, side effects, and performance.

## Allowed operations

- extract or inline functions, components, types, or modules;
- improve naming;
- eliminate real duplication;
- simplify conditions with explicit states or guards;
- strengthen types and null handling;
- unify error handling without changing semantics;
- isolate side effects behind narrow seams;
- remove dead code only with evidence.

Do not change business rules, interface meaning, user-visible behavior, or persisted formats. Do not add an unnecessary framework or design pattern.

## Execute safely

1. Run tests, build, type checking, and static analysis.
2. Make one coherent structural change.
3. Re-run the relevant tests, build, type checking, and static analysis.
4. Inspect the diff for behavior changes and unrelated cleanup.
5. Repeat until the stated quality problem is resolved.
6. Run the full applicable validation and manual checks.

If running the full suite after every tiny edit is impractical, run focused validation after each edit and the complete required suite at each stable checkpoint; state this choice explicitly.

## Completion criteria

- the quality problem is measurably reduced;
- external behavior and public contracts remain equivalent;
- baseline and final validation pass;
- no speculative abstractions or unrelated cleanup remain;
- any discovered behavior, architecture, or technology change has been reclassified.
