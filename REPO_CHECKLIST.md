# Role Suite Repo Checklist

## role-node

### 1. Contract Ownership

- [ ] Make `role-node` the canonical API contract source.
- [ ] Add a dedicated `contracts/` or `openapi/` directory.
- [ ] Define every public route, request body, response body, and error shape there.
- [ ] Ensure docs, tests, and SDK mappings derive from this source.

### 2. API Versioning

- [ ] Introduce explicit API versioning such as `/api/v1/...`, or publish a strict version policy if path versioning is deferred.
- [ ] Document what counts as breaking, additive, and patch-level API changes.
- [ ] Add deprecation rules for routes and fields.

### 3. Error Model

- [ ] Replace weak error responses with a stable machine-readable structure.
- [ ] Standardize fields like `code`, `message`, `details`, `requestId`.
- [ ] Document all public error codes per module.
- [ ] Ensure the same error structure is returned everywhere.

### 4. Route Consistency

- [ ] Audit all route names against docs and SDK.
- [ ] Fix drift such as `export` vs `exports` and `import` vs `imports`.
- [ ] Lock route naming conventions and avoid later churn.

### 5. Response Consistency

- [ ] Standardize success envelopes across all modules.
- [ ] Standardize pagination/cursor response shapes.
- [ ] Standardize `deleted`, `left`, `revoked` style success payloads.

### 6. Schema Validation

- [ ] Ensure request and response schemas are explicit and reusable.
- [ ] Export validation schemas or derived contract artifacts for SDK consumption.
- [ ] Add CI checks that detect undocumented route or schema changes.

### 7. Request Correlation

- [ ] Add `x-request-id` support.
- [ ] Return `requestId` in error payloads.
- [ ] Log request ids consistently server-side.

### 8. Compatibility Policy

- [ ] Publish a compatibility matrix for `role-sdk` and `role-client`.
- [ ] State minimum supported versions by release.
- [ ] Add migration notes for contract changes.

### 9. Consumer Docs

- [ ] Upgrade `docs/guides/client-integration.md` into a formal consumer contract guide.
- [ ] Include auth lifecycle, pagination, retries, rate/size limits, and error behavior.
- [ ] Keep examples synced with real tests.

### 10. CI and Release

- [ ] Add contract snapshot tests.
- [ ] Add an API breaking-change check in CI.
- [ ] Ensure releases update changelog and compatibility notes.
- [ ] Keep release process as strict as the SDK depends on it.

### 11. Package/Repo Hygiene

- [ ] Improve `package.json` description and keywords.
- [ ] Clarify whether `pnpm-workspace.yaml` is needed for a single-package repo.
- [ ] Keep repo naming/docs strictly aligned with `role-node`.

## role-sdk

### 1. Product Positioning

- [ ] Make `role-sdk` explicitly `role-node`-first for v1.
- [ ] Mark `serverpod` support as experimental until parity is real and tested.
- [ ] Avoid equal-weight messaging if one backend is not equally mature.

### 2. Contract-Driven Development

- [ ] Stop manually owning route details independently from backend contract.
- [ ] Pull route/schema truth from `role-node` contract artifacts.
- [ ] Add CI that fails when backend contract changes are not reflected in the SDK.

### 3. Package Readiness

- [ ] Add missing `LICENSE`.
- [ ] Add `repository`, `bugs`, `homepage`, `keywords`, `author` metadata.
- [ ] Replace `0.0.0` with real prerelease or release versioning.
- [ ] Confirm `exports`, `types`, and published files are complete and intentional.

### 4. README Overhaul

- [ ] Replace scaffold-oriented README with a consumer-facing README.
- [ ] Add installation instructions.
- [ ] Add quick start examples.
- [ ] Add auth/token refresh examples.
- [ ] Add workspace-scoped examples.
- [ ] Add error handling examples.
- [ ] Add compatibility table with `role-node`.

### 5. Public API Stabilization

- [ ] Freeze naming conventions before `1.0.0`.
- [ ] Audit all input/output names for ergonomics and consistency.
- [ ] Remove backend leakage from public types where possible.
- [ ] Keep capability flags additive and semver-safe.

### 6. Error Handling

- [ ] Ensure all thrown SDK errors have stable `code`.
- [ ] Preserve `status`, `requestId`, and safe diagnostics.
- [ ] Document retryable vs non-retryable failures.

### 7. Testing Strategy

- [ ] Keep unit and mocked integration tests.
- [ ] Add real integration tests against a running `role-node`.
- [ ] Add contract parity tests against backend fixtures or generated samples.
- [ ] Add regression tests for every route drift issue found.

### 8. Release Process

- [ ] Confirm semantic-release is configured for npm publishing and changelog generation.
- [ ] Add prerelease strategy if SDK is still beta.
- [ ] Publish release notes with backend compatibility info.
- [ ] Only release when tested against a known `role-node` version.

### 9. Distribution Quality

- [ ] Verify generated `dist/` output is clean ESM.
- [ ] Decide whether CJS support is needed; if not, document ESM-only clearly.
- [ ] Document supported runtimes: Node, browser, edge.
- [ ] Add bundle-size awareness if browser use is important.

### 10. Examples and Developer Experience

- [ ] Add `examples/` directory.
- [ ] Include Node example.
- [ ] Include browser/fetch example.
- [ ] Include framework example such as Next.js.
- [ ] Show token store customization.
- [ ] Show hook instrumentation usage.

### 11. Scope Control

- [ ] Keep package single-package for now.
- [ ] Do not split into `sdk-core` until there is a real need.
- [ ] Prefer fewer abstractions until `role-node` support is stable.

## role-client

### 1. Architecture Clarity

- [ ] Position `role-client` clearly as the Flutter product app, not a contract owner.
- [ ] Keep backend contract definitions outside the app repo.
- [ ] Treat app networking as a consumer layer only.

### 2. Naming Cleanup

- [ ] Remove naming drift between `relay`, `role-server`, `role-node`, and `role-serverpod`.
- [ ] Choose one consistent naming scheme for remote backend integrations.
- [ ] Update docs and folder names to match that decision.

### 3. Backend Integration Layer

- [ ] Audit `lib/core/services/role_node_api/*` against current `role-node` contract.
- [ ] Remove route drift and duplicated assumptions.
- [ ] Centralize endpoint naming and payload mapping to reduce inconsistencies.

### 4. Contract Sync

- [ ] Add a process to validate Flutter API clients against backend contract changes.
- [ ] Use generated models/spec snapshots if possible.
- [ ] Add tests for critical auth/workspace/collections flows.

### 5. Supported Backend Story

- [ ] Clarify whether the client supports both `role-node` and `role-serverpod` equally.
- [ ] If not equal, document primary and secondary backend support explicitly.
- [ ] Remove outdated references like `role-server` if that is no longer the product term.

### 6. Auth Handling

- [ ] Align token refresh, logout, and session restoration behavior with `role-node`.
- [ ] Ensure app-side error handling can consume stable backend error codes.
- [ ] Avoid parsing only free-form messages.

### 7. Offline vs API Mode

- [ ] Keep local mode and API mode boundaries explicit.
- [ ] Document sync rules and conflict rules more formally.
- [ ] Ensure remote mode maps cleanly to backend contract terminology.

### 8. Docs

- [ ] Update architecture docs to reflect actual current backend integration approach.
- [ ] Add a compatibility section for supported backend versions.
- [ ] Document how API mode differs by backend, if applicable.

### 9. Testing

- [ ] Add tests for remote API clients against mocked contract fixtures.
- [ ] Add high-value integration tests for login, workspace switching, collections sync.
- [ ] Add regression tests for route and payload mismatches.

### 10. UX and Operational Behavior

- [ ] Standardize user-facing messaging for `401`, `403`, `409`, `410`, `422`, `502`.
- [ ] Tie UI states to backend error codes instead of brittle message parsing.
- [ ] Ensure loading/retry/session-expired flows are consistent.

## Organization-Level

### 1. Contract Governance

- [ ] Declare `role-node` as the contract owner.
- [ ] Require SDK and client updates whenever contract changes.
- [ ] Add a lightweight RFC/change-note process for breaking API changes.

### 2. Release Coordination

- [ ] Publish version compatibility across repos.
- [ ] Coordinate releases when backend changes affect SDK or client.
- [ ] Keep changelogs meaningful across all three repos.

### 3. Naming and Branding

- [ ] Standardize product/backend naming across org profile and repo READMEs.
- [ ] Remove legacy/internal names where possible.
- [ ] Keep repo descriptions aligned.

### 4. Documentation Map

- [ ] Org profile should explain repo responsibilities in one sentence each.
- [ ] Point developers to the right source.
- [ ] Backend contract: `role-node`.
- [ ] JS/TS consumer package: `role-sdk`.
- [ ] End-user app: `role-client`.

## Suggested Priority Order

### 1. role-node

- [ ] Lock routes.
- [ ] Lock error model.
- [ ] Lock versioning and compatibility.

### 2. role-sdk

- [ ] Fix contract drift.
- [ ] Add package readiness items.
- [ ] Add real integration tests.
- [ ] Rewrite README.

### 3. role-client

- [ ] Align naming and contract usage.
- [ ] Add regression tests for backend sync.
