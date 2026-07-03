# BMC Read-Only Configuration Unwinding Plan

## Purpose

Flesh out the first vendor read-only unwinding plan for BMC products before any BMC installation or maintenance automation is designed.

Gap coverage: G-003, G-004, and G-018 from [Top-to-Bottom Gap Assessment and Remediation Plan](top-to-bottom-gap-assessment.md). This plan also supports the BMC-specific intake docs:

- [BMC Documentation Acquisition Plan](bmc-documentation-acquisition-plan.md)
- [BMC First-Wave Documentation Intake](bmc-first-wave-documentation-intake.md)
- [BMC Public Documentation Collection](bmc-public-documentation-collection.md)
- [BMC Alternative Documentation Sources](bmc-alternative-documentation-sources.md)

Review pass: 2026-07-03.

## Scope

First BMC families to unwind:

- BMC AMI Ops monitors.
- BMC AMI Ops Automation.
- BMC AMI DevX.
- BMC Control-M suite, including Control-M for z/OS and INCONTROL lineage where present.

This is read-only discovery. It must not submit install jobs, alter runtime libraries, change security, recycle started tasks, or update product configuration.

## Objectives

1. Identify installed BMC products, versions, FMIDs, maintenance levels, and aliases.
2. Locate BMC SMP/E CSIs, target zones, distribution zones, and HOLDDATA evidence.
3. Locate BMC runtime libraries, sample libraries, parameter libraries, USS paths, Java paths, panels, skeletons, CLIST/REXX assets, and product documentation snapshots.
4. Locate BMC started tasks, PROCLIB members, PARMLIB hooks, APF/LINKLIST/LPA entries, exits, scheduler jobs, and automation hooks.
5. Locate BMC integrations into CICS, IMS, Db2, MQ, TCP/IP, security products, and z/OSMF.
6. Capture current configuration without judging whether it is correct.
7. Produce candidate BMC product definitions for human review.

## Read-Only Discovery Surfaces

| Surface | Data to collect | Candidate method | Evidence |
| --- | --- | --- | --- |
| SMP/E CSI | FMIDs, SYSMODs, target and distribution zones, DDDEFs | SMP/E LIST jobs or site CSI query jobs | Job output and parsed summary |
| Product HLQs | Base, runtime, sample, parameter, panel, skeleton, CLIST/REXX libraries | Catalog searches and dataset pattern inventory | Dataset list with volumes and attributes |
| APF | Authorized BMC load libraries | PROGxx, dynamic APF query, or site report | Before snapshot |
| LINKLIST and LPA | Runtime library exposure | PROGxx, LNKLST display, LPALST/IEALPAxx review | Before snapshot |
| PROCLIB | Started task procedures and symbolic overrides | Read approved PROCLIB members | Member snapshot |
| PARMLIB | Product hooks, automation hooks, subsystem references | Read approved PARMLIB members | Member snapshot |
| Started tasks | Product STCs, job names, user IDs, runtime status | SDSF/JES queries, operator display commands where approved | Status snapshot |
| Security | STC IDs, dataset profiles, FACILITY/OPERCMDS/SERVER/APPL or equivalent permits | RACF/ACF2/Top Secret query path only | Security snapshot |
| CICS | PLT/SIT references, resource definitions, exits, transactions, programs | CMCI queries or CSD extract where approved | CICS snapshot |
| IMS | PROCLIB hooks, exits, commands, DBRC/catalog references | IMS command or dataset read path | IMS snapshot |
| Db2 | Plans, packages, collections, grants, repository objects, bind jobs | Batch SQL/catalog query path | Catalog snapshot |
| USS and Java | Product USS trees, scripts, jars, permissions, file tags | USS find/stat/read-only inventory | USS snapshot |
| z/OSMF | Workflow definitions, workflow history, PSI/software-management evidence | z/OSMF read-only queries | Workflow inventory |
| Historical jobs | Install, customization, maintenance, bind, and recovery jobs | JES archive, scheduler, or site runbook review | Job inventory |

## Three-Pass Unwinding

### Pass 1: Inventory

Collect without attribution:

- All candidate BMC libraries.
- All candidate BMC FMIDs and SYSMODs.
- All BMC-named started tasks and procedures.
- All BMC parameter members.
- All BMC-related APF, LINKLIST, LPA, PARMLIB, PROCLIB, and security references.
- All BMC-related CICS, IMS, Db2, MQ, TCP/IP, USS, Java, and automation hooks.
- All current and historical BMC install/customization jobs.

Output: raw inventory evidence and a `bmc-observed-artifacts.yml` candidate file.

### Pass 2: Attribution

Classify each observed artifact:

- BMC product base.
- BMC common infrastructure.
- AMI Ops monitor-specific runtime.
- AMI Ops Automation-specific runtime.
- AMI DevX-specific runtime.
- Control-M or INCONTROL-specific runtime.
- CICS integration.
- IMS integration.
- Db2 integration.
- MQ or TCP/IP integration.
- Security integration.
- Local site override.
- Historical/dead configuration.
- Unknown.

Output: ownership map and unknown-config backlog.

### Pass 3: Normalization

Turn approved findings into product-definition candidates:

- Product identity and aliases.
- Version and FMID evidence.
- Install method classification.
- Media and documentation gaps.
- Runtime overlay.
- Verification probes.
- Backout notes.
- Evidence references.

Output: candidate product definition drafts. These remain non-executable until product owners approve them.

## BMC Family Notes

### AMI Ops Monitors

Likely surfaces:

- SMP/E FMIDs and target libraries.
- Product parameter members.
- Started tasks.
- Db2, CICS, IMS, MQ, TCP/IP, or z/OS monitor hooks depending on licensed components.
- Common services or shared BMC infrastructure.
- Panels, CLIST/REXX, and ISPF assets.

Priority evidence:

- Which monitors are installed.
- Which subsystems each monitor attaches to.
- Which runtime libraries are APF-authorized or exposed through LINKLIST/STEPLIB.
- Which product commands prove runtime health.

### AMI Ops Automation

Likely surfaces:

- Automation started tasks.
- Rules, execs, policies, or automation members.
- Message and command authority.
- OPERCMDS, FACILITY, SERVER, SURROGAT, or equivalent security classes.
- Integration with NetView, console automation, scheduler, or product-specific automation services.

Priority evidence:

- Automation authority boundaries.
- Rule/policy libraries and ownership.
- Recovery path if automation hooks are removed or disabled.

### AMI DevX

Likely surfaces:

- Multiple subproducts and aliases.
- ISPF panels, skeletons, CLIST/REXX.
- Compilers, build tooling, debug tooling, source management, test tooling, and possible distributed components.
- CICS, IMS, and Db2 development hooks depending on product mix.

Priority evidence:

- Licensed subproduct inventory.
- Local aliases and acquired names.
- Which components are mainframe-only versus distributed or workstation-integrated.

### Control-M Suite

Likely surfaces:

- Control-M for z/OS and INCONTROL component libraries.
- Scheduler started tasks.
- Agent or distributed integration endpoints.
- Calendars, tables, scheduling definitions, IOA or shared infrastructure where present.
- Security definitions and operator authorities.
- Automation API or distributed orchestration dependencies where used.

Priority evidence:

- Mainframe versus distributed responsibility boundary.
- Scheduler definition custody.
- Which components are safe to inspect read-only.
- Whether any product update requires distributed-side coordination.

## Product Definition Candidate Fields

Each candidate BMC product definition should include:

```yaml
product:
  vendor: bmc
  product_code: TBD
  product_name: TBD
  aliases: []
  family: ami_ops | ami_ops_automation | ami_devx | control_m | common
observed_state:
  fmids: []
  smpe_zones: []
  libraries: []
  started_tasks: []
  parameter_members: []
  security_references: []
  subsystem_hooks:
    cics: []
    ims: []
    db2: []
    mq: []
  uss_paths: []
  historical_jobs: []
classification:
  install_method: unknown_pending_discovery
  confidence: low | medium | high
  unknowns: []
evidence:
  inventory_files: []
  owner_review: TBD
```

## Safety Rules

- Use only read-only jobs, queries, and dataset reads.
- Do not infer install method from dataset names alone.
- Do not treat public documentation as proof of local install state.
- Do not treat SMP/E state as complete until runtime hooks are reconciled.
- Do not classify local overrides as vendor base without owner review.
- Do not run BMC utilities that alter repositories, parameters, schedules, or runtime state.
- Do not capture secrets, license keys, passwords, or private certificates into evidence.

## Exit Criteria

This gap is meaningfully reduced when:

- The first BMC product inventory exists.
- Unknown BMC artifacts are listed rather than hidden.
- At least two BMC product candidates have observed-state maps.
- Each candidate has a tentative install method and confidence level.
- Each candidate lists required canonical docs and media still missing.
- Product owners can approve, reject, or correct the captured model.

## Immediate Actions

1. Define read-only dataset and CSI query jobs for BMC discovery.
2. Build a BMC HLQ and started-task inventory from site evidence.
3. Pick two BMC candidates for deeper unwinding: one AMI Ops or AMI Ops Automation product and one Control-M or AMI DevX product.
4. Reconcile public BMC docs against installed evidence only after local inventory is captured.
5. Feed approved findings into product definition schema examples.
