# z/OS CICS, IMS, and BMC Product Deployment Through Ansible

## Purpose

Design the architecture, requirements, scope, and execution flows for deploying CICS regions, IMS regions, and multiple BMC Software products on z/OS from a Linux or macOS Ansible control node.

This document deliberately avoids implementation code. It defines the operating model and module boundaries that should be agreed before playbooks, roles, inventories, or JCL templates are written.

## Documentation Diagram Convention

All diagrams and flowcharts must be maintained as standalone `.mmd` Mermaid source files under [docs/diagrams](diagrams/). Markdown documents should link to those files instead of embedding Mermaid blocks inline.

## Design Principles

1. Ansible is the orchestrator, not the only execution engine.
2. z/OS-native mechanisms remain authoritative for z/OS-native work:
   - SMP/E for SMP/E-packaged software.
   - JCL for batch install, generation, and verification jobs.
   - Operator commands for started task and subsystem control.
   - CMCI for CICS resource definition and runtime management.
   - IMS commands, DBRC, DBD/PSB/ACB generation, and catalog operations for IMS.
3. Product deployment is metadata-driven. Product-specific values belong in inventory and product definition files, not hardcoded task logic.
4. Every flow must support plan, apply, verify, and recover phases.
5. Idempotency is required where z/OS gives reliable state APIs. Where it does not, automation must use probes, markers, CSI queries, product libraries, or job output assertions.
6. Separation of duties must be preserved: system programming, security, storage, middleware, and application responsibilities should map to different variable scopes, approvals, and run tags.

## Target Architecture

Diagram source: [docs/diagrams/target-architecture.mmd](diagrams/target-architecture.mmd)

## Control Node Requirements

The control node should be Linux for production automation. macOS is acceptable for development and validation if the same Ansible versions, Python version, collections, linting, and execution environment are pinned.

Required:

- Ansible Automation Platform or ansible-core with pinned versions.
- IBM z/OS core collection.
- IBM z/OS CICS collection.
- IBM z/OS IMS collection.
- Optional IBM z/OSMF collection if REST-based z/OSMF workflows are needed.
- SSH client with key or certificate-based authentication.
- Vault integration for credentials and product license material.
- Git repository for all automation, inventories, templates, and product metadata.
- CI runner capable of syntax checks, linting, template rendering tests, and offline validation.

Recommended:

- Ansible execution environment image for production consistency.
- Central artifact store for product media checksums, generated JCL, JES output, reports, and evidence bundles.
- Ansible Automation Platform controller for RBAC, surveys, approvals, scheduling, and audit trails.

## z/OS Managed Node Requirements

Required:

- z/OS UNIX System Services with SSH enabled.
- Ansible service ID with OMVS segment.
- IBM Open Enterprise SDK for Python installed on z/OS.
- IBM Z Open Automation Utilities installed and configured.
- Correct environment variables for ZOAU, Python, PATH, LIBPATH, codepage handling, and tagging.
- RACF, ACF2, or Top Secret permissions for:
  - Data set allocation and update.
  - USS file creation.
  - JCL submission.
  - JES output read.
  - Operator commands as required.
  - Started task management as required.
  - APF, PROGxx, PARMLIB, LINKLIST, LPA, and security definitions where in scope.
- Storage classes, SMS rules, volume policies, HLQ standards, and dataset naming standards.
- JES classes, job accounting fields, and installation exits understood before automation design is finalized.

For CICS:

- CICS TS installed and licensed.
- CICS product libraries known.
- Region HLQ standards defined.
- CSD, catalog, temporary storage, trace, dump, TD, and JCL standards defined.
- CMCI endpoint available if resource definitions or runtime actions will use CICSPlex SM / CMCI.

For IMS:

- IMS installed and licensed.
- IMS RESLIB, PROCLIB, DBDLIB, PSBLIB, ACBLIB, FORMAT, MFS, RECON, and catalog conventions documented.
- DBRC and RECON security agreed.
- IMS command route and permissions agreed.

For BMC products:

- Product list, versions, FMIDs, maintenance level, dependencies, and license requirements documented.
- Product media acquisition path agreed.
- Internal BMC software depot strategy agreed.
- SMP/E CSI strategy agreed: shared CSI, product-family CSI, or product-specific CSI.
- Target and distribution zone naming standards agreed.
- Runtime library, APF, LINKLIST, LPA, STEPLIB, PARMLIB, PROCLIB, started task, and security requirements documented per product.

## Repository Architecture

The automation repository should use a layered structure:

```text
ansible.cfg
collections/requirements.yml
inventories/
  dev/
  test/
  prod/
group_vars/
  all/
  zos_sysplex/
  cics/
  ims/
  bmc/
host_vars/
roles/
  zos_common/
  zos_storage/
  zos_security_model/
  zos_jcl/
  zos_smpe/
  cics_region/
  cics_cmci_resources/
  ims_region/
  ims_database_artifacts/
  bmc_product/
  bmc_runtime_config/
  verification/
  evidence/
playbooks/
  preflight.yml
  deploy_cics_region.yml
  deploy_ims_region.yml
  deploy_bmc_product.yml
  deploy_environment.yml
  verify_environment.yml
  rollback_or_recover.yml
product_definitions/
  bmc/
  cics/
  ims/
templates/
  jcl/
  proclib/
  parmlib/
  security/
tests/
docs/
```

## Data Model

### Environment Model

Each environment should define:

- Sysplex and LPAR names.
- z/OS hostname, SSH port, and Ansible connection user.
- JES classes, MSGCLASS, accounting, notify, region size, and job card policy.
- HLQ roots for runtime, install, SMP/E, temporary, backup, and evidence datasets.
- Storage classes and volume preferences.
- USS roots for staging and logs.
- Security backend and profile naming conventions.
- Maintenance windows and restart policy.

### CICS Region Model

Each CICS region should be described declaratively:

- Region name, APPLID, job name, region type, and environment.
- CICS release and product library HLQ.
- Region dataset HLQ.
- CSD dataset and groups.
- Global catalog and local catalog names.
- Auxiliary temporary storage, temporary data, trace, dump, and local request queue datasets.
- Startup JCL procedure name and symbolic overrides.
- GRPLIST, SIT, PLT, TCPIPSERVICE, DB2CONN, JVMSERVER, PROGRAM, TRANSACTION, FILE, TDQUEUE, and related definitions if in scope.
- CMCI connection details for resource management.
- Startup, warm start, cold start, and shutdown policy.
- Verification commands and expected states.

### IMS Region Model

Each IMS region should be described declaratively:

- IMSID, control region name, dependent regions, message processing regions, batch message processing regions, and Fast Path regions where applicable.
- IMS release and product library HLQ.
- PROCLIB members and JCL procedures.
- DBDLIB, PSBLIB, ACBLIB, RESLIB, MODBLKS, FORMAT, MFS, staging, and generated artifact datasets.
- RECON datasets and DBRC policy.
- IMS catalog usage and catalog population policy.
- DBD, PSB, ACB generation inputs and outputs.
- Type-1 or type-2 command strategy.
- Startup, shutdown, checkpoint, drain, and verification policy.

### BMC Product Model

Each BMC product should be described with a product definition file:

- Product code and product name.
- Version, release, maintenance level, FMIDs, and dependent FMIDs.
- Media source, checksum, unpack method, and staging requirements.
- Install method:
  - SMP/E receive/apply/accept.
  - Vendor-provided generated JCL.
  - Runtime customization only.
  - Hybrid.
- SMP/E CSI, target zone, distribution zone, global zone, and DDDEF requirements.
- Target libraries and distribution libraries.
- APF, LINKLIST, LPA, PROGxx, PARMLIB, PROCLIB, and started task requirements.
- Security definitions and started task user IDs.
- Product license keys, load modules, exits, subsystem hooks, or IPL requirements.
- Post-install customization steps.
- Verification steps and expected messages.
- Backout plan and recoverability classification.

See [BMC Software Lifecycle Strategy](bmc-software-lifecycle-strategy.md) for the deeper product lifecycle model covering internal depot design, varied install adapters, configuration unwinding, update promotion, evidence, and aggressive future-state ideas.

## Role Boundaries

The roles should be intentionally narrow. Region provisioning, resource definition, and product runtime hooks should not be collapsed into one role because they have different owners, prerequisites, approvals, and recovery behavior.

### `zos_common`

Owns connectivity, environment setup, facts, codepage-sensitive settings, temporary directories, and basic health checks.

### `zos_storage`

Owns dataset allocation standards, existence checks, GDG setup, backup datasets, and cleanup policy.

### `zos_jcl`

Owns JCL rendering, upload, submission, polling, return-code policy, JES output capture, and evidence extraction.

### `zos_smpe`

Owns SMP/E orchestration through generated JCL and state probes:

- CSI discovery.
- RECEIVE CHECK and RECEIVE.
- APPLY CHECK and APPLY.
- ACCEPT CHECK and ACCEPT.
- HOLDDATA handling.
- BYPASS policy only through explicit approval.
- FMID and SYSMOD verification.

### `cics_region`

Owns region dataset provisioning, catalog initialization, CSD management, region JCL creation, startup, shutdown, and basic region verification.

### `cics_cmci_resources`

Owns CICS and CICSPlex SM resource definitions and runtime actions through CMCI where available.

### `ims_region`

Owns IMS procedure generation, dataset preparation, control/dependent region lifecycle, IMS commands, and base verification.

### `ims_database_artifacts`

Owns DBD generation, PSB generation, ACB generation, DBRC commands, DDL, and IMS catalog population or purge.

### `bmc_product`

Owns product media staging, product definition interpretation, SMP/E or vendor JCL install orchestration, and product-specific install evidence.

### `bmc_runtime_config`

Owns APF, LINKLIST, LPA, PARMLIB, PROCLIB, started tasks, security hooks, product parameter libraries, and post-install customization.

This role must model hooks into CICS and IMS separately from base product installation. For example, installing BMC product libraries through SMP/E is not the same change as adding CICS PLT/SIT/resource definitions, changing IMS PROCLIB members, adding exits, or recycling a monitored subsystem.

### `verification`

Owns cross-product readiness checks, started task status, operator command assertions, dataset assertions, SMP/E state assertions, and smoke tests.

### `evidence`

Owns collection of job logs, rendered JCL, configuration snapshots, reports, and deployment manifests.

## Execution Flows

### Flow 1: Preflight

Diagram source: [docs/diagrams/preflight-flow.mmd](diagrams/preflight-flow.mmd)

Required outputs:

- Pass/fail by host.
- Effective Python and ZOAU paths.
- Effective codepage/tagging environment.
- Security permission gaps.
- Dataset allocation test result.
- JES submission test result.
- CMCI availability for CICS if in scope.
- IMS command capability if in scope.
- SMP/E CSI accessibility if BMC install is in scope.

### Flow 2: CICS Region Deployment

Diagram source: [docs/diagrams/cics-region-deployment-flow.mmd](diagrams/cics-region-deployment-flow.mmd)

Recovery model:

- Dataset creation is reversible before region start if no production data exists.
- Catalog initialization requires explicit recreate approval.
- CSD changes require backup before update.
- Runtime CMCI changes should have before/after resource snapshots.

### Flow 3: IMS Region Deployment

Diagram source: [docs/diagrams/ims-region-deployment-flow.mmd](diagrams/ims-region-deployment-flow.mmd)

Recovery model:

- Generated artifacts should be staged before promotion.
- ACB, DBD, and PSB libraries require backups before replacement.
- RECON and DBRC changes require stricter approval and should be isolated behind run tags.

### Flow 4: BMC Product Deployment

Diagram source: [docs/diagrams/bmc-product-deployment-flow.mmd](diagrams/bmc-product-deployment-flow.mmd)

Accept policy:

- `ACCEPT` should be a separate, explicit playbook or workflow after soak validation.
- `APPLY` can be part of deployment.
- `ACCEPT` should require approval because it materially changes backout options.

### Flow 5: Full Environment Deployment

Diagram source: [docs/diagrams/full-environment-deployment-flow.mmd](diagrams/full-environment-deployment-flow.mmd)

The exact ordering depends on which BMC products are deployed. Products that instrument, monitor, or integrate with CICS or IMS may need base libraries installed before region customization, but final hooks should happen after the target regions exist.

## Dependency Governance

Product dependencies should be represented as data and resolved before execution. The resolver does not need to be complex initially, but it must prevent accidental ordering mistakes.

Each deployable unit should declare:

- Required z/OS level.
- Required middleware level.
- Required FMIDs or products.
- Required product maintenance level.
- Required APF/LINKLIST/PARMLIB state.
- Required started tasks.
- Required CICS or IMS regions.
- Required prior deployment units.
- Whether the dependency is install-time, customization-time, or runtime-only.

The deployment planner should produce an ordered manifest before applying changes:

1. Base z/OS prerequisites.
2. BMC shared infrastructure and common libraries.
3. BMC product base installation.
4. CICS and IMS region provisioning.
5. Product hooks into CICS and IMS.
6. Runtime starts, refreshes, or recycles.
7. Verification.
8. Evidence publication.

The manifest should be reviewed for production changes before execution.

## Idempotency Strategy

Use native Ansible modules where they expose state safely:

- Dataset existence and attributes.
- USS file presence.
- JCL submission and return-code validation.
- Started task state.
- Operator command output.
- CICS provisioning module state.
- CMCI create/update/delete/get/action behavior.
- IMS command and generation module behavior.

Use custom state probes where native idempotency is not available:

- SMP/E CSI query job.
- Product FMID installed state.
- Product maintenance level.
- APF list membership.
- LINKLIST membership.
- PARMLIB member checksum.
- PROCLIB member checksum.
- Product startup message presence.
- Product-specific command output.

Never treat successful JCL submission alone as success. Success requires:

- Expected job return code.
- Expected step return codes.
- Absence of known fatal messages.
- Presence of expected completion messages.
- Optional state verification after the job.

## Approval Gates

Approval gates should be enforced in the automation platform or through explicit playbook tags and prompts for non-platform runs.

Required approval gates:

- Security profile creation or alteration.
- APF authorization changes.
- LINKLIST, LPA, or PROGxx changes.
- PARMLIB changes.
- Started task creation in production.
- SMP/E APPLY for production.
- SMP/E ACCEPT in all controlled environments.
- CICS catalog recreation.
- IMS RECON or DBRC changes.
- Any BYPASS of SMP/E HOLDDATA or requisites.
- Any product step requiring IPL, LLA refresh, VARY, or subsystem restart.

## Security Model

Use a dedicated Ansible service ID per environment or per trust boundary. Avoid personal IDs for scheduled automation.

Secrets must not be stored in plaintext inventory:

- z/OS passwords.
- SSH private keys.
- BMC license keys.
- Product download credentials.
- Certificates.
- API tokens.

Preferred order:

1. SSH certificates or managed SSH keys.
2. Ansible Automation Platform credentials.
3. Enterprise vault integration.
4. Ansible Vault for development only.

The security role should render requested definitions but should not silently apply broad permissions. Production security changes should be reviewable artifacts.

## Evidence and Audit

Each run should produce an evidence bundle containing:

- Git commit SHA of automation.
- Inventory name and host list.
- Product definitions used.
- Rendered JCL.
- Submitted job IDs.
- JES output.
- Return-code summary.
- Operator command outputs.
- CICS CMCI before/after snapshots where applicable.
- IMS command outputs where applicable.
- SMP/E reports.
- Changed dataset and member list.
- Final deployment manifest.

## Testing Strategy

### Offline Tests

- YAML syntax.
- ansible-lint.
- Inventory schema validation.
- Product definition schema validation.
- JCL template rendering tests.
- Required variable tests.
- Naming convention tests.

### z/OS Sandbox Tests

- SSH and ZOAU preflight.
- Dataset allocation and cleanup.
- JCL submit and JES capture.
- CICS test region create/start/stop/destroy.
- IMS test artifact generation and command execution.
- SMP/E dry-run CHECK jobs against non-production CSI.

### Production Readiness Tests

- Full dry run where possible.
- CHECK jobs for SMP/E.
- Backup and restore proof.
- Evidence bundle review.
- Operator handoff checklist.
- Rollback drill for at least one CICS region, one IMS customization, and one BMC product flow.

## Delivery Phases

### Phase 0: Discovery

Deliverables:

- Product inventory.
- Environment inventory.
- Security matrix.
- Storage and dataset standards.
- CICS and IMS region catalog.
- BMC product dependency graph.
- Install media and license handling plan.

### Phase 1: Automation Foundation

Deliverables:

- Repository structure.
- Execution environment or pinned control-node dependencies.
- Inventories and variable schemas.
- Preflight playbook design.
- Shared JCL and evidence framework design.

### Phase 2: CICS Region Automation

Deliverables:

- CICS region data model.
- Region provisioning role.
- CMCI resource role.
- Start/stop/verify workflows.
- Test region deployment.

### Phase 3: IMS Region Automation

Deliverables:

- IMS region data model.
- IMS artifact generation workflows.
- IMS command and verification workflows.
- Test IMS deployment.

### Phase 4: BMC Product Framework

Deliverables:

- Internal BMC software depot structure.
- BMC product definition schema.
- Read-only BMC estate discovery and configuration unwinding.
- Media staging framework.
- SMP/E orchestration framework.
- Product customization JCL framework.
- First product onboarded end to end.

### Phase 5: Product Factory

Deliverables:

- Remaining BMC products onboarded through product definitions and product-specific adapters.
- Cross-product dependency handling.
- Environment deployment workflow.
- Evidence and audit publication.

### Phase 6: Production Operating Model

Deliverables:

- Runbooks.
- Approval workflow.
- Break-glass procedure.
- Rollback and recovery procedure.
- Maintenance and service update process.
- Ownership matrix.

## Initial Scope Recommendation

Start with a narrow but representative pilot:

1. One z/OS sandbox LPAR.
2. One new CICS test region.
3. One IMS test region or IMS artifact generation flow.
4. One BMC product with SMP/E packaging.
5. One BMC product with post-install runtime customization.
6. Full preflight, deploy, verify, and evidence bundle.

Do not start by automating every product. Build the factory pattern with two BMC product shapes, then scale.

## Key Open Questions

1. Which BMC products are in scope, and which versions?
2. Are the BMC products SMP/E-packaged, vendor-JCL-packaged, runtime-only, or mixed?
3. Will Ansible Automation Platform be used, or only command-line ansible-core?
4. Which security product is used: RACF, ACF2, or Top Secret?
5. Are CICS resources managed through CMCI/CICSPlex SM, CSD batch utilities, or both?
6. Are IMS operations authorized through type-2 commands, type-1 commands, generated JCL, or a mix?
7. Is z/OSMF available and approved for any deployment functions?
8. What are the production approval boundaries for APPLY, ACCEPT, APF, PARMLIB, and started tasks?
9. What is the required backout posture for each product?
10. Are IPLs allowed, avoided, or scheduled only by separate change process?

## Implementation Readiness Checklist

Code should not begin until these are complete:

- Product inventory is complete.
- Environment and LPAR list is complete.
- Control-node and managed-node prerequisites are validated.
- Security model is approved.
- Dataset naming and storage standards are approved.
- CICS region model is approved.
- IMS region model is approved.
- BMC product schema is approved.
- SMP/E CSI strategy is approved.
- Approval gates are agreed.
- Pilot scope is selected.
- Test LPAR access is available.

## Source References

Design assumptions in this document are based on the current IBM Ansible for IBM Z documentation reviewed on 2026-07-02:

- Red Hat Ansible Content for IBM Z overview and collection model: https://ibm.github.io/z_ansible_collections_doc/index.html
- z/OS collection requirements and configuration entry points: https://ibm.github.io/z_ansible_collections_doc/requirements/software-requirements.html
- z/OS core configuration, including SSH, Python, ZOAU, inventory, and environment variables: https://ibm.github.io/z_ansible_collections_doc/ibm_zos_core/docs/source/configuration.html
- z/OS core module set for datasets, copy, JCL, operator commands, started tasks, TSO, APF, and related primitives: https://ibm.github.io/z_ansible_collections_doc/ibm_zos_core/docs/source/modules.html
- z/OS CICS module set, including CMCI modules and CICS provisioning modules: https://ibm.github.io/z_ansible_collections_doc/ibm_zos_cics/docs/source/modules.html
- z/OS IMS module set, including IMS command, DBRC, DBD, PSB, ACB, DDL, and catalog modules: https://ibm.github.io/z_ansible_collections_doc/ibm_zos_ims/docs/source/modules.html
