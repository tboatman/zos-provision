# z/OS CICS, IMS, Db2, and Vendor Product Deployment Through Ansible

## Purpose

Design the architecture, requirements, scope, and execution flows for deploying CICS regions, IMS regions, Db2 for z/OS assets, and multiple third-party vendor software products on z/OS from a Linux or macOS Ansible control node.

This document deliberately avoids implementation code. It defines the operating model and module boundaries that should be agreed before playbooks, roles, inventories, or JCL templates are written.

## Documentation Diagram Convention

All diagrams and flowcharts must be maintained as standalone `.mmd` Mermaid source files under [docs/diagrams](diagrams/). Markdown documents should link to those files instead of embedding Mermaid blocks inline.

## Design Principles

1. Ansible is the orchestrator, not the only execution engine.
2. z/OS-native mechanisms remain authoritative for z/OS-native work:
   - SMP/E for SMP/E-packaged software.
   - Internal SMP/E repository metadata for curated service levels and clone manifests.
   - JCL for batch install, generation, and verification jobs.
   - Operator commands for started task and subsystem control.
   - RACF command streams and verification reports for security control.
   - CMCI for CICS resource definition and runtime management.
   - IMS commands, DBRC, DBD/PSB/ACB generation, and catalog operations for IMS.
   - Db2 utilities, DSN command processor jobs, SQL execution jobs, BIND/REBIND jobs, catalog queries, and subsystem commands for Db2.
3. Product deployment is metadata-driven. Product-specific values belong in inventory and product definition files, not hardcoded task logic.
4. Every flow must support plan, apply, verify, and recover phases at minimum. This is a floor, not the full model: the canonical lifecycle in [Vendor Adapter Skeletons](vendor-adapter-skeletons.md#shared-lifecycle-phases) defines twelve phases in total, and every other phase list in this documentation set is a scoped subset of that one.
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
- IBM z/OSMF collection or equivalent z/OSMF API access. z/OSMF is the preferred forward path where available, and this design assumes required z/OSMF APIs are available for approved z/OSMF-driven install and configuration workflows.
- No specialized Db2 automation tools are assumed. In particular, the architecture must work without IBM Db2 DevOps Experience, UrbanCode Deploy plugins, or other Db2-specific deployment products.
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
- z/OSMF APIs available for approved z/OSMF-driven software management, workflow, and configuration paths.
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
- Internal SMP/E repository and service-level metadata available for environments and disconnected VM guest clones.
- RACF command execution, review, and verification path agreed.
- VM guest clone provisioning path agreed for disconnected development environments.

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

For Db2:

- Db2 for z/OS installed and licensed.
- Db2 subsystem IDs, data sharing group names, catalog aliases, and member topology documented.
- Db2 SDSNLOAD, SDSNEXIT, SDSNCLST, SDSNSAMP, RUNLIB, and site library conventions documented.
- DSN command processor execution pattern agreed.
- SQL execution pattern agreed, such as DSNTEP2, DSNTEP4, DSNTIAD, or site-standard batch SQL runner.
- BIND, REBIND, FREE, GRANT, REVOKE, DDL, utility, and catalog query authority agreed.
- Plan, package, collection, qualifier, owner, schema, and authorization naming conventions documented.
- CICS DB2CONN/DB2ENTRY and IMS Db2 attachment dependencies documented where applicable.

For vendor products:

- Vendor families in initial scope: BMC Software, Broadcom CA, and Rocket Software.
- Product list, versions, FMIDs, maintenance level, dependencies, and license requirements documented.
- Product media acquisition path agreed.
- Internal vendor software depot strategy agreed.
- z/OSMF workflow availability and API access agreed where vendor or site workflows exist.
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
  db2/
  vendors/
host_vars/
roles/
  zos_common/
  zos_storage/
  zos_security_model/
  zos_jcl/
  zos_smpe/
  zos_smp_repository/
  zos_maintenance/
  zos_racf/
  zos_clone_factory/
  zos_parmlib/
  zos_proclib/
  zos_apf_linklist_lpa/
  cics_region/
  cics_cmci_resources/
  ims_region/
  ims_database_artifacts/
  db2_subsystem/
  db2_database_artifacts/
  vendor_legacy_capture/
  vendor_product/
  vendor_runtime_config/
  verification/
  evidence/
playbooks/
  preflight.yml
  maintain_zos.yml
  create_disconnected_vm_clone.yml
  apply_racf_model.yml
  deploy_cics_region.yml
  deploy_ims_region.yml
  deploy_db2_artifacts.yml
  deploy_vendor_product.yml
  deploy_environment.yml
  verify_environment.yml
  rollback_or_recover.yml
product_definitions/
  cics/
  ims/
  db2/
  vendors/
platform_definitions/
  smp_repository/
  maintenance/
  racf/
  clones/
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

### Db2 Model

Each Db2 deployment target should be described declaratively:

- Subsystem ID, data sharing group, member name, and environment.
- Db2 release and product library HLQs.
- SDSNLOAD, SDSNEXIT, RUNLIB, SDSNSAMP, and site execution libraries.
- SQL execution method and job templates.
- Plan, package, collection, qualifier, owner, schema, and authorization conventions.
- DDL artifacts: databases, storage groups, table spaces, tables, indexes, views, aliases, synonyms, routines, triggers, and sequences.
- Bind artifacts: DBRMs, packages, plans, collections, bind options, explain policy, validate policy, isolation, release, currentdata, and owner.
- Security artifacts: GRANT, REVOKE, roles, trusted context, and RACF-externalized checks where applicable.
- Utility artifacts: image copy, runstats, reorg, check data, repair policy, and recoverability checks where in scope.
- CICS integration: DB2CONN, DB2ENTRY, DB2TRAN, and transaction-to-plan/package dependencies.
- IMS integration: IMS Db2 attachment, dependent region requirements, and PSB/program dependency mapping.
- Vendor product integration: product repository databases, plans/packages, exits, collection IDs, and product-specific catalog objects.
- Verification queries, expected catalog state, expected binds, and expected return codes.

### Vendor Product Model

Each vendor product should be described with a product definition file:

- Product code and product name.
- Version, release, maintenance level, FMIDs, and dependent FMIDs.
- Media source, checksum, unpack method, and staging requirements.
- Install method:
  - SMP/E receive/apply/accept.
  - z/OSMF workflow.
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

See [Third-Party Vendor Software Lifecycle Strategy](third-party-software-lifecycle-strategy.md) for the deeper product lifecycle model covering BMC, Broadcom CA, and Rocket onboarding; internal depot design; z/OSMF workflow adapters; varied install adapters; configuration unwinding; update promotion; evidence; and aggressive future-state ideas.

See [Vendor Adapter Skeletons](vendor-adapter-skeletons.md) for the initial vendor-dependent process overlays that specialize the generic lifecycle for BMC, Broadcom CA, and Rocket.

See [Platform Maintenance and Disconnected Clone Strategy](platform-maintenance-and-disconnected-clones.md) for z/OS maintenance, RACF control, internal SMP/E repository governance, and disconnected VM guest clone generation.

See [Top-to-Bottom Gap Assessment and Remediation Plan](top-to-bottom-gap-assessment.md) for the cross-document gap register and remediation phases that must be closed or explicitly waived before implementation begins.

See [Pilot and Governance Controls](pilot-and-governance-controls.md), [Schema and Evidence Foundation](schema-and-evidence-foundation.md), and [Automation Contract and Security Gap Plan](automation-contract-and-security-gap-plan.md) for the first detailed closure plans behind the readiness checklist.

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

### `zos_smp_repository`

Owns internal SMP/E repository metadata, service-level manifests, HOLDDATA registration, CSI templates, receive input metadata, clone manifests, and repository evidence.

### `zos_maintenance`

Owns z/OS and IBM middleware maintenance planning, z/OSMF workflow execution where approved, SMP/E CHECK/APPLY/ACCEPT orchestration, service-level verification, and maintenance evidence.

### `zos_racf`

Owns RACF desired-state modeling, command rendering, backout rendering, authority validation, command execution, query-based verification, and security evidence.

### `zos_clone_factory`

Owns disconnected VM guest clone planning, clone manifest interpretation, local RACF/security generation, clone-local configuration rendering, VM guest activation handoff, and clone evidence.

### `zos_parmlib`, `zos_proclib`, and `zos_apf_linklist_lpa`

Own shared platform control surfaces for PARMLIB, PROCLIB, APF, LINKLIST, LPA, PROGxx, activation notes, refresh requirements, and backout.

### `cics_region`

Owns region dataset provisioning, catalog initialization, CSD management, region JCL creation, startup, shutdown, and basic region verification.

### `cics_cmci_resources`

Owns CICS and CICSPlex SM resource definitions and runtime actions through CMCI where available.

### `ims_region`

Owns IMS procedure generation, dataset preparation, control/dependent region lifecycle, IMS commands, and base verification.

### `ims_database_artifacts`

Owns DBD generation, PSB generation, ACB generation, DBRC commands, DDL, and IMS catalog population or purge.

### `db2_subsystem`

Owns Db2 subsystem discovery, command execution, subsystem state checks, library conventions, DSN command processor job setup, and non-invasive subsystem-level verification.

This role must not assume specialized Db2 deployment tooling. It should use z/OS core primitives to submit JCL, run Db2 commands, capture output, and query catalog state through approved batch SQL mechanisms.

### `db2_database_artifacts`

Owns Db2 DDL, DCL, bind, rebind, free, explain, utility, and catalog-verification workflows.

This role should treat SQL and bind control files as versioned deployment artifacts. It should support plan-only validation where possible, strict return-code and message assertions, before/after catalog snapshots, and explicit approval for destructive DDL or privilege removal.

### `vendor_legacy_capture`

Owns the Legacy Capture Adapter and the three-pass Configuration Unwinding Strategy (Inventory, Attribution, Normalization) from [Third-Party Vendor Software Lifecycle Strategy](third-party-software-lifecycle-strategy.md#configuration-unwinding-strategy): read-only discovery of an existing installation, classification of what's vendor base versus local override versus unknown, and generation of a candidate product definition for human approval.

This role is kept separate from `vendor_product` rather than folded into it because it is behaviorally opposite: it never applies a change, never stages install media, and never runs Accept or Publish. Its only output is a proposed product definition and a drift/unknown-config backlog. A product only moves to `vendor_product` once its captured model is approved and its install method is reclassified out of `legacy_manual_capture` or `unknown_pending_discovery` (an approval gate in its own right; see Approval Gates).

### `vendor_product`

Owns product media staging, product definition interpretation, z/OSMF workflow orchestration, SMP/E or vendor JCL install orchestration, and product-specific install evidence.

### `vendor_runtime_config`

Owns APF, LINKLIST, LPA, PARMLIB, PROCLIB, started tasks, security hooks, product parameter libraries, and post-install customization.

This role must model hooks into CICS, IMS, and Db2 separately from base product installation. For example, installing vendor product libraries through SMP/E is not the same change as adding CICS PLT/SIT/resource definitions, changing IMS PROCLIB members, binding Db2 packages, adding exits, or recycling a monitored subsystem.

### `verification`

Owns cross-product readiness checks, started task status, operator command assertions, dataset assertions, SMP/E state assertions, and smoke tests.

### `evidence`

Owns collection of job logs, rendered JCL, configuration snapshots, reports, and deployment manifests.

## Execution Flows

### Executive 50k Overview

Diagram source: [docs/diagrams/executive-50k-overview.mmd](diagrams/executive-50k-overview.mmd)

This diagram is the deliberately simplified executive view. Keep it synchronized with the master combined flow whenever the major phases, vendor families, clone posture, or delivery model change.

### Master Combined Flow

Diagram source: [docs/diagrams/master-combined-flow.mmd](diagrams/master-combined-flow.mmd)

This diagram intentionally combines the architecture, intake, preflight, vendor, CICS, IMS, Db2, RACF, maintenance, clone factory, verification, and evidence flows into one large view. Keep it synchronized with the executive overview so the simplified message and detailed dependency map tell the same story. Use it to communicate project scale and dependency breadth. Use the individual flow diagrams below as drill-downs for implementation design.

### Flow 1: Preflight

Diagram source: [docs/diagrams/preflight-flow.mmd](diagrams/preflight-flow.mmd)

Required outputs:

- Pass/fail by host.
- Effective Python and ZOAU paths.
- Effective codepage/tagging environment.
- Security permission gaps.
- RACF command and query capability if security control is in scope.
- Dataset allocation test result.
- JES submission test result.
- z/OSMF API and workflow capability if z/OSMF paths are in scope.
- CMCI availability for CICS if in scope.
- IMS command capability if in scope.
- Db2 command, batch SQL, bind, and catalog-query capability if in scope.
- SMP/E CSI accessibility if vendor install is in scope.

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

### Flow 4: Vendor Product Deployment

Diagram source: [docs/diagrams/vendor-product-deployment-flow.mmd](diagrams/vendor-product-deployment-flow.mmd)

Recovery model:

- This is the least standardized flow in the architecture -- the recovery mechanism depends on which install adapter handled the deployment, per [Vendor Adapter Skeletons](vendor-adapter-skeletons.md) and the [Install Adapter Pattern](third-party-software-lifecycle-strategy.md#install-adapter-pattern): SMP/E-managed products RESTORE or reactivate the prior accepted service level; vendor-JCL-managed products resubmit prior-version JCL or restore previously promoted generated members from the depot; runtime-only products restore the prior PARMLIB/PROCLIB/APF/LINKLIST/security member content from a before/after snapshot.
- Runtime hooks into CICS, IMS, or Db2 must be recoverable independently of the base product install, since they are modeled as a separate change (see `vendor_runtime_config` in Role Boundaries).
- Legacy-capture products have no recovery model because they are never applied; recovery is not meaningful until a product is promoted out of `legacy_manual_capture`.

Accept policy:

- `ACCEPT` should be a separate, explicit playbook or workflow after soak validation.
- `APPLY` can be part of deployment.
- `ACCEPT` should require approval because it materially changes backout options.

### Flow 5: Db2 Artifact Deployment

Diagram source: [docs/diagrams/db2-artifact-deployment-flow.mmd](diagrams/db2-artifact-deployment-flow.mmd)

Recovery model:

- DDL changes require explicit reversibility classification before deployment.
- BIND and REBIND jobs require prior package/plan state capture.
- GRANT and REVOKE changes require before/after authorization snapshots.
- Utility execution must record object scope, return codes, and recoverability impact.

### Flow 6: Full Environment Deployment

Diagram source: [docs/diagrams/full-environment-deployment-flow.mmd](diagrams/full-environment-deployment-flow.mmd)

The exact ordering depends on which vendor products are deployed. Products that instrument, monitor, or integrate with CICS, IMS, or Db2 may need base libraries installed before subsystem customization, but final hooks should happen after the target regions, subsystems, databases, and bind targets exist.

Recovery model: this flow has no recovery model of its own. It composes Flows 2 through 5, so recovery means running each composed flow's own recovery model in reverse dependency order (product runtime hooks before base install, subsystem artifacts before region provisioning), not a single rollback action.

### Flow 7: Platform Maintenance and Clone Factory

Diagram sources:

- [docs/diagrams/platform-maintenance-flow.mmd](diagrams/platform-maintenance-flow.mmd)
- [docs/diagrams/smp-repo-clone-factory.mmd](diagrams/smp-repo-clone-factory.mmd)
- [docs/diagrams/racf-control-flow.mmd](diagrams/racf-control-flow.mmd)

## Dependency Governance

Product dependencies should be represented as data and resolved before execution. The resolver does not need to be complex initially, but it must prevent accidental ordering mistakes.

Each deployable unit should declare:

- Required z/OS level.
- Required internal SMP/E repository service level.
- Required RACF/security template.
- Required VM guest clone manifest where applicable.
- Required middleware level.
- Required FMIDs or products.
- Required product maintenance level.
- Required APF/LINKLIST/PARMLIB state.
- Required started tasks.
- Required CICS or IMS regions.
- Required Db2 subsystems, data sharing groups, plans, packages, collections, schemas, or catalog objects.
- Required prior deployment units.
- Whether the dependency is install-time, customization-time, or runtime-only.

The deployment planner should produce an ordered manifest before applying changes:

1. Base z/OS prerequisites.
2. Platform maintenance and security baseline.
3. Disconnected VM guest clone generation if applicable.
4. Vendor shared infrastructure and common libraries.
5. Vendor product base installation.
6. CICS, IMS, and Db2 provisioning or artifact deployment.
7. Product hooks into CICS, IMS, and Db2.
8. Runtime starts, refreshes, or recycles.
9. Verification.
10. Evidence publication.

The manifest should be reviewed for production changes before execution.

This ten-step manifest is the unit of work executed once per environment tier, through Apply and Verify. It is not a competing model with the [Update Strategy promotion ring](third-party-software-lifecycle-strategy.md#update-strategy): the promotion ring is the sequence of environment tiers (sandbox, integration test, dev, test, QA, production) the same manifest gets executed against in turn. ACCEPT is deliberately not one of the ten steps above -- it only happens at the ring's final "Accept or hold" stage, after soak on the last tier, as its own separately approved step per the Approval Gates below, not as part of every tier's manifest run.

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
- Db2 batch SQL, bind job, utility job, command output, and catalog query assertions.

Use custom state probes where native idempotency is not available:

- SMP/E CSI query job.
- Internal SMP/E repository manifest state.
- RACF user, group, connect, profile, permit, OMVS, STARTED, and keyring state.
- Disconnected VM guest clone manifest and local authority state.
- Product FMID installed state.
- Product maintenance level.
- APF list membership.
- LINKLIST membership.
- PARMLIB member checksum.
- PROCLIB member checksum.
- Product startup message presence.
- Product-specific command output.
- Db2 catalog state for objects, grants, packages, plans, collections, and utility-relevant metadata.

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
- RACF template changes for disconnected VM guest clones.
- APF authorization changes.
- LINKLIST, LPA, or PROGxx changes.
- PARMLIB changes.
- Started task creation in production.
- SMP/E APPLY for production.
- SMP/E ACCEPT in all controlled environments.
- z/OS maintenance APPLY or ACCEPT.
- Internal SMP/E repository promotion.
- Disconnected VM guest clone templates that grant privileged local IDs.
- CICS catalog recreation.
- IMS RECON or DBRC changes.
- Db2 destructive DDL, REVOKE, FREE, mass REBIND, RUNSTATS/REORG/COPY policy changes, and subsystem parameter changes.
- Any BYPASS of SMP/E HOLDDATA or requisites.
- Any product step requiring IPL, LLA refresh, VARY, or subsystem restart.
- Vendor JCL Adapter Apply for `vendor_jcl_managed` or `hybrid_smpe_plus_runtime` products in production -- this does not go through SMP/E APPLY and would otherwise pass with no gate.
- Runtime Configuration Adapter Apply for `runtime_only` products in production, even when the change touches only product parameter members and not PARMLIB/APF/LINKLIST directly.
- Promoting a product out of `unknown_pending_discovery` or `legacy_manual_capture` into an actively managed install method -- this is the point where the Legacy Capture Adapter's captured model is approved and automation starts making changes for the first time.

## Security Model

Use a dedicated Ansible service ID per environment or per trust boundary. Avoid personal IDs for scheduled automation.

Secrets must not be stored in plaintext inventory:

- z/OS passwords.
- SSH private keys.
- Vendor license keys.
- Product download credentials.
- Clone-local break-glass credentials.
- Clone-local certificate private keys.
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
- z/OSMF workflow output where applicable.
- Return-code summary.
- Operator command outputs.
- RACF command streams, query output, and before/after snapshots where applicable.
- Clone manifests and disconnected VM guest evidence where applicable.
- CICS CMCI before/after snapshots where applicable.
- IMS command outputs where applicable.
- Db2 catalog snapshots, SQL output, bind reports, and utility output where applicable.
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
- Db2 test DDL, DCL, BIND, REBIND, and catalog verification in an isolated schema or sandbox subsystem.
- SMP/E dry-run CHECK jobs against non-production CSI.
- RACF render/validate/apply/verify tests in a sandbox or disconnected VM guest clone.
- Disconnected VM guest clone creation, boot/activation handoff, and local-only authority verification.

### Production Readiness Tests

- Full dry run where possible.
- CHECK jobs for SMP/E.
- Backup and restore proof.
- Evidence bundle review.
- Operator handoff checklist.
- Rollback drill for at least one CICS region, one IMS customization, one Db2 artifact deployment, and one vendor product flow.
- RACF backout drill and disconnected VM guest clone rebuild/destroy drill.

## Delivery Phases

### Phase 0: Discovery

Deliverables:

- Product inventory.
- Environment inventory.
- Security matrix.
- Storage and dataset standards.
- CICS and IMS region catalog.
- Db2 subsystem, schema, package, plan, and application dependency catalog.
- Vendor product dependency graph.
- Internal SMP/E repository and service-level catalog.
- RACF model and disconnected VM guest clone template catalog.
- Install media and license handling plan.

### Phase 1: Automation Foundation

Deliverables:

- Repository structure.
- Execution environment or pinned control-node dependencies.
- Inventories and variable schemas.
- Preflight playbook design.
- Shared JCL and evidence framework design.
- Internal SMP/E repository metadata design.
- RACF and disconnected VM guest clone schema design.

### Phase 2: Platform Maintenance and Clone Foundation

Deliverables:

- z/OS maintenance manifest schema.
- Internal SMP/E repository skeleton.
- RACF render/apply/verify skeleton.
- Disconnected VM guest clone manifest schema.
- Clone factory plan/check/apply/verify/evidence skeleton.

### Phase 3: CICS Region Automation

Deliverables:

- CICS region data model.
- Region provisioning role.
- CMCI resource role.
- Start/stop/verify workflows.
- Test region deployment.

### Phase 4: IMS Region Automation

Deliverables:

- IMS region data model.
- IMS artifact generation workflows.
- IMS command and verification workflows.
- Test IMS deployment.

### Phase 5: Db2 Artifact Automation

Deliverables:

- Db2 model and artifact schema.
- Batch SQL execution framework.
- Bind and rebind framework.
- Catalog snapshot and verification framework.
- DDL/DCL deployment and backout classification.
- Test Db2 deployment in an isolated schema or sandbox subsystem.

### Phase 6: Vendor Product Framework

Deliverables:

- Internal vendor software depot structure.
- Vendor product definition schema.
- Vendor adapter skeletons for BMC, Broadcom CA, and Rocket.
- `vendor_legacy_capture` role: read-only vendor estate discovery and configuration unwinding.
- Media staging framework.
- SMP/E orchestration framework.
- Product customization JCL framework.
- First product onboarded end to end.

### Phase 7: Product Factory

Deliverables:

- Remaining vendor products onboarded through product definitions and product-specific adapters.
- Cross-product dependency handling.
- Environment deployment workflow.
- Evidence and audit publication.

### Phase 8: Production Operating Model

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
2. One disconnected VM guest clone generated from the internal SMP/E repository.
3. One local RACF model applied to that VM guest clone.
4. One new CICS test region.
5. One IMS test region or IMS artifact generation flow.
6. One Db2 isolated schema or sandbox subsystem deployment flow.
7. One vendor product with SMP/E packaging or z/OSMF workflow packaging.
8. One vendor product with post-install runtime customization, preferably including at least one Db2 bind or catalog dependency if representative.
9. Full preflight, deploy, verify, and evidence bundle.

Do not start by automating every product. Build the factory pattern with two vendor product shapes, then scale.

## Key Open Questions

1. Which vendor products are in scope, and which versions?
2. Are the vendor products SMP/E-packaged, vendor-JCL-packaged, runtime-only, or mixed?
3. Will Ansible Automation Platform be used, or only command-line ansible-core?
4. Which security product is used: RACF, ACF2, or Top Secret?
5. Are CICS resources managed through CMCI/CICSPlex SM, CSD batch utilities, or both?
6. Are IMS operations authorized through type-2 commands, type-1 commands, generated JCL, or a mix?
7. Which Db2 execution method is approved for automation: DSNTEP2, DSNTEP4, DSNTIAD, site-standard JCL, or another batch SQL runner?
8. What are the approval boundaries for Db2 DDL, DCL, BIND, REBIND, FREE, utilities, and subsystem commands?
9. Which z/OSMF workflow definitions, variable conventions, API credentials, and workflow ownership rules are approved for vendor installation and configuration?
10. What is the internal SMP/E repository promotion policy?
11. What is the disconnected VM guest clone lifecycle: create, expire, rebuild, destroy, and evidence export?
12. Which RACF classes and local credentials are mandatory for disconnected VM guest clones?
13. What are the production approval boundaries for APPLY, ACCEPT, APF, PARMLIB, and started tasks?
14. What is the required backout posture for each product?
15. Are IPLs allowed, avoided, or scheduled only by separate change process?
16. Which Ansible for IBM Z collection versions, dependency matrices, and module contracts are approved for z/OS, z/OSMF, CICS, IMS, System Automation, and Z HMC?
17. What is the approved automation path for IBM subsystems without confirmed first-class collection support, especially Db2 and MQ?
18. What is the approved z/VM automation interface for disconnected VM guest clone lifecycle?

## Implementation Readiness Checklist

Code should not begin until these are complete:

- Critical gaps in [Top-to-Bottom Gap Assessment and Remediation Plan](top-to-bottom-gap-assessment.md) are closed or explicitly waived.
- Product inventory is complete.
- Environment and LPAR list is complete.
- IBM Ansible collection, subsystem, and z/VM documentation gathering is complete.
- Control-node and managed-node prerequisites are validated.
- Security model is approved.
- Dataset naming and storage standards are approved.
- Internal SMP/E repository model is approved.
- RACF model and command execution path are approved.
- Disconnected VM guest clone model is approved.
- CICS region model is approved.
- IMS region model is approved.
- Db2 model and artifact schema are approved.
- Vendor product schema is approved.
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
- IBM Ansible, z/OS, IBM subsystem, and z/VM documentation gathering plan reviewed on 2026-07-03: [IBM Ansible Documentation Gathering](ibm-ansible-documentation-gathering.md)
- z/OSMF module set, including authentication, security requirement validation, and workflow operation: https://ibm.github.io/z_ansible_collections_doc/ibm_zosmf/docs/source/modules.html
- IBM product documentation entry points for z/OS, CICS TS, IMS, Db2 for z/OS, MQ for z/OS, System Automation, and z/VM are tracked in [IBM Ansible Documentation Gathering](ibm-ansible-documentation-gathering.md)
- Focused z/VM Ansible source discovery, including IBM-owned GitHub repositories, Galaxy negative evidence, SMAPI, Feilong, and Management Access Point implications, is tracked in [z/VM Ansible Documentation Sources](zvm-ansible-documentation-sources.md)
