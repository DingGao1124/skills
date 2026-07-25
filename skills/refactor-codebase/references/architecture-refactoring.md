# Architecture Refactoring

Use for Level 3 work. Keep the main product and technology stack unless another level is explicitly classified.

## First pass: analyze only

Do not edit code. Output:

1. system entry points;
2. main modules and responsibilities;
3. module dependency relationships;
4. data flow and control flow;
5. calls between presentation, business logic, and persistence;
6. external services and storage dependencies;
7. circular dependencies, cross-layer calls, shared state, and overlapping responsibilities;
8. transaction, security, deployment, observability, and recovery boundaries.

Model the architecture the system actually runs, not only its documentation.

## Design the target architecture

Define:

- module boundaries and responsibilities;
- public interfaces and versioning;
- allowed dependency direction;
- authoritative data ownership;
- transaction and consistency boundaries;
- error and failure behavior;
- authentication and authorization;
- test and observability strategy;
- migration, compatibility, rollout, and rollback.

Write ADRs for consequential choices, including alternatives and validation.

Define enforceable constraints appropriate to the system, for example:

- UI does not access persistence directly;
- controllers do not own core business rules;
- repositories perform data access rather than workflow orchestration;
- cross-module calls use public contracts;
- dependencies flow toward stable domain/application boundaries;
- one authority owns each mutable data set.

## Plan before large edits

The first round must produce current-state analysis, target design, architecture constraints, and an incremental migration plan. Do not begin a large restructuring until these are accepted.

## Execute incrementally

1. Characterize cross-boundary behavior.
2. Introduce a contract, adapter, or seam.
3. Route one caller or business slice through it.
4. Move responsibility and data ownership together.
5. Verify contracts, runtime behavior, failures, performance, and observability.
6. Expand callers or traffic gradually.
7. Remove the old path only after authority and usage reach zero.

Avoid moving files without moving responsibility, distributed cycles, uncontrolled shared databases, and permanent dual ownership.
