---
name: Register and version a model in Verta ModelDB
description: Create a registered model and a model version in the Verta ModelDB model registry, then fetch it back for deployment.
api: openapi/verta-RegistryService.swagger.json
operations:
  - RegistryService_CreateRegisteredModel
  - RegistryService_CreateModelVersion
  - RegistryService_GetModelVersion
---

# Register and version a model in Verta ModelDB

Use this skill to promote a trained model into the Verta ModelDB model registry.

## Preconditions
- A reachable ModelDB / Verta host via `verta.Client(host)`.
- REST base path `/v1`, JSON bodies.

## Steps
1. **Create a registered model** — `RegistryService_CreateRegisteredModel`. Supply a `name` (and
   optional `workspace`); capture the registered model `id`.
2. **Create a model version** — `RegistryService_CreateModelVersion` under that `registered_model_id`.
   Attach the model artifact and version metadata.
3. **Verify** — `RegistryService_GetModelVersion` to read the version back before wiring it into an
   endpoint / stage transition.

## Conventions & errors
- Authorization is enforced by the UAC action/resource model (RegistryService actions); a
  `PERMISSION_DENIED` (gRPC 7 -> HTTP 403) means the caller lacks the required action.
- Error envelope and pagination conventions: see `errors/verta-problem-types.yml` and
  `conventions/verta-conventions.yml`.
