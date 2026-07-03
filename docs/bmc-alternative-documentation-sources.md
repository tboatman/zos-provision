# BMC Alternative Documentation Sources

## Purpose

Capture BMC documentation and evidence sources beyond the primary Mainframe Ops and DevX product catalogs. The goal is to preserve source paths for latest documentation and documentation available back to 2024 where the public site exposes it.

This document is a source-acquisition map. It does not replace product-specific documentation intake.

Current public-source collection results are tracked in [BMC Public Documentation Collection](bmc-public-documentation-collection.md).

## Source Discovery Flow

Diagram source: [docs/diagrams/bmc-alternative-documentation-sources-flow.mmd](diagrams/bmc-alternative-documentation-sources-flow.mmd)

## Confirmed Source Classes

### 1. BMC Docs XWiki Product Catalogs

Primary URL patterns:

```text
https://docs.bmc.com/xwiki/bin/view/Mainframe/Ops/
https://docs.bmc.com/xwiki/bin/view/Mainframe/DevX/
https://docs.bmc.com/xwiki/bin/view/Main/
```

Use these for:

- Product and product-family discovery.
- Visible version discovery.
- Product page canonical URLs.
- Last-modified timestamps.
- Export to PDF, HTML, RTF, or ODT where permitted.
- Login-gated documentation detection.

Observed value:

- Mainframe Ops exposes versions that include 2024-era releases for multiple products, plus current or newer versions for active products.
- Mainframe DevX exposes multiple 24.xx, 25.xx, and 26.x documentation branches for selected products.
- XWiki pages expose export options even when product content requires login.

### 2. BMC Product Support Pages

Primary URL:

```text
https://docs.bmc.com/xwiki/bin/view/Standalone/Product-Support/productinfo/
```

Use this as a product support hub when a product page is difficult to find from the normal catalog.

Capture:

- Product support page URL.
- Links to downloads, patches, product compatibility, product support pages, mainframe maintenance, mainframe installation, FTP sites, supported products, knowledge base, parameter reference database, product name finder, Mainframe DevOps, and zAdviser.
- Whether login is required.
- Any exportable support-page snapshot.

### 3. BMC Support Central

Primary URL:

```text
https://www.bmc.com/support/support-central.html
```

Use this for operational support sources, not just documents.

Important navigation paths:

- Licensed Products.
- Downloads.
- Product Patches.
- Product Support Pages.
- Mainframe Maintenance.
- Mainframe Installation.
- FTP Sites.
- Supported Products A-Z.
- Product Compatibility.
- Knowledge Base.
- Parameter Reference Database.
- Withdrawn Product List.
- Product Name Finder.
- Mainframe DevOps.
- zAdviser.

Capture:

- Login status.
- Accessible licensed-product list.
- Product names and aliases.
- Download and patch availability.
- Knowledge base article URLs.
- Compatibility and withdrawal status.

### 4. Product Download Tool

Primary URL:

```text
https://www.bmc.com/support/resources/product-downloads.html
```

Observed signal:

- Legacy EPD was retired after May 2024.
- The Product Download Tool is the path for latest product downloads, patches, and installation tools.

Use this for:

- Base product media.
- Product patches.
- Installation tools.
- Download package identifiers.
- Checksums or signatures where available.
- Evidence that a product/version is entitled and downloadable.

### 5. Mainframe Installation Resource Page

Primary URL:

```text
https://www.bmc.com/support/resources/mainframe-installation.html
```

Use this as the first stop for install-method classification.

Observed source paths:

- z/OSMF installation via Portable Software Instance.
- Electronic Product Distribution for BMC Mainframe.
- Supported products for PSI/z/OSMF.
- BMC Installation System.
- Installation System documentation.
- Product codes and FMIDs.
- Installation System download JCL.
- BMC AMI DevX ordering and installation services.
- File server information.

Capture:

- Whether product is PSI/z/OSMF eligible.
- Whether product uses the BMC Installation System.
- Product code and FMID mappings.
- Installation download JCL.
- File-server and ESD requirements.
- Legacy installation fallback.

### 6. Mainframe Maintenance Resource Page

Primary URL:

```text
https://www.bmc.com/support/resources/mainframe-maintenance.html
```

Use this as the first stop for maintenance-method classification.

Observed source paths:

- Quarterly Recommended Service Level files.
- PTFs and HOLDDATA.
- IBM SMP/E RECEIVE ORDER.
- Web-based maintenance request service.
- BMC AMI Ops Automation for Batch ThruPut APAR maintenance.
- eFix PTF Distribution Services for INCONTROL.
- Mainframe Security and Integrity portal.
- Enhanced HOLDDATA.
- FIXCAT Enhanced HOLDDATA.
- Installation System documentation.
- Secure File Transfer User Guide.

Capture:

- Product maintenance path.
- RSL and RSU relationship.
- RECEIVE ORDER requirements.
- eFix applicability.
- Enhanced HOLDDATA and FIXCAT use.
- Security and integrity portal access.

### 7. Parameter Reference Database

Primary URL:

```text
https://docs.bmc.com/xwiki/bin/view/IT-Operations-Management/Operations-Management/BMC-Parameter-Reference-Database/PRD/
```

Use this for parameter-level evidence when a product guide is insufficient.

Observed signal:

- Login or registration can be required.
- The page is export-capable.
- Last-modified metadata is visible.

Capture:

- Parameter names.
- Product ownership.
- Valid values and defaults.
- Version applicability.
- Cross-product shared parameters.

### 8. Product Name Finder

Primary URL:

```text
https://webapps.bmc.com/portal/name-finder
```

Use this for legacy-to-current product-name mapping.

Capture:

- Current BMC name.
- Legacy name.
- Local alias.
- Entitlement name.
- Download package name.
- Product definition aliases.

### 9. Mainframe DevOps Documentation

Primary URL:

```text
https://docs.bmc.com/xwiki/bin/view/Mainframe/DevX/BMC-AMI-DevX-Mainframe-DevOps/mdvops/
```

Use this for integration guidance rather than install media.

Observed source areas:

- Introduction to Mainframe DevOps.
- Configuration.
- Pipelines.
- Tutorials.
- APIs, CLIs, and SDKs.

Capture:

- Pipeline examples.
- API and CLI integration points.
- DevX dependency hints.
- CI/CD integration requirements.

### 10. Control-M Static Documentation Tree

Primary URL patterns:

```text
https://documents.bmc.com/supportu/<version>/en-US/Documentation/home.htm
https://documents.bmc.com/supportu/INC/<version>/en-US/home.htm
```

Confirmed examples:

```text
https://documents.bmc.com/supportu/9.0.22/en-US/Documentation/home.htm
https://documents.bmc.com/supportu/9.0.21.300/en-US/Documentation/home.htm
https://documents.bmc.com/supportu/9.0.21.200/en-US/Documentation/home.htm
https://documents.bmc.com/supportu/INC/9.0.22/en-US/home.htm
https://documents.bmc.com/supportu/INC/9.0.21/en-US/home.htm
```

Observed public version signals:

- Control-M 9.0.22 published July 02, 2026.
- Control-M 9.0.21.300 published May 15, 2025.
- Control-M 9.0.21.200 published July 28, 2024.
- INCONTROL 9.0.22.100 published June 19, 2026.
- INCONTROL 9.0.21.300 published May 17, 2025.

Use this for:

- Control-M suite documentation.
- Control-M for z/OS and INCONTROL documentation.
- Static documentation snapshots by version.
- 2024 baseline comparison.
- Version path enumeration.

Capture:

- Version path.
- Published date.
- Release notes link.
- Installation.
- Architecture.
- Security.
- Agents.
- Authentication and authorizations.
- Control-M for z/OS.
- Control-M/Restart.
- Control-M Analyzer.
- Control-M Assist.
- Control-O.
- COSMOS.
- Control-D and Control-V.
- Control-M/Tape.
- JCL Verify.

## Practical Acquisition Order

1. Start with BMC Docs Mainframe Ops and DevX catalogs for product names, version branches, and canonical URLs.
2. Use BMC Product Support Pages to locate support-side source paths for the same products.
3. Use Mainframe Installation to classify PSI/z/OSMF, Installation System, DevX installation service, or legacy path.
4. Use Mainframe Maintenance to classify RECEIVE ORDER, RSL, eFix, HOLDDATA, and security/integrity paths.
5. Use Product Download Tool to confirm media and patches.
6. Use Parameter Reference Database and Product Name Finder to fill parameter and alias gaps.
7. Use Mainframe DevOps docs for CI/CD and API integration guidance.
8. Use the `documents.bmc.com/supportu` static tree for Control-M and INCONTROL current and historical versions.

## Source Reliability Notes

- Public product catalogs are useful for discovery but are not proof of entitlement.
- Login-gated pages are still useful because they expose product identity, version, export behavior, and access gaps.
- Product Download Tool evidence is stronger than catalog evidence for deployability.
- Mainframe Installation and Mainframe Maintenance pages are stronger than individual product pages for install-method and maintenance-method classification.
- Static Control-M documentation URLs can be version-enumerated, but every discovered version must be opened and recorded before use.
- Do not use third-party mirrors as authoritative sources unless BMC official access cannot answer a historical question; if used, mark the evidence as non-authoritative.

## Product Definition Fields To Add

Each BMC product definition should add:

```text
documentation_sources:
  primary_catalog_url:
  product_support_url:
  installation_resource_url:
  maintenance_resource_url:
  product_download_tool_status:
  parameter_reference_url:
  product_name_finder_status:
  control_m_static_doc_url:
  export_formats_available:
  login_required:
  retrieval_date:
  observed_versions:
  observed_last_modified:
```
