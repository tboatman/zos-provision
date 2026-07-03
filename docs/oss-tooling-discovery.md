# OSS Tooling Discovery

## Purpose

Record open source tools that may help with z/OS, z/VM, IBM subsystem, disconnected clone, validation, and control-node automation.

This is a discovery ledger, not an approval decision. Open source tools can reduce custom code and provide useful evidence surfaces, but they must not bypass canonical IBM or vendor documentation, z/OSMF, SMP/E, site security controls, or product-supported installation methods.

Retrieval pass: 2026-07-03.

## Discovery Flow

Diagram source: [docs/diagrams/oss-tooling-discovery-flow.mmd](diagrams/oss-tooling-discovery-flow.mmd)

Disposition states and sidequest promotion rules are defined in [OSS and Sidequest Disposition Plan](oss-sidequest-disposition-plan.md).

## Ground Rules

- Treat every open source candidate as supply-chain input until license, provenance, support posture, versioning, mirroring, and security review are captured.
- Prefer tools that reduce custom code, improve repeatability, or provide stronger validation evidence.
- Do not let open source helper tooling replace the authoritative installation and maintenance path for z/OS, IBM subsystems, BMC, Broadcom CA, Rocket, or other licensed products.
- Prefer z/OSMF APIs and IBM-supported Ansible collections where they exist.
- Mirror and pin every adopted tool into an internal depot before using it in disconnected clone or product-maintenance automation.
- Keep screen automation, emulator-based testing, and abandoned or unclear projects out of executable automation unless there is a written exception.
- Separate control-node tools, z/OS USS managed-node tools, z/VM clone-factory tools, validation tools, and lab-only tools.

## Initial Classification

| Tool or project | URL | License or status evidence | Best fit | Initial classification | Adoption posture |
| --- | --- | --- | --- | --- | --- |
| Open Mainframe Project Feilong | https://github.com/openmainframeproject/feilong | Apache-2.0; active Open Mainframe Project repository; describes an SDK and API layer for z/VM guest, image, network, and volume operations | z/VM guest lifecycle substrate for clone factory work | `candidate_for_zvm_clone_factory` | High-priority research. Validate with IBM `zvm_ansible`, SMAPI, zthin, `smcli`, site z/VM authority, and security review. |
| IBM `zvm_ansible` and `zvm_ansible_collection` | https://github.com/IBM/zvm_ansible and https://github.com/IBM/zvm_ansible_collection | IBM-owned public repositories; collection source observed outside Ansible Galaxy in the z/VM source pass | Ansible wrapper pattern around z/VM SMAPI and Feilong | `candidate_local_collection` | Build and inspect locally only after support posture is clarified. Capture `ansible-doc` output before any role design. |
| Zowe CLI | https://github.com/zowe/zowe-cli | EPL-2.0; active Zowe ecosystem repository | Control-node probes, job submission, data set and USS inspection, developer utility, evidence fallback | `candidate_control_node_tool` | Useful helper candidate. Do not replace Ansible modules or z/OSMF workflow orchestration. |
| Zowe API Mediation Layer | https://github.com/zowe/api-layer | EPL-2.0; active Zowe ecosystem repository for mainframe REST API mediation | API gateway, catalog, certificate, and REST consolidation if Zowe is already present | `candidate_api_gateway` | Optional integration. Do not make it a prerequisite for the project unless the site already standardizes on Zowe. |
| Zowe Explorer VS Code | https://github.com/zowe/zowe-explorer-vscode | EPL-2.0; active Zowe ecosystem repository | Human inspection of data sets, USS files, and jobs | `human_operator_tool` | Good for developer and operator workflows. Not part of unattended provisioning. |
| zopen community meta repository | https://github.com/zopencommunity/meta | Apache-2.0; active zopen community metadata and package-manager repository | z/OS USS package ecosystem and possible internal package-depot seed | `candidate_managed_node_tooling_source` | Evaluate for offline mirror and approved USS tooling. Avoid uncontrolled internet dependency from managed z/OS nodes. |
| zopen community Git port | https://github.com/zopencommunity/gitport | Apache-2.0; active z/OS Git port repository | Git on z/OS USS for development clones or local inspection workflows | `candidate_uss_tool` | Optional. Useful for clone developer experience, not required for core Ansible execution. |
| Galasa | https://github.com/galasa-dev/galasa | EPL-2.0; active test orchestration framework with mainframe-oriented extension history | Post-provision validation, scenario execution, and evidence collection | `candidate_validation_harness` | Evaluate after pilot provisioning flows exist. Keep separate from installation execution. |
| py3270 and x3270 or s3270 family | https://github.com/py3270/py3270 | BSD-3-Clause Python interface to x3270 or s3270; project activity is lower than core candidates | Legacy terminal automation where no API, command, JCL, or product utility path exists | `last_resort_legacy_screen_automation` | Quarantine by default. Use only for discovery or manual validation unless an exception is approved. |
| SDL Hercules Hyperion | https://github.com/SDL-Hercules-390/hyperion | Active emulator project; license status needs legal review before use | Education, syntax experiments, and non-licensed lab images only | `lab_only_emulation` | Not suitable for z/OS, z/VM, or vendor product deployment testing without explicit legal and licensing approval. |

## Watchlist

| Candidate | Current status | Next action |
| --- | --- | --- |
| COBOL Check | Only weak sample-level references found in this pass; no strong upstream source confirmed | Re-check by exact upstream name and owner before recommending. |
| Tessia | No useful direct repository evidence found in this pass | Re-run discovery with IBM Z provisioning keywords and known organization names. |
| zigi | No useful direct repository evidence found in this pass | Re-run discovery with z/OS Git, ISPF, and repository-management keywords. |
| Other Zowe plugins | Likely relevant, but too broad without exact subsystem needs | Inspect once the first pilot control surfaces are selected. |

## Where These Tools Fit

Open source tooling should be mapped into explicit lanes:

- Control node helper lane: Zowe CLI, Zowe SDKs, local validation scripts, and reporting helpers.
- z/OS managed-node helper lane: approved z/OS USS packages from an internal mirror, not direct internet pulls.
- z/VM clone-factory lane: IBM z/VM Ansible repositories, Feilong, SMAPI, zthin, and `smcli` after support review.
- Validation lane: Galasa or other harnesses that prove provisioning outcomes without owning installation logic.
- Quarantine lane: py3270, x3270 or s3270 automation, emulators, stale projects, unclear licenses, and sample-only code.

The clone-factory lane is the most urgent because disconnected development environments are VM guests. The immediate technical question is whether Feilong plus IBM's z/VM Ansible source can be turned into a supportable internal capability for guest creation, minidisk operations, NIC configuration, spool input, IPL, shutdown, expiration, and evidence capture.

## Required Capture Fields

For each OSS candidate, capture:

- Project name and owner.
- Repository URL and documentation URL.
- License.
- Latest release or commit evidence.
- Maintainer and foundation affiliation, if any.
- Supported platforms.
- Required runtime dependencies.
- Authentication and credential model.
- Network ports and certificate requirements.
- Whether it runs on the control node, z/OS USS, Linux on Z, or z/VM management guest.
- Whether it needs internet access at runtime.
- Internal mirror or depot strategy.
- Version pinning strategy.
- Security review status.
- Support owner inside the project.
- Failure mode and backout posture.
- Whether use is core, optional, validation-only, or quarantined.

## Design Implications

1. Add an OSS tooling lane to documentation intake, separate from IBM and vendor product intake.
2. Add an internal mirror/depot requirement for any OSS package used by the control node, z/OS USS, Linux on Z, or z/VM Management Access Point.
3. Treat Feilong as a candidate z/VM substrate, not merely a documentation source.
4. Treat Zowe CLI as a useful control-node inspection and fallback tool, but keep Ansible and z/OSMF as the primary orchestration model.
5. Treat Galasa as a validation harness candidate after the first provisioning pilots have stable expected outcomes.
6. Keep screen automation and emulators outside the production path unless no better interface exists and the risk is accepted.

## Immediate Next Pass

1. Capture exact release and license evidence for Feilong, Zowe CLI, Zowe API Mediation Layer, zopen community tooling, Galasa, and py3270.
2. Determine whether each candidate can be mirrored internally and installed without internet access.
3. Build a small capability matrix for control-node, z/OS USS, Linux on Z, z/VM Management Access Point, and validation-host use.
4. Inspect Feilong zthin installation documentation and IBM `zvm_ansible` module contracts together.
5. Decide whether the project needs an `oss_tooling/` metadata namespace later, or whether OSS dependencies belong inside platform capability maps.
