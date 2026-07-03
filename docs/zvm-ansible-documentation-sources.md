# z/VM Ansible Documentation Sources

## Purpose

Record canonical and non-canonical sources for managing z/VM through Ansible or Ansible-adjacent tooling.

This is a source ledger, not an implementation decision. z/VM automation is a special case in this project because disconnected development clones are assumed to be VM guests, not LPARs, and because current first-party Ansible coverage is not as clearly packaged as the z/OS, CICS, IMS, z/OSMF, System Automation, and Z HMC collections.

Retrieval pass: 2026-07-03.

## Source Discovery Flow

Diagram source: [docs/diagrams/zvm-ansible-documentation-sources-flow.mmd](diagrams/zvm-ansible-documentation-sources-flow.mmd)

For the broader OSS candidate ledger that includes Feilong, Zowe, zopen community tooling, Galasa, and quarantine-only tools, see [OSS Tooling Discovery](oss-tooling-discovery.md).

For the VM-side implementation contract that grows out of this source ledger, see [z/VM SMAPI Implementation Design](zvm-smapi-implementation-design.md).

## Result Summary

The z/VM Ansible source landscape is sparse but not empty.

Findings:

- Ansible Galaxy API searches for `zvm` and `z/VM` returned zero collection-version results in this pass.
- GitHub repository search found two IBM-owned repositories: `IBM/zvm_ansible` and `IBM/zvm_ansible_collection`.
- `IBM/zvm_ansible_collection` has a `galaxy.yml` declaring namespace `ibm`, collection name `zvm_ansible`, and version `0.0.3`, but the README still documents local clone/build/install rather than direct Galaxy installation.
- `IBM/zvm_ansible` is the better documentation/examples repository; it explains the management path through a Linux virtual machine on the target z/VM LPAR, SMAPI, Feilong zthin, and `smcli`.
- Open Mainframe Project Feilong is a key adjacent source and candidate z/VM clone-factory substrate because the IBM Ansible material depends on its zthin/smcli layer.
- IBM z/VM product documentation remains canonical for SMAPI, CP, CMS, directory management, networking, security, and guest lifecycle behavior.

Practical conclusion: treat the IBM z/VM Ansible repositories as useful IBM-owned but not yet productized Ansible source material. Treat Feilong and IBM z/VM documentation as required supporting sources. Do not treat the z/VM path as equivalent to the supported Red Hat Ansible Content for IBM Z collection set until support status, installation path, and module contracts are verified.

## Source Inventory

| Source | URL | Status from this pass | Use |
| --- | --- | --- | --- |
| IBM z/VM documentation | https://www.ibm.com/docs/en/zvm/7.4.0 | Canonical IBM product docs; base URL reachable through redirect from `/zvm/7.4` | z/VM release, CP, CMS, SMAPI, directory, TCP/IP, security, networking, service, and guest lifecycle authority |
| z/VM 7.4 PDFs and more | https://www.ibm.com/docs/en/zvm/7.4.0/SSB27U_7.4.0/com.ibm.zvm.v740/allpubs.htm | IBM Documentation index page identified during SMAPI retrieval pass | Starting point for exact book and PDF capture |
| z/VM Systems Management APIs topic | https://www.ibm.com/docs/en/zvm/7.4.0/SSB27U_7.4.0/com.ibm.zvm.v740.dmsa3/dmsa319.htm | IBM Documentation topic path identified during SMAPI retrieval pass | Starting point for SMAPI programming and operation mapping |
| IBM zvm_ansible examples | https://github.com/IBM/zvm_ansible | IBM-owned GitHub repo; public; Apache-2.0; default branch `master`; repository pushed in 2025 according to GitHub metadata | Best current source for Ansible call path, prerequisites, examples, limitations, and operational pattern |
| IBM zvm_ansible_collection | https://github.com/IBM/zvm_ansible_collection | IBM-owned GitHub repo; public; Apache-2.0; `galaxy.yml` declares `ibm.zvm_ansible` version `0.0.3`; not found by Galaxy keyword search | Candidate local-built Ansible collection; source for module list and collection packaging |
| Ansible Galaxy keyword search: `zvm` | https://galaxy.ansible.com/api/v3/plugin/ansible/search/collection-versions/?keywords=zvm | API returned `count: 0` | Evidence that direct Galaxy install path was not found in this pass |
| Ansible Galaxy keyword search: `z/VM` | https://galaxy.ansible.com/api/v3/plugin/ansible/search/collection-versions/?keywords=z%2FVM | API returned `count: 0` | Evidence that slash-form keyword did not find a collection in this pass |
| Open Mainframe Project Feilong | https://github.com/openmainframeproject/feilong | Public GitHub repo; Apache-2.0; active metadata; README describes an SDK/API layer for z/VM guest, image, network, and volume operations | Required adjacent source for zthin/smcli behavior and API model |
| Feilong documentation | https://feilong.readthedocs.io/en/latest/ | Linked by Feilong README; direct fetch to ReadTheDocs returned a challenge/rate response in this pass | Retry through browser or unrestricted network; likely source for zthin installation and API docs |
| IBM Cloud Infrastructure Center z/VM setup docs | https://www.ibm.com/docs/en/cic | Referenced by IBM zvm_ansible README as an end-to-end setup source | Useful for SMAPI, directory manager, compute user IDs, and external security manager setup patterns if ICIC is relevant |

## IBM zvm_ansible Pattern

The IBM `zvm_ansible` README describes this control path:

1. Ansible runs from the normal control node.
2. Module code is pushed to a Linux virtual machine running on the target z/VM LPAR.
3. That Linux VM acts as a Management Access Point.
4. The Management Access Point must be authorized to issue z/VM SMAPI commands.
5. Feilong zthin must be installed because the modules use `smcli`.
6. z/VM SMAPI and a z/VM directory manager must be configured and working.
7. Some operations require additional z/VM privilege classes and local SMAPI extensions.

The README also documents a likely guest provisioning flow:

1. Create a virtual machine.
2. Update virtual machine settings.
3. Add NICDEF and VLAN-related configuration.
4. Create or clone minidisks.
5. Dedicate DASD or FCP devices if needed.
6. Empty the virtual reader.
7. Send firstboot material to the virtual reader.
8. Start or stop the virtual machine.
9. Adjust relative or absolute share.

This maps well to the disconnected clone requirement, but it must be validated against local z/VM authority, security, directory manager, storage, and z/OS guest IPL requirements.

## SMAPI Implementation Linkage

The source ledger now feeds a dedicated SMAPI implementation design. That design owns the operation mapping, prerequisites, clone manifest additions, evidence requirements, and lane decision between Feilong zthin, a site-owned provisioning service, a direct SMAPI client, or a manual handoff path.

The next documentation retrieval pass must therefore capture not only general z/VM docs, but the exact SMAPI server setup, TCP/IP SSL, CP/CMS, directory-manager, reader/spool, and Feilong zthin material required to prove that each clone lifecycle operation has an approved interface.

## Candidate Module Surface From IBM zvm_ansible_collection

The collection README identifies these module-level operations:

| Module | Backing operation or interface | Notes |
| --- | --- | --- |
| `zvm_clone_disk.py` | `Image_Disk_Copy_DM` | Clone disk through SMAPI/directory manager path |
| `zvm_minidisk.py` | `Image_Disk_Create_DM` / `Image_Disk_Delete_DM` | Create and delete minidisks |
| `zvm_startstop_user.py` | `Image_Activate` / `Image_Deactivate`; also `vmcp xautolog` for IPL device use cases | Start and stop guests; specific IPL device support requires local path |
| `zvm_update_user.py` | `Image_Definition_Update_DM` | Update guest directory definition |
| `zvm_user.py` | `Image_Create_DM` / `Image_Delete_DM` | Create and delete guests |
| `zvm_dedicate_dev.py` | `Image_Device_Dedicate_DM` / `Image_Device_Undedicate_DM` | Dedicate and undedicate devices |
| `zvm_fileto_reader.py` | `vmur punch` | Send file to target reader; requires extra z/VM privilege |
| `zvm_reader_empty.py` | `vmcp purge` | Empty reader; requires extra z/VM privilege |
| `zvm_setshare.py` | Local `CP_SetShare` SMAPI extension | Requires local REXX extension and SSL-protected SMAPI TCP path |
| `zvm_dirm_nicdef.py` | Local `Dirm_Nicdef` SMAPI extension | Required because base SMAPI does not manage VLAN tag on NICDEF in the documented pattern |

## Support And Risk Classification

| Source | Classification | Risk |
| --- | --- | --- |
| IBM z/VM docs | Canonical product documentation | Must still match local z/VM release and maintenance |
| IBM zvm_ansible repositories | IBM-owned public source, not confirmed as supported product documentation | Must verify support status, owner, release process, and whether local build is acceptable |
| Ansible Galaxy search | Negative evidence only | Keyword search can miss unpublished or differently named collections |
| Feilong | Open Mainframe Project source; Ansible-adjacent dependency | Must verify zthin installation, version, security posture, and operational support |
| ICIC setup docs | Product-specific setup guidance | Useful if the site has ICIC or follows its MAP/compute-node pattern; otherwise a reference model only |

## Open Questions

1. Is `IBM/zvm_ansible_collection` acceptable as an internal-built collection if it is not published in Galaxy?
2. Who owns support for the IBM z/VM Ansible repositories today?
3. Which z/VM release and maintenance level will host disconnected z/OS guest clones?
4. Is SMAPI configured and approved for clone lifecycle automation?
5. Which directory manager is used locally?
6. Can the site provide a Management Access Point Linux guest per z/VM LPAR?
7. Can Feilong zthin and `smcli` be installed and maintained on that Management Access Point?
8. Which privilege classes and command modifications are acceptable for automation?
9. Is SSL-protected SMAPI TCP access approved?
10. Can local SMAPI extensions be installed, reviewed, and supported?
11. How will z/OS guest IPL device, spool, console, and shutdown behavior be automated?
12. What evidence proves that a generated VM guest clone is disconnected from central authority?

## Next Pass

1. Clone or fetch `IBM/zvm_ansible_collection` metadata and run `ansible-galaxy collection build` in a disposable workspace.
2. Run `ansible-doc` against the built collection to capture module contracts.
3. Capture exact Feilong zthin installation documentation through a browser or unrestricted network.
4. Classify Feilong in the OSS tooling ledger for license, support, mirroring, version pinning, and internal ownership.
5. Capture IBM z/VM 7.4 SMAPI, CP command, directory manager, TCP/IP SSL, and user directory documentation URLs.
6. Ask IBM or the site z/VM owner whether the IBM z/VM Ansible repositories are supported, sample-only, or abandoned.
7. Decide whether z/VM clone lifecycle belongs in Ansible roles, a custom collection, a wrapper around Feilong, or a site-owned service API.
8. Fill the SMAPI capability matrix described in [z/VM SMAPI Implementation Design](zvm-smapi-implementation-design.md).
