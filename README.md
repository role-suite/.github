# role-suite org defaults

This repository centralizes organization-wide community and documentation
defaults for repositories in `role-suite`.

## What this repo manages

- Issue templates: `.github/ISSUE_TEMPLATE/*`
- Pull request template: `.github/PULL_REQUEST_TEMPLATE.md`
- Security policy: `SECURITY.md`
- Support policy: `SUPPORT.md`
- Code of conduct: `CODE_OF_CONDUCT.md`
- Organization profile page: `profile/README.md`

## How defaults are applied

GitHub uses this special `.github` repository as the source of default community
health files for public repositories in the organization, unless a target
repository defines its own local override.

## Scope

Current primary repositories:

- `role-client`
- `role-node`
- `role-serverpod`
- `role-sdk`
