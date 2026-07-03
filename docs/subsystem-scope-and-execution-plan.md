# Subsystem Scope and Execution Plan

## Purpose

Flesh out the CICS, IMS, Db2, and MQ gaps that block role design and pilot selection.

Gap coverage: G-014, G-015, G-016, and G-017 from [Top-to-Bottom Gap Assessment and Remediation Plan](top-to-bottom-gap-assessment.md).

Review pass: 2026-07-03.

## Decision Flow

Diagram source: [docs/diagrams/subsystem-gap-decision-flow.mmd](diagrams/subsystem-gap-decision-flow.mmd)

## Shared Subsystem Rule

Subsystem automation must declare three things before any role is written:

- State authority: the source used to decide current state.
- Execution path: the approved method used to apply or test a change.
- Evidence path: the output that proves the change worked or that no change was needed.

If any of those are missing, the subsystem is discovery-only for the pilot.

## CICS Plan

Gap: G-016.

Initial posture: include one CICS test region in the pilot if CMCI or an approved CSD batch path is available.

Required decisions:

| Decision | Options | Preferred first answer |
| --- | --- | --- |
| Resource authority | CMCI, CSD batch utility, site process | CMCI where approved |
| Region provisioning | IBM CICS provisioning modules, generated JCL, site skeletons | IBM CICS modules where they match site standards |
| Runtime resource changes | CMCI create/update/delete/action, CSD batch, manual | CMCI for pilot resources |
| Verification | CMCI get, CEMT-style command output, JES output, logs | CMCI get plus startup message evidence |
| Backout | CMCI delete/revert, CSD restore, dataset restore | CMCI revert for pilot resources |

Minimum CICS manifest:

```yaml
cics_region:
  region_name: TBD
  applid: TBD
  cics_release: TBD
  cmci_endpoint: TBD
  definition_authority: cmci | csd_batch | site_process | unknown
  datasets:
    region_hlq: TBD
    csd: TBD
    catalogs: []
  resources:
    groups: []
    programs: []
    transactions: []
    files: []
    db2conn: []
  verification:
    probes: []
  backout:
    class: TBD
```

Pilot constraint: avoid destructive catalog recreation unless the region is disposable.

## IMS Plan

Gap: G-017.

Initial posture: include IMS artifact generation before full IMS region lifecycle if command authority or RECON risk is not approved.

Required decisions:

| Decision | Options | Preferred first answer |
| --- | --- | --- |
| IMS scope | Full region, artifact generation only, deferred | Artifact generation or disposable region |
| Command path | Type-2, type-1, generated JCL, site wrapper | Type-2 where approved |
| Artifact path | DBD/PSB/ACB generation, IMS catalog DDL, DBRC | Start with DBD/PSB/ACB if low-risk |
| DBRC/RECON changes | Allowed, sandbox only, deferred | Sandbox only or deferred |
| Verification | IMS command output, generated library checks, catalog query | Generated artifact and command evidence |

Minimum IMS manifest:

```yaml
ims_target:
  imsid: TBD
  scope: region | artifact_generation_only | deferred
  ims_release: TBD
  command_path: type2 | type1 | jcl | site_wrapper | unknown
  libraries:
    reslib: TBD
    proclib: TBD
    dbdlib: TBD
    psblib: TBD
    acblib: TBD
  dbrc:
    recon_datasets: []
    change_allowed: false
  catalog:
    enabled: TBD
  verification:
    probes: []
  backout:
    class: TBD
```

Pilot constraint: RECON and DBRC changes require explicit approval even in sandbox.

## Db2 Plan

Gap: G-014.

Initial posture: include one isolated schema or sandbox subsystem path. No IBM Db2 DevOps tooling is assumed.

Required decisions:

| Decision | Options | Preferred first answer |
| --- | --- | --- |
| SQL runner | DSNTEP2, DSNTEP4, DSNTIAD, site runner | Site-approved runner, otherwise DSNTEP2 or DSNTEP4 |
| Command path | DSN command processor JCL, site wrapper, REST if approved | DSN command processor JCL |
| Bind path | Generated bind JCL, site bind process | Generated bind JCL in sandbox |
| Catalog snapshot | Static SQL queries, site reports | Static SQL query set |
| Destructive changes | Allowed with gate, sandbox only, prohibited | Prohibited until backout class exists |

Minimum Db2 manifest:

```yaml
db2_target:
  subsystem_id: TBD
  data_sharing_group: TBD
  scope: isolated_schema | sandbox_subsystem | deferred
  execution:
    sql_runner: dsntep2 | dsntep4 | dsntiad | site_runner | unknown
    command_path: dsn_jcl | site_wrapper | unknown
    bind_path: generated_jcl | site_process | unknown
  naming:
    schema: TBD
    qualifier: TBD
    owner: TBD
    collection: TBD
  artifacts:
    ddl: []
    dcl: []
    bind: []
    utilities: []
  verification:
    catalog_queries: []
  backout:
    class: TBD
```

Pilot constraint: destructive DDL, REVOKE, FREE, mass REBIND, and utilities that alter recoverability are blocked until explicit backout classification exists.

## MQ Plan

Gap: G-015.

Initial posture: defer MQ from the first pilot unless a selected vendor product requires MQ hooks. Keep MQ modeled as an explicit dependency surface so it does not appear accidentally through vendor products.

Required decisions:

| Decision | Options | Preferred first answer |
| --- | --- | --- |
| Pilot scope | Include, dependency-only, defer | Dependency-only |
| Queue manager authority | Commands, JCL, MQSC, site wrapper | Site-approved command or JCL path |
| Security | CHLAUTH, certificates, RACF/ACF2/TSS profiles | Discover only |
| Verification | DISPLAY commands, queue manager status, channel status | DISPLAY command evidence |
| Vendor hook handling | Product-specific queue definitions, exits, agents | Product-definition dependency only |

Minimum MQ manifest if included:

```yaml
mq_target:
  qmgr: TBD
  scope: include | dependency_only | deferred
  command_path: mqsc_jcl | operator_command | site_wrapper | unknown
  libraries: []
  started_tasks: []
  channels: []
  certificates: []
  security: []
  verification:
    probes: []
  backout:
    class: TBD
```

Pilot constraint: MQ is not a first-wave build target unless the pilot vendor products require it and an MQ owner approves the command and evidence path.

## Cross-Subsystem Manifest Rules

- Every subsystem manifest must declare `scope`.
- Every apply-capable subsystem action must declare an owner, approval gate, and backout class.
- Vendor product hooks must reference the target subsystem manifest rather than embedding subsystem details inside a vendor-only file.
- If a subsystem is `deferred`, product definitions may list it only as an unresolved dependency or out-of-scope hook.
- Verification must include before and after evidence for any runtime hook.

## Immediate Actions

1. Decide whether the first pilot includes full CICS, IMS, and Db2, or CICS plus IMS artifact-only plus Db2 isolated schema.
2. Decide MQ as dependency-only or included.
3. Draft pilot subsystem manifests using placeholder values.
4. Add subsystem manifests to the pilot selection record.
5. Reject any vendor pilot product that requires an unapproved subsystem path.
