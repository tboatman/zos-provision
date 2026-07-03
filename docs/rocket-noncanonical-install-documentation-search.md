# Rocket Non-Canonical Install Documentation Search

## Purpose

Record non-canonical Rocket Software installation documentation leads found outside the current official Rocket documentation portal, support portal, and product catalog.

This document is intentionally cautious. Rocket's mainframe portfolio was assembled through acquisitions (ASG Technologies in 2021, Mainstar Software in 2016, Aldon Computer Group in 2011, Zephyr Development Corporation in 2012, and Micro Focus/Serena ChangeMan more recently), so non-canonical sources can help discover legacy product names, old vendor domains, historical install mechanics, and missing search terms. They must not be treated as implementation authority. Rocket documentation portal (`docs.rocketsoftware.com`), Rocket support portal (`my.rocketsoftware.com`), the Rocket product catalog, and entitled product media remain the canonical sources.

Retrieval pass: 2026-07-02.

## Search Flow

Diagram source: [docs/diagrams/rocket-noncanonical-install-doc-search-flow.mmd](diagrams/rocket-noncanonical-install-doc-search-flow.mmd)

## Result Summary

The public non-canonical search was moderate yield, mostly because Rocket's acquisition history leaves a trail of legacy vendor domains and third-party mirrors that plain product-catalog browsing does not surface.

Useful findings:

- Legacy ASG Technologies domains (`support.asg.com`, `isp.asg.com`, `forum.asg-rd.com`) still appear in search-engine indexes with real ASG-TMON and ASG ACCESS Portal content, but every domain probed in this pass is in some state of decommissioning: `isp.asg.com` no longer resolves in DNS, `support.asg.com` and `forum.asg-rd.com` returned TLS certificate mismatches (the certificate served does not match the requested hostname), and `forum.asg-rd.com`'s certificate now names `remotedesktop.rocketsoftware.com`, implying consolidation into a Rocket-owned host.
- The legacy Micro Focus ChangeMan ZMF documentation tree (`microfocus.com/documentation/changeman-zmf/`) has been retired: the root path now redirects to an OpenText 404 page, and a previously-indexed deep link for the 8.2 Patch 7 Installation Guide returned HTTP 404 in this pass. Micro Focus/Serena documentation URLs found via search should be treated as decaying, not stable.
- A third-party manual mirror, Manualzz, has an indexed page titled "Serena ChangeMan ZMF Installation Guide," but the page returned HTTP 403 when fetched directly in this pass, so its actual content and completeness could not be verified.
- CiteSeerX, an academic paper mirror, hosts a copy of "ASG-TMON for CICS TS for z/OS System Administrator's Guide Version 2.3" — an unusual and unofficial hosting location for vendor installation documentation.
- SlideShare hosts historical Serena Software Virtual User Group (VUG) presentation decks (2013, 2016) and partner decks (e.g., "Top 10 ChangeMan ZMF Customizations") that discuss ChangeMan ZMF administration and customization practices.
- IBM Mainframe Forum has a public ChangeMan ZMF board, but direct fetch returned HTTP 403 in this pass; its content could not be confirmed beyond the fact that it is indexed and exists.
- SHARE (share.org / share.confex.com) conference proceedings are a plausible venue, but this pass found no TMON-, PRO/JCL-, or ChangeMan-specific SHARE session handouts; general CICS and OMEGAMON sessions were the closest matches.
- General public web search did not surface reliable third-party copies of Mainstar, Enterprise Suite, or DataEdge installation manuals; the ASG-TMON and ChangeMan ZMF families are the ones with a visible non-canonical footprint, likely because both have long pre-Rocket histories (ASG, Serena/Micro Focus).
- Public GitHub search for exact product names plus `SMP/E`, `FMID`, or `.jcl` terms returned no relevant unauthenticated results; matches were unrelated Zowe, IBM, and third-party projects.
- Internet Archive / Wayback Machine could not be queried directly in this pass — the CDX API and `web.archive.org` page fetches were both blocked by the retrieval environment used for this search ("unable to fetch from web.archive.org"). This is an open item: a human with unrestricted web access should retry CDX lookups for `asg.com`, `mainstar.com`, `aldon.com`, and `docs.rocketsoftware.com` before concluding there is no archival evidence.

Practical conclusion: non-canonical public sources are not enough to reconstruct Rocket installation procedures, and several of the most promising legacy-domain leads are actively decaying (dead DNS, broken TLS, 404s). They are useful only as breadcrumbs for product naming history and to corroborate that the ASG-TMON and ChangeMan ZMF lineages are the most externally documented. The project still needs Rocket support portal login, entitlements, licensed product inventory, and product-specific documentation exports through canonical channels.

## Non-Canonical Source Classes Checked

| Source class | Status | Evidence value | Use |
| --- | --- | --- | --- |
| Legacy ASG Technologies domains (`support.asg.com`, `isp.asg.com`, `forum.asg-rd.com`) | Indexed content found; live reachability failed in this pass (DNS dead / TLS cert mismatch) | Medium for historical product naming and structure; low for current install mechanics | Use only to recover legacy product names and confirm consolidation into Rocket domains |
| Legacy Micro Focus/Serena ChangeMan documentation tree | Root retired to OpenText 404; deep-linked version PDF returned 404 in this pass | Low and decaying | Do not rely on specific URLs; re-search each time |
| Third-party PDF mirrors (Manualzz, CiteSeerX) | Indexed but unverifiable in this pass (403 on Manualzz; CiteSeerX page not opened) | Low | Do not use unless provenance, version, and licensing are reviewed |
| Public GitHub code search | No usable unauthenticated result for exact product plus SMP/E/FMID/JCL terms | Low | Recheck only with a signed-in account or GitHub code search API if needed |
| Community forums (IBM Mainframe Forum) and conference decks (SlideShare VUG decks) | Indexed and exist; direct content confirmation blocked or partial in this pass | Low to medium as symptom/customization context, not install authority | Use only for administration and customization discussion, not baseline installation |
| SHARE conference proceedings | No product-specific session found in this pass | Very low | Recheck with narrower session-title searches by year |
| Internet Archive / Wayback Machine | Could not be queried in this pass (tool-level block) | Unknown | Retry with a browser or unrestricted fetch tool before concluding no snapshots exist |

## Legacy Domain and Mirror Leads

These are non-canonical remnants of pre-Rocket vendor branding. Use them as historical evidence only.

| Source | Evidence from this pass | Notes |
| --- | --- | --- |
| `https://support.asg.com/PERF/TMI/lsmig/...` and `https://support.asg.com/perf/TCE/tcerg42/...` | Search-indexed pages titled with real ASG-TMON content (e.g., "TMON for Web Services Considerations," "The TMON for CICS TS Environment") | Direct fetch failed with a TLS certificate mismatch (`support.asg.com` served a `*.dnsmadeeasy.com` certificate); live status unverified |
| `https://isp.asg.com/default.aspx` (ASG ACCESS Portal) | Referenced from Rocket's own JCLPREP user guide page as a legacy portal name | DNS lookup failed (`ENOTFOUND`) in this pass; domain appears fully decommissioned |
| `https://forum.asg-rd.com/portal.php` (ASG Remote Desktop community forum) | Indexed as a legacy Remote Desktop support forum | TLS certificate presented names `remotedesktop.rocketsoftware.com`, indicating the legacy forum has been folded into a Rocket-owned host |
| `https://www.microfocus.com/documentation/changeman-zmf/` | Root path | Returns HTTP 301 to `opentext.com/404` in this pass; documentation tree is retired |
| `https://www.microfocus.com/documentation/changeman-zmf/8-2-patch-7/pdfs/zmf-installation-guide.pdf` | Previously indexed direct PDF | Returned HTTP 404 in this pass |
| `https://manualzz.com/doc/36113449/serena-changeman-zmf-installation-guide` | Indexed title: "Serena ChangeMan ZMF Installation Guide" | Returned HTTP 403 on direct fetch in this pass; content and version unverified |
| CiteSeerX copy of "ASG-TMON for CICS TS for z/OS System Administrator's Guide Version 2.3" | Indexed academic-mirror PDF | Unusual hosting location; not opened in this pass, treat as unverified provenance |
| SlideShare Serena Software VUG decks (2013, 2016) and partner customization decks | Indexed titles confirm ChangeMan ZMF administration/customization topics | Community/vendor-partner material, not vendor-authoritative installation instructions |
| IBM Mainframe Forum ChangeMan ZMF board (`ibmmainframeforum.com/changeman-zmf/`) | Indexed as an existing public forum section | Direct fetch returned HTTP 403 in this pass; content unconfirmed |

## Public Search Terms Tried

These terms produced only the leads captured above, or nothing useful, in this pass:

```text
"TMON" ASG installation guide filetype:pdf
Mainstar "installation guide" filetype:pdf mainframe
ChangeMan ZMF installation guide filetype:pdf
"TMON for CICS" OR "TMON for z/OS" SHARE.org conference session PDF
TMON ChangeMan ZMF conference presentation slides SHARE mainframe
site:github.com rocketsoftware TMON OR ChangeMan OR "PRO/JCL" SMP/E
"ChangeMan ZMF" OR "PRO/JCL" OR TMON site:github.com "SMP/E" OR "FMID" OR ".jcl"
manualzz OR manualslib "ChangeMan" OR "TMON" OR "Mainstar" mainframe
web.archive.org asg.com OR mainstar.com OR aldon.com
```

## Risk Notes

- Third-party copies of Rocket, ASG, or Micro Focus/Serena manuals, if found later, may be stale, incomplete, license-restricted, tied to a pre-acquisition release, or tampered with.
- Legacy ASG and Micro Focus domains are actively decaying (dead DNS, broken TLS certificates, retired documentation trees). Any URL recovered from search-engine caches must be re-verified live before use, and should be expected to fail.
- Search snippets and forum threads are not enough to infer install method, FMIDs, SMP/E zones, HOLDDATA, z/OSMF workflows, generated JCL, or USS layout.
- Community forum and conference-deck content reflects customization and administration practices from specific sites and specific historical versions; it does not represent current Rocket-supported procedure.
- Non-canonical documents should never override current Rocket documentation, current product media, or entitled support instructions.

## How To Use Non-Canonical Leads Safely

1. Use non-canonical hits only to discover product names, former (ASG/Mainstar/Aldon/Zephyr/Micro Focus/Serena) names, URL fragments, release labels, or missing search terms.
2. Re-resolve every lead through the Rocket documentation portal, Rocket support portal, or the Rocket product catalog.
3. Record the canonical replacement URL before using the information in a product definition.
4. If no canonical replacement exists, classify the fact as `unverified_noncanonical` and keep it out of executable automation.
5. Require human review before using any third-party PDF mirror, forum post, or conference deck as evidence, and expect legacy-domain URLs to be dead or redirected.

## Next Search Pass

The next non-canonical pass should be narrower and entitlement-informed:

1. Start with actual licensed product names and exact versions from the Rocket support portal.
2. Search for exact title strings from entitled Rocket product pages, including legacy ASG, Mainstar, Aldon, Zephyr, and Micro Focus/Serena names.
3. Retry Internet Archive CDX lookups for `asg.com`, `mainstar.com`, `aldon.com`, and `docs.rocketsoftware.com` using a tool or browser that is not blocked from reaching `web.archive.org`.
4. Attempt to open the Manualzz and IBM Mainframe Forum pages found in this pass through a normal browser session to confirm or refute the 403 responses seen here.
5. Use signed-in GitHub search only for accidental exposure of local JCL/runbooks, not for authoritative docs.
