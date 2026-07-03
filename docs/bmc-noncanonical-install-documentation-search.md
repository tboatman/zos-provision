# BMC Non-Canonical Install Documentation Search

## Purpose

Record non-canonical BMC installation documentation leads found outside the current official BMC documentation and support pages.

This document is intentionally cautious. Non-canonical sources can help discover old names, old URLs, historical install mechanics, and missing search terms, but they must not be treated as implementation authority. BMC Support Central, BMC Docs, Product Download Tool, Mainframe Installation, Mainframe Maintenance, and entitled product media remain the canonical sources.

Retrieval pass: 2026-07-03.

## Search Flow

Diagram source: [docs/diagrams/bmc-noncanonical-install-doc-search-flow.mmd](diagrams/bmc-noncanonical-install-doc-search-flow.mmd)

## Result Summary

The public non-canonical search was low yield.

Useful findings:

- Internet Archive has historical snapshots of BMC Mainframe Installation and Mainframe Maintenance entry pages.
- Those snapshots can help compare old installation and maintenance portal behavior over time.
- Public GitHub code search is not useful without login for code search; repository-level and unauthenticated code probes did not produce usable BMC install artifacts in this pass.
- General public web search did not surface reliable third-party copies of BMC AMI Ops, BMC AMI DevX, BMC Installation System, or Control-M for z/OS installation manuals.

Practical conclusion: non-canonical public sources are not enough to reconstruct BMC installation procedures. They are useful only as breadcrumbs. The project still needs BMC account access, entitlements, Product Download Tool access, Product Patches access, Mainframe Installation access, Mainframe Maintenance access, and product-specific documentation exports.

## Non-Canonical Source Classes Checked

| Source class | Status | Evidence value | Use |
| --- | --- | --- | --- |
| Internet Archive snapshots | Some useful hits | Medium for historical portal structure; low for detailed install mechanics | Compare changes in BMC install and maintenance entry pages |
| Public GitHub search | No usable unauthenticated result | Low | Recheck only with a signed-in account or GitHub search API if needed |
| Public web search by exact install terms | Low yield | Low | Use only to discover alternate names and stale URLs |
| Third-party PDF mirrors | No trustworthy BMC install manuals found in this pass | Very low | Do not use unless provenance, version, and licensing are reviewed |
| Customer blogs and forum snippets | No strong install-document leads found in this pass | Low | May be useful later for symptom searches, not baseline installation |

## Internet Archive Leads

These are non-canonical snapshots of BMC pages. Use them as historical evidence only.

| Source | Snapshot evidence | Notes |
| --- | --- | --- |
| Mainframe Installation page | `https://web.archive.org/web/*/http://www.bmc.com/support/resources/mainframe-installation.html` | CDX API returned archived `200` HTML captures from 2017 through 2019 for the old HTTP URL |
| Mainframe Maintenance page | `https://web.archive.org/web/*/https://www.bmc.com/support/resources/mainframe-maintenance.html` | CDX API returned archived `200` HTML captures including 2021, 2022, 2023, 2024, 2025, and 2026 snapshots |
| BMC Docs XWiki downloads under Mainframe | `https://web.archive.org/web/*/https://docs.bmc.com/xwiki/bin/download/Mainframe/*` | CDX prefix query returned no useful captures in this pass |
| Control-M static documentation tree | `https://web.archive.org/web/*/https://documents.bmc.com/supportu/*` | CDX prefix query returned no useful captures in this pass |

## Public Search Terms Tried

These terms did not produce useful non-canonical install manuals in this pass:

```text
"BMC Installation System" +"download JCL"
"BMC Installation System" +"SMP/E"
"BMC AMI" +"SMP/E"
"BMC AMI Ops" +"Installing" +PDF
"BMC AMI Ops Automation" +"Installing" +PDF
"BMC AMI Ops Monitor for CICS" +"Installing" +PDF
"BMC AMI DevX" +"Installation" +PDF
"Control-M for z/OS" +"Installation" +PDF
"INCONTROL" +"Installation Guide" +PDF
```

## Risk Notes

- Third-party copies of BMC manuals, if found later, may be stale, incomplete, license-restricted, or tampered with.
- Search snippets are not enough to infer install method, FMIDs, SMP/E zones, HOLDDATA, z/OSMF workflows, or generated JCL.
- Historical snapshots can preserve old URL paths but cannot prove current supportability or compatibility.
- Non-canonical documents should never override current BMC documentation, current product media, or entitled support instructions.

## How To Use Non-Canonical Leads Safely

1. Use non-canonical hits only to discover product names, former names, URL fragments, release labels, or missing search terms.
2. Re-resolve every lead through BMC Support Central, BMC Docs, Product Support Pages, Product Download Tool, Mainframe Installation, or Mainframe Maintenance.
3. Record the canonical replacement URL before using the information in a product definition.
4. If no canonical replacement exists, classify the fact as `unverified_noncanonical` and keep it out of executable automation.
5. Require human review before using any third-party PDF or copied manual as evidence.

## Next Search Pass

The next non-canonical pass should be narrower and entitlement-informed:

1. Start with actual licensed product names and exact versions from BMC Support Central.
2. Search for exact title strings from entitled BMC product pages.
3. Search for exact FMIDs, product codes, RSL labels, and generated JCL member names.
4. Use signed-in GitHub search only for accidental exposure of local JCL/runbooks, not for authoritative docs.
5. Use Internet Archive only to map old URL structures or compare current-vs-old portal pages.
