# Top-to-Bottom Gap Assessment and Remediation Plan

## Purpose

Record the current gaps across the whole design and convert them into sequenced remediation work before implementation code is written.

This document is the project gap register. It does not replace the architecture, vendor lifecycle strategy, platform clone strategy, IBM Ansible documentation gathering plan, z/VM source ledger, OSS tooling ledger, or vendor-specific documentation intake files. It points to the gaps that cut across those documents and defines the order in which they should be closed.

Review pass: 2026-07-03.

## Remediation Roadmap

Diagram source: [docs/diagrams/gap-remediation-roadmap.mmd](diagrams/gap-remediation-roadmap.mmd)

## Current Top-Level Assessment

The design is broad enough and internally consistent enough to proceed with detailed scoping, but it is not ready for implementation. The missing work is mostly in four categories:

1. Decision gates and source-of-truth artifacts are not concrete enough.
2. Product, platform, security, z/VM, and OSS facts are still unresolved.
3. Normalized schemas are described but not yet specified.
4. Pilot selection has not been made, so the first executable slice is still undefined.

The immediate priority is not to add more flowcharts. The immediate priority is to turn the current design into a gated backlog with evidence requirements, artifact schemas, and a narrow pilot path.

## Detailed Gap Plans

Several high-priority gaps now have dedicated detail plans:

- [Pilot and Governance Controls](pilot-and-governance-controls.md) covers G-001, G-002, G-023, G-026, and G-027.
- [Schema and Evidence Foundation](schema-and-evidence-foundation.md) covers G-005, G-019, G-020, G-021, and G-022.
- [Automation Contract and Security Gap Plan](automation-contract-and-security-gap-plan.md) covers G-006, G-007, G-008, G-009, G-010, G-011, G-012, and G-013.
- [BMC Read-Only Configuration Unwinding Plan](bmc-read-only-unwinding-plan.md) covers the BMC-first path through G-003, G-004, and G-018.

## Gap Register

| ID | Gap | Why it matters | Current evidence | Remediation | Exit criterion | Priority |
| --- | --- | --- | --- | --- | --- | --- |
| G-001 | No canonical gap register or decision log existed before this file | Open questions are spread across many docs and can drift | Architecture, platform, vendor, IBM, z/VM, OSS, and vendor intake docs each carry local open questions | Use this document as the cross-document gap register; update it whenever a gap is opened, closed, split, or promoted to implementation backlog | Every major open item has one ID, owner placeholder, evidence requirement, and exit criterion | Critical |
| G-002 | Pilot scope is not selected | Without a pilot, schema and role design cannot be validated against real product shapes | Architecture recommends a narrow pilot but no product, subsystem, clone, or environment has been selected | Pick one sandbox environment, one VM guest clone target, one CICS region, one IMS or IMS-artifact path, one Db2 isolated schema or subsystem, and two vendor products | Pilot manifest approved and linked from architecture docs | Critical |
| G-003 | Product inventory is not complete | Vendor adapter design depends on actual products, versions, FMIDs, install media, service level, and local runtime hooks | BMC, Broadcom CA, and Rocket first-wave docs identify candidates, but entitlement-backed inventory is pending | Build a product inventory template and collect installed-state evidence from CSIs, HLQs, started tasks, PROCLIB, APF, LINKLIST, CICS, IMS, Db2, USS, and security | Product inventory contains product code, aliases, version, FMIDs, owners, install method, media status, and runtime hooks for first-wave scope | Critical |
| G-004 | Vendor media and documentation acquisition is not entitlement-complete | Public docs are not enough to automate licensed product install and maintenance | Public and alternative source ledgers exist for BMC, Broadcom CA, and Rocket | Execute entitlement-backed acquisition for BMC first, then Broadcom CA and Rocket; snapshot docs and media into depot structure | For pilot products, canonical docs, media, service, HOLDDATA, checksums, install instructions, and license notes are captured | Critical |
| G-005 | Internal SMP/E and vendor depot schema is conceptual | Clone factory and product lifecycle depend on precise metadata and immutable artifact custody | Platform and vendor lifecycle docs define recommended layouts | Specify metadata files for service levels, packages, checksums, HOLDDATA, product BOM, dependency graph, promotion status, and evidence indexes | Schema draft exists with examples for IBM z/OS, IBM middleware, and one vendor product version | Critical |
| G-006 | z/OSMF contract is assumed but not enumerated | z/OSMF is a forward requirement where available, but exact services, APIs, workflow ownership, certificates, and auth are not captured | Architecture requires z/OSMF APIs; IBM Ansible gathering doc lists z/OSMF module docs | Create a z/OSMF capability checklist covering workflows, jobs, files, data sets, software management, console, security, certificates, and API credentials | A target environment can be marked `zosmf_ready`, `zosmf_partial`, or `zosmf_unavailable` with evidence | Critical |
| G-007 | IBM Ansible collection versions and module contracts are not pinned | Role design depends on module names, return values, requirements, and idempotency limits | IBM Ansible documentation gathering doc lists collection URLs and required capture fields | Capture exact versions, dependency matrices, `ansible-doc` output, and execution-environment dependencies | Collection capability map exists for z/OS Core, CICS, IMS, z/OSMF, System Automation, Z HMC, and any local z/VM collection | Critical |
| G-008 | Control-node execution environment is not specified | Production and macOS development must use the same pinned behavior | Architecture lists control-node requirements but no versioned execution environment | Define ansible-core version, Python version, collection versions, linter versions, and optional Zowe CLI or other helper tooling | Execution-environment manifest and developer bootstrap requirements are documented | High |
| G-009 | Managed-node prerequisites are not verified | z/OS automation depends on SSH, Python, ZOAU, codepage behavior, dataset access, JES, and z/OSMF | Architecture and preflight flow describe checks | Turn preflight outputs into a concrete evidence checklist and schema | A sandbox z/OS target can produce a complete preflight evidence bundle | High |
| G-010 | Security model is RACF-centered but must tolerate ACF2 and Top Secret estates | Vendor scope includes Broadcom CA security tooling and real sites may not use RACF | Architecture mentions RACF, ACF2, or Top Secret; platform doc deeply models RACF | Define security backend abstraction and mark RACF as the first implementation path; create ACF2 and Top Secret discovery placeholders | Security model states which backend is authoritative per environment and which operations are supported or deferred | Critical |
| G-011 | RACF clone template schema is not specified | Disconnected clones depend on local users, groups, profiles, keyrings, certificates, and break-glass credentials | Platform doc describes RACF governance and phases | Draft clone-local security schema and command-rendering evidence requirements | First clone security template can render commands and backout commands without applying them | High |
| G-012 | z/VM clone factory path is promising but not supportable yet | Disconnected clones are VM guests, so z/VM authority, SMAPI, directory manager, Feilong, zthin, and `smcli` are central | z/VM ledger identifies IBM repositories and Feilong path | Confirm z/VM host release, SMAPI availability, directory manager, MAP Linux guest, Feilong zthin install, support owner, and privilege model | z/VM clone lifecycle capability map is approved or a different site-owned API is selected | Critical |
| G-013 | Clone materialization method is undecided | A clone may be full image, software instance, volume-level restore, or hybrid; automation differs radically | Platform open questions identify this directly | Decide minimum viable clone shape and storage/network strategy for the first pilot | Clone manifest schema has enough fields to build and destroy the first VM guest clone | Critical |
| G-014 | Db2 execution path is not nailed down | No Db2-specific DevOps tooling is assumed, so generic JCL/SQL/bind patterns must be explicit | Architecture defines Db2 model and role boundaries | Select approved SQL runner, DSN command pattern, bind job templates, catalog snapshot queries, and destructive-change gates | Db2 artifact deployment design can run plan-only validation in sandbox | High |
| G-015 | MQ is in documentation scope but not yet modeled like CICS, IMS, and Db2 | Vendor products may depend on MQ and the IBM doc plan names MQ | IBM Ansible gathering doc lists MQ but architecture has no MQ role/model | Decide whether MQ is first-wave or deferred; if included, add MQ model, flow, and role boundary | MQ is explicitly scoped in or out for pilot and product definitions | Medium |
| G-016 | CICS model needs concrete schema and CMCI fallback rules | Current CICS design is sufficient conceptually but not enough for role inputs | Architecture lists CICS fields and CMCI role | Define CICS region schema, CMCI capability map, CSD fallback rules, and verification probes | CICS test region manifest can be reviewed without code | High |
| G-017 | IMS model needs concrete schema and command/generation fallback rules | IMS has multiple artifact and command paths with different risk profiles | Architecture lists IMS fields and IMS modules | Define IMS region and artifact schemas, type-1/type-2 command strategy, DBRC/RECON gates, and catalog policy | IMS test manifest can be reviewed without code | High |
| G-018 | Configuration unwinding is conceptual only | The hardest vendor problem is installed-state discovery and local override attribution | Vendor lifecycle defines inventory, attribution, normalization passes | Create read-only discovery catalog: which datasets, commands, jobs, and queries collect each surface | First read-only unwinding plan exists for BMC pilot products | Critical |
| G-019 | Product definition schema is not formalized | Vendor adapter and dependency planner need validated data, not prose | Architecture and vendor lifecycle list required fields | Draft YAML schema for product definitions, runtime overlays, dependencies, evidence, and backout classification | Two pilot product definitions can validate against schema in dry form | Critical |
| G-020 | Dependency graph format is not specified | Execution order and recovery depend on product, subsystem, runtime, and clone dependencies | Architecture describes dependency governance; vendor lifecycle suggests graph files | Define minimal dependency graph representation and resolver expectations | Pilot manifest can be generated manually from dependency data | High |
| G-021 | Evidence bundle format is not specified | Trust depends on repeatable evidence across JCL, z/OSMF, Ansible, security, Db2, CICS, IMS, and vendor installs | Architecture lists evidence contents | Define evidence directory layout, manifest file, naming rules, retention, redaction, and checksum strategy | Dry-run evidence bundle can be created from sample artifacts | High |
| G-022 | Recovery and backout classification is not formalized | Some changes are reversible, some are compensating, and some require manual recovery | Architecture and vendor lifecycle list recovery concepts | Define backout classes and require each deployment unit to declare one | Pilot manifest rejects any deployment unit without recovery classification | High |
| G-023 | Approval gate ownership is not assigned | Gates are listed, but owners and enforcement mechanics are not | Architecture lists required approval gates | Add ownership matrix for system programming, security, storage, middleware, Db2, vendor owner, operations, and change control | Each gate has an accountable owner and evidence requirement | High |
| G-024 | OSS tooling is classified but not version-pinned or mirrored | OSS can help, but disconnected clone and production paths need supply-chain controls | OSS tooling discovery ledger exists | Capture versions, licenses, dependencies, install paths, mirror strategy, and allowed runtime lane | Feilong, Zowe CLI, zopen community tooling, Galasa, and py3270 have adoption dispositions | Medium |
| G-025 | IOF is intentionally a sidequest but has no promotion rule | Sidequests can silently become scope unless promotion criteria are explicit | IOF sidequest exists | Define sidequest promotion criteria: custody, license, docs, install state, owner, and pilot relevance | IOF remains quarantined or is promoted through normal product lifecycle with a product definition | Low |
| G-026 | Documentation synchronization is manual | The user requires docs in sync, but there is no documented sync checklist or CI gate | Diagram convention exists; docs are currently maintained manually | Add doc-sync checklist and later CI checks for links, no inline Mermaid, diagram presence, and schema references | Every substantive design change updates affected docs and diagram sources in the same commit | High |
| G-027 | Implementation backlog is not split into safe atomic chunks | Atomic commits are required and the project is too large for coarse milestones | Delivery phases exist but not an issue-sized backlog | Convert remediation phases into issue-sized work packages with dependencies | Backlog contains small, reviewable chunks mapped to gaps and docs | High |

## Remediation Phases

### Phase A: Governance and Source-of-Truth Setup

Close G-001, G-002, G-023, G-026, and G-027.

Deliverables:

- Gap register maintained here.
- Pilot selection record.
- Decision log format.
- Approval ownership matrix.
- Documentation synchronization checklist.
- Atomic backlog structure.

Exit gate: the project can explain what is in scope, what is blocked, who approves each high-risk class, and which document owns each decision.

### Phase B: Artifact and Schema Foundation

Close G-005, G-019, G-020, G-021, and G-022.

Deliverables:

- Internal SMP/E and vendor depot metadata schema.
- Product definition schema.
- Dependency graph schema.
- Deployment manifest schema.
- Evidence bundle schema.
- Backout classification vocabulary.

Exit gate: a pilot manifest can be represented as data before any Ansible role exists.

### Phase C: IBM Automation Contract

Close G-006, G-007, G-008, G-009, G-014, G-015, G-016, and G-017.

Deliverables:

- z/OSMF capability checklist.
- IBM Ansible collection capability map.
- Control-node execution environment plan.
- Managed-node preflight evidence checklist.
- Db2 generic execution standard.
- MQ scope decision.
- CICS and IMS pilot schemas.

Exit gate: the team knows which IBM-supported module, z/OSMF API, JCL path, or explicit gap owns each platform operation.

### Phase D: Security and Clone Factory

Close G-010, G-011, G-012, and G-013.

Deliverables:

- Security backend abstraction.
- RACF-first clone-local security template.
- z/VM clone lifecycle capability map.
- Feilong or alternate z/VM API decision.
- Minimum viable VM guest clone shape.

Exit gate: disconnected clone creation is either technically viable through the selected path or explicitly blocked with evidence.

### Phase E: Vendor Intake and Configuration Unwinding

Close G-003, G-004, G-018, G-024, and G-025.

Deliverables:

- Entitlement-backed product inventory.
- BMC pilot product evidence first, then Broadcom CA and Rocket.
- Read-only configuration unwinding plan.
- OSS adoption disposition table.
- Sidequest promotion rule.

Exit gate: at least two pilot vendor products can move out of `unknown_pending_discovery` or `legacy_manual_capture` into an approved adapter path.

### Phase F: Pilot Design Readiness

Close any remaining critical and high gaps before implementation.

Deliverables:

- Pilot environment manifest.
- Pilot product definitions.
- Pilot clone manifest.
- Pilot security template.
- Pilot deployment manifest.
- Pilot evidence expectations.
- Explicit go or no-go decision for first code skeletons.

Exit gate: implementation can begin with narrowly scoped role skeletons and tests, not broad product automation.

## First Ten Work Packages

1. Create a pilot-selection record with candidate environment, clone target, CICS, IMS, Db2, and two vendor products.
2. Draft product inventory and installed-state intake template.
3. Draft product definition YAML schema.
4. Draft evidence bundle schema and naming convention.
5. Draft z/OSMF capability checklist.
6. Capture IBM Ansible collection versions and `ansible-doc` contracts.
7. Draft z/VM clone lifecycle capability map and Feilong decision record.
8. Draft security backend abstraction with RACF-first command model.
9. Draft BMC read-only configuration unwinding plan for AMI Ops, AMI Ops Automation, AMI DevX, and Control-M.
10. Draft doc-sync and decision-log checklist for every future design change.

## Readiness Statement

Implementation should remain blocked until all critical gaps are closed or explicitly waived. High-priority gaps can be closed during pilot design only if they are outside the pilot path. Medium and low gaps may remain in the backlog if they are not dependencies of the first pilot.

The first implementation target should be a skeleton that validates schemas, renders plans, and produces evidence from static inputs. It should not submit JCL, alter security, drive z/OSMF workflows, or touch vendor software until the pilot manifest, approval gates, and preflight evidence are complete.
