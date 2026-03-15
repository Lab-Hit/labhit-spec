# LabHit Specification

Interface contracts for the [LabHit](https://labhit.dev) CI/CD platform.

LabHit is a modular CI/CD engine. The core runs pipelines; all actual work (git clone, container build, deploy, scan, notify) happens in sandboxed WASM extensions. This repo contains the public specs that extensions and tools are built against -- not the engine itself.

## Repository layout

```
wit/                  WIT interface definitions (core extension contract)
proto/                gRPC/Protobuf service definitions
schema/               JSON Schema for pipeline YAML validation
extensions/           Extension manifest format and example
examples/             Example pipeline definitions
```

## Specs

| Spec | File | What it defines |
|------|------|-----------------|
| Extension contract | [`wit/labhit-extension.wit`](wit/labhit-extension.wit) | WIT interface every extension must implement |
| Pipeline API | [`proto/labhit/v1/pipeline.proto`](proto/labhit/v1/pipeline.proto) | gRPC service: trigger, query, and stream pipeline runs |
| Pipeline schema | [`schema/pipeline.schema.json`](schema/pipeline.schema.json) | Validates `.labhit/pipeline.yaml` files |
| Manifests | [`extensions/`](extensions/) | TOML format declaring extension capabilities and I/O |
| Example | [`examples/pipeline.yaml`](examples/pipeline.yaml) | Working pipeline using all major features |

## Pipeline format

Pipelines live in `.labhit/pipeline.yaml`. The vocabulary is intentionally small:

```yaml
engine: "1"

pipeline:
  name: my-pipeline

stages:
  fetch:
    use: source/git              # invoke an extension by category/name
    with:
      depth: 1                   # pass config to the extension

  build:
    after: [fetch]               # DAG edge: build waits for fetch
    use: build/container
    sandbox:
      image: node:20-slim
      resources:
        cpu: 2
        memory: 4Gi

  deploy:
    after: [build]
    use: deploy/kubernetes
    gate:                        # policy gate evaluated before execution
      approval: required
      policy: production-deploy
```

Six keywords: `use` | `after` | `with` | `sandbox` | `gate` | `run`

Full example: [`examples/pipeline.yaml`](examples/pipeline.yaml)

## Writing an extension

An extension is a WASM module that exports the `execute` function from the [`pipeline-step`](wit/labhit-extension.wit) WIT interface, plus a TOML manifest declaring its capabilities. See [`extensions/`](extensions/) for the manifest format and an example.

Build steps:

1. Implement `execute` against the WIT contract
2. Write an `extension.toml` manifest (capabilities, resource limits, I/O schema)
3. Compile to `wasm32-wasip1`

The engine provides host imports for secrets, logging, and scoped filesystem access. The WIT file is the authoritative reference.

## gRPC API

Three RPCs on `labhit.v1.PipelineService`:

| RPC | Purpose |
|-----|---------|
| `TriggerPipeline` | Start a pipeline run from a git ref |
| `GetPipelineRun` | Poll run status and per-stage breakdown |
| `StreamLogs` | Server-stream log entries, optionally filtered by stage |

Full definitions: [`proto/labhit/v1/pipeline.proto`](proto/labhit/v1/pipeline.proto)

## Versioning

Follows [SemVer](https://semver.org/). While on `0.x`, minor bumps may break compatibility. All breaking changes are listed in the [CHANGELOG](CHANGELOG.md).

Current spec version: **1.0.0** (WIT interface frozen)
Engine version: **0.3.0**

## Links

| Resource | Status |
|----------|--------|
| Engine source | Pre-launch (Apache 2.0, will be published separately) |
| Extension marketplace | [app.labhit.dev/marketplace](https://app.labhit.dev/marketplace) |
| CLI install | `curl -fsSL https://api.labhit.dev/install \| sh` |
| [labhit.dev](https://labhit.dev) | Live |
| [app.labhit.dev](https://app.labhit.dev) | Live (sign in with GitHub) |

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md).

## License

[Apache License 2.0](LICENSE)
