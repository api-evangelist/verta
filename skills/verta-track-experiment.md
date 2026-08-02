---
name: Track an ML experiment in Verta ModelDB
description: Create a project and experiment, start an experiment run, and log hyperparameters, metrics, and artifacts for reproducible model tracking.
api: openapi/verta-ProjectService.swagger.json
operations:
  - ProjectService_createProject
  - ExperimentService_createExperiment
  - ExperimentRunService_createExperimentRun
  - ExperimentRunService_logHyperparameter
  - ExperimentRunService_logMetric
  - ExperimentRunService_logArtifact
---

# Track an ML experiment in Verta ModelDB

Use this skill to record a reproducible machine-learning experiment in Verta ModelDB.

## Preconditions
- A reachable ModelDB / Verta host. Connect with `verta.Client(host)` (add email + developer key
  credentials on the hosted platform; local OSS ModelDB may run unauthenticated).
- REST base path is `/v1`; all bodies are `application/json` (snake_case fields).

## Steps
1. **Create a project** — `ProjectService_createProject` (`POST /v1/project/createProject`). Supply a
   unique `name`; capture the returned project `id`.
2. **Create an experiment** — `ExperimentService_createExperiment`. Pass the `project_id` from step 1.
3. **Start a run** — `ExperimentRunService_createExperimentRun` with the `project_id` and
   `experiment_id`. Capture the experiment run `id`.
4. **Log hyperparameters** — `ExperimentRunService_logHyperparameter` for each config value, keyed by
   the run `id`.
5. **Log metrics** — `ExperimentRunService_logMetric` for accuracy/loss/etc.
6. **Log artifacts** — `ExperimentRunService_logArtifact` to attach model files or plots.

## Conventions & errors
- List/find calls paginate via `page_number` / `page_limit` and return `total_records`
  (see `conventions/verta-conventions.yml`).
- Errors use the grpc-gateway `runtimeError` envelope (`error`, `code`, `message`, `details`) — the
  `code` field is a gRPC status; see `errors/verta-problem-types.yml`.
