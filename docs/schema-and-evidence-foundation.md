# Schema and Evidence Foundation

## Purpose

Flesh out the schema and evidence gaps that must close before implementation: depot metadata, product definitions, dependency graph, deployment manifest, evidence bundle, and backout classification.

Gap coverage: G-005, G-019, G-020, G-021, and G-022 from [Top-to-Bottom Gap Assessment and Remediation Plan](top-to-bottom-gap-assessment.md).

Review pass: 2026-07-03.

## Design Rule

The first implementation slice should validate and render data. It should not submit JCL, alter security, drive z/OSMF workflows, or install vendor products.

That means these schemas are not secondary documentation. They are the first real product of the project.

## Proposed Metadata Namespaces

Future repository namespaces should be reserved as follows:

```text
schemas/
  product-definition.schema.yml
  depot-package.schema.yml
  service-level.schema.yml
  dependency-graph.schema.yml
  deployment-manifest.schema.yml
  evidence-manifest.schema.yml
  backout-classification.schema.yml
product_definitions/
  vendors/
  cics/
  ims/
  db2/
platform_definitions/
  smp_repository/
  maintenance/
  racf/
  clones/
evidence/
  samples/
```

These files should not be created as executable implementation yet. The first pass should be schema drafts and static examples.

## Depot Package Metadata

A depot package records immutable input received from IBM, BMC, Broadcom CA, Rocket, OSS mirrors, or site-authored artifacts.

Minimum shape:

```yaml
package_id: bmc-amiops-example-1.0.0-base
vendor: bmc
product_code: TBD
package_type: base_media | service_media | holdata | documentation | zosmf_workflow | generated_jcl | oss_mirror | site_artifact
version: TBD
retrieval_date: 2026-07-03
source:
  source_type: support_portal | public_docs | entitlement_download | site_archive | oss_repo | internal
  url_or_reference: TBD
  login_required: true
custody:
  immutable: true
  checksum_algorithm: sha256
  checksum: TBD
  stored_at: depot/smp/vendors/bmc/products/TBD/TBD/media/TBD
license:
  restricted: true
  notes: TBD
evidence:
  retrieval_log: TBD
  human_notes: TBD
```

Required validations:

- Restricted packages must not appear in public examples.
- Every package must have checksum metadata before use.
- Entitled packages must record login requirement without storing credentials.
- z/OSMF workflows must record workflow name, version, and variable file references.

## Service-Level Metadata

A service level is the state that deployment and clone manifests depend on.

Minimum shape:

```yaml
service_level_id: zos-sandbox-2026q3-example
scope: zos_base | ibm_middleware | vendor_product | oss_tooling | site_artifact
applies_to:
  vendor: ibm
  product_code: zos
  version: TBD
components:
  fmids:
    - TBD
  symods:
    - TBD
  packages:
    - package_id: TBD
promotion:
  status: proposed | sandbox_applied | integration_tested | accepted | held | retired
  accepted: false
dependencies:
  requires:
    - service_level_id: TBD
evidence:
  smpe_reports: []
  workflow_runs: []
  approvals: []
```

## Product Definition

A live product definition describes the intended product model independent of one LPAR.

Minimum shape:

```yaml
product:
  vendor: bmc | broadcom-ca | rocket | ibm | site | oss
  product_code: TBD
  product_name: TBD
  aliases: []
  version: TBD
  lifecycle_status: unknown_pending_discovery | legacy_manual_capture | managed
install:
  method: smpe_managed | zosmf_workflow_managed | vendor_jcl_managed | runtime_only | hybrid_zosmf_plus_runtime | hybrid_smpe_plus_runtime | legacy_manual_capture | unknown_pending_discovery
  fmids: []
  required_packages: []
  zosmf_workflows: []
  vendor_jcl_members: []
runtime:
  datasets: []
  uss_paths: []
  apf: []
  linklist: []
  lpa: []
  parmlib: []
  proclib: []
  started_tasks: []
  security: []
  cics_hooks: []
  ims_hooks: []
  db2_hooks: []
verification:
  probes: []
backout:
  class: reversible | compensating | restore_required | manual_recovery | not_applicable | unknown
evidence:
  expected_artifacts: []
ownership:
  product_owner: TBD
  technical_owner: TBD
```

Required validations:

- `unknown_pending_discovery` cannot be applied.
- `legacy_manual_capture` can only run read-only discovery.
- Any runtime hook into CICS, IMS, Db2, or security must declare an owner and verification probe.
- Any apply-capable product must declare a backout class.

## Dependency Graph

The dependency graph can start as YAML. It does not need a graph database for the pilot.

Minimum shape:

```yaml
graph_id: pilot-001-dependencies
nodes:
  - id: zos.base
    type: platform_service_level
  - id: bmc.product.TBD
    type: vendor_product
edges:
  - from: bmc.product.TBD
    to: zos.base
    relationship: requires
    phase: install_time
  - from: bmc.product.TBD
    to: db2.subsystem.TBD
    relationship: customizes
    phase: runtime
```

Relationship vocabulary:

- `requires`
- `provides`
- `customizes`
- `binds_to`
- `starts_after`
- `verifies_against`
- `exports_evidence_to`
- `must_recover_before`

Phase vocabulary:

- `intake`
- `install_time`
- `customization_time`
- `runtime`
- `verification`
- `recovery`

## Deployment Manifest

The deployment manifest is the reviewed plan for one environment run.

Minimum shape:

```yaml
manifest_id: pilot-001-run-0001
status: planned
git_commit: TBD
target_environment: TBD
scope:
  clone_manifest: TBD
  product_definitions: []
  platform_definitions: []
ordered_units:
  - unit_id: preflight
    phase: check
    owner_role: platform
    apply_capable: false
    evidence_expected:
      - preflight-report.yml
  - unit_id: bmc.product.TBD.install
    phase: apply
    owner_role: product
    apply_capable: true
    approval_gates:
      - smpe_apply
    backout_class: TBD
```

Required validations:

- No apply-capable unit without an approval gate where one is required.
- No apply-capable unit without backout class.
- No unit depending on an unknown product definition.
- No security change without security owner evidence.

## Evidence Manifest

The evidence manifest is the index for everything produced by a run.

Minimum shape:

```yaml
evidence_id: pilot-001-run-0001
manifest_id: pilot-001-run-0001
created_at: TBD
redaction_level: none | credentials_removed | restricted
artifacts:
  - artifact_id: rendered-jcl-001
    type: rendered_jcl
    path: evidence/pilot-001/run-0001/rendered/TBD.jcl
    checksum: TBD
  - artifact_id: jes-output-001
    type: jes_output
    job_id: TBD
    return_code: TBD
    path: evidence/pilot-001/run-0001/jes/TBD.txt
  - artifact_id: zosmf-workflow-001
    type: zosmf_workflow_output
    workflow_key: TBD
    status: TBD
approvals:
  - gate_id: TBD
    owner_role: TBD
    approved_at: TBD
```

Evidence types:

- `input_manifest`
- `rendered_jcl`
- `rendered_sql`
- `rendered_racf_commands`
- `zosmf_workflow_definition`
- `zosmf_workflow_output`
- `jes_output`
- `smpe_report`
- `operator_command_output`
- `catalog_snapshot`
- `security_snapshot`
- `dataset_snapshot`
- `uss_snapshot`
- `cics_snapshot`
- `ims_snapshot`
- `db2_snapshot`
- `clone_manifest`
- `approval_record`
- `human_note`

## Backout Classification

Backout classes:

| Class | Meaning | Required evidence |
| --- | --- | --- |
| `reversible` | Automation can restore prior state directly | Before snapshot, rendered backout, after verification |
| `compensating` | A different action can neutralize the change | Compensating procedure, owner approval, verification |
| `restore_required` | Recovery requires restoring datasets, members, volumes, or prior service level | Backup reference, restore procedure, validation probe |
| `manual_recovery` | Recovery cannot be safely automated yet | Manual runbook, owner, escalation path |
| `not_applicable` | Read-only or evidence-only unit | Proof that no state is changed |
| `unknown` | Not allowed for apply-capable units | Must be resolved before Apply |

Hard rule: `unknown` is acceptable during discovery and planning, but it blocks Apply.

## Immediate Actions

1. Draft schema files as documentation-first YAML schemas.
2. Create sample product definitions for one BMC candidate and one generic runtime-only product.
3. Create a sample dependency graph for the pilot shape.
4. Create a sample evidence manifest using placeholder artifacts.
5. Update the gap register when each schema has an example and validation rule.
