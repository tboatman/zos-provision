# Pilot and Governance Controls

## Purpose

Flesh out the governance gaps that must close before implementation: gap ownership, pilot selection, approval ownership, documentation synchronization, and atomic backlog structure.

Gap coverage: G-001, G-002, G-023, G-026, and G-027 from [Top-to-Bottom Gap Assessment and Remediation Plan](top-to-bottom-gap-assessment.md).

Review pass: 2026-07-03.

## Pilot Selection Record

The pilot selection record is the first control artifact that turns this program from architecture into a bounded implementation path. It should be created before any role skeletons.

Required fields:

```yaml
pilot_id: pilot-001
status: proposed
decision_owner: TBD
review_date: TBD
environment:
  sysplex: TBD
  lpar_or_guest: TBD
  zos_release: TBD
  security_backend: racf | acf2 | top_secret | unknown
  zosmf_status: zosmf_ready | zosmf_partial | zosmf_unavailable | unknown
clone_target:
  include_clone: true
  zvm_host: TBD
  materialization_method: full_image | software_instance | volume_restore | hybrid | unknown
  independence_requirement: local_authority_only
middleware:
  cics_region: TBD
  ims_path: region | artifact_generation_only | deferred
  db2_path: isolated_schema | sandbox_subsystem | deferred
vendors:
  product_a:
    vendor: bmc | broadcom-ca | rocket
    product: TBD
    target_shape: smpe_or_zosmf
  product_b:
    vendor: bmc | broadcom-ca | rocket
    product: TBD
    target_shape: runtime_customization_heavy
out_of_scope:
  - TBD
approval_gates:
  - gate_id: TBD
    owner_role: TBD
    evidence_required: TBD
```

Pilot selection criteria:

- Uses a sandbox or disposable VM guest, not production.
- Exercises z/OSMF where available, but still records fallback behavior.
- Includes at least one SMP/E or z/OSMF-managed product shape.
- Includes at least one runtime customization-heavy product shape.
- Includes at least one subsystem integration surface: CICS, IMS, or Db2.
- Produces evidence without requiring enterprise authentication from the disconnected clone at runtime.
- Avoids products whose license, media, or support posture is unresolved.

Pilot rejection criteria:

- No approved target environment.
- No approved security execution path.
- No way to retrieve or snapshot product media and docs.
- No preflight evidence for SSH, Python, ZOAU, JES, data set access, and z/OSMF where required.
- No recoverability classification for planned changes.

## Decision Log Format

Every material design choice should be captured as a decision record. These can later live under `docs/decisions/`, but the required shape is:

```yaml
decision_id: ADR-0001
title: TBD
status: proposed | accepted | superseded | rejected
date: TBD
owners:
  - role: TBD
context: TBD
decision: TBD
alternatives_considered:
  - option: TBD
    reason_not_selected: TBD
consequences:
  positive:
    - TBD
  negative:
    - TBD
related_gaps:
  - G-000
related_docs:
  - docs/TBD.md
```

Decision records should be required for:

- Pilot scope.
- Security backend and first implementation path.
- z/VM clone lifecycle interface.
- Clone materialization method.
- z/OSMF minimum required capability.
- Db2 execution method.
- Vendor pilot product selection.
- Any waiver of a critical gap.
- Any use of quarantined OSS or screen automation.

## Approval Ownership Matrix

Initial owner roles:

| Gate class | Required owner role | Evidence required |
| --- | --- | --- |
| Security user, group, profile, permit, keyring, or certificate changes | Security owner | Rendered commands, before snapshot, backout commands, after verification query |
| APF, LINKLIST, LPA, PROGxx, PARMLIB, or PROCLIB changes | z/OS system programming owner | Before member or list, rendered update, activation plan, backout member or list |
| z/OSMF workflow registration or execution | z/OSMF service owner and product owner | Workflow definition, variables, API identity, workflow run output |
| SMP/E APPLY | System programming owner and product owner | RECEIVE/APPLY CHECK output, HOLDDATA review, deployment manifest |
| SMP/E ACCEPT | System programming owner, product owner, change owner | Soak evidence, backout impact statement, ACCEPT CHECK output |
| CICS region or CMCI resource changes | CICS owner | Before resource snapshot, rendered change, after resource snapshot |
| IMS RECON, DBRC, DBD, PSB, ACB, or catalog changes | IMS owner | Generated artifacts, command/JCL output, recovery classification |
| Db2 DDL, DCL, BIND, REBIND, FREE, utility, or subsystem command | Db2 owner | SQL or bind input, catalog before/after snapshot, recovery classification |
| Vendor runtime hooks | Product owner and affected subsystem owner | Product definition, runtime overlay, subsystem impact list |
| Disconnected clone template | Platform owner and security owner | Clone manifest, local authority template, expiration policy |
| OSS adoption into runtime path | Security owner and platform owner | License, version pin, mirror plan, support owner |

## Documentation Sync Checklist

Every design change must answer these questions before commit:

- Does the README need a new or changed link?
- Does the main architecture need a changed assumption, requirement, role, flow, or readiness gate?
- Does the top-to-bottom gap register need a gap opened, closed, split, or linked?
- Does a vendor, platform, IBM, z/VM, OSS, or sidequest doc need the same decision?
- Does a standalone `.mmd` diagram need to change?
- Does the executive overview still tell the same story as the master combined flow?
- Does the change create or retire an approval gate?
- Does the change affect pilot scope or readiness?
- Does the change introduce a new artifact schema or evidence expectation?

Minimum local checks before commit:

```text
git diff --check
rg -n '<inline-diagram-pattern>' docs --glob '*.md'
rg -n '[^\x00-\x7F]' <new-or-touched-files>
```

## Atomic Backlog Unit

Each work package should fit in one atomic commit and should declare:

```yaml
work_id: WP-0001
title: TBD
related_gaps:
  - G-000
inputs:
  - TBD
outputs:
  - TBD
docs_to_update:
  - README.md
  - docs/TBD.md
diagrams_to_update:
  - docs/diagrams/TBD.mmd
validation:
  - git diff --check
  - no inline mermaid in Markdown
blocked_by:
  - TBD
done_when:
  - TBD
```

Work packages should stay small enough that a reviewer can answer:

- What gap did this close?
- What evidence changed?
- Which docs were synchronized?
- Which future implementation step is now less ambiguous?

## Immediate Actions

1. Create `docs/decisions/` only when the first decision record is ready.
2. Draft the pilot selection record before any executable skeleton work.
3. Assign provisional owner roles for the approval matrix, even if named people are not known yet.
4. Use the documentation sync checklist on every future design commit.
5. Split implementation backlog work by gap ID rather than by vendor brand alone.
