# Changelog

Format: [Keep a Changelog](https://keepachangelog.com/en/1.1.0/).
Versioning: [SemVer](https://semver.org/spec/v2.0.0.html).

## [1.0.0] - 2026-03-15

WIT interface frozen at `labhit:extension@1.0.0`. No breaking changes since 0.1.0. The spec is stable — engine development continues independently.

### Notes

- WIT interface and JSON schema unchanged since 0.1.0
- Engine has reached v0.3.0 with full pipeline execution, extension marketplace, user authentication, usage metering, and CLI distribution
- Extension manifest format (TOML) unchanged
- Pipeline YAML vocabulary unchanged: `use` | `after` | `with` | `sandbox` | `gate` | `run`

## [0.1.0] - 2026-02-20

Initial public release of the LabHit specification.

### Added

- **WIT interface** (`labhit:extension@1.0.0`)
  - `pipeline-step` interface: `execute` function, input/output records, execution status enum
  - `host-capabilities` interface: `get-secret`, `log`, `read-file`, `write-file`
  - `labhit-extension` world combining imports and exports
- **gRPC service** (`labhit.v1.PipelineService`)
  - `TriggerPipeline`, `GetPipelineRun`, `StreamLogs` RPCs
  - Message types: `PipelineRunResponse`, `StageDetail`, `LogEntry`, status enums
- **JSON Schema** (`pipeline.schema.json`)
  - Pipeline metadata, triggers (push, PR, schedule, manual, webhook), stage definitions
  - Stage keywords: `use`, `after`, `run`, `with`, `sandbox`, `gate`, `timeout`, `retry`, `condition`
- **Extension manifest format** with example template
- **Example pipeline** (`examples/pipeline.yaml`)
- **CONTRIBUTING.md** with spec change process
