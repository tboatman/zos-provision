# BMC Public Documentation Collection

## Purpose

Record the BMC documentation sources that are publicly reachable now for the first BMC scope. This is a collection ledger, not a mirror of vendor manuals.

Do not commit copied BMC manuals, exported HTML, or PDFs into this repository until the project has an explicit entitlement, retention, and redistribution decision. Store source URLs, version evidence, access status, and retrieval notes here first.

Retrieval pass: 2026-07-03.

## Collection Flow

Diagram source: [docs/diagrams/bmc-public-documentation-collection-flow.mmd](diagrams/bmc-public-documentation-collection-flow.mmd)

## Collection Rules

- Prefer official BMC URLs over third-party mirrors.
- Capture public landing pages, visible table-of-contents data, export options, version lists, modified dates, and published dates.
- Mark product details as gated when the page requires BMC login or registration.
- Record downloadable PDF candidates as candidates only; do not vendor BMC PDF files into git.
- Treat Product Download Tool, Product Patches, Mainframe Installation, Mainframe Maintenance, and Support Central as entitlement-backed sources.
- Re-check every URL before using it for implementation, because public documentation versions and login behavior can change.

## Public Source Inventory

| Source | URL | Current collection status | Use |
| --- | --- | --- | --- |
| BMC Documentation home | https://docs.bmc.com/ | Public entry point | Cross-family discovery |
| Mainframe Ops catalog | https://docs.bmc.com/xwiki/bin/view/Mainframe/Ops/ | Public product/version index | AMI Ops products, versions, export options |
| Mainframe DevX catalog | https://docs.bmc.com/xwiki/bin/view/Mainframe/DevX/ | Public product/version index | DevX products, versions, export options |
| Product Support Pages | https://docs.bmc.com/xwiki/bin/view/Standalone/Product-Support/productinfo/ | Public hub | Support-side discovery |
| Product Downloads | https://www.bmc.com/support/resources/product-downloads.html | Public page; tool requires entitlement | Base media, patches, installation tools |
| Mainframe Installation | https://www.bmc.com/support/resources/mainframe-installation.html | Public page; linked services can require login | z/OSMF PSI, Installation System, FMIDs, JCL |
| Mainframe Maintenance | https://www.bmc.com/support/resources/mainframe-maintenance.html | Public page; maintenance retrieval requires entitlement | RSL, RECEIVE ORDER, eFix, HOLDDATA |
| Control-M static docs | https://documents.bmc.com/supportu/9.0.22/en-US/Documentation/home.htm | Public static documentation | Distributed Control-M suite |
| INCONTROL static docs | https://documents.bmc.com/supportu/INC/9.0.22/en-US/home.htm | Public static documentation | Control-M for z/OS and INCONTROL |

## Publicly Reachable BMC AMI Ops Pages

| Product area | Public URL | Observed version signal | Publicly visible sections | Access status |
| --- | --- | --- | --- | --- |
| BMC AMI Ops Automation | https://docs.bmc.com/xwiki/bin/view/Mainframe/Ops/BMC-AMI-Ops-Automation/bao84/ | 8.4; prior visible versions 8.3.01 and 8.3 | Getting started, Rules, basic applications, advanced automation, Installing, Upgrading, Customizing, Total Object Manager, MQ automation, BiiZ, options, solutions, infrastructure, Messages | Landing page public; detailed content requires BMC login or registration |
| BMC AMI Ops Monitor for CICS | https://docs.bmc.com/xwiki/bin/view/Mainframe/Ops/BMC-AMI-Ops-Monitor-for-CICS/bmcics75/ | 7.5; prior visible versions 7.4, 7.3, and 7.2 | Getting started, Installing, Customizing, data collection monitors, Performance Reporter, online services, reference, infrastructure, Messages | Landing page public; detailed content requires BMC login or registration |
| BMC AMI Ops Monitoring solution | https://docs.bmc.com/xwiki/bin/view/Mainframe/Ops/BMC-AMI-Ops-Monitoring/opsm71/ | 7.1; solution releases 7.1.02, 7.1.01, and 7.1.00 | Component map to CMF, z/OS, UNIX System Services, and SYSPROG Services | Landing page public; detailed content requires BMC login or registration |
| BMC AMI Ops Monitor for z/OS | https://docs.bmc.com/xwiki/bin/view/Mainframe/Ops/BMC-AMI-Ops-Monitor-for-zOS/opsmzos65/ | 6.5; prior visible version 6.4 | Getting started, Installing, Customizing, Using, Reference, Administering, Integrating, Messages, infrastructure | Landing page public; detailed content requires BMC login or registration |
| BMC AMI Ops Monitor for Db2 | https://docs.bmc.com/xwiki/bin/view/Mainframe/Ops/BMC-AMI-Ops-Monitor-for-Db2/opsmdb2131/ | 13.1; prior visible version 12.2 from catalog | Getting started, Planning, Installing, Using, Reference, Administering, infrastructure, Messages, Integrating, PDFs and videos | Landing page public; detailed content requires BMC login or registration; ready-made PDF candidates visible |
| BMC AMI Ops Monitor for IMS Online | https://docs.bmc.com/xwiki/bin/view/Mainframe/Ops/BMC-AMI-Ops-Monitor-for-IMS-Online/mfionv56/ | 5.6; prior visible version 5.5 | Getting started, Installing, Customizing, Integrating, Using, Messages, infrastructure | Landing page public; detailed content requires BMC login or registration |
| BMC AMI Ops Monitor for MQ | https://docs.bmc.com/xwiki/bin/view/Mainframe/Ops/BMC-AMI-Ops-Monitor-for-MQ/AMIMQ56/ | 5.6 | Getting started, Installing, Customizing, Using, Views reference, infrastructure, Messages, Troubleshooting | Landing page public; detailed content requires BMC login or registration |

## Publicly Reachable BMC AMI DevX Pages

| Product area | Public URL | Observed version signal | Publicly visible sections | Access status |
| --- | --- | --- | --- | --- |
| Mainframe DevX catalog | https://docs.bmc.com/xwiki/bin/view/Mainframe/DevX/ | Mixed product index with 24.xx, 25.xx, and 26.x branches | Product family names and visible versions | Public catalog |
| BMC AMI DevX Workbench for VS Code | https://docs.bmc.com/xwiki/bin/view/Mainframe/DevX/BMC-AMI-DevX-Workbench-for-VS-Code/bawvsc267/ | 26.7; visible prior versions include 26.4, 26.1, 25.7, 25.4, 25.10, 25.1, 24.7, 24.4, 24.10, 24.1, and older | Getting started, Using, Uninstalling, Planning, Installing, Settings, Troubleshooting | Landing page public; detailed content requires BMC login or registration |
| BMC AMI DevX Total Test | Catalog entry only in this pass | 25.04, 25.03, 25.02, 25.01, 24.01, and older visible in catalog | Not collected beyond catalog in this pass | Direct page retrieval failed from public browser session; retry with authenticated browser |
| BMC AMI Web Products Installation and Configuration | Catalog entry only in this pass | 24.07, 24.06, 24.05, 24.04, 24.03, 24.02, 24.01, and older visible in catalog | Not collected beyond catalog in this pass | Direct page retrieval failed from public browser session; retry with authenticated browser |

## Control-M and INCONTROL Static Documentation

| Product area | Public URL | Published date | Version signal | Publicly visible sections |
| --- | --- | --- | --- | --- |
| Control-M | https://documents.bmc.com/supportu/9.0.22/en-US/Documentation/home.htm | 2026-07-02 | 9.0.22 | Get Started, Architecture, Add-ons, Planning, Monitoring, Administration, Installation, Security, Agents, Authentication, Authorizations, System Settings, Plug-ins, Conversion, Automation API |
| Control-M | https://documents.bmc.com/supportu/9.0.21.300/en-US/Documentation/home.htm | 2025-05-15 | 9.0.21.300 | Get Started, Planning, Monitoring, Administration, Installation, Security, Agents, Authentication, Authorizations, System Settings, Plug-ins, Reports, Conversion, Automation API |
| Control-M | https://documents.bmc.com/supportu/9.0.21.200/en-US/Documentation/home.htm | 2024-07-28 | 9.0.21.200 | Get Started, Planning, Monitoring, Administration, Installation, Security, Agents, Authentication, Authorizations, System Settings, Application Plug-ins, Reports, Conversion, Automation API |
| INCONTROL | https://documents.bmc.com/supportu/INC/9.0.22/en-US/home.htm | 2026-06-19 | 9.0.22.100 | Installing INCONTROL, Control-M for z/OS, Control-M/Restart, Control-M Analyzer, Control-M Assist, Control-O, COSMOS, Administration, Security, Messages, KSL, JCL Verify, Control-D, Control-V, Control-M/Tape, Conversion |
| INCONTROL | https://documents.bmc.com/supportu/INC/9.0.21/en-US/home.htm | 2025-05-17 | 9.0.21.300 | Installing INCONTROL, Control-M for z/OS, Control-M/Restart, Control-M Analyzer, Control-M Assist, Control-O, COSMOS, Administration, Security, Messages, KSL, JCL Verify, Control-D, Control-V, Control-M/Tape, Conversion |

## Ready-Made PDF Candidates

These are candidates discovered on public pages. Do not commit the PDFs until the vendor-document retention decision is made.

| Product | Candidate | Date | Size | Source |
| --- | --- | --- | --- | --- |
| BMC AMI Ops Monitor for Db2 13.1 | Version 13.1 SPE2404 PDF | 2024-04-08 | 13,105 KB | Ready-made PDFs table on https://docs.bmc.com/xwiki/bin/view/Mainframe/Ops/BMC-AMI-Ops-Monitor-for-Db2/opsmdb2131/ |
| BMC AMI Ops Monitor for Db2 13.1 | Version 13.1 SPE2401 PDF | 2024-01-08 | 12,820 KB | Ready-made PDFs table on https://docs.bmc.com/xwiki/bin/view/Mainframe/Ops/BMC-AMI-Ops-Monitor-for-Db2/opsmdb2131/ |

## Installation and Maintenance Evidence Collected

The BMC Mainframe Installation page confirms two major install paths:

- z/OSMF installation through Portable Software Instance and z/OSMF Software Management.
- Legacy/product-specific paths through the BMC Installation System where PSI is not available.

The same page identifies useful follow-on sources:

- z/OSMF getting started documentation.
- EPD / z/OSMF Installed Products tab.
- PSI supported products.
- Installation System documentation.
- Installation System supported products.
- Product codes and FMIDs.
- Installation System download JCL.
- File server information.

The BMC Mainframe Maintenance page confirms maintenance paths:

- Quarterly Recommended Service Level files.
- PTFs and HOLDDATA.
- IBM SMP/E RECEIVE ORDER.
- Web-based maintenance request service.
- eFix PTF Distribution Services for INCONTROL.
- BMC Mainframe Security and Integrity portal.
- Enhanced HOLDDATA.
- FIXCAT Enhanced HOLDDATA.

## Immediate Gaps

- Authenticated BMC Docs access is still required for detailed install, customization, security, and integration pages for most AMI Ops and DevX products.
- Product Download Tool access is required to collect media package names, checksums, file layouts, and actual deliverables.
- Product Patches and Mainframe Maintenance access are required to collect concrete PTF, HOLDDATA, RECEIVE ORDER, RSL, and eFix details.
- Product Support Pages should be queried with actual entitled product names once the licensed-product inventory is available.
- DevX pages beyond Workbench for VS Code need a retry with authenticated browsing because some direct public page retrieval failed in this pass.

## Next Collection Pass

1. Log in to BMC Docs and Support Central.
2. Export the public-gated product spaces as internal evidence where BMC permits export.
3. Capture Product Download Tool package names for each entitled product.
4. Capture PSI/z/OSMF eligibility and Installation System mappings from the supported-products and FMID pages.
5. Capture RSL, RECEIVE ORDER, HOLDDATA, and eFix mechanics for the first AMI Ops, DevX, and INCONTROL pilots.
6. Convert each product row into a product-definition source map with canonical docs, media, install method, maintenance method, security hooks, and implementation blockers.
