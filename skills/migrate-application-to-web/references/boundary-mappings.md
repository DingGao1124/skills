# Runtime Boundary Mappings

Use this reference to translate privileged-runtime capabilities into web architecture. Select patterns from requirements; do not assume every row needs a service.

| Source capability | Browser-safe target patterns | Required decisions |
|---|---|---|
| Read/write arbitrary local path | Upload/download; backend repository; object storage; explicitly authorized File System Access API | Durable owner, size limits, accepted types, versioning, retention |
| App-owned local folder | Database plus object storage; server-managed tenant/project namespace | Tenant isolation, quotas, backup, lifecycle |
| Local JSON/project file | Resource API backed by database or versioned blob | Identity, schema migration, conflict policy, export compatibility |
| Atomic local save | Database transaction; conditional update with version/ETag; staged object commit | Consistency boundary, retries, recovery |
| File picker/dialog | Browser file input, drag/drop, directory upload where supported | Browser support, batch size, validation |
| Native export dialog | Browser download, export job, signed URL | Naming, expiry, streaming, large exports |
| File watcher | Server events, polling, database change stream, explicit refresh | Latency, ordering, reconnection |
| Child process or shell command | Backend job worker, sandboxed execution service, managed external API | Trust, resource limits, cancellation, logs |
| Long-running in-process task | Durable job record plus worker/queue | Idempotency, retry, progress, ownership, restart recovery |
| Native event emitter | SSE, WebSocket, polling, or webhook | Ordering, replay cursor, authorization, disconnect recovery |
| Local image/media protocol | Authorized media endpoint, object URL, CDN/signed URL | Range requests, caching, MIME validation, access control |
| Clipboard/device/native integration | Browser API when supported; optional companion service; redesign | Permissions, browser compatibility, graceful fallback |
| Environment variables/local credentials | Server secret manager; user OAuth/session; backend provider credentials | Secret ownership, rotation, tenancy, audit |
| Local database | Managed server database; sync layer only when offline is required | Schema, tenancy, migrations, consistency |
| OS user/machine identity | Application account, organization, tenant, device registration | Authentication, authorization, privacy |
| Custom URI/deep link | HTTPS route, registered web link, application callback | Authentication state, validation, browser routing |
| Direct provider/model SDK | Backend provider adapter behind a product-owned inference contract | Streaming schema, files, model selection, errors, cost |

## Classify each boundary

For every source call, ask:

1. What user outcome does this capability enable?
2. Is the capability domain behavior or merely an adapter?
3. Who owns the authoritative data before and after migration?
4. Can the browser perform it safely and portably?
5. If moved to the backend, what authentication and tenant boundary applies?
6. Is the operation synchronous, asynchronous, streaming, or resumable?
7. What failure or recovery behavior is user-visible?
8. Does compatibility require importing old data or reproducing its exact format?

## File and asset rules

- Represent resources with IDs and metadata, never server-local paths.
- Validate file type by content as well as extension.
- Stream large uploads and downloads; avoid unbounded memory buffering.
- Define deduplication, checksums, immutable originals, derivatives, and retention.
- Separate import from export. Uploading creates a managed resource; exporting produces a user-downloadable artifact.
- Use signed URLs only as short-lived transport capabilities, not durable identifiers.
- Decide whether edits mutate a resource, create a version, or create a derived asset.

## Background work rules

Model long work as a durable resource:

```text
POST /jobs           -> accepted job with id
GET /jobs/{id}       -> current state and progress
POST /jobs/{id}/cancel
GET /jobs/{id}/events or SSE/WebSocket stream
```

Define terminal states, retry ownership, idempotency keys, cancellation guarantees, event ordering, reconnection, and what survives a server restart. Persist the result before announcing completion.

## Model-provider adapter rules

Keep this boundary provider-neutral:

```text
Product request
  -> inference port
  -> provider adapter
  -> normalized event stream
  -> validated product result
```

Normalize:

- input messages, images, files, and structured output requirements;
- request ID, job ID, result ID, and causation/correlation IDs;
- deltas, progress, final results, usage, warnings, and terminal errors;
- provider authentication, model identifiers, rate limits, and transport errors;
- timeout, cancellation, retry, and deduplication behavior.

Do not make UI state depend on a provider's raw event sequence. Write adapter contract tests using recorded fixtures for success, partial streams, disconnects, late terminal events, duplicate results, cancellation, and invalid structured output.
