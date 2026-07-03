# Broadcom CA Non-Canonical Install Documentation Search

## Purpose

Record non-canonical Broadcom CA installation documentation leads found outside the current official Broadcom Support Portal, TechDocs, My Downloads, and Installation and Maintenance Tools paths.

This document is intentionally cautious. Non-canonical sources can help discover legacy CA names, old DocOps paths, historical install mechanics, FMIDs, old generated JCL names, and stale product URLs, but they must not be treated as implementation authority. Broadcom Support Portal, TechDocs, My Downloads, Compatibility Matrix, Product Lifecycle, Knowledge Base Articles, Installation and Maintenance Tools, entitled media, and site-owned historical install evidence remain the authoritative sources.

Retrieval pass: 2026-07-03.

## Search Flow

Diagram source: [docs/diagrams/broadcom-ca-noncanonical-install-doc-search-flow.mmd](diagrams/broadcom-ca-noncanonical-install-doc-search-flow.mmd)

## Result Summary

The public non-canonical search was low yield.

Useful findings:

- General public web search did not surface reliable third-party installation manuals for Endevor, OPS/MVS, ACF2, Top Secret, or related Broadcom CA mainframe products.
- Internet Archive broad-prefix checks for Broadcom TechDocs, Broadcom Support, and legacy CA DocOps paths returned no useful broad capture inventory in this pass.
- Exact Internet Archive CDX checks against selected current TechDocs product URLs timed out with 504 responses, so archive availability for those exact URLs remains unresolved.
- Public GitHub repository searches for exact Broadcom CA product plus `SMP/E` terms returned zero useful repository results in this pass; unauthenticated code search is not a dependable source.
- The most important finding was not non-canonical: direct Broadcom TechDocs product URLs for selected first-wave products are publicly reachable and expose an `Installing` section marker.

Practical conclusion: non-canonical public sources are not enough to reconstruct Broadcom CA installation procedures. The project should treat non-canonical sources as breadcrumbs only, then re-resolve every useful lead through Broadcom TechDocs, Support Portal, entitled downloads, Installation and Maintenance Tools, or site-owned historical install evidence.

## Non-Canonical Source Classes Checked

| Source class | Status | Evidence value | Use |
| --- | --- | --- | --- |
| Public web search by exact install terms | Low yield | Low | Use only to discover old names, stale URLs, and missing search vocabulary |
| Internet Archive broad-prefix checks | No useful broad inventory found | Low in this pass | Recheck with exact URLs, old DocOps URL patterns, or product-specific historical paths |
| Internet Archive exact TechDocs checks | Timed out with 504 responses | Unknown | Retry later or use targeted Wayback UI checks |
| Public GitHub repository search | No usable repository result for exact product plus `SMP/E` terms | Low | Recheck only with signed-in code search for accidental local runbooks or JCL exposure |
| Third-party PDF mirrors | No trustworthy install manuals found in this pass | Very low | Do not use unless provenance, version, and licensing are reviewed |
| Legacy CA DocOps paths | No useful broad capture inventory found | Low in this pass | Use later only to map legacy URL fragments to current TechDocs pages |

## Canonical TechDocs URLs Discovered During Search

These are official Broadcom TechDocs URLs found while looking for non-canonical sources. They are not non-canonical leads, but they correct the earlier assumption that unauthenticated TechDocs did not expose useful product-level entry points.

| Product area | Product/version page | Public evidence from this pass | HTTP evidence |
| --- | --- | --- | --- |
| DevOps and change management | https://techdocs.broadcom.com/us/en/ca-mainframe-software/devops/ca-endevor-software-change-manager/19-0.html | Page title exposed `Endevor 19.0`; page text exposed `Installing` | HTTP 200; last-modified 2026-06-29 |
| Security | https://techdocs.broadcom.com/us/en/ca-mainframe-software/security/ca-acf2-for-z-os/16-0.html | Page title exposed `ACF2 for z/OS 16.0`; page text exposed `Installing` | HTTP 200; last-modified 2026-07-02 |
| Security | https://techdocs.broadcom.com/us/en/ca-mainframe-software/security/ca-top-secret-for-z-os/16-0.html | Page title exposed `Top Secret for z/OS 16.0`; page text exposed `Installing` | HTTP 200; last-modified 2026-06-28 |
| Automation | https://techdocs.broadcom.com/us/en/ca-mainframe-software/automation/ca-ops-mvs-event-management-and-automation/14-0.html | Page title exposed `OPS/MVS Event Management and Automation 14.0`; page text exposed `Installing` | HTTP 200; last-modified 2026-06-25 |

## Internet Archive Checks

These checks were attempted as non-canonical historical leads:

| Source | Status | Notes |
| --- | --- | --- |
| `https://web.archive.org/web/*/https://techdocs.broadcom.com/us/en/ca-mainframe-software/*` | No useful broad-prefix inventory found | CDX broad-prefix query returned no useful captures in this pass |
| `https://web.archive.org/web/*/https://support.broadcom.com/web/ecx/*` | No useful broad-prefix inventory found | CDX broad-prefix query returned no useful captures in this pass |
| `https://web.archive.org/web/*/https://docops.ca.com/*` | No useful broad-prefix inventory found | CDX broad-prefix query returned no useful captures in this pass |
| Exact current TechDocs product URLs for Endevor, ACF2, Top Secret, and OPS/MVS | 504 timeout | Retry later before concluding there are no snapshots |

## Public Search Terms Tried

These terms did not produce useful non-canonical install manuals in this pass:

```text
"CA Endevor" +"SMP/E"
"CA Endevor" +"Installation Guide" +PDF
"Endevor" +"Installing" +"SMP/E"
"CA OPS/MVS" +"Installation Guide" +PDF
"OPS/MVS" +"Installing" +"SMP/E"
"CA ACF2" +"Installation Guide" +PDF
"ACF2 for z/OS" +"Installing" +"SMP/E"
"CA Top Secret" +"Installation Guide" +PDF
"Top Secret for z/OS" +"Installing" +"SMP/E"
"docops.ca.com" +"Endevor" +"Installing"
"docops.ca.com" +"ACF2" +"Installing"
"docops.ca.com" +"Top Secret" +"Installing"
```

## Risk Notes

- Third-party copies of Broadcom CA manuals, if found later, may be stale, incomplete, license-restricted, or tampered with.
- Legacy CA DocOps pages may describe unsupported releases, old install tooling, or pre-Broadcom packaging.
- Search snippets are not enough to infer install method, FMIDs, SMP/E zones, HOLDDATA, z/OSMF workflow availability, generated JCL, or runtime customization.
- Security-product documentation for ACF2 and Top Secret can directly affect authority, access control, and clone-local authentication posture; do not infer behavior from stale or unofficial documents.
- Non-canonical documents should never override current Broadcom TechDocs, current product media, entitled support instructions, or site-owned install evidence.

## How To Use Non-Canonical Leads Safely

1. Use non-canonical hits only to discover product names, former names, URL fragments, release labels, FMIDs, generated JCL member names, and missing search terms.
2. Re-resolve every lead through Broadcom Support Portal, TechDocs, My Downloads, Compatibility Matrix, Product Lifecycle, Knowledge Base Articles, or Installation and Maintenance Tools.
3. Record the canonical replacement URL before using the information in a product definition.
4. If no canonical replacement exists, classify the fact as `unverified_noncanonical` and keep it out of executable automation.
5. Require human review before using any third-party PDF, mirrored manual, customer runbook, or copied install job as evidence.

## Next Search Pass

The next non-canonical pass should be narrower and entitlement-informed:

1. Start with actual licensed product names, exact versions, and aliases from Broadcom My Entitlements.
2. Search exact title strings from the confirmed TechDocs pages.
3. Search exact FMIDs, product codes, package identifiers, SMP/E function names, generated JCL member names, and legacy CA product names.
4. Retry Internet Archive exact URL checks for current TechDocs URLs and any legacy DocOps URLs found in site runbooks.
5. Use signed-in GitHub search only for accidental exposure of local JCL/runbooks, not for authoritative docs.
6. Promote only canonical Broadcom-confirmed facts into product definitions.
