---
name: migration-planner
description: "Coordinates the end-to-end discovery, translation, and execution of cloud migrations."
version: 1.0.0
author: Hermes Agent
tags: [migration, cloud, aws, gcp, manifest]
---

# Migration Planner

This skill provides a structured workflow for migrating infrastructure between cloud providers. It relies on a **Migration Manifest** as the central source of truth.

## Workflow Steps

1. **Discovery Phase**
   - Audit the source environment.
   - Export all resources to `manifests/<project>/discovery.yaml`.
   - Identify hard dependencies (e.g., DNS, VPC peering).

2. **Translation Phase**
   - Map each source resource to a target provider equivalent.
   - Document the translation in `manifests/<project>/mapping.yaml`.
   - Define the migration strategy (e.g., 'rehost', 'replatform', 'refactor').

3. **Execution Phase**
   - Provision target resources based on the mapping.
   - Perform data transport (backup/restore).
   - Update configuration/DNS for cutover.

4. **Verification Phase**
   - Run the validation suite defined in the manifest.
   - Perform a smoke test of the application.
   - Sign off on the migration.

## Manifest-Driven State Tracking

Every migration must maintain a manifest. Use the template in `/templates/base-manifest.yaml`.
- **Audited**: Resource is identified and documented.
- **Translated**: Target equivalent is chosen and validated.
- **Migrated**: Resource exists in the target environment.
- **Verified**: Post-migration tests passed.

## Pitfalls
- **Hidden Dependencies**: Always check for undocumented API keys or internal DNS entries.
- **Data Gravity**: Large databases should be migrated via snapshot/transport tools, not API calls.
- **Downtime Window**: Ensure the cutover plan fits within the acceptable downtime.
