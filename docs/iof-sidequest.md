# IOF Sidequest

## Purpose

Track the product IOF separately from the main BMC, Broadcom CA, and Rocket design path.

IOF appears in mainframe estates as a JES2 and TSO operations tool and may matter for clone usability, operator workflows, and JES2 security posture. It should not drive the main architecture unless it is installed locally or needed as a pilot for orphan or small-vendor product handling.

Retrieval pass: 2026-07-03.

## Investigation Flow

Diagram source: [docs/diagrams/iof-sidequest-flow.mmd](diagrams/iof-sidequest-flow.mmd)

## Current Public Finding

The strongest public evidence points to Fischer International Systems Corporation as the current public custodian of IOF.

The current Fischer site has an active IOF product page:

```text
https://fisc.com/products/iof/
```

The IOF page identifies the product as:

```text
IOF (Interactive Output Facility)
JES2 Management
```

It describes IOF as an alternative to SDSF for TSO and JES2 resource management, and lists product capabilities around:

- Job list and job summary display.
- JES2 resource display.
- Full-screen JES2 device support.
- JES2 access control.
- Dynamic SYSLOG index.
- JES2 queue management.
- SDSF command familiarity.
- IOF programmable interface for JES2.

The page says the latest version is IOF 8G and directs users to contact Fischer to request it. The Fischer support page also lists IOF support and a dedicated IOF support email address.

## Open-Source Status

No public evidence found in this pass indicates that IOF has been open sourced.

Evidence against treating it as open source:

- The Fischer IOF product page is active.
- The Fischer support page is active and lists IOF support.
- The site footer says Fischer International Systems Corporation, all rights reserved.
- The IOF product page says to contact Fischer to request the latest version, rather than linking public source or binary distribution.
- The IOF WordPress page attachments visible through the public API are images, not source archives or product media.
- A public GitHub repository search for `Interactive Output Facility IOF` returned zero repository results.

Conclusion for this project: treat IOF as proprietary unless Fischer or a confirmed successor provides explicit written license, source-release, or transfer evidence.

## Public Source Inventory

| Source | URL | Evidence |
| --- | --- | --- |
| Fischer home page | https://fisc.com/ | Current Fischer International Systems Corporation site resolves and carries 2026 site/footer metadata |
| IOF product page | https://fisc.com/products/iof/ | Active IOF product page; page modified 2024-06-25 through WordPress JSON; latest version named as IOF 8G |
| Fischer support page | https://fisc.com/support/ | Lists IOF support, IOF support email, and contact path to request latest IOF 8G |
| Fischer products page | https://fisc.com/products/ | Lists Interactive Output Facility as a product and says latest IOF 8G is available through contact path |
| IOF page WordPress API | https://fisc.com/wp-json/wp/v2/pages/585 | Confirms IOF page publication and modification metadata |
| IOF page media API | https://fisc.com/wp-json/wp/v2/media?parent=585 | Shows only image attachments in this public pass |
| GitHub repository search | https://github.com/search?q=%22Interactive+Output+Facility%22+IOF&type=repositories | Returned zero repository results in this pass |

## Ownership and Custody Assessment

Current confidence:

- Fischer current public custody: high.
- Open-source release: no evidence found.
- Product media publicly downloadable: no evidence found.
- Successor or acquirer other than Fischer: no evidence found in this pass.

The user noted that the maker shut down not long ago. Public web evidence from this pass does not confirm a shutdown. Instead, `fisc.com` currently resolves, the site presents Fischer International Systems Corporation as active, and the IOF and support pages publish current contact paths. The practical next step is to contact Fischer directly and ask whether IOF support, licensing, source escrow, or transfer of ownership changed.

## Product Handling Guidance

If IOF is installed on a target z/OS image, classify it as:

```text
vendor: fischer
product_code: fischer-iof
product_name: IOF
install_method: unknown_pending_discovery
ownership_status: proprietary_current_public_custody_signal
source_status: not_publicly_open_sourced
```

Before automation, capture:

- Installed IOF version.
- SMP/E CSI and FMIDs, if SMP/E managed.
- IOF load libraries and sample libraries.
- TSO/ISPF command tables, panels, CLIST/REXX, and logon PROC references.
- APF, LINKLIST, LPA, PROCLIB, PARMLIB, and JES2 exits or parameters.
- RACF or equivalent access-control rules for JES2 resources and IOF administrative functions.
- JES2 command authority and OPERCMDS implications.
- Any IOF programmable-interface users.
- Local SDSF-to-IOF transition conventions.
- Vendor media, installation JCL, maintenance artifacts, and license evidence.

## Next Actions

1. Contact `Information@FischerInternational.com` for current ownership, licensing, and IOF 8G availability.
2. Contact `IOFTech@FischerInternational.com` for support status, maintenance availability, and install documentation.
3. Ask specifically whether IOF has been open sourced, transferred, escrowed, retired, or placed into customer-only maintenance.
4. Search local z/OS images for IOF load libraries, commands, panels, started tasks, and SMP/E FMIDs.
5. If installed locally, create a product definition only after installed-state evidence and license posture are known.
