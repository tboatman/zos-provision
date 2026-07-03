# Repository Population and Clone Lifecycle Workflows

## Purpose

Define the next workflow layer for initial z/OS repository population, service-level publication, disconnected VM guest clone creation, environment provisioning, refresh, suspend, evidence collection, and destroy.

This document is still design-only. It identifies the orchestration flows and the missing workflow decisions before any Ansible code, z/VM automation, z/OSMF workflow execution, SMP/E jobs, or product deployment playbooks are written.

Gap coverage: G-028, G-029, G-030, G-031, and G-032 from [Top-to-Bottom Gap Assessment and Remediation Plan](top-to-bottom-gap-assessment.md).

Review pass: 2026-07-03.

## Workflow Diagrams

- Initial repository population: [docs/diagrams/initial-zos-repository-population-flow.mmd](diagrams/initial-zos-repository-population-flow.mmd)
- Clone provision, lifecycle, and destroy: [docs/diagrams/clone-provision-destroy-flow.mmd](diagrams/clone-provision-destroy-flow.mmd)

## Operating Model

The internal repository is the source of reproducible environments. Clones are products emitted from that repository, not hand-built systems.

Core principle:

```text
Repository service level + clone manifest + local security template + VM guest policy = reproducible disconnected clone
```

The clone should not need central authority after creation. Central systems may build, approve, and export the clone, but the resulting VM guest must be able to boot and operate with clone-local authority, clone-local credentials, and clone-local evidence collection.

## Workflow 1: Initial Repository Population

The first repository population flow establishes enough trusted material to build a clone and later install products.

### Inputs

- z/OS base installation and maintenance evidence.
- IBM middleware media and service where in scope.
- Vendor product media, service, HOLDDATA, and documentation snapshots.
- z/OSMF workflow assets and variable templates.
- SMP/E CSI templates, DDDEF patterns, receive inputs, and zone conventions.
- RACF or security templates for connected environments and clones.
- PARMLIB, PROCLIB, APF, LINKLIST, JES, TCP/IP, USS, WLM, SMS, certificate, and clone-local policy templates.
- OSS mirrors approved for control node, z/OS USS, Linux on Z, z/VM MAP, or validation host use.
- Existing installed-state evidence from discovery and unwinding.

### Phases

1. **Bootstrap repository structure**
   - Create the depot layout, metadata roots, evidence roots, and naming rules.
   - Do not store restricted media in Git unless the retention and redistribution decision permits it.

2. **Register source material**
   - Register media, service, HOLDDATA, workflows, generated JCL, product docs, and OSS mirrors as package metadata.
   - Capture checksums, retrieval date, source, entitlement status, and restrictions.

3. **Normalize service levels**
   - Convert raw package inputs into service-level records for z/OS, IBM middleware, vendor products, site artifacts, and OSS tooling.
   - Link FMIDs, SYSMODs, packages, HOLDDATA, and evidence.

4. **Build dependency graph**
   - Connect z/OS base, middleware, vendor products, subsystem hooks, security templates, clone manifests, and approved OSS tooling.

5. **Create repository validation manifest**
   - Validate required inputs, missing entitlements, checksum coverage, media custody, documentation snapshots, and blocked products.

6. **Publish service-level baseline**
   - Publish only service levels that have enough evidence to be selected by a clone or deployment manifest.

7. **Seal baseline**
   - Mark the baseline immutable for clone creation.
   - Record who approved it and which gaps remain.

### Outputs

- Repository structure.
- Package metadata.
- Service-level metadata.
- Dependency graph.
- Repository validation report.
- Baseline ID.
- Evidence bundle.
- Open gap list.

### Missing Decisions

- Where restricted media physically lives: Git LFS, S3-compatible storage, Artifactory/Nexus, SharePoint, file share, or a hybrid.
- Whether repository metadata and restricted binaries share one custody system.
- How service-level baselines are signed or sealed.
- Whether z/OSMF Software Management software instances are stored as first-class repository objects.
- How HOLDDATA expiration and refresh are tracked.
- How clone baselines are retired when maintenance supersedes them.

## Workflow 2: Clone Planning

Clone creation starts with a plan, not VM operations.

### Inputs

- Baseline ID.
- Clone request.
- Clone manifest.
- VM guest sizing policy.
- Disk and volume layout.
- Network and hostname policy.
- Local security template.
- Product and middleware scope.
- Expiration policy.
- Evidence export policy.

### Planning Steps

1. Validate the requested baseline exists and is not retired.
2. Validate all required packages and service levels are available.
3. Validate clone-local security template and break-glass policy.
4. Validate z/VM capacity, guest ID, minidisk, NIC, VSWITCH, and spool policy.
5. Validate z/OSMF, Feilong, site z/VM API, direct SMAPI, or manual handoff path.
6. Validate the VM-side SMAPI operation mapping from [z/VM SMAPI Implementation Design](zvm-smapi-implementation-design.md).
7. Build ordered clone work plan.
8. Produce an approval package.

### Plan Outputs

- Clone work plan.
- Required approvals.
- Backout class per step.
- Expected evidence.
- Destroy plan.
- Expiration timestamp.

Hard rule: every clone create plan must include its destroy plan before Apply.

## Workflow 3: Clone Materialization

Materialization is where the VM guest and z/OS content come into existence.

Supported design shapes:

- `full_image`: clone from a known-good z/OS VM image.
- `software_instance`: materialize from z/OSMF Software Management software instance.
- `volume_restore`: restore volumes or datasets from backup/copy.
- `hybrid`: combine image, repository overlays, and post-clone localization.

Generic materialization phases:

1. Create or reserve VM guest identity.
2. Create, attach, copy, or restore minidisks or volumes.
3. Configure NICs, VSWITCH, VLAN, and guest network policy.
4. Inject firstboot or localization material through reader, disk, USS staging, or approved z/VM path.
5. IPL or start the guest.
6. Wait for SSH, z/OSMF, JES, and base services where expected.
7. Record low-level VM evidence.

For SMAPI-backed materialization, each phase must map to an approved SMAPI, Feilong, site-service, direct-client, or manual-handoff operation before Apply.

Missing decisions:

- Which materialization shape is first.
- Which component owns volume copy or restore.
- Whether firstboot is reader-based, disk-based, z/OSMF-based, or JCL-based.
- How IPL device and z/OS shutdown are controlled.
- How failed half-created guests are cleaned up.

## Workflow 4: Clone Localization

Localization makes the clone independent from central authority.

Localization steps:

1. Rewrite system identity: system name, host name, TCP/IP profile, resolver, JES policy, and dataset roots where required.
2. Generate local RACF or security database changes.
3. Generate clone-local IDs, groups, certificates, keyrings, and break-glass credential handling.
4. Remove or neutralize central authentication dependencies.
5. Apply PARMLIB, PROCLIB, APF, LINKLIST, LPA, JES, SMS, WLM, USS, and certificate overlays.
6. Recycle or IPL where required by the clone policy.
7. Verify local-only authority.
8. Export localization evidence.

Missing decisions:

- Which central dependencies must be actively removed versus left unreachable.
- How local credentials are escrowed, rotated, and destroyed.
- Whether clone-local certificate authority material is generated per clone or per baseline.
- Which security backend is allowed for the first clone.

## Workflow 5: Clone Provisioning

Provisioning installs or activates the selected middleware, subsystem, and vendor scope for the clone.

Provisioning lanes:

- Base z/OS verification.
- CICS region provisioning.
- IMS region or artifact provisioning.
- Db2 isolated schema or sandbox subsystem provisioning.
- MQ dependency-only or included path.
- Vendor base install or activation.
- Vendor runtime hooks.
- OSS helper tooling where approved.

Provisioning order:

1. Run clone preflight.
2. Verify baseline service levels.
3. Apply local security template.
4. Apply platform overlays.
5. Provision subsystem surfaces.
6. Install or activate vendor products.
7. Apply runtime hooks into CICS, IMS, Db2, MQ, security, scheduler, USS, TCP/IP, or automation.
8. Run verification probes.
9. Publish evidence bundle.
10. Mark clone ready for handoff.

Missing decisions:

- Whether vendor products are pre-baked into the baseline or installed after clone creation.
- Whether CICS/IMS/Db2 are part of the base clone or per-request provisioning.
- Whether product activation means full install, runtime overlay, or enablement of dormant installed bits.
- How long a failed provisioning attempt remains available for investigation before cleanup.

## Workflow 6: Clone Handoff and Operation

Once ready, the clone needs an operating contract.

Handoff package:

- Clone ID.
- Expiration timestamp.
- Hostname and access path.
- Local user IDs and credential custody instructions.
- Installed service levels.
- Enabled subsystems and vendor products.
- Known limitations.
- Evidence bundle location.
- Destroy plan.
- Support boundary.

Operational states:

- `requested`
- `planned`
- `approved`
- `materializing`
- `localizing`
- `provisioning`
- `verifying`
- `ready`
- `suspended`
- `expired`
- `destroying`
- `destroyed`
- `failed`
- `quarantined`

Missing decisions:

- Who can extend clone lifetime.
- Whether a clone can be refreshed in place.
- Whether clone users can change local security templates.
- What makes a clone quarantined rather than destroyed.

## Workflow 7: Refresh, Rebuild, and Drift

Disconnected does not mean unknowable. The project needs a drift and refresh model.

Refresh options:

- Destroy and recreate from newer baseline.
- Apply maintenance to clone from repository.
- Re-run selected provisioning lanes.
- Leave clone frozen and mark baseline stale.

Drift checks:

- Service-level baseline versus clone manifest.
- RACF or security state versus clone-local template.
- APF, LINKLIST, PARMLIB, PROCLIB, JES, TCP/IP, USS, certificate state.
- CICS/IMS/Db2/MQ state.
- Vendor product installed and runtime state.
- Expiration and evidence export status.

Initial recommendation: prefer destroy/recreate over in-place refresh for development clones until the maintenance workflow is proven.

Missing decisions:

- Maximum allowed drift.
- Who can approve in-place refresh.
- Whether disconnected clones can phone home evidence opportunistically.
- How stale clones are discovered if they stop exporting evidence.

## Workflow 8: Suspend, Expire, and Destroy

Destroy is a first-class workflow, not cleanup afterthought.

Destroy phases:

1. Validate destroy request and authority.
2. Export final evidence where connectivity exists.
3. Notify users or owner.
4. Stop applications, vendor products, middleware, and z/OS cleanly where possible.
5. Shutdown z/OS guest.
6. Remove or revoke temporary network entries.
7. Destroy or archive VM guest disks according to retention policy.
8. Destroy or archive local credentials and certificates.
9. Release guest ID, NICs, minidisks, spool, volumes, and names.
10. Mark clone destroyed in repository evidence.
11. Record exceptions and leaked resources.

Destroy classes:

- `normal_destroy`: clean shutdown and resource release.
- `forced_destroy`: cleanup after failed guest or expired inaccessible clone.
- `quarantine_destroy`: evidence preserved for investigation before final cleanup.
- `retain_for_debug`: explicit exception with owner and expiration.

Missing decisions:

- Data retention for clone disks and evidence.
- Who can force-destroy a clone.
- Whether final evidence is mandatory or best effort.
- How leaked resources are detected after destroy.
- How secrets are destroyed or escrow records retired.

## Workflow 9: Repository Maintenance After Clone Use

Clone outcomes should feed the repository.

Inputs back to repository:

- Provisioning failures.
- Missing packages or docs.
- Bad dependencies.
- Wrong service-level assumptions.
- Security template errors.
- Subsystem provisioning drift.
- Vendor runtime hook errors.
- Destroy leaks.

Repository update actions:

- Open or update gap records.
- Update product definitions.
- Update dependency graph.
- Retire bad baselines.
- Promote good baselines.
- Add verification probes.
- Add destroy checks.

## Missing Work You Probably Have Not Modeled Yet

The current design was missing or under-specific on these workflow surfaces:

1. Repository bootstrap versus steady-state intake.
2. Baseline seal/sign-off before clone creation.
3. Clone request approval package.
4. Clone destroy plan generated before clone create.
5. Half-created clone cleanup.
6. Clone operational state machine.
7. Suspend and quarantine states.
8. Expiration extension process.
9. Final evidence export before destroy.
10. Secret and certificate destruction.
11. Resource leak detection after destroy.
12. Clone drift checks.
13. In-place refresh versus destroy/recreate policy.
14. How clone failures feed repository metadata.
15. Whether vendor products are pre-baked into baselines or provisioned per clone.
16. Whether CICS/IMS/Db2 are baseline components or per-request components.
17. Who owns z/VM capacity and guest ID allocation.
18. Who owns storage cleanup for minidisks, volumes, catalogs, and datasets.
19. How stale disconnected clones are detected.
20. Whether evidence export is required, opportunistic, or manual for disconnected clones.

## Immediate Actions

1. Decide the first clone materialization shape.
2. Draft a clone manifest that includes create and destroy sections.
3. Draft a repository baseline record with seal/approval fields.
4. Draft a clone state machine as metadata.
5. Decide pre-baked versus per-clone provisioning for CICS, IMS, Db2, and first vendor products.
6. Add destroy evidence requirements to the evidence manifest.
7. Decide whether development clones are refreshed in place or destroyed/recreated.
8. Define leaked-resource checks for z/VM, network, storage, credentials, and repository metadata.
