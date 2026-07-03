# Rocket Alternative Documentation Sources

## Purpose

Capture Rocket Software documentation and evidence sources beyond the primary product catalog and the login-gated interactive documentation portal. The goal is to preserve source paths that expose real product documentation without requiring the project to have Rocket support-portal access yet.

This document is a source-acquisition map. It does not replace product-specific documentation intake.

Current public-source collection results are tracked in [Rocket Public Documentation Collection](rocket-public-documentation-collection.md).

Non-canonical public install-documentation searches are tracked separately in [Rocket Non-Canonical Install Documentation Search](rocket-noncanonical-install-documentation-search.md). Treat those results as breadcrumbs only; they do not replace entitled Rocket sources.

Retrieval pass: 2026-07-02.

## Source Discovery Flow

Diagram source: [docs/diagrams/rocket-alternative-documentation-sources-flow.mmd](diagrams/rocket-alternative-documentation-sources-flow.mmd)

## Result Summary

Rocket's confirmed alternative-source footprint is narrower than BMC's, but one source class in this pass is unusually strong: a raw-resource PDF export tree (`docs-be.rocketsoftware.com`) that returns full, unauthenticated PDF manuals for the same products whose interactive documentation pages on `docs.rocketsoftware.com` are login-gated. This was confirmed directly by fetching a PDF and receiving a genuine multi-page PDF binary rather than a login page, and by repeating the same pattern across TMON, Mainstar, PRO/JCL, Enterprise Developer, and ChangeMan ZMF searches. By contrast, `docs.rocketsoftware.com` HTML pages and even direct `.pdf` resource links under the main `docs.rocketsoftware.com` domain (not `docs-be`) were confirmed login-gated in this pass — the "Log in to get a better experience" wall appeared consistently.

This asymmetry matters for automation: exact `docs-be.rocketsoftware.com` raw-resource URLs are the strongest confirmed alternative-source class found for Rocket.

Six source classes were confirmed with direct or strong indirect evidence. Several more (support/entitlement portal structure, legacy product forums) are confirmed to exist but could not be fully verified past a login or fetch block in this pass.

## Confirmed Source Classes

### 1. docs-be.rocketsoftware.com Raw Resource PDF Tree

Primary URL pattern:

```text
https://docs-be.rocketsoftware.com/bundle/<bundle-id>/raw/resource/enus/<filename>.pdf
```

Confirmed examples across first-wave product families:

```text
https://docs-be.rocketsoftware.com/bundle/TMONforCICSTS_MainframeInstallationGuide_V4.0/raw/resource/enus/TMONforCICSTS_MainframeInstallationGuide_V4.0.pdf
https://docs-be.rocketsoftware.com/bundle/TMONforCICSTS_SystemAdministratorGuide_V4.0/raw/resource/enus/TMONforCICSTS_SystemAdministratorGuide_V4.0.pdf
https://docs-be.rocketsoftware.com/bundle/CatalogRecoveryPlus_InstallGuide_V77/raw/resource/enus/CatalogRecoveryPlus_InstallGuide_V77.pdf
https://docs-be.rocketsoftware.com/bundle/PROJCL_CA7UserGuide_V3.5/raw/resource/enus/PROJCL_CA7UserGuide_V3.5.pdf
https://docs-be.rocketsoftware.com/bundle/PROJCL_TWSUserGuide_V3.5/raw/resource/enus/PROJCL_TWSUserGuide_V3.5.pdf
https://docs-be.rocketsoftware.com/bundle/changemanzmf_ig_82patch7_pdf/raw/resource/enus/changeman_zmf_zmf_installation_guide_v8_2_patch_7.pdf
https://docs-be.rocketsoftware.com/bundle/enterprisedeveloper_ig_100_pdf/raw/resource/enus/enterprise_developer_mfa_inst_ved100.pdf
```

Observed value:

- Fetching one of these URLs directly in this pass returned a genuine multi-page (266+ page) PDF binary, not a login page.
- The pattern recurs across ASG-TMON (CICS, z/OS, MQ, Web-Based Applications), Mainstar (Catalog RecoveryPlus, MXI), PRO/JCL (CA-7 and TWS scheduler interfaces), ChangeMan ZMF, and Enterprise Developer/Micro Focus legacy content.
- The same product content is often gated on the interactive `docs.rocketsoftware.com` portal but reachable on `docs-be.rocketsoftware.com` via the exact bundle/resource path.

Use this for:

- Full installation, administrator, reference, and release-notes PDFs once the exact bundle ID is known.
- Confirming a product/version documentation set exists even when the interactive portal is gated.

Caveat: this pass only confirmed exact known URLs (surfaced via search-engine indexing). Whether `docs-be.rocketsoftware.com` exposes its own browsable index or search without login is unconfirmed — treat bundle IDs as needing discovery via search or the gated `docs.rocketsoftware.com` portal first.

### 2. Rocket Software Product Renaming and Acquisition Reference

Primary URL:

```text
https://www.rocketsoftware.com/en-us/rocket-software-product-renaming-acquisition-reference
```

Use this as the legacy-to-current product-name mapping page, equivalent in purpose to BMC's Product Name Finder.

Observed signal:

- Search-engine indexing confirms the page exists and covers Rocket's acquisition history, including Aldon (acquired 2011), Zephyr Development Corporation (acquired 2012), Mainstar Software (acquired 2016), and ASG Technologies (acquired 2021), plus Micro Focus/Serena-origin products such as ChangeMan ZMF and Enterprise Developer.
- Direct fetch returned HTTP 403 in this pass (likely a bot/WAF block), so the full table contents could not be captured here. A human should open this page in a normal browser and record the exact legacy-to-current mappings.

Capture:

- Current Rocket product name.
- Legacy/acquired-company product name.
- Acquisition year.
- Any renamed SKU or entitlement identifiers.

### 3. Rocket Community Portal

Primary URL:

```text
https://community.rocketsoftware.com/
```

Use this for operational and configuration context, not authoritative install documentation.

Observed signal:

- Confirmed publicly browsable without mandatory login in this pass.
- Exposes large product-specific boards, including Rocket COBOL (~11,752 topics), Uniface (~4,186 topics), Rocket Enterprise Suite (~3,196 topics), Rocket MultiValue products, IBM i solutions, mainframe optimization/development tools, and open-source-for-z/OS discussion.
- Site-wide activity counters (~25,977 discussions, ~52,313 replies) confirm active, substantial content.

Capture:

- Product board URLs relevant to first-wave families (TMON, Mainstar, PRO/JCL, ChangeMan, Enterprise Suite).
- Recurring configuration/troubleshooting themes that indicate undocumented gaps in official guides.
- Whether posting requires an account (read access did not, in this pass).

### 4. Rocket Support/Entitlement Portal

Primary URLs:

```text
https://my.rocketsoftware.com/
https://my.rocketsoftware.com/RocketCommunity
```

Use this as the authoritative entitlement, download, and case-management system.

Observed signal:

- Confirmed login-gated in this pass; unauthenticated fetch does not expose entitlement or download content.
- The public Rocket Software Technical Support Users Guide describes the portal's role for entitlements, downloads, and case management, but the PDF's actual text could not be extracted in this pass (it returned as opaque compressed PDF stream data rather than readable text). A human should open it directly.

Capture once access is available:

- Licensed product list and entitlement status.
- Download/patch access paths.
- Case management workflow.

### 5. Official Rocket GitHub Organizations

Primary URLs:

```text
https://github.com/RocketSoftware
https://github.com/RocketSoftwareCOBOLandMainframe
```

Use these for developer-facing integration evidence, not install media.

Observed signal:

- `github.com/RocketSoftware` hosts roughly 95 public repositories, including `jcl-projcl-vscode-ext` (public resources for the PRO/JCL VS Code extension, exposing REST API, Zowe-conformant service, and scan-automation details), several AMC VS Code extensions (core, enterprise, analyzer, JVM COBOL), and COBOL training material.
- `github.com/RocketSoftwareCOBOLandMainframe` hosts 7 public repositories of COBOL/Enterprise Developer tutorials and samples (BankDemo, COBOL-Samples, VisualCOBOL-Samples, DialogSystem-Samples, Rocket-Unit-Testing-Framework-Samples).

Capture:

- CI/CD and REST/Zowe integration points for PRO/JCL and Enterprise Developer.
- Tutorial/sample code that clarifies expected runtime layout for Enterprise Developer/Enterprise Server.

### 6. Product-Line Support Forums Outside the Main Community Portal

Primary URL:

```text
https://remotedesktop.rocketsoftware.com/portal.php
```

Observed signal:

- This is the current host for what was formerly the legacy ASG Remote Desktop community forum (`forum.asg-rd.com`); the legacy domain's TLS certificate now names this Rocket-owned host, indicating consolidation.
- Confirms Rocket maintains product-line-specific forums separate from the main `community.rocketsoftware.com` portal for at least one acquired product line.

Capture:

- Whether other acquired product lines (Mainstar, Aldon, ASG monitoring products) have similar dedicated forums still active under Rocket-owned hosts.

## Partially Confirmed / Needs Human Follow-Up

- **rocketsoftware.com static resource_files tree** (`https://www.rocketsoftware.com/sites/default/files/...`) — confirmed to host brochures, datasheets, and at least one full guide (the Technical Support Users Guide PDF) without login, but content is overview-level, not deep installation detail. Useful as a supplementary asset path only.
- **AWS Marketplace and Carahsoft listings** (e.g., `aws.amazon.com/marketplace/pp/prodview-tpptlcbcincvo` for Enterprise Developer for Eclipse; `static.carahsoft.com` product wrapper PDFs) — confirmed to exist as alternative distribution/reseller channels; not authoritative for install procedure, but may matter for licensing/procurement automation later.
- **Rocket Data Virtualization interactive docs** (`docs.rocketsoftware.com/bundle/RDV_2.1_HTM/...`) — page exists and is indexed, but confirmed login-gated in this pass, consistent with the general `docs.rocketsoftware.com` gating pattern.

## Practical Acquisition Order

1. Start with the Rocket product catalog and Rocket First-Wave Documentation Intake to confirm product names and families.
2. Search for exact product/version strings to discover `docs-be.rocketsoftware.com` raw-resource bundle IDs; fetch those directly for full PDF manuals before assuming login is required.
3. Use the Product Renaming and Acquisition Reference page (opened via a normal browser, since it returned HTTP 403 to automated fetch in this pass) to resolve legacy ASG/Mainstar/Aldon/Zephyr/Micro Focus/Serena names to current Rocket product names.
4. Use the Rocket Community portal to find configuration and troubleshooting context once product names are confirmed.
5. Use official Rocket GitHub organizations for CI/CD, REST, and Zowe integration evidence for PRO/JCL and Enterprise Developer.
6. Use the Rocket support/entitlement portal, once access is obtained, as the authoritative source for downloads, patches, and case-based documentation.

## Source Reliability Notes

- `docs-be.rocketsoftware.com` exact raw-resource URLs are stronger evidence than the interactive `docs.rocketsoftware.com` portal for deployability, because they return the actual document instead of a login wall — but only for URLs already discovered; no browsable index was confirmed in this pass.
- The Product Renaming and Acquisition Reference page is the single most important alias-resolution source for Rocket given how many product lines were acquired, but it could not be fetched directly in this pass and needs human confirmation of its exact table contents.
- Community and GitHub sources are useful for configuration, customization, and integration evidence but are not proof of entitlement or current supportability.
- Do not use third-party mirrors as authoritative sources; if a `docs-be.rocketsoftware.com` document later proves stale or wrong, escalate to the entitled Rocket support portal rather than trusting the raw-resource copy blindly.

## Product Definition Fields To Add

Each Rocket product definition should add:

```text
documentation_sources:
  primary_catalog_url:
  docs_be_raw_resource_url:
  interactive_docs_portal_url:
  login_required_for_interactive_docs:
  product_renaming_reference_checked:
  legacy_product_names:
  community_portal_board_url:
  github_org_repo_url:
  support_portal_entitlement_status:
  export_formats_available:
  retrieval_date:
  observed_versions:
```
