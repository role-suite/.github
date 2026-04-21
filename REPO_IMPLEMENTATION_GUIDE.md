# Role Suite Implementation Guide

This document expands `REPO_CHECKLIST.md` into implementation-ready instructions.

Use this guide as the execution reference for `role-node`, `role-sdk`, and `role-client`.

## How To Use This Guide

- Treat `role-node` as the source of truth for public API behavior.
- Do not change SDK or client contracts first and then backfill the backend.
- For each item, complete the output listed under `Deliverable` before moving on.
- Prefer small PRs grouped by concern: contract, errors, docs, tests, release.

## role-node

### 1. Contract Ownership

**Goal**

Make `role-node` the only repo that defines the public API contract.

**Implementation**

- Create a top-level `contracts/` directory.
- Add subfolders by module:
  - `contracts/auth/`
  - `contracts/workspaces/`
  - `contracts/collections/`
  - `contracts/environments/`
  - `contracts/runs/`
  - `contracts/import-export/`
- For each public endpoint, define:
  - method
  - path
  - auth requirement
  - request schema
  - success response schema
  - error response schema
- If you stay with Zod internally, export schemas or generate JSON/OpenAPI artifacts from them.

**Deliverable**

- A committed `contracts/` directory covering every public route.
- A `contracts/README.md` explaining how the contract is maintained.

**Done when**

- SDK and client teams can inspect one place to understand the full API.

### 2. API Versioning

**Goal**

Prevent silent breaking changes.

**Implementation**

- Choose one approach now:
  - add `/api/v1/...` prefixes, or
  - keep current paths and publish an explicit API version policy document.
- Add `docs/api-versioning.md`.
- Define:
  - what is a breaking route change
  - what is a breaking field change
  - what is an additive change
  - how long deprecated fields/routes remain supported

**Deliverable**

- A versioning policy doc linked from `README.md`.

**Done when**

- Future SDK releases can say exactly which backend contract they support.

### 3. Error Model

**Goal**

Give SDK and client stable machine-readable failures.

**Implementation**

- Standardize one error envelope for all modules.
- Recommended shape:

```json
{
  "success": false,
  "error": {
    "code": "WORKSPACE_NOT_FOUND",
    "message": "Workspace not found",
    "details": {},
    "requestId": "req_123"
  }
}
```

- Add a central error code registry file, for example:
  - `src/shared/errors/error-codes.ts`
- Group codes by domain:
  - auth
  - workspaces
  - collections
  - environments
  - runs
  - import-export
  - common/system
- Replace ad hoc string messages with typed error creation helpers.

**Deliverable**

- Shared error formatter.
- Shared error code registry.
- Error documentation table in `docs/errors.md`.

**Done when**

- SDK and client can branch on `error.code` without parsing `message`.

### 4. Route Consistency

**Goal**

Eliminate route drift across backend, SDK, docs, and client.

**Implementation**

- Create a single route definition file, for example:
  - `src/shared/http/routes.ts`
- Move route strings out of scattered controllers/modules where practical.
- Audit current route naming with these checks:
  - plural resources use plural forms consistently
  - actions use consistent verbs
  - nested resource ordering is stable
- Immediately fix known drift:
  - `import-export/exports`
  - `import-export/imports`
- Update docs and tests in the same PR.

**Deliverable**

- One route registry file.
- One route audit PR that aligns docs and tests.

**Done when**

- There is no mismatch between README/docs/test fixtures and actual routes.

### 5. Response Consistency

**Goal**

Make all success responses predictable for consumers.

**Implementation**

- Define shared success helpers for:
  - object result
  - list result
  - cursor page result
  - action result such as `{ deleted: true }`
- Pick one cursor shape and use it everywhere.
- Pick one style for action confirmations:
  - `deleted`
  - `left`
  - `revoked`
  - `cancelled`
- Avoid endpoint-specific one-off success payloads unless necessary.

**Deliverable**

- Shared response helper utilities.
- A short response-shape spec in `contracts/README.md`.

### 6. Schema Validation

**Goal**

Ensure contract, runtime validation, and docs stay aligned.

**Implementation**

- For each endpoint, keep request and response schemas close to the handler.
- Export public schemas into `contracts/` or generate them during build.
- Add CI validation that detects:
  - undocumented routes
  - changed request/response shape
  - removed public fields

**Deliverable**

- Schema export/generation step.
- CI check for contract drift.

### 7. Request Correlation

**Goal**

Make production debugging easier.

**Implementation**

- Add middleware that assigns a request id on every request.
- Set `x-request-id` in responses.
- Include the same id in structured error payloads.
- Add request id to application logs.

**Deliverable**

- Middleware and docs entry describing correlation id behavior.

### 8. Compatibility Policy

**Goal**

Tell consumers which repo versions work together.

**Implementation**

- Add `docs/compatibility.md`.
- Publish a table:
  - `role-node` version
  - supported `role-sdk` versions
  - supported `role-client` versions
- Update the table every release.

**Deliverable**

- Compatibility table linked from `README.md`.

### 9. Consumer Docs

**Goal**

Make backend integration documentation production-grade.

**Implementation**

- Expand `docs/guides/client-integration.md` to include:
  - authentication lifecycle
  - token refresh rules
  - pagination/cursor semantics
  - common error codes
  - retry recommendations
  - rate/size/time limits
- Ensure examples are copied from test fixtures, not hand-written from memory.

**Deliverable**

- A contract guide that SDK/client engineers can implement from directly.

### 10. CI and Release

**Goal**

Make contract changes visible before release.

**Implementation**

- Add a contract snapshot job.
- Add a breaking-change detector if contract artifacts differ in incompatible ways.
- Require docs update when public routes or shapes change.
- Add a release checklist item: update changelog and compatibility matrix.

**Deliverable**

- CI pipeline step named clearly, such as `Contract Check`.

### 11. Package and Repo Hygiene

**Goal**

Keep the repo understandable to contributors.

**Implementation**

- Improve `package.json` description from generic wording to product wording.
- Add useful keywords such as `api`, `express`, `typescript`, `role-suite`.
- Remove `pnpm-workspace.yaml` if no workspace packages exist.
- Ensure README and package metadata use the same repo naming.

**Deliverable**

- A cleanup PR that changes no behavior but improves maintainability.

## role-sdk

### 1. Product Positioning

**Goal**

Ship a reliable official SDK without overcommitting to unfinished parity.

**Implementation**

- Update README and docs to say:
  - `role-node` is the primary supported backend for v1
  - `serverpod` support is experimental until declared stable
- Do not market both providers equally until real parity and integration coverage exist.

**Deliverable**

- Updated README and architecture docs reflecting actual support status.

### 2. Contract-Driven Development

**Goal**

Prevent route and schema drift.

**Implementation**

- Stop manually maintaining endpoint assumptions independent of backend contract.
- Pull route definitions and sample contract fixtures from `role-node`.
- Add a CI job that validates SDK provider mappings against backend contract artifacts.

**Deliverable**

- A contract sync process documented in `docs/README.md`.

### 3. Package Readiness

**Goal**

Make the repository publishable and trustworthy.

**Implementation**

- Add missing `LICENSE`.
- Add package metadata:
  - `repository`
  - `bugs`
  - `homepage`
  - `keywords`
  - `author` or maintainers if desired
- Replace `0.0.0` with either:
  - `0.1.0` for active preview
  - `1.0.0-beta.1` for prerelease strategy
- Verify published files only include what consumers need.

**Deliverable**

- A package ready for npm publication with complete metadata.

### 4. README Overhaul

**Goal**

Make the SDK usable without reading internal architecture docs first.

**Implementation**

- Rewrite README sections in this order:
  - what the SDK is
  - installation
  - minimal setup
  - auth example
  - workspace example
  - error handling
  - compatibility matrix
  - links to deeper docs
- Add real copy-pasteable code samples.
- Move implementation planning docs lower in priority and out of the README intro.

**Deliverable**

- Consumer-first README.

### 5. Public API Stabilization

**Goal**

Lock down the API before `1.0.0`.

**Implementation**

- Review every exported input/output type for naming consistency.
- Prefer names that reflect domain concepts, not provider internals.
- Freeze method names before GA.
- Document any unstable parts as experimental.

**Deliverable**

- A public API review pass with breaking changes done before stable release.

### 6. Error Handling

**Goal**

Expose a clean and reliable error model to SDK consumers.

**Implementation**

- Ensure every thrown SDK error includes:
  - `code`
  - `message`
  - optional `status`
  - optional `requestId`
  - safe `details`
- Add docs for retryable and non-retryable failure classes.
- Map backend error codes explicitly instead of relying on free-form strings.

**Deliverable**

- Error handling section in README and a dedicated errors doc.

### 7. Testing Strategy

**Goal**

Test real behavior, not only mocked assumptions.

**Implementation**

- Keep current unit and mocked integration tests.
- Add test suites that run against a real `role-node` instance.
- Cover:
  - auth login/refresh/logout
  - workspace listing and membership flows
  - collection CRUD
  - environment CRUD
  - runs
  - import/export
- Add regression tests for every discovered drift issue.

**Deliverable**

- CI path that can run end-to-end tests against `role-node`.

### 8. Release Process

**Goal**

Release only when compatibility is known.

**Implementation**

- Keep semantic-release if you want automated publishing.
- Add release documentation that requires:
  - tested `role-node` version
  - changelog update
  - compatibility table update
- If still unstable, publish prereleases only.

**Deliverable**

- `docs/releasing.md` and a stable release checklist.

### 9. Distribution Quality

**Goal**

Avoid packaging surprises for users.

**Implementation**

- Verify the built package works in:
  - Node 18+
  - browser with fetch
  - edge/serverless fetch environments if claimed
- Decide clearly whether to support ESM only.
- If ESM only, document it prominently.

**Deliverable**

- Runtime support matrix in README.

### 10. Examples and Developer Experience

**Goal**

Reduce adoption friction.

**Implementation**

- Add `examples/` with:
  - `node-basic/`
  - `browser-basic/`
  - `nextjs/` or another server framework example
- Include examples for:
  - login and token persistence
  - `inWorkspace(...)`
  - error handling
  - hooks
  - custom token store

**Deliverable**

- Working examples that are tested or smoke-checked in CI.

### 11. Scope Control

**Goal**

Keep the SDK maintainable while it matures.

**Implementation**

- Do not split into `sdk-core` yet.
- Do not add extra abstraction layers unless they solve a real current problem.
- Prefer direct, clear mapping code over speculative architecture.

**Deliverable**

- A leaner roadmap with fewer moving parts.

## role-client

### 1. Architecture Clarity

**Goal**

Keep the app as a consumer of the backend contract, not an owner of it.

**Implementation**

- Update docs to say the client consumes backend APIs defined by `role-node`.
- Keep domain and UI logic in the app repo.
- Avoid embedding unofficial API rules in app docs.

**Deliverable**

- Updated architecture docs with clear contract ownership boundaries.

### 2. Naming Cleanup

**Goal**

Reduce confusion across code and docs.

**Implementation**

- Audit these terms across the repo:
  - `relay`
  - `role-server`
  - `role-node`
  - `role-serverpod`
- Decide a naming rule:
  - app-facing generic names like `backend_api`
  - provider-specific folders like `role_node_api`
- Apply one consistent standard.

**Deliverable**

- One naming cleanup PR with updated docs and directory references.

### 3. Backend Integration Layer

**Goal**

Keep API client code consistent and easy to audit.

**Implementation**

- Audit `lib/core/services/role_node_api/*` against current backend contract.
- Centralize shared concerns:
  - route constants
  - envelope unwrapping
  - auth header handling
  - error normalization
- Remove duplicate path building logic if repeated.

**Deliverable**

- One backend integration layer with consistent conventions.

### 4. Contract Sync

**Goal**

Detect backend changes before they break the app.

**Implementation**

- Add a contract sync process using backend snapshots or generated docs.
- At minimum, store example fixtures or route references from `role-node`.
- Add tests for critical flows that depend on exact payload structure.

**Deliverable**

- A documented sync workflow in `docs/`.

### 5. Supported Backend Story

**Goal**

Be explicit about support level.

**Implementation**

- Document whether `role-node` is the primary backend.
- Document what features differ by backend, if any.
- Remove stale references to older backend naming.

**Deliverable**

- README/backend docs that match reality.

### 6. Auth Handling

**Goal**

Make app auth flows resilient and aligned with backend semantics.

**Implementation**

- Normalize app handling for:
  - expired access token
  - invalid refresh token
  - logout token revocation
  - session restore on app startup
- Move from message parsing toward backend error code handling once `role-node` provides stable codes.

**Deliverable**

- A single documented auth flow and matching integration tests.

### 7. Offline vs API Mode

**Goal**

Keep local and remote behavior understandable.

**Implementation**

- Document:
  - what data belongs only to local mode
  - what syncs to API mode
  - how conflicts are handled
  - what happens when backend data differs from local state
- Ensure terminology in UI and docs matches backend concepts like workspace, environment, run, import/export.

**Deliverable**

- A dedicated local-vs-api behavior doc.

### 8. Docs

**Goal**

Keep technical docs current and actionable.

**Implementation**

- Update architecture docs to reflect the current actual service layout.
- Add a compatibility section showing tested backend versions.
- Explain any backend-specific differences in API mode.

**Deliverable**

- Docs that match current production behavior.

### 9. Testing

**Goal**

Catch integration breakage early.

**Implementation**

- Add mocked tests for remote API clients.
- Add high-value integration tests for:
  - login
  - workspace selection
  - collection fetch/save
  - environment fetch/save
  - sync to remote
- Add regression tests for every route or payload mismatch fixed.

**Deliverable**

- A remote API test suite focused on contract stability.

### 10. UX and Operational Behavior

**Goal**

Make backend failures understandable to users.

**Implementation**

- Define user-facing UI behavior for these statuses/codes:
  - `401`
  - `403`
  - `409`
  - `410`
  - `422`
  - `502`
- Map these to consistent actions:
  - prompt sign-in again
  - hide forbidden actions
  - show retry option
  - explain expired invite
- Avoid showing raw backend error strings directly when not user-friendly.

**Deliverable**

- Error-to-UX mapping table for the app.

## Organization-Level

### 1. Contract Governance

**Goal**

Prevent cross-repo drift.

**Implementation**

- State in org docs that `role-node` owns the public contract.
- Require contract-impact notes in backend PRs.
- Require SDK/client follow-up work for public API changes.

**Deliverable**

- Governance note in org profile and contribution docs.

### 2. Release Coordination

**Goal**

Keep repos compatible in public releases.

**Implementation**

- Publish version compatibility tables.
- Coordinate releases when contract changes affect consumers.
- Do not release SDK updates without a tested backend version reference.

**Deliverable**

- One visible compatibility source per repo.

### 3. Naming and Branding

**Goal**

Keep repo messaging aligned.

**Implementation**

- Standardize how the org describes:
  - product app
  - node backend
  - serverpod backend
  - SDK
- Remove mixed legacy wording from READMEs and docs.

**Deliverable**

- Consistent naming across org profile and repositories.

### 4. Documentation Map

**Goal**

Help contributors know where to look.

**Implementation**

- In the org profile, define each repo in one sentence.
- Explicitly point to:
  - backend contract source: `role-node`
  - TypeScript package: `role-sdk`
  - application product: `role-client`

**Deliverable**

- Cleaner onboarding for contributors and users.

## Recommended Execution Order

### Phase 1: Stabilize role-node

- Build contract directory.
- Standardize routes.
- Standardize error model.
- Add compatibility/versioning docs.

### Phase 2: Align role-sdk

- Fix route and schema drift from backend contract.
- Rewrite README and package metadata.
- Add real `role-node` integration tests.
- Publish prerelease with documented compatibility.

### Phase 3: Align role-client

- Clean naming.
- Sync API client layer to backend contract.
- Add regression tests around auth, workspaces, and sync.

### Phase 4: Org cleanup

- Update org profile.
- Standardize descriptions.
- Publish coordinated compatibility guidance.

## Suggested PR Breakdown

To keep work manageable, prefer this PR order:

1. `role-node`: contract folder + route audit
2. `role-node`: error model + error docs
3. `role-node`: compatibility + versioning docs
4. `role-sdk`: contract sync fixes
5. `role-sdk`: package metadata + README + LICENSE
6. `role-sdk`: real backend integration tests
7. `role-client`: naming cleanup + contract alignment
8. `role-client`: remote API regression tests
9. org profile/docs alignment across repos
