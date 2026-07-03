# z/VM SMAPI Implementation Design

## Purpose

Define the VM-side implementation design for disconnected z/OS VM guest clone creation, update, activation, shutdown, and destruction through z/VM SMAPI.

This document turns the z/VM source discovery work into an implementation contract. It does not add Ansible code yet. It defines the control path, required documentation, required site facts, operation mapping, evidence, and fallback lanes that must be settled before the clone factory can become executable.

Design pass: 2026-07-03.

Gap coverage: G-012, G-029, G-032, and G-033 from [Top-to-Bottom Gap Assessment and Remediation Plan](top-to-bottom-gap-assessment.md).

## Implementation Flow

Diagram source: [docs/diagrams/zvm-smapi-implementation-flow.mmd](diagrams/zvm-smapi-implementation-flow.mmd)

## Control Path

The preferred VM-side architecture is:

1. Ansible control node.
2. z/VM clone factory role or collection.
3. Linux Management Access Point guest on the target z/VM system.
4. Feilong zthin and `smcli`.
5. z/VM SMAPI TCP service.
6. Directory manager, CP, and CMS operations.
7. z/OS VM guest lifecycle operations.

This keeps Ansible as the orchestrator and z/VM SMAPI as the VM authority boundary. The Management Access Point is the execution bridge because the current IBM z/VM Ansible examples and collection source expect a Linux VM with Feilong zthin available.

Acceptable alternate lanes:

- `site_service`: a site-owned provisioning API that wraps SMAPI and directory-manager operations.
- `direct_smapi`: a custom internal SMAPI client if Feilong or `smcli` cannot be supported.
- `manual_handoff`: documented manual CP, CMS, and directory-manager steps for early evidence gathering only.

The implementation must not hide which lane was used. Clone evidence must record the lane, tool versions, SMAPI endpoint, service identity, and operation evidence.

## Documentation Retrieval Plan

Canonical source roots:

| Source | URL | Retrieval use |
| --- | --- | --- |
| IBM z/VM 7.4 documentation | https://www.ibm.com/docs/en/zvm/7.4.0 | Canonical entry point for z/VM release docs |
| z/VM 7.4 PDFs and more | https://www.ibm.com/docs/en/zvm/7.4.0/SSB27U_7.4.0/com.ibm.zvm.v740/allpubs.htm | Locate exact book/PDF titles for SMAPI, CP, CMS, TCP/IP, security, and directory management |
| Systems Management APIs topic | https://www.ibm.com/docs/en/zvm/7.4.0/SSB27U_7.4.0/com.ibm.zvm.v740.dmsa3/dmsa319.htm | Starting point for SMAPI programming documentation |
| IBM zvm_ansible examples | https://github.com/IBM/zvm_ansible | Example control path and prerequisites |
| IBM zvm_ansible_collection | https://github.com/IBM/zvm_ansible_collection | Candidate module surface and backing SMAPI operations |
| Feilong | https://github.com/openmainframeproject/feilong | zthin, `smcli`, SDK, and z/VM operation abstraction |

Retrieval work items:

1. Capture the exact z/VM SMAPI book or topic set for the target local release, not just the 7.4 public entry point.
2. Capture SMAPI server setup, TCP/IP, SSL/TLS, certificate, and authentication documentation.
3. Capture CP command, CMS command, reader/spool, and virtual machine lifecycle documentation.
4. Capture the locally used directory manager documentation, including automation command interface and authorization model.
5. Capture Feilong zthin and `smcli` installation, configuration, privilege, and support-status documentation.
6. Capture IBM `zvm_ansible` and `zvm_ansible_collection` module contracts with repository commit IDs.
7. Record whether IBM PDFs or downloaded documentation can be retained in the internal doc depot, or whether only metadata and URLs may be stored.

Doc retrieval evidence should store title, URL, release, retrieval date, entitlement status, checksum where a file is retained, and the design decision that depends on it.

## SMAPI Prerequisites

The clone factory cannot proceed until these facts are known:

| Area | Required fact |
| --- | --- |
| z/VM host | Release, maintenance level, service policy, and target clone-host systems |
| SMAPI service | Enabled status, endpoint, port, TCP/IP stack, SSL/TLS mode, and logging |
| Authorization | Service identity, privilege classes, command authorization, and approval owner |
| Directory manager | Product or local method, automation interface, authority, and change evidence |
| Management Access Point | Linux guest ID, OS level, network path, package source, hardening, and owner |
| Feilong path | zthin version, `smcli` version, install method, support owner, and patch policy |
| Network | VSWITCH, NICDEF, VLAN, hostname, IP allocation, and disconnection rules |
| Storage | Minidisk, full-pack, volume-copy, DASD, FCP, and cleanup authority |
| Reader and spool | Punch, purge, firstboot input, log collection, and spool cleanup authority |
| z/OS guest | IPL device, shutdown method, console/automation expectations, and evidence probes |
| Recovery | Half-created guest cleanup, leaked-resource checks, and forced destroy authority |

## Operation Mapping

The first implementation design should map every clone lifecycle action to an exact interface before code is written.

| Clone action | Preferred interface | Alternate interface | Evidence |
| --- | --- | --- | --- |
| Discover z/VM capability | SMAPI query plus Feilong or site probe | Manual CP/CMS report | Endpoint, version, authority, and probe output |
| Create guest identity | `Image_Create_DM` through SMAPI and directory manager | Site provisioning service | Guest directory evidence |
| Update guest definition | `Image_Definition_Update_DM` | Directory-manager command wrapper | Before/after definition excerpt or approved redacted equivalent |
| Delete guest identity | `Image_Delete_DM` | Site provisioning service | Destroy evidence and missing-guest verification |
| Create or delete minidisk | `Image_Disk_Create_DM` / `Image_Disk_Delete_DM` | Storage wrapper or manual handoff | Disk allocation and cleanup evidence |
| Copy minidisk | `Image_Disk_Copy_DM` | Storage-level copy or image restore | Source, target, checksum, and completion evidence |
| Dedicate or undedicate device | `Image_Device_Dedicate_DM` / `Image_Device_Undedicate_DM` | Site wrapper | Device assignment and release evidence |
| Configure NIC or VLAN | Directory-manager path or reviewed local SMAPI extension | Site network wrapper | NICDEF, VSWITCH, VLAN, and disconnect evidence |
| Send firstboot input | `vmur punch` or equivalent through approved MAP path | Disk or USS staging | Reader contents, purge state, and firstboot artifact checksum |
| Empty reader | `vmcp purge` or equivalent through approved MAP path | Manual purge evidence | Reader empty verification |
| Activate or stop guest | `Image_Activate` / `Image_Deactivate` | `XAUTOLOG`, CP command wrapper, or operations handoff | Activation, IPL, shutdown, and console evidence |
| Set CPU share | Reviewed local SMAPI extension or site policy | Manual policy assignment | Share policy evidence |
| Destroy leaked resources | SMAPI, directory manager, CP/CMS, network, storage, and security cleanup sequence | Forced manual cleanup | Leaked-resource check results |

Local SMAPI extensions must be treated as software artifacts. They need owner, source, review status, installation path, rollback, and evidence just like any other privileged automation component.

## Clone Manifest Additions

The clone manifest needs a VM-side section before implementation starts.

```yaml
zvm_smapi:
  implementation_lane: feilong_zthin
  zvm_release: TBD
  smapi_endpoint: TBD
  smapi_tls: required
  management_access_point:
    guest_id: TBD
    os: TBD
    owner: TBD
    zthin_version: TBD
    smcli_version: TBD
  directory_manager:
    product: TBD
    interface: TBD
    owner: TBD
  operations:
    create_guest: TBD
    update_guest: TBD
    delete_guest: TBD
    create_disk: TBD
    copy_disk: TBD
    network: TBD
    reader: TBD
    activate: TBD
    deactivate: TBD
  authorities:
    service_identity: TBD
    privilege_classes: []
    local_extensions: []
  evidence:
    retain_smapi_logs: true
    retain_directory_change_evidence: true
    retain_reader_punch_checksums: true
    retain_destroy_verification: true
```

## Evidence Requirements

Create evidence:

- SMAPI endpoint and authenticated identity used.
- MAP guest identity and tool versions.
- Guest directory create or update evidence.
- Disk allocation, copy, attach, or restore evidence.
- NIC, VSWITCH, VLAN, and network isolation evidence.
- Reader input checksums and purge state.
- Guest activation or IPL evidence.
- z/OS base readiness probes where applicable.

Destroy evidence:

- Workload stop and z/OS shutdown evidence where available.
- Guest deactivation evidence.
- Directory entry deletion or expiration evidence.
- Disk and device release evidence.
- Reader and spool cleanup evidence.
- Network release evidence.
- Credential and certificate handling evidence.
- Leaked-resource check result.

## Risks And Design Rules

- SMAPI availability is necessary but not sufficient; the site still needs directory-manager, storage, network, and operations authority.
- Feilong and IBM `zvm_ansible` source are promising reference paths, but support status must be verified before they become production dependencies.
- Some required operations may need local SMAPI extensions. Those extensions must go through the same custody, review, and evidence process as internal code.
- The clone factory must generate the destroy plan before create operations run.
- z/OS IPL, shutdown, and readiness are partly guest operating-system concerns. Do not model them as pure z/VM operations.
- Disconnected clone proof belongs inside the z/OS guest and in VM-side evidence. SMAPI success alone does not prove clone independence.
- The design must limit concurrent clone operations until leaked-resource and retry behavior are proven.

## Immediate Actions

1. Retrieve and register exact IBM z/VM SMAPI, TCP/IP SSL, CP, CMS, directory, and security docs for the target z/VM release.
2. Build a SMAPI capability matrix from IBM docs, IBM z/VM Ansible source, Feilong docs, and local site constraints.
3. Ask the z/VM owner whether SMAPI, SSL/TLS, directory-manager automation, and a Linux Management Access Point are approved.
4. Decide whether the first pilot uses `feilong_zthin`, `site_service`, `direct_smapi`, or `manual_handoff`.
5. Add the `zvm_smapi` manifest section to the schema work once the first lane is selected.
