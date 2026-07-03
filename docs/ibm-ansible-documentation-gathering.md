# IBM Ansible Documentation Gathering

## Purpose

Define how to gather, classify, and normalize Ansible documentation for z/OS, IBM subsystems, and z/VM before writing automation.

This workstream is different from the BMC, Broadcom CA, and Rocket vendor-intake tracks. It captures the automation substrate itself: IBM-supported Ansible collections, IBM product documentation, subsystem APIs, managed-node prerequisites, and gaps where no first-class Ansible collection exists.

Retrieval pass: 2026-07-03.

## Gathering Flow

Diagram source: [docs/diagrams/ibm-ansible-documentation-gathering-flow.mmd](diagrams/ibm-ansible-documentation-gathering-flow.mmd)

## Ground Rules

- Treat IBM and Red Hat Ansible Content for IBM Z documentation as canonical for collection behavior, requirements, and module contracts.
- Treat IBM product documentation as canonical for subsystem behavior, security requirements, and operational constraints.
- Do not infer module support from product existence; confirm the collection, module, role, version, and requirements.
- Capture both Ansible collection documentation and native IBM product documentation for each managed surface.
- Prefer z/OSMF APIs and workflows where the target system exposes them and the relevant collection supports them.
- Treat z/VM as a separate discovery track unless a current, supported Ansible collection is verified.
- Re-check all URLs and collection versions before implementation.

## Primary Ansible Source Inventory

| Source | URL | Current status | Use |
| --- | --- | --- | --- |
| Red Hat Ansible Content for IBM Z overview | https://ibm.github.io/z_ansible_collections_doc/index.html | Public documentation index; copyright line shows IBM 2020-2026 in this pass | Collection map, support model, install/configuration entry points |
| Software requirements | https://ibm.github.io/z_ansible_collections_doc/requirements/software-requirements.html | Public; lists per-collection control-node, managed-node, and dependency-matrix pages | Prerequisite capture for control node and managed nodes |
| z/OS Core modules | https://ibm.github.io/z_ansible_collections_doc/ibm_zos_core/docs/source/modules.html | Public; module list includes datasets, copy/fetch/find, jobs, operator, TSO, started tasks, APF, RACF user/group, volumes, zFS, and fact gathering | Base z/OS automation primitives |
| z/OS CICS modules | https://ibm.github.io/z_ansible_collections_doc/ibm_zos_cics/docs/source/modules.html | Public; documents CMCI modules and CICS provisioning modules | CICS region provisioning, CICS resources, CICSPlex SM/CMCI interaction |
| z/OS IMS modules | https://ibm.github.io/z_ansible_collections_doc/ibm_zos_ims/docs/source/modules.html | Public; documents IMS ACB, catalog, command, DBRC, DBD, DDL, and PSB modules | IMS artifact generation, catalog population, DBRC, and command execution |
| z/OSMF modules | https://ibm.github.io/z_ansible_collections_doc/ibm_zosmf/docs/source/modules.html | Public; documents authentication, security requirements automation, and workflow operation modules | z/OSMF-first workflow execution and security validation |
| z/OS System Automation collection | https://ibm.github.io/z_ansible_collections_doc/ibm_zos_sysauto/docs/ansible_content.html | Public collection entry point from the IBM Z collections page; module details must be captured in the next pass | Automation policy and operations integration if licensed |
| Z HMC collection | https://ibm.github.io/z_ansible_collections_doc/zhmc-ansible-modules/docs/ansible_content.html | Public collection entry point from the IBM Z collections page; module details must be captured in the next pass | Hardware management and partition-adjacent orchestration, not guest-level z/VM replacement |

## IBM Product Documentation Source Inventory

| Surface | Canonical documentation start | Use |
| --- | --- | --- |
| z/OS | https://www.ibm.com/docs/en/zos | z/OS base services, JES, MVS, USS, SMP/E, RACF, z/OSMF, storage, TCP/IP, operator commands |
| z/OSMF | https://www.ibm.com/docs/en/zos | z/OSMF server configuration, REST APIs, workflow authoring and execution, security setup |
| CICS Transaction Server | https://www.ibm.com/docs/en/cics-ts | CICS regions, CSD, CMCI, CICSPlex SM, startup/shutdown, resource definitions, security |
| IMS | https://www.ibm.com/docs/en/ims | IMS control region, commands, DBRC, catalog, DBD, PSB, ACB, DDL, security, utilities |
| Db2 for z/OS | https://www.ibm.com/docs/en/db2-for-zos | Db2 subsystem operations, DDL/DCL, BIND/REBIND/FREE, utilities, catalog queries, security |
| IBM MQ for z/OS | https://www.ibm.com/docs/en/ibm-mq | Queue managers, channel authentication, certificates, started tasks, command paths |
| IBM Z System Automation | https://www.ibm.com/docs/en/z-system-automation | Policy database, resources, automation control files, NetView integration, operations model |
| z/VM | https://www.ibm.com/docs/en/zvm | CP, CMS, directory management, guest lifecycle, minidisks, networking, service, and operations |

## Required Capture Fields

For each collection, capture:

- Collection name and namespace.
- Current public documentation URL.
- Collection version and release date, if visible.
- Certification or support status.
- Control-node requirements.
- Managed-node requirements.
- Required Python, ZOAU, SSH, z/OSMF, Java, or subsystem prerequisites.
- Module and role list.
- Idempotency claims and limitations.
- Authentication method.
- Required ports and certificates.
- Return values and evidence available from modules.
- Known limitations and unsupported operations.
- Example playbooks and command-line `ansible-doc` names.

For each IBM subsystem or platform surface, capture:

- IBM product documentation URL and version.
- Required APIs and interfaces.
- Required security classes, users, groups, certificates, keyrings, and permits.
- Required started tasks, PROCLIB/PARMLIB members, datasets, USS paths, and ports.
- Required operator commands, TSO commands, batch utilities, or REST APIs.
- Ansible collection and module coverage.
- Gaps requiring shell, JCL, z/OSMF workflow, vendor utility, or manual review.
- Verification commands and expected evidence.
- Backout and recovery posture.

## Scope By Platform Surface

| Surface | First Ansible path | Documentation to gather | Gap posture |
| --- | --- | --- | --- |
| z/OS base | `ibm.ibm_zos_core` | Requirements, configuration, datasets, jobs, operator, TSO, APF, started tasks, users, volumes, zFS, facts | Use z/OSMF, JCL, or site utility wrappers only when core modules do not cover the operation |
| z/OSMF | `ibm.ibm_zosmf` | Authentication, SCA, workflow module, z/OSMF REST and workflow docs | Preferred path for workflow-capable installs and security requirement validation |
| CICS | `ibm.ibm_zos_cics` | CMCI, CICS provisioning modules, CICS TS docs, CICSPlex SM docs, region datasets | Use site-standard CSD/DFHCSDUP jobs only when CMCI/provisioning modules are not approved |
| IMS | `ibm.ibm_zos_ims` | IMS module docs, IMS product docs, DBRC, catalog, DBD/PSB/ACB, DDL, commands | Use generated JCL or site utilities for unsupported lifecycle pieces |
| Db2 | No first-class IBM Z collection confirmed in this pass | Db2 for z/OS docs, approved SQL runner, bind utilities, catalog queries, DSN commands | Model as site-standard JCL/SQL execution until a supported collection or API path is confirmed |
| MQ | No first-class IBM Z collection confirmed in this pass | MQ for z/OS docs, command paths, started tasks, certificates, CHLAUTH, queue manager lifecycle | Model as command/JCL/runtime overlay until a supported collection or API path is confirmed |
| System Automation | IBM Z System Automation collection plus product docs | Collection modules, SA z/OS policy and operations documentation | Treat as licensed optional integration; do not assume availability |
| Z HMC | IBM Z HMC collection | HMC collection docs and hardware-management boundaries | Useful for hardware/partition-adjacent actions; not a substitute for z/VM guest management |
| z/VM | No current Ansible Galaxy `zvm` or `z/VM` collection hit found in this pass | z/VM docs, CP/CMS commands, directory, service, network, guest provisioning, available REST or local APIs | Treat as custom integration discovery, likely through z/VM-native tooling, APIs, SSH to Linux guests, or HMC-adjacent orchestration |

## z/VM Discovery Track

z/VM matters because disconnected development clones are assumed to be VM guests, not LPARs. The Ansible model must therefore capture both guest provisioning and guest independence.

Required z/VM documentation questions:

1. Which z/VM release is the clone host running?
2. Which interface is approved for automation: CP commands, CMS/REXX, directory manager tooling, SMAPI, z/VM Cloud Connector, HMC, or site wrapper?
3. How are guest IDs, passwords, minidisks, NICs, VSWITCH connections, and spool files created and destroyed?
4. How is a z/OS guest IPLed, stopped, recycled, snapshotted, cloned, and expired?
5. Which parts of the VM guest lifecycle are owned by z/VM automation versus z/OS automation inside the guest?
6. Which security authority exists on z/VM, and how is it separated from clone-local z/OS authentication?
7. What evidence proves that a disconnected VM guest clone is independent from central authority?

## Output Files To Create Later

Once documentation is collected, create source maps under:

```text
product_definitions/automation/ansible/ibm-z-collections.yml
product_definitions/platforms/zos/zos-base.yml
product_definitions/platforms/zos/zosmf.yml
product_definitions/platforms/zos/cics.yml
product_definitions/platforms/zos/ims.yml
product_definitions/platforms/zos/db2.yml
product_definitions/platforms/zos/mq.yml
product_definitions/platforms/zvm/zvm-host.yml
```

These are not implementation files yet. They are normalized documentation and capability maps that will drive later role design.

## Immediate Next Pass

1. Capture exact collection versions and dependency matrices for z/OS Core, CICS, IMS, z/OSMF, System Automation, and Z HMC.
2. Run `ansible-galaxy collection list` and `ansible-doc` in the selected control-node execution environment once it exists.
3. Confirm whether Db2 and MQ will be driven by collection modules, z/OSMF workflows, site JCL, REST APIs, or approved command utilities.
4. Gather IBM product documentation URLs for target versions of z/OS, CICS TS, IMS, Db2, MQ, z/OSMF, System Automation, and z/VM.
5. Identify the approved z/VM automation interface for VM guest clone lifecycle.
6. Convert the gathered material into platform and subsystem capability maps.
