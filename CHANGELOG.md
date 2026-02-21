# Changelog

Format: [Keep a Changelog](https://keepachangelog.com/en/1.1.0/).
Versioning: [SemVer](https://semver.org/spec/v2.0.0.html).

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
