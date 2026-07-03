# Platform Maintenance and Disconnected Clone Strategy

## Purpose

Define the platform-level scope for z/OS maintenance, RACF control, and an internal SMP/E-backed repository that can produce arbitrary development clones disconnected from central authority.

This document sits above middleware and vendor-product deployment. It defines the substrate that CICS, IMS, Db2, and vendor product automation depend on, expanding on the `zos_smp_repository` and `zos_clone_factory` roles from [z/OS CICS, IMS, Db2, and Vendor Product Deployment Through Ansible](zos-cics-ims-db2-vendor-ansible-architecture.md).

## Core Assumption

The platform will maintain an internal SMP/E-backed software repository and use it to produce development clones on demand.

Development clones are VM guests and are intentionally **not connected to central authority**:

- No dependency on enterprise authentication at runtime.
- No dependency on central RACF, LDAP, MFA, certificate authority, or secrets service after clone creation.
- No callback to central inventory or governance systems required for normal clone operation.
- Local RACF or equivalent security database is generated from clone templates.
- Local credentials, certificates, service IDs, groups, and permissions are clone-scoped.
- Clone evidence is exported as artifacts, not enforced by central runtime control.

This does not mean clones are unmanaged. It means their management boundary is artifact-driven rather than authority-driven.

Disconnected clones are not modeled as LPARs. They are VM guests built from repository-defined service levels, local security templates, local network/storage policy, and clone manifests.

The focused z/VM automation source ledger is [z/VM Ansible Documentation Sources](zvm-ansible-documentation-sources.md). Current evidence points to a candidate IBM-owned but not Galaxy-published Ansible collection path using a Linux Management Access Point, SMAPI, Feilong zthin, and `smcli`; that path must be validated before clone lifecycle automation is implemented. The VM-side implementation contract, operation mapping, and doc-retrieval plan are tracked in [z/VM SMAPI Implementation Design](zvm-smapi-implementation-design.md).

Adjacent open source tooling is tracked in [OSS Tooling Discovery](oss-tooling-discovery.md). Any adopted OSS component must be pinned, mirrored internally, and treated as repository input before it can support disconnected clone generation or maintenance evidence.

The workflow layer for initial repository population, baseline sealing, clone creation, clone provisioning, refresh, and destroy is defined in [Repository Population and Clone Lifecycle Workflows](repository-population-and-clone-lifecycle-workflows.md).

## Scope Additions

Platform automation must cover:

- z/OS maintenance.
- SMP/E repository construction and curation.
- Software instance packaging.
- Development clone generation.
- RACF maintenance and control.
- PARMLIB and PROCLIB management.
- APF, LINKLIST, LPA, and PROGxx management.
- JES, SMS, catalog, TCP/IP, USS, WLM, and automation policy baselines where in scope.
- Certificate, keyring, and local trust material generation for disconnected clones.
- Evidence bundles for clone provenance and maintenance state.

## Internal SMP/E Repository

The repository should be the authoritative source for:

- z/OS base maintenance.
- IBM middleware maintenance.
- Vendor product media and service.
- HOLDDATA.
- SMP/E RECEIVE inputs.
- SMP/E reports.
- Product metadata.
- Dependency graphs.
- Clone manifests.
- Accepted service levels.
- Promotion state.
- Evidence.

Recommended layout:

```text
depot/
  smp/
    ibm-zos/
    ibm-middleware/
    vendors/
    holdata/
    csi-templates/
    receive-inputs/
    service-levels/
    reports/
    clone-manifests/
    evidence/
```

`smp/vendors/` is the mount point for the third-party vendor depot. Its internal per-product, per-version layout (`vendors/<vendor>/products/<product-code>/<version>/...`) is defined in [Third-Party Vendor Software Lifecycle Strategy](third-party-software-lifecycle-strategy.md) rather than repeated here, so the two documents do not drift against each other.

The repository must separate immutable vendor/IBM input from site-generated deployment state:

- Immutable: downloaded media, service packages, HOLDDATA, checksums, signatures, documentation snapshots.
- Generated: CSI templates, DDDEF templates, workflow variables, JCL, clone manifests, rendered RACF commands, and evidence.

## z/OS Maintenance Model

z/OS maintenance should use the same twelve-phase lifecycle as vendor products (see [Vendor Adapter Skeletons](vendor-adapter-skeletons.md#shared-lifecycle-phases)), with one phase intentionally skipped: Classify does not apply, because z/OS base maintenance is always `smpe_managed` -- there is no install-method ambiguity to resolve the way there is for a vendor product.

1. **Discover**
   - Query current FMIDs, SYSMODs, CSI state, target libraries, distribution libraries, HOLDDATA state, APF/LINKLIST/LPA impact, and IPL requirements.

2. **Intake**
   - Register service, HOLDDATA, checksums, SMP/E input, z/OSMF workflow assets, and documentation snapshots in the SMP repository.

3. **Plan**
   - Build a maintenance manifest with affected FMIDs, zones, libraries, PARMLIB/PROCLIB changes, IPL/recycle requirements, and backout posture.

4. **Stage**
   - Stage SMPPTFIN, SMPHOLD, and RECEIVE/APPLY JCL for this maintenance run; run RECEIVE, which stages SYSMODs into SMP/E-controlled state.

5. **Check**
   - Run RECEIVE CHECK, APPLY CHECK, HOLDDATA review, prerequisite checks, and conflict checks.

6. **Apply**
   - Prefer z/OSMF workflows where available and approved. Otherwise submit generated SMP/E JCL.

7. **Verify**
   - Verify CSI state, target libraries, reports, IPL/recycle completion, subsystem readiness, and clone compatibility.

8. **Recover**
   - RESTORE SMP/E elements where supported, restore PARMLIB/PROCLIB/APF/LINKLIST members from backup, or reactivate the prior service level when verification fails.

9. **Accept**
   - ACCEPT only after soak validation and explicit approval.

10. **Publish**
    - Publish service level metadata back to the internal SMP repository.

11. **Evidence**
    - Publish the maintenance run's evidence bundle: SMP/E reports, rendered JCL, JES output, before/after snapshots, and approvals.

Diagram source: [docs/diagrams/platform-maintenance-flow.mmd](diagrams/platform-maintenance-flow.mmd)

## Clone Factory Model

The clone factory should produce self-contained development clones from repository manifests.

Clone inputs:

- z/OS service level.
- Middleware service levels.
- Vendor product service levels.
- VM guest size and topology.
- VM guest identity.
- VM guest disk and volume layout.
- Dataset naming roots.
- Volume and storage model.
- JES policy.
- TCP/IP and hostname policy.
- Local RACF template.
- Local certificate/keyring template.
- Initial user and service IDs.
- Expiration and cleanup policy.

Clone outputs:

- VM guest definition or provisioning request.
- Clone manifest.
- Allocated or restored datasets.
- Local security database or command stream.
- Local credentials and key material.
- Generated PARMLIB/PROCLIB members.
- Generated z/OSMF workflow variables.
- Generated JCL.
- Evidence bundle.

Clone isolation rules:

- Never require a dedicated LPAR.
- Never reuse production RACF databases.
- Never require enterprise authentication to boot or use the clone.
- Never embed production secrets.
- Never assume production dataset prefixes.
- Never point clone middleware at production Db2, IMS, CICS, MQ, or vendor runtime repositories unless explicitly modeled as a test dependency.
- Every clone must have a local break-glass ID and documented expiration posture.

Diagram source: [docs/diagrams/smp-repo-clone-factory.mmd](diagrams/smp-repo-clone-factory.mmd)

## RACF Maintenance and Control

RACF should be treated as declarative infrastructure for both connected environments and disconnected clones.

The RACF model should include:

- Users.
- Groups.
- Connects.
- Started task identities.
- Dataset profiles.
- General resource profiles.
- FACILITY, OPERCMDS, STARTED, SURROGAT, SERVER, APPL, PROGRAM, UNIXPRIV, and product-specific classes where applicable.
- OMVS segments.
- Keyrings and certificates where applicable.
- Password phrase and initial credential handling.
- Revocation, expiration, and cleanup policy.

RACF automation phases use their own vocabulary rather than the shared twelve-phase lifecycle, because RACF state is authored declaratively rather than received as vendor media: Intake and Classify do not apply (there is no media to register or install method to classify), and Accept/Publish do not apply (there is no SMP/E-style permanence step or dependency-graph entry for a security profile). Model, Render, and Validate together cover what Discover, Plan, Stage, and Check do for vendor products.

1. **Model**
   - Define desired RACF state in environment or clone templates.

2. **Render**
   - Generate RACF commands, review reports, and backout command streams.

3. **Validate**
   - Validate syntax, ownership, naming standards, profile collisions, and dangerous authorities.

4. **Apply**
   - Submit approved RACF command JCL or execute through the approved security administration path.

5. **Verify**
   - Query users, groups, connects, profiles, permits, OMVS segments, started task mappings, and keyrings.

6. **Recover**
   - Submit the backout command stream rendered at Render to restore the prior users, groups, connects, profiles, permits, and keyrings captured in the before snapshot. This is the mechanism the RACF backout drill in Production Readiness Tests exercises.

7. **Evidence**
   - Publish before/after security snapshots and approval evidence.

For disconnected clones, RACF generation is part of clone creation, not a later enterprise integration step.

Diagram source: [docs/diagrams/racf-control-flow.mmd](diagrams/racf-control-flow.mmd)

## Other Platform Surfaces

The platform model should reserve roles for:

- `zos_parmlib`: PARMLIB members, symbolic substitutions, policy validation, and activation notes.
- `zos_proclib`: procedure members, symbolic substitutions, started task procedures, and backout.
- `zos_apf_linklist_lpa`: APF, LINKLIST, LPA, PROGxx, LLA refresh, and IPL/recycle implications.
- `zos_jes`: JES classes, initiators, job card policy, spool assumptions, and clone JES policy.
- `zos_sms_catalog`: SMS classes, ACS assumptions, catalogs, aliases, and volume rules.
- `zos_tcpip`: TCP/IP profiles, hostnames, ports, resolver config, and clone-local networking.
- `zos_uss`: USS filesystem layout, ownership, permissions, tagging, mounts, and clone-local paths.
- `zos_wlm`: service policy references and activation notes.
- `zos_certificates`: local certificates, keyrings, trust stores, and clone-local certificate authority material.

These should be separate from middleware and vendor product roles because they are shared control surfaces.

## Disconnected Clone Governance

Disconnected clones need governance by artifact:

- Clone manifest identifies source service levels.
- Clone manifest records generated identities and authorities.
- Clone manifest records expiration policy.
- Evidence bundle records all generated commands and outputs.
- Clone should be reproducible from repository content and variables.
- Clone should be destroyable without central authority.
- Clone should export evidence back to the repository if connectivity is available, but it must not require that connectivity.

## Near-Term Skeleton

Build these skeletons before product-level code:

1. SMP repository metadata schema.
2. z/OS maintenance manifest schema.
3. Clone manifest schema.
4. RACF template schema.
5. RACF render/apply/verify workflow.
6. Clone factory plan/check/apply/verify/evidence workflow.
7. Platform surface placeholders for PARMLIB, PROCLIB, APF/LINKLIST/LPA, JES, SMS/catalog, TCP/IP, USS, WLM, and certificates.

## Open Questions

1. Are VM guest clones built as full z/OS images, software-instance clones, volume-level clones, or hybrid restored environments?
2. Is z/OSMF Software Management the preferred mechanism for software instance deployment and clone materialization?
3. What is the minimum viable VM guest clone: base z/OS only, z/OS plus middleware, or z/OS plus selected vendor products?
4. How are clone-local secrets generated, rotated, escrowed, and destroyed?
5. How long may disconnected clones live?
6. Which RACF classes are mandatory for the first clone template?
7. Are clones allowed to send evidence back automatically when network is available?
8. What is the allowed divergence between clone maintenance level and central repository maintenance level?
