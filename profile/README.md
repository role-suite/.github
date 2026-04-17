# Röle

<p align="center">
  <img src="https://raw.githubusercontent.com/role-suite/role-client/main/assets/image/app_logo.png" alt="Röle Logo" width="120" height="120">
</p>

<p align="center">
  <strong>Open-source API testing client with optional backend</strong>
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

## Repositories

| Repository | Description |
|------------|-------------|
| [role-client](https://github.com/role-suite/role-client) | Flutter desktop/mobile app - API testing client |
| [role-node](https://github.com/role-suite/role-node) | Node.js/Express backend (TypeScript) |
| [role-serverpod](https://github.com/role-suite/role-serverpod) | Dart/Serverpod backend (native RPC) |

## Backend Options

You can use Röle with either backend:

### role-node (Express + TypeScript)

- **Stack**: Node.js, Express 5, TypeScript, Prisma, PostgreSQL
- **API Style**: REST
- **Best for**: Teams familiar with Node.js/Express, need REST API compatibility

### role-serverpod (Dart + Serverpod)

- **Stack**: Dart, Serverpod, PostgreSQL
- **API Style**: Typed RPC (no REST wrapper)
- **Best for**: Type-safe full-stack Dart projects, Serverpod ecosystem

### Feature Comparison

| Feature | role-node | role-serverpod |
|---------|-----------|----------------|
| Auth | JWT + refresh tokens | JWT + refresh tokens |
| Workspaces | ✓ | ✓ |
| Collections | ✓ | ✓ |
| Environments | ✓ | ✓ |
| Request Runs | ✓ | ✓ |
| Import/Export | ✓ | ✓ |
| API Style | REST | Typed RPC |
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

## Links

- [Website](https://role-client.demo.epistola.app) *(coming soon)*
- [Discord](https://discord.gg/role) *(coming soon)*

---

<p align="center">Built with ❤️ for API developers</p>
