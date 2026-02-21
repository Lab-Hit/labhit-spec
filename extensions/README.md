# Extension Manifests

Every extension ships with a TOML manifest. The engine reads it before loading the WASM module to enforce capability restrictions and resource limits.

## Format

```toml
[extension]
name = "my-extension"          # lowercase, alphanumeric, dashes
category = "build"             # source | build | test | scan | deploy | notify | gate
version = "0.1.0"
description = "What it does"
author = "Your Name"
license = "Apache-2.0"

[extension.compatibility]
engine_version = ">=0.1.0"

[extension.capabilities]
network = ["registry.example.com"]   # allowed destinations (glob patterns)
filesystem = { workspace = "read" }  # "read", "write", or "none"
secrets = ["my_api_key"]             # secret names the extension can request

[extension.resources]
max_memory_mb = 512            # WASM linear memory ceiling
max_cpu_ms = 60000             # CPU time budget in milliseconds
max_execution_time_s = 300     # wall-clock timeout in seconds

[extension.inputs]
# maps to the pipeline `with:` block
my_param = { type = "string", default = "value", description = "..." }

[extension.outputs]
# exposed to downstream stages via ${{ stage.<name>.output.<key> }}
my_output = { type = "string", description = "..." }
```

## Categories

| Category | Purpose | Examples |
|----------|---------|----------|
| `source` | Fetch code or data | `source/git`, `source/s3` |
| `build` | Compile, bundle, package | `build/container`, `build/npm` |
| `test` | Run tests | `test/jest`, `test/pytest` |
| `scan` | Security and quality scanning | `scan/trivy`, `scan/semgrep` |
| `deploy` | Push to target environments | `deploy/kubernetes`, `deploy/lambda` |
| `notify` | Send notifications | `notify/slack`, `notify/email` |
| `gate` | Policy and approval gates | `gate/jira-ticket`, `gate/change-window` |

## Capabilities

Extensions run inside a WASM sandbox. Every capability must be declared upfront; undeclared capabilities are denied at runtime.

**Network** -- List of allowed host patterns. Prefer specific domains over wildcards. The engine may further restrict access via policy rules regardless of what the manifest declares.

**Filesystem** -- Access level to the pipeline workspace directory. Extensions with `"none"` cannot read or write any files.

**Secrets** -- Named secrets that the extension may request at runtime. Secrets are resolved via workload identity attestation and never persisted by the engine.

## Resource limits

The engine enforces three resource bounds per extension:

| Field | Controls | Why it matters |
|-------|----------|----------------|
| `max_memory_mb` | WASM linear memory | Prevents a single extension from exhausting host RAM |
| `max_cpu_ms` | Total CPU time | Kills infinite loops and runaway computation |
| `max_execution_time_s` | Wall-clock timeout | Kills extensions stuck on network I/O or deadlocks |

## Inputs and outputs

**Inputs** are typed parameters populated from the pipeline's `with:` block. Types: `string`, `integer`, `bool`, `list`, `map`.

**Outputs** are values the extension produces. Downstream stages reference them as `${{ stage.<name>.output.<key> }}`.

## Included files

| File | Description |
|------|-------------|
| `example.toml` | Template manifest demonstrating the format |
