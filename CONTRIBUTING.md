# Contributing

This repo defines the public contracts that extension developers and tooling authors build against. Changes here affect the whole ecosystem, so the bar for merging is higher than a typical code repo.

## Scope

**In scope:** WIT interface changes, protobuf/gRPC additions, JSON Schema updates, manifest format changes, example pipelines.

**Out of scope:** Engine internals, infrastructure config, frontend code, business docs.

## Process

### 1. File an issue

Before writing a PR, open an issue with:
- What you want to change and why
- Which spec files are affected (WIT, proto, schema, manifests)
- Whether it's backward-compatible

Tag it `spec-proposal`.

### 2. Get a sign-off

A maintainer will triage and label the issue. Spec changes that affect the extension contract or gRPC API typically need discussion before implementation.

### 3. Open a PR

- Branch from `main` (e.g. `spec/add-cache-capability`)
- Keep all spec files in sync -- a WIT type change means corresponding updates to proto and schema
- Add a CHANGELOG entry under `[Unreleased]`
- Reference the issue number

### Requirements

- Spec files must stay consistent with each other
- Breaking changes must be called out explicitly in the PR description
- One logical change per PR
- Include a CHANGELOG entry

## Compatibility

We follow [SemVer](https://semver.org/):

- **Patch** (0.1.x): Docs, clarifications, non-functional fixes
- **Minor** (0.x.0): Additive changes -- new optional fields, new capabilities
- **Major** (x.0.0): Breaking changes to existing interfaces

While on `0.x`, minor versions may include breaking changes. These will be documented in the CHANGELOG with migration notes.

## License

Contributions are licensed under [Apache 2.0](LICENSE).
