# Broadcom CA Public Documentation Collection

## Purpose

Record Broadcom CA documentation, support, and product sources that are publicly reachable now for the first Broadcom CA scope. This is a collection ledger, not a mirror of vendor manuals.

Do not commit copied Broadcom manuals, exported HTML, or PDFs into this repository until the project has an explicit entitlement, retention, and redistribution decision. Store source URLs, product evidence, access status, and retrieval notes here first.

Retrieval pass: 2026-07-03.

## Collection Flow

Diagram source: [docs/diagrams/broadcom-ca-public-documentation-collection-flow.mmd](diagrams/broadcom-ca-public-documentation-collection-flow.mmd)

## Collection Rules

- Prefer official Broadcom URLs over third-party mirrors.
- Treat Broadcom Support Portal as the front door for Mainframe Software.
- Treat TechDocs, My Downloads, Compatibility Matrix, Product Lifecycle, Knowledge Base Articles, and Installation and Maintenance Tools as required source systems.
- Mark product technical details as gated when TechDocs or support functions do not expose useful unauthenticated product content; do not assume all TechDocs product pages are gated without checking direct product/version URLs.
- Preserve both Broadcom names and legacy CA names during collection.
- Re-check every URL before implementation.

## Public Source Inventory

| Source | URL | Current collection status | Use |
| --- | --- | --- | --- |
| Broadcom Support Portal | https://support.broadcom.com/ | Public front door; exposes Mainframe Software support paths | Entitlements, downloads, docs, lifecycle, compatibility, KB, tools |
| Broadcom Mainframe All Products | https://support.broadcom.com/web/ecx/all-products?segment=MF | Public page shell; specific product rows were not exposed in this unauthenticated pass | Product lookup after login/session context |
| Broadcom TechDocs | https://techdocs.broadcom.com/ | Public front door; selected direct product/version URLs exposed titles and `Installing` section markers unauthenticated | Canonical product/version documentation |
| Broadcom mainframe product area | https://www.broadcom.com/products/mainframe | Public product-area URL; browser extraction returned no useful text | Public product-family orientation |
| Broadcom Compatibility Matrix | Linked from Support Portal Mainframe Software navigation | Public link; useful content likely login/session-backed | Compatibility evidence |
| Broadcom Product Lifecycle | Linked from Support Portal Mainframe Software navigation | Public link; useful content likely login/session-backed | Lifecycle/support status |
| Broadcom Knowledge Base Articles | Linked from Support Portal Mainframe Software navigation | Public link; useful content likely login/session-backed | Install and maintenance issue evidence |
| Broadcom Installation and Maintenance Tools | Linked from Support Portal Mainframe Software navigation | Public link; useful content likely login/session-backed | Generated JCL, maintenance, workflow/tooling evidence |

## Publicly Reachable Support-Portal Evidence

The Broadcom Support Portal exposes these Mainframe Software navigation paths:

- My Dashboard.
- My Entitlements.
- My Downloads.
- My Cases.
- Documentation on TechDocs.
- Security Advisories.
- Communities.
- Compatibility Matrix.
- All Products.
- Learnings.
- Product Lifecycle.
- Partner Portal.
- Knowledge Base Articles.
- Installation and Maintenance Tools.

This confirms the required acquisition architecture but does not provide enough unauthenticated product-specific detail for implementation.

## Public TechDocs Product URLs Found

These direct TechDocs URLs were publicly reachable in this pass and should be treated as canonical starting points for documentation intake. They do not replace entitlement, download, lifecycle, compatibility, or installation-tool capture.

| Product family | Product/version URL | Public evidence |
| --- | --- | --- |
| DevOps and change management | https://techdocs.broadcom.com/us/en/ca-mainframe-software/devops/ca-endevor-software-change-manager/19-0.html | Endevor 19.0 product page exposed an `Installing` section marker |
| Automation and operations | https://techdocs.broadcom.com/us/en/ca-mainframe-software/automation/ca-ops-mvs-event-management-and-automation/14-0.html | OPS/MVS Event Management and Automation 14.0 product page exposed an `Installing` section marker |
| Security | https://techdocs.broadcom.com/us/en/ca-mainframe-software/security/ca-acf2-for-z-os/16-0.html | ACF2 for z/OS 16.0 product page exposed an `Installing` section marker |
| Security | https://techdocs.broadcom.com/us/en/ca-mainframe-software/security/ca-top-secret-for-z-os/16-0.html | Top Secret for z/OS 16.0 product page exposed an `Installing` section marker |

## First-Wave Product Families To Resolve After Login

| Product family | Candidate product names | Public status from this pass | Required authenticated evidence |
| --- | --- | --- | --- |
| DevOps and change management | Endevor-class tooling; Panvalet-class tooling; Librarian-class tooling | Endevor 19.0 direct TechDocs page is publicly reachable; entitlement still required for downloads, lifecycle, compatibility, and exact licensed scope | Product/version docs, lifecycle, downloads, generated JCL, SMP/E FMIDs, repository and processor model |
| Automation, operations, and scheduling | OPS/MVS-class tooling; workload automation products; console automation products | OPS/MVS 14.0 direct TechDocs page is publicly reachable; broader automation and scheduling scope still must be resolved from entitlements and installed state | Install/customization docs, rule/policy model, started tasks, OPERCMDS/FACILITY/security requirements |
| Security | ACF2-class tooling; Top Secret-class tooling | ACF2 16.0 and Top Secret 16.0 direct TechDocs pages are publicly reachable; security scope and media remain entitlement-driven | Install, migration, command, administration, audit, certificate/keyring, SAF/resource-class requirements |
| Database and data management | Datacom-class tooling; IDMS-class tooling | Public search was not reliable enough for official product docs in this pass | Install, upgrade, dictionary/catalog, utilities, CICS/IMS/Db2/batch integration, recovery and maintenance docs |
| Common services and tools | Common Services, installation and maintenance tooling, compatibility/lifecycle tools | Support Portal exposes the source systems | Common component dependencies, tool outputs, generated JCL, z/OSMF workflow evidence |

## Technical Signals To Preserve

Broadcom CA products can be unusually invasive in clone and automation design:

- Security products may replace or coexist with RACF and can control clone-local authentication posture.
- Automation products can alter console, OPERCMDS, started-task, REXX, JES, scheduler, and event behavior.
- Endevor-class products can become a source-of-truth for software movement and therefore can conflict with a disconnected clone model.
- Datacom and IDMS products require database lifecycle modeling, not only SMP/E installation.
- Common services and installation tooling can be shared dependencies across otherwise unrelated product families.

## Immediate Gaps

- Broadcom account access is still required for entitlement, downloads, lifecycle, compatibility, installation tooling, and complete product scope, even when selected TechDocs product pages are public.
- My Entitlements and My Downloads are required to distinguish licensed products from candidate products.
- Direct TechDocs product/version URL checks are required before marking documentation as gated.
- Compatibility Matrix and Product Lifecycle access are required to determine supported target z/OS, CICS, IMS, Db2, Java, and maintenance levels.
- Installation and Maintenance Tools access is required to determine generated JCL, z/OSMF workflow availability, maintenance mechanics, and SMP/E artifacts.

## Next Collection Pass

1. Log in to Broadcom Support Portal.
2. Export the Mainframe Software entitlement list.
3. For each entitled product, open TechDocs and record canonical product/version documentation URLs.
4. Capture My Downloads media identifiers, generated JCL, z/OSMF workflows, checksums, and maintenance artifacts.
5. Capture Compatibility Matrix and Product Lifecycle entries for target z/OS and subsystem levels.
6. Capture Knowledge Base and Installation and Maintenance Tools evidence for install and service mechanics.
7. Convert each collected product into a Broadcom CA product-definition source map.
