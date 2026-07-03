# OSS and Sidequest Disposition Plan

## Purpose

Flesh out the remaining OSS and sidequest gaps so useful tools and orphan products do not enter the runtime path without license, custody, support, and evidence decisions.

Gap coverage: G-024 and G-025 from [Top-to-Bottom Gap Assessment and Remediation Plan](top-to-bottom-gap-assessment.md).

Review pass: 2026-07-03.

## OSS Disposition States

Every OSS candidate should be assigned exactly one state:

| State | Meaning | Automation impact |
| --- | --- | --- |
| `watchlist` | Interesting but not currently needed | No runtime use |
| `research_candidate` | Needs license, version, dependency, or support review | No runtime use |
| `control_node_helper` | Allowed on control node for inspection, evidence, or development support | Not a state authority unless explicitly approved |
| `managed_node_helper` | Allowed on z/OS USS, Linux on Z, or z/VM MAP after mirror and install approval | Must be pinned and mirrored |
| `validation_harness` | Allowed to verify outcomes but not perform installation | Cannot apply changes |
| `clone_factory_candidate` | Candidate for z/VM guest lifecycle or clone support | Requires security and platform owner approval |
| `quarantined` | Risky, brittle, unclear license, screen automation, emulator, stale, or sample-only | Cannot be used in executable automation |
| `rejected` | Not useful or not acceptable | Remove from active planning |

## OSS Adoption Record

Minimum shape:

```yaml
oss_tool:
  name: TBD
  upstream_url: TBD
  license: TBD
  version_or_commit: TBD
  disposition: watchlist | research_candidate | control_node_helper | managed_node_helper | validation_harness | clone_factory_candidate | quarantined | rejected
  runtime_lane: control_node | zos_uss | linux_on_z | zvm_map | validation_host | none
  mirror_required: true
  internal_mirror: TBD
  dependencies: []
  security_review: not_started | requested | approved | rejected
  support_owner: TBD
  allowed_uses: []
  prohibited_uses: []
  related_gaps: []
```

## Initial OSS Dispositions

| Tool | Initial disposition | Reason | Next action |
| --- | --- | --- | --- |
| Feilong | `clone_factory_candidate` | Directly relevant to z/VM clone lifecycle through zthin and SMAPI path | Verify support owner, install path, version pin, security posture, and IBM z/VM Ansible fit |
| IBM `zvm_ansible` repositories | `research_candidate` | IBM-owned source but not confirmed as supported or Galaxy-published | Build locally only in disposable workspace and capture module contracts |
| Zowe CLI | `control_node_helper` | Useful for jobs, data sets, USS, and z/OSMF-adjacent inspection | Pin version and decide whether it is evidence helper only |
| Zowe API Mediation Layer | `research_candidate` | Useful only if site already standardizes on Zowe | Do not make prerequisite without site decision |
| Zowe Explorer | `control_node_helper` | Human inspection and developer workflow | Keep out of unattended provisioning |
| zopen community tooling | `managed_node_helper` | Potential z/OS USS package source | Evaluate internal mirror and package custody before use |
| zopen Git port | `managed_node_helper` | Useful for clone developer experience | Optional, not core provisioning |
| Galasa | `validation_harness` | Candidate for post-provision scenario tests | Evaluate after pilot expectations are stable |
| py3270 and x3270 or s3270 | `quarantined` | Brittle screen automation; useful only when no API/JCL/command path exists | Manual discovery only unless exception approved |
| Hercules Hyperion | `quarantined` | Lab or education only; not suitable for licensed z/OS/vendor testing without legal approval | Keep out of project runtime path |

## Sidequest Promotion States

Sidequests should use these states:

- `sidequest_reference`: researched for context only.
- `custody_unknown`: owner, support, or license unclear.
- `installed_state_candidate`: product appears installed locally and needs discovery.
- `promotion_candidate`: product has owner, license, docs, and local relevance.
- `promoted_to_product_lifecycle`: product now follows the normal product-definition lifecycle.
- `closed_no_action`: no local relevance or no acceptable license/support path.

## Sidequest Promotion Criteria

A sidequest may be promoted only when all are true:

- Current custodian or license posture is known.
- Product is installed locally or selected for a pilot.
- Product owner or technical owner exists.
- Canonical documentation or site-owned install evidence exists.
- Media, maintenance, or support path is known.
- Installed-state discovery can be performed safely.
- Promotion does not displace first-wave BMC, Broadcom CA, Rocket, IBM, or platform work without explicit decision.

## IOF Disposition

Current IOF state:

```yaml
sidequest:
  product: IOF
  vendor_or_custodian: Fischer International Systems Corporation signal from public web
  state: sidequest_reference
  source_status: not_publicly_open_sourced
  promotion_blockers:
    - local installation not confirmed
    - license and support posture not confirmed directly with Fischer
    - media and maintenance path not confirmed
    - owner not assigned
```

IOF should stay a sidequest unless it is installed on a target image or needed as the first orphan/small-vendor product pilot.

## Immediate Actions

1. Create OSS adoption records for Feilong, Zowe CLI, zopen community tooling, and Galasa.
2. Keep py3270/x3270 and Hercules quarantined.
3. Ask whether Zowe API Mediation Layer exists in the target estate before designing around it.
4. Keep IOF as `sidequest_reference` until local installed-state or owner evidence exists.
5. Add sidequest promotion state to future non-core product investigations.
