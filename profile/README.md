# Röle

<p align="center">
  <img src="https://raw.githubusercontent.com/role-suite/role-client/main/assets/image/app_logo.png" alt="Röle Logo" width="120" height="120">
</p>

<p align="center">
  <strong>Open-source API testing platform with local mode, optional cloud backends, and an official SDK</strong>
</p>

<p align="center">
  <a href="#overview">Overview</a> •
  <a href="#repositories">Repositories</a> •
  <a href="#backend-options">Backend Options</a> •
  <a href="#getting-started">Getting Started</a>
</p>

---

## Overview

**Röle** is a modern, cross-platform API testing client built with Flutter. It supports both local mode (store collections locally) and cloud mode (connect to a backend for team collaboration).

To support integrations and automation, the organization also maintains an official TypeScript SDK.

For contract governance, `role-node` is the primary API contract source for REST and gRPC integrations.

## Repositories

| Repository | Description |
|------------|-------------|
| [role-client](https://github.com/role-suite/role-client) | Flutter desktop/mobile app - API testing client |
| [role-node](https://github.com/role-suite/role-node) | Primary Node.js/Express backend (TypeScript, REST + gRPC) |
| [role-serverpod](https://github.com/role-suite/role-serverpod) | Alternative Dart/Serverpod backend (typed RPC) |
| [role-sdk](https://github.com/role-suite/role-sdk) | Official TypeScript SDK (primary support: `role-node`) |

## Backend Options

Röle can connect to either backend depending on your stack and deployment goals:

### role-node (Express + TypeScript)

- **Stack**: Node.js, Express 5, TypeScript, PostgreSQL, gRPC
- **API Style**: REST + gRPC
- **Best for**: Teams familiar with Node.js/Express that need REST compatibility and gRPC support

### role-serverpod (Dart + Serverpod)

- **Stack**: Dart, Serverpod, PostgreSQL
- **API Style**: Typed RPC (no REST wrapper)
- **Best for**: Type-safe full-stack Dart projects in the Serverpod ecosystem

### Feature Comparison

| Feature | role-node | role-serverpod |
|---------|-----------|----------------|
| Auth | JWT + refresh tokens | JWT + refresh tokens |
| Workspaces | ✓ | ✓ |
| Collections | ✓ | ✓ |
| Environments | ✓ | ✓ |
| Request Runs | ✓ | ✓ |
| Import/Export | ✓ | ✓ |
| API Style | REST + gRPC | Typed RPC |
| Stack | Express + TypeScript | Serverpod + Dart |

## Getting Started

### Frontend

```bash
git clone https://github.com/role-suite/role-client.git
cd role-client
flutter run
```

### Backend (choose one)

**Node.js backend:**
```bash
git clone https://github.com/role-suite/role-node.git
cd role-node
npm install
npm run dev
```

**Serverpod backend:**
```bash
git clone https://github.com/role-suite/role-serverpod.git
cd role-serverpod/relay_server_server
dart pub get
dart bin/main.dart
```

### SDK (TypeScript)

```bash
git clone https://github.com/role-suite/role-sdk.git
cd role-sdk
npm install
```

For package usage and integration examples, see the `role-sdk` README.

## Community & Governance

For organization-wide contribution and community standards, see:

- [Security policy](https://github.com/role-suite/.github/blob/main/SECURITY.md)
- [Support policy](https://github.com/role-suite/.github/blob/main/SUPPORT.md)
- [Code of Conduct](https://github.com/role-suite/.github/blob/main/CODE_OF_CONDUCT.md)
- [Pull request template](https://github.com/role-suite/.github/blob/main/.github/PULL_REQUEST_TEMPLATE.md)

For responsible disclosure, use GitHub Security Advisories in the affected repository or the org defaults repository:

- https://github.com/role-suite/.github/security/advisories/new

Issue and pull request templates are managed centrally from the [`role-suite/.github`](https://github.com/role-suite/.github) repository.

---

<p align="center">Built with ❤️ for developers</p>
