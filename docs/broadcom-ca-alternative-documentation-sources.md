# Broadcom CA Alternative Documentation Sources

## Purpose

Capture Broadcom CA documentation and evidence sources beyond the primary Broadcom Support Portal and TechDocs front doors. The goal is to preserve source paths that are reachable without an entitled login, including a legacy CA static documentation tree, compatibility and lifecycle static pages, public knowledge base articles, and community sources.

This document is a source-acquisition map. It does not replace product-specific documentation intake.

Current public-source collection results are tracked in [Broadcom CA Public Documentation Collection](broadcom-ca-public-documentation-collection.md).

Non-canonical public install-documentation searches are tracked separately in [Broadcom CA Non-Canonical Install Documentation Search](broadcom-ca-noncanonical-install-documentation-search.md). Treat those results as breadcrumbs only; they do not replace entitled Broadcom sources.

## Source Discovery Flow

Diagram source: [docs/diagrams/broadcom-ca-alternative-documentation-sources-flow.mmd](diagrams/broadcom-ca-alternative-documentation-sources-flow.mmd)

## Confirmed Source Classes

### 1. Broadcom TechDocs Product/Version Documentation Trees

Primary URL pattern:

```text
https://techdocs.broadcom.com/us/en/ca-mainframe-software/<category>/<product-slug>/<version>.html
```

Confirmed examples:

```text
https://techdocs.broadcom.com/us/en/ca-mainframe-software/devops/ca-endevor-software-change-manager/19-0.html
https://techdocs.broadcom.com/us/en/ca-mainframe-software/automation/ca-ops-mvs-event-management-and-automation/14-0.html
https://techdocs.broadcom.com/us/en/ca-mainframe-software/security/ca-acf2-for-z-os/16-0.html
https://techdocs.broadcom.com/us/en/ca-mainframe-software/security/ca-top-secret-for-z-os/16-0.html
```

Use this for:

- Full product/version table of contents, not just a single "Installing" marker.
- Whole-book PDF download of the current documentation set.
- Section-level navigation into Installing, Getting Started, Securing, Administrating, Using, Integrating, Reporting, Reference, and Messages.

Observed signal:

- The Endevor 19.0 landing page exposes a complete table of contents (Announcements and News, Release Notes, Installing, Getting Started, Securing, Administrating, Using, Integrating, Reporting, Reference, Messages, Additional Resources) and a "Download PDF" link for the full book, all without login.
- The Endevor 19.0 `installing.html` subsection is itself reachable without login and contains genuine installation workflow guidance (prepare for installation, verify requirements, install using z/OSMF, and related subsections), though the deepest step-by-step SMP/E procedures live further down the linked subsection tree.
- This is a meaningfully deeper result than assuming TechDocs only exposes a section-title marker; the practical limit is how far into the subsection tree a given product/version publishes without an entitlement wall, and that must be re-checked per product.

Capture:

- Full table of contents per product/version.
- Whether a whole-book PDF download is offered.
- How deep the public subsection tree goes before hitting a login wall, if any.
- Last-modified/HTTP evidence for each page opened.

### 2. Legacy CA Static Documentation Tree on ftpdocs.broadcom.com

Primary URL pattern:

```text
https://ftpdocs.broadcom.com/cadocs/0/<product-and-release-folder>/Bookshelf_Files/PDF/<file>.pdf
https://ftpdocs.broadcom.com/cadocs/0/<file>.pdf
```

Confirmed examples:

```text
https://ftpdocs.broadcom.com/cadocs/0/CA%20View%2012%202-ENU/Bookshelf_Files/PDF/View_User_ENU.pdf
https://ftpdocs.broadcom.com/cadocs/0/CA%20IDMS%2018%205%20User%20Bookshelf-ENU/Bookshelf_Files/PDF/IDMS_Navigational_DML_ENU.pdf
https://ftpdocs.broadcom.com/cadocs/0/CA%20Top%20Secret%20%20Security%20for%20z%20OS%20r15-ENU/Bookshelf_Files/PDF/TSS_User_zOS_ENU.pdf
https://ftpdocs.broadcom.com/cadocs/0/CA%20DLP%2014%200-ENU/Bookshelf.html
https://ftpdocs.broadcom.com/cadocs/0/k044675e.pdf
https://ftpdocs.broadcom.com/cadocs/0/d000623e.pdf
```

Use this for:

- Legacy CA-branded product manuals (CA View, CA Deliver-class, CA IDMS, CA Top Secret r15, CA DLP, CA Harvest Software Change Manager, CA EPIC for z/VSE, CA RC/Migrator for DB2, and others) that predate or sit alongside current TechDocs branding.
- Historical release-level documentation (r15, 12.x, 14.0, 18.5) useful for reconciling old CA-era installed state with current Broadcom naming.
- Bookshelf-style HTML front matter (`Bookshelf.html`) that indexes an individual product's PDF set.

Observed signal:

- This tree is served through a CrushFTP web interface; individual known file and folder paths (found via web search, not by browsing an index) return content directly without login.
- Attempting to browse a bare directory (for example `https://ftpdocs.broadcom.com/cadocs/`) returns the CrushFTP file-manager shell with pagination controls, not a usable listing; folder and file names must be discovered through search engines or existing runbooks rather than by crawling.
- File and folder names still carry legacy CA product branding and old release numbering (e.g. `r15`, `12 6`, `18 5`), which is useful evidence for local CA-prefixed aliases.

Capture:

- Exact file/folder URL.
- Legacy product name and release as it appears in the path.
- Whether the file is a standalone PDF or part of a `Bookshelf_Files` set.
- Retrieval date and HTTP status.

### 3. ftpdocs.broadcom.com Compatibility Matrix Static Pages

Primary URL pattern:

```text
https://ftpdocs.broadcom.com/WebInterface/phpdocs/0/MSPSaccount/COMPAT/<matrix-name>.HTML
https://ftpdocs.broadcom.com/WebInterface/redirectFTPDocs.html?path=/phpdocs/0/MSPSaccount/COMPAT/<matrix-name>.HTML
```

Confirmed examples:

```text
https://ftpdocs.broadcom.com/WebInterface/phpdocs/0/MSPSaccount/COMPAT/ims_compat.HTML
https://ftpdocs.broadcom.com/WebInterface/redirectFTPDocs.html?path=/phpdocs/0/MSPSaccount/COMPAT/zos_compat.HTML
https://ftpdocs.broadcom.com/WebInterface/phpdocs/0/MSPSaccount/COMPAT/db2_compat.HTML
https://ftpdocs.broadcom.com/WebInterface/redirectFTPDocs.html?path=%2Fphpdocs%2F0%2FMSPSaccount%2FCOMPAT%2Fcics_compat.HTML
https://ftpdocs.broadcom.com/WebInterface/phpdocs/0/MSPSaccount/COMPAT/java.HTML
https://ftpdocs.broadcom.com/WebInterface/phpdocs/0/MSPSaccount/COMPAT/IBM_hardware.HTML
https://ftpdocs.broadcom.com/WebInterface/phpdocs/0/MSPSaccount/COMPAT/MFA.HTML
https://ftpdocs.broadcom.com/phpdocs/0/MSPSaccount/COMPAT/TPX.HTML
```

Use this for:

- Product-by-product compatibility grids against a given subsystem or platform version (confirmed live for IMS V14/V15, showing over 40 Broadcom mainframe products with YES/NO/IN PROGRESS/N-A status and PTF references).
- Cross-checking whether an installed product/version is certified against a target z/OS, CICS, Db2, IMS, Java, or IBM zEnterprise hardware level.

Observed signal:

- The IMS compatibility matrix is reachable without login and states a "last updated" date, confirming it is actively maintained.
- This directly corrects the assumption in the public-collection ledger that Compatibility Matrix content is entirely login/session-gated; the static `COMPAT/*.HTML` pages are a working unauthenticated path to the same category of data, even though the Support Portal's own in-context Compatibility Matrix links still route through login for per-account upgrade details.

Capture:

- Matrix name and target subsystem/platform.
- Product/version rows and compatibility status.
- Last-updated date.
- Any linked PTF or additional-information references.

### 4. Broadcom Mainframe Product Lifecycle Page

Primary URL:

```text
https://ftpdocs.broadcom.com/WebInterface/phpdocs/0/MSPSaccount/COMPAT/AllProdDates.HTML
```

Use this for:

- General Availability (GA), End of Service (EOS), and End of Life (EOL) dates across the Broadcom mainframe product line in one table.

Observed signal:

- Reachable without login; returned example rows include ACF2 for z/OS 17.0 (GA 2026-03-16) and 16.0 (GA 2015-11-11, EOS 2028-03-16), CA 7 Workload Automation 12.0 (GA 2014-09-08, EOS 2025-03-15), and Endevor 19.0 (GA 2022-08-22).
- This is a second correction to the public-collection ledger's assumption that Product Lifecycle content is entirely login-gated: this static page is a working unauthenticated path to lifecycle dates, independent of the Support Portal's own gated Product Lifecycle Details flow.

Capture:

- Product name as printed (often still CA-prefixed).
- Release/version.
- GA, EOS, and EOL dates.
- Any linked product-detail URL.

### 5. Broadcom Support Portal Mainframe Compatibility Hub

Primary URL:

```text
https://support.broadcom.com/web/ecx/support-content-notification/-/external/content/release-announcements/Mainframe-Compatibilities/8659
```

Use this as the indexed jumping-off point into the `COMPAT/*.HTML` static pages above.

Observed signal:

- Reachable without login; lists the categories of compatibility information available (z/OS, z/VM, z/VSE, CICS TS, Db2, IMS, IBM zEnterprise, zIIP-enabled products, TLS, IPv6, Pervasive Encryption, MFA, PCI DSS compliance, Java) with links into the underlying matrices.
- Some embedded links still route into "Support Online" for per-account upgrade-requirement detail, so treat this page as an index, not a substitute for entitled review.

Capture:

- Which compatibility categories are indexed.
- Which linked matrices resolve to public `ftpdocs.broadcom.com` pages versus login-gated Support Online pages.

### 6. Broadcom Public Knowledge Base Articles

Primary URL pattern:

```text
https://knowledge.broadcom.com/external/article/<article-id>/<slug>.html
```

Confirmed examples:

```text
https://knowledge.broadcom.com/external/article/142936/download-search-product-help-mainframe.html
https://knowledge.broadcom.com/external/article/150550/product-lifecycle-and-end-of-life-inform.html
https://knowledge.broadcom.com/external/article/122711/broadcom-mainframe-compatibility-and-lif.html
```

Use this for:

- Explainer articles that describe how to navigate My Downloads, Product Lifecycle, and Compatibility Matrix flows, including the direct `ftpdocs.broadcom.com` links captured above.

Observed signal:

- Individual `knowledge.broadcom.com/external/article/...` pages returned readable content without login in this pass.
- The bare `knowledge.broadcom.com` root domain 301-redirects to `support.broadcom.com`, so treat `knowledge.broadcom.com` as an article-ID-addressed public knowledge base layered on the same Support Portal estate, not as an independently browsable site; individual article URLs must be discovered via search rather than by browsing the domain root.

Capture:

- Article ID and title.
- Whether the article links to a canonical `ftpdocs.broadcom.com` or `techdocs.broadcom.com` resource.
- Retrieval date.

### 7. Broadcom Mainframe Software Community

Primary URL:

```text
https://community.broadcom.com/mainframesoftware/home
```

Use this for:

- Product-name discovery, including legacy CA-prefixed community names (the community index lists 40+ product-specific sub-communities, including one still named "CA 7").
- Practical troubleshooting breadcrumbs, blog posts on recent product updates, and an education/course catalog entry point.

Observed signal:

- Reachable without login for discussion titles, blog posts, and the community list; participation (posting) likely requires an account, but reading is not blocked.

Capture:

- Product-specific sub-community name and URL, especially where it preserves a legacy CA-prefixed name.
- Any troubleshooting or install-adjacent thread relevant to a target product, marked as community evidence, not vendor authority.
- Cross-reference every fact pulled from here against TechDocs or an entitled source before use, per the non-canonical handling rules in [Broadcom CA Non-Canonical Install Documentation Search](broadcom-ca-noncanonical-install-documentation-search.md).

### 8. TechDocs "Product Names and Abbreviations" Pages

Primary URL pattern:

```text
https://techdocs.broadcom.com/us/en/ca-mainframe-software/<category>/<product-slug>/<version>/release-notes/product-names-and-abbreviations.html
```

Confirmed example:

```text
https://techdocs.broadcom.com/us/en/ca-mainframe-software/traditional-management/ca-common-services-for-z-os/15-0/release-notes/product-names-and-abbreviations.html
```

Use this for:

- Per-product-set long-form/short-form/trademark naming conventions (for example, "CA 1(R) Tape Management (CA 1)").

Observed signal:

- Reachable without login; lists roughly 80 or more Broadcom mainframe product names and their accepted short forms for that documentation set.
- This is a per-product-family terminology reference, not a single global CA-to-Broadcom name-mapping page; no standalone "Product Name Finder" equivalent to BMC's `webapps.bmc.com/portal/name-finder` was found for Broadcom CA in this pass. Treat each product family's own "Product Names and Abbreviations" page as the closest available substitute and expect to open several of them to build a working alias table.

Capture:

- Long-form name, short-form name, and trademark symbol as printed.
- Which product-family documentation set the page belongs to.
- Cross-reference against any locally used CA-prefixed alias.

## Practical Acquisition Order

1. Start with the confirmed TechDocs product/version URLs already on file (Endevor, OPS/MVS, ACF2, Top Secret) and walk their full table of contents, not just the landing page, before concluding content is gated.
2. For each product family, open its "Product Names and Abbreviations" release-notes page to build a working long-form/short-form/legacy-alias table.
3. Use the Support Portal Mainframe Compatibility Hub to find which compatibility categories matter, then resolve them against the `ftpdocs.broadcom.com/.../COMPAT/*.HTML` static pages.
4. Use the `ftpdocs.broadcom.com` Product Lifecycle page (`AllProdDates.HTML`) to establish GA/EOS/EOL posture for every candidate product before deeper intake.
5. Search for legacy product names plus known file-naming conventions (`Bookshelf_Files`, `.pdf`) to locate applicable entries in the `ftpdocs.broadcom.com/cadocs/0/` static tree.
6. Use `knowledge.broadcom.com` article search to fill process gaps (how downloads work, how lifecycle/compatibility data is organized) before requesting entitled access.
7. Use the Mainframe Software community to confirm legacy CA-prefixed product names still in active use and to collect troubleshooting breadcrumbs, then re-resolve every fact through an official source before use.

## Source Reliability Notes

- TechDocs product/version pages are stronger evidence than assumed in the initial public-collection pass: full tables of contents, whole-book PDF downloads, and at least the first level of installation-subsection content are reachable without login for the products checked. This must still be re-verified per product/version, since entitlement gating can differ by product.
- The `ftpdocs.broadcom.com` estate (CrushFTP-served) is a genuine unauthenticated static tree, but it has no browsable index; usable URLs must come from web search results, existing runbooks, or links embedded in other Broadcom pages, not from crawling directory listings.
- The Compatibility Matrix and Product Lifecycle static pages under `ftpdocs.broadcom.com/WebInterface/phpdocs/0/MSPSaccount/COMPAT/` are a working unauthenticated substitute for the Support Portal's gated Compatibility Matrix and Product Lifecycle flows for the categories confirmed here; per-account upgrade-requirement detail still routes through login.
- `knowledge.broadcom.com` is not an independently browsable public knowledge base; it is an article-ID-addressed layer that 301-redirects at its root into `support.broadcom.com`. Only cite specific article URLs that were actually opened.
- Community content is useful for product-name discovery and troubleshooting breadcrumbs only; it must be re-resolved through TechDocs, the legacy static tree, or entitled Support Portal evidence before use in a product definition, matching the non-canonical handling rules already established for Broadcom CA.
- No standalone cross-product "Product Name Finder" equivalent to BMC's was found; per-product-family "Product Names and Abbreviations" pages are the closest available substitute and must be aggregated manually.
- Do not treat any `ftpdocs.broadcom.com/cadocs/` document as current just because it is reachable; many carry old release numbers (e.g., r15) and must be checked against the Product Lifecycle page before being used as installation authority.

## Product Definition Fields To Add

Each Broadcom CA product definition should add:

```text
documentation_sources:
  techdocs_product_version_url:
  techdocs_toc_depth_public:
  techdocs_pdf_download_available:
  legacy_cadocs_static_url:
  compatibility_matrix_static_url:
  product_lifecycle_static_status:
  knowledge_base_article_urls:
  community_subcommunity_url:
  product_names_and_abbreviations_url:
  export_formats_available:
  login_required:
  retrieval_date:
  observed_versions:
  observed_ga_eos_eol_dates:
```
