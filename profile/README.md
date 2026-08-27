# Röle

<p align="center">
  <img src="https://raw.githubusercontent.com/role-suite/role-client/main/assets/image/app_logo.png" alt="Röle Logo" width="120" height="120">
</p>

<p align="center">
  <strong>Open-source API testing platform with local mode and an optional cloud backend</strong>
</p>

<p align="center">
  <a href="#overview">Overview</a> •
  <a href="#repositories">Repositories</a> •
  <a href="#getting-started">Getting Started</a>
</p>

---

## Overview

**Röle** is a modern, cross-platform API testing client built with Flutter. It supports both
local mode (store collections in plain files, sync with Git, no login required) and online mode
(connect to `role-node` for team-synced workspaces).

`role-node` is the API contract source of truth for `role-client`'s online mode.

## Repositories

| Repository | Description |
|------------|-------------|
| [role-client](https://github.com/role-suite/role-client) | Flutter desktop/mobile app — API testing client |
| [role-node](https://github.com/role-suite/role-node) | Node.js/Express/TypeScript backend for team-synced workspaces |

## Getting Started

### Frontend

```bash
git clone https://github.com/role-suite/role-client.git
cd role-client
flutter run
```

### Backend

```bash
git clone https://github.com/role-suite/role-node.git
cd role-node
npm install
npm run dev
```

## Community & Governance

For organization-wide contribution and community standards, see:

- [Security policy](https://github.com/role-suite/.github/blob/main/SECURITY.md)
- [Support policy](https://github.com/role-suite/.github/blob/main/SUPPORT.md)
- [Code of Conduct](https://github.com/role-suite/.github/blob/main/CODE_OF_CONDUCT.md)
- [Pull request template](https://github.com/role-suite/.github/blob/main/.github/PULL_REQUEST_TEMPLATE.md)

For responsible disclosure, use GitHub Security Advisories in the affected repository or the org
defaults repository:

- https://github.com/role-suite/.github/security/advisories/new

Issue and pull request templates are managed centrally from the
[`role-suite/.github`](https://github.com/role-suite/.github) repository.

---

<p align="center">Built with ❤️ for developers</p>
