# Technology Migration

Use for Level 4 work. Treat it as compatibility, architecture, data, and cutover work—not syntax conversion.

## First pass: analyze only

Do not edit code. Inventory:

1. user-visible capabilities;
2. source modules and dependencies;
3. core data structures and serialized formats;
4. file, database, import, export, and storage behavior;
5. runtime, OS, browser, device, process, or native calls;
6. reusable technology-independent logic;
7. capabilities that must be reimplemented;
8. capabilities that require redesign under target constraints;
9. deprecated capabilities;
10. migration, compatibility, security, performance, and operational risks.

Classify each capability:

- **Reuse**: keep as-is or with negligible adaptation;
- **Port**: reimplement while preserving behavior;
- **Redesign**: intentionally change for the target platform;
- **Replace**: substitute a different product or infrastructure capability;
- **Remove**: do not migrate;
- **Unknown**: require evidence or a decision.

Do not guess Unknown items.

## Build the migration matrix

| Capability | Source location | Target UI/client | Target backend/module | API/contract | Data structure | Storage/owner | Verification | Status | Risk/decision |
|---|---|---|---|---|---|---|---|---|---|

Add identity mapping, compatibility, dependencies, rollout, and rollback when applicable.

## Design the target system

Define target runtime, frontend/client, backend services, application/service layer, repositories, storage, contracts, common error format, authentication, events/jobs, drafts/cache, import/export, observability, and deployment.

For a native or desktop application moving to a browser:

- move privileged filesystem, process, credential, device, and local-runtime capabilities behind authorized backend APIs or explicitly supported browser capabilities;
- use resource IDs and metadata instead of exposing server paths;
- use durable server storage for formal data;
- use browser storage only for drafts, cache, offline recovery, or an explicitly accepted local authority;
- design upload, download, background-job, progress, cancellation, and reconnect behavior;
- do not translate each native function mechanically into an endpoint.

For provider or model migration, define a provider-neutral port for requests, assets, streaming events, results, errors, cancellation, retries, quotas, and correlation IDs.

## Execute by vertical capability

1. Establish source behavior and performance baselines.
2. Accept target contracts and intentional differences.
3. Build one thin end-to-end target path.
4. Migrate one complete capability across client, API, backend, storage, and tests.
5. Compare results, failures, and recovery.
6. Migrate data idempotently and reconcile it.
7. Shift callers or traffic gradually.
8. Observe, roll back if required, and transfer authority.
9. Remove the old implementation only after compatibility obligations end.

Use in-place upgrade, adapters, strangler migration, shadow comparison, or parallel implementation according to risk. Avoid big-bang rewrites and line-by-line translation.
