# Automation Contract and Security Gap Plan

## Purpose

Flesh out automation and security gaps that must close before role design: z/OSMF capability, IBM Ansible collection contracts, execution environment, managed-node preflight, security backend abstraction, RACF-first clone template, z/VM clone path, and clone materialization.

Gap coverage: G-006, G-007, G-008, G-009, G-010, G-011, G-012, and G-013 from [Top-to-Bottom Gap Assessment and Remediation Plan](top-to-bottom-gap-assessment.md).

Review pass: 2026-07-03.

## z/OSMF Capability Checklist

Each target environment should be classified as:

- `zosmf_ready`: required APIs are enabled, authenticated, authorized, and tested.
- `zosmf_partial`: z/OSMF exists but one or more required services are missing or blocked.
- `zosmf_unavailable`: z/OSMF cannot be used for this environment.
- `unknown`: no verified evidence yet.

Required capability fields:

| Capability | Evidence required | Blocking if absent |
| --- | --- | --- |
| Authentication | Auth method, user or technical ID, certificate or token model | Yes |
| TLS and certificates | Endpoint, trust chain, keyring or truststore notes | Yes |
| Workflow service | Ability to create, start, inspect, and delete workflow instances | Yes for z/OSMF-managed products |
| Workflow variable binding | Variable file format and substitution evidence | Yes for workflow-driven install |
| Jobs API | Submit, inspect, retrieve spool, and classify return codes | Yes unless JES is only reached through SSH modules |
| Files and data sets APIs | Read/write USS and MVS artifacts where required | Yes for workflow staging paths |
| Console or operator command API | Command execution and output retrieval where approved | No if SSH or Ansible module path is approved |
| Software Management | Software instance or portable software instance capability | Yes for PSI or z/OSMF software-management clone/install path |
| Security Configuration Assistant | Security-requirement validation output | No, but preferred where available |
| Audit output | Workflow logs, API response capture, and run identifiers | Yes |

z/OSMF should be preferred where usable, but z/OSMF completion is never sufficient by itself. Every workflow-driven operation still needs post-state verification through CSI, datasets, subsystem queries, product commands, or evidence probes.

## IBM Ansible Collection Contract

For each collection, capture:

```yaml
collection: ibm.ibm_zos_core
version: TBD
source: TBD
control_node_requirements:
  python: TBD
  ansible_core: TBD
managed_node_requirements:
  python: TBD
  zoau: TBD
  environment_variables: []
modules:
  - name: TBD
    purpose: TBD
    idempotency: full | partial | none | unknown
    return_values: []
    authority_required: []
    evidence_available: []
    known_limits: []
```

Collections to capture first:

- `ibm.ibm_zos_core`
- `ibm.ibm_zos_cics`
- `ibm.ibm_zos_ims`
- `ibm.ibm_zosmf`
- IBM Z System Automation collection, if licensed and relevant.
- Z HMC collection, if hardware-adjacent orchestration is relevant.
- Local-built `ibm.zvm_ansible`, only if support posture is acceptable.

Contract decisions:

- If a module can prove state, prefer it over generated JCL.
- If a module only submits work, require a separate verification probe.
- If a module has unclear idempotency, treat it as an execution helper, not as state authority.
- If a collection is not supported or not pinned, it cannot be part of a production path.

## Control-Node Execution Environment

The control-node contract should eventually become an execution-environment manifest. Before implementation, document:

```yaml
control_node:
  production_runtime: ansible_automation_platform | ansible_core
  developer_runtime: macos_allowed
  python_version: TBD
  ansible_core_version: TBD
  collections:
    - name: ibm.ibm_zos_core
      version: TBD
  helper_tools:
    zowe_cli:
      allowed: optional
      use: evidence_or_probe_helper
    py3270:
      allowed: quarantined
      use: manual_discovery_only
  linting:
    ansible_lint: TBD
    yamllint: TBD
```

Hard rule: macOS development is acceptable only if it uses the same pinned Ansible, Python, collection, and lint behavior as the production control path.

## Managed-Node Preflight Evidence

A managed z/OS target must produce evidence for:

- SSH login.
- Effective user and OMVS segment.
- Python path and version.
- ZOAU path, version, and environment variables.
- Codepage and file tagging behavior.
- Temporary USS path.
- MVS data set allocation and deletion in approved HLQ.
- USS file write, chmod, chown if needed, and cleanup.
- JCL submit, poll, and spool retrieval.
- Operator command authority where in scope.
- z/OSMF endpoint status where in scope.
- SMP/E CSI read access where in scope.
- RACF, ACF2, or Top Secret query path where in scope.
- CICS CMCI endpoint where in scope.
- IMS command path where in scope.
- Db2 command, SQL, bind, and catalog query path where in scope.

Preflight output should be an evidence artifact, not just terminal text.

## Security Backend Abstraction

The first implementation path may be RACF, but the design cannot assume all estates are RACF.

Common security model:

```yaml
security_backend:
  type: racf | acf2 | top_secret
  authority_model:
    users: supported | discover_only | deferred
    groups: supported | discover_only | deferred
    dataset_profiles: supported | discover_only | deferred
    general_resources: supported | discover_only | deferred
    started_tasks: supported | discover_only | deferred
    omvs_segments: supported | discover_only | deferred
    keyrings_and_certificates: supported | discover_only | deferred
  execution_path:
    render_commands: true
    apply_commands: false
    query_state: true
  approval_owner: security
```

RACF-first scope:

- Render RACF commands from declarative templates.
- Render backout commands from before snapshots.
- Query users, groups, connects, dataset profiles, general resource profiles, STARTED mappings, OMVS segments, keyrings, and certificates.
- Apply only after security owner approval.

ACF2 and Top Secret first scope:

- Discover installed backend and command/query paths.
- Capture equivalent resource classes and naming rules.
- Do not claim command parity until a security owner validates it.

## Clone-Local Security Template

Disconnected clone security is not a normal enterprise integration. It must be local, generated, reviewable, and destroyable.

Minimum template fields:

```yaml
clone_security_template:
  template_id: TBD
  backend: racf
  local_authority_only: true
  users:
    - id: TBD
      purpose: break_glass | automation | developer | started_task
      expire_after: TBD
  groups: []
  dataset_profiles: []
  general_resources: []
  started_tasks: []
  omvs_segments: []
  certificates:
    local_ca: true
    keyrings: []
  evidence:
    render_commands: true
    render_backout: true
    verify_after_apply: true
```

Clone-local security must never embed production secrets or depend on central authentication at runtime.

## z/VM Clone Lifecycle Decision

The current candidate path is:

1. Ansible control node.
2. Linux Management Access Point on target z/VM LPAR.
3. Feilong zthin and `smcli`.
4. z/VM SMAPI.
5. Directory manager and CP/CMS operations.
6. z/OS VM guest creation, disk operations, NIC configuration, reader input, IPL, shutdown, and expiration.

Required evidence before adoption:

- z/VM release and maintenance level.
- SMAPI enabled and approved.
- Directory manager product and automation interface.
- Management Access Point Linux guest availability.
- Feilong zthin install path and support owner.
- `smcli` version and command coverage.
- Required privilege classes.
- SSL/TLS and certificate requirements for SMAPI.
- Local extensions required by IBM `zvm_ansible`, if any.
- IPL and shutdown pattern for z/OS guests.
- Evidence that clone-local z/OS authority is independent from central authority.

Decision outcomes:

- Adopt Feilong plus IBM z/VM Ansible source as internal-supported path.
- Wrap Feilong directly with site-owned modules or service API.
- Use a site-owned z/VM provisioning service instead.
- Block disconnected clone automation until z/VM authority and support posture are resolved.

## Clone Materialization Decision

The first clone must choose one materialization method:

| Method | Fit | Risk |
| --- | --- | --- |
| `full_image` | Fast recreation from known-good guest image | Requires image custody, storage, identity rewriting, and service-level tracking |
| `software_instance` | Aligns with z/OSMF Software Management direction | Requires z/OSMF software instance maturity and workflow evidence |
| `volume_restore` | Matches traditional z/OS backup/restore patterns | Storage-specific and may be hard to make portable |
| `hybrid` | Realistic for early pilots | More moving parts and more evidence required |

Minimum viable clone recommendation for pilot design: use the simplest method the site can destroy and recreate repeatedly, then record it honestly. Do not choose the theoretically best method if it blocks evidence collection.

## Immediate Actions

1. Create a z/OSMF capability checklist for the target sandbox.
2. Capture `ansible-doc` output for pinned IBM collections once the control-node environment exists.
3. Draft the control-node execution environment manifest.
4. Convert preflight requirements into an evidence checklist.
5. Draft the RACF-first clone-local security template.
6. Confirm whether Feilong plus IBM z/VM Ansible is supportable or only reference material.
7. Decide the first clone materialization method for the pilot.
