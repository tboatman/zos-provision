# Product Inventory and Entitlement Intake

## Purpose

Flesh out the product inventory and entitlement gaps for BMC, Broadcom CA, Rocket, IBM middleware, and sidequest products before any product automation is designed.

Gap coverage: G-003 and G-004 from [Top-to-Bottom Gap Assessment and Remediation Plan](top-to-bottom-gap-assessment.md). This document complements the BMC-specific unwinding plan, which is the first deep vendor slice.

Review pass: 2026-07-03.

## Inventory Flow

Diagram source: [docs/diagrams/product-inventory-entitlement-flow.mmd](diagrams/product-inventory-entitlement-flow.mmd)

## Core Rule

Product names are not inventory. Public documentation pages are not entitlement. Installed libraries are not enough to prove supported install method.

A product enters the managed path only after these evidence streams agree closely enough for a product owner to approve the model:

- Entitlement or license evidence.
- Canonical documentation evidence.
- Media and maintenance evidence.
- Installed-state evidence.
- Runtime integration evidence.
- Local ownership evidence.

## Inventory Record

Each discovered product should get an inventory record before a product definition.

Minimum shape:

```yaml
inventory_id: product-inventory-TBD
vendor: bmc | broadcom-ca | rocket | ibm | fischer | site | oss
product_name: TBD
product_code: TBD
aliases: []
family: TBD
business_owner: TBD
technical_owner: TBD
license:
  entitlement_source: TBD
  entitlement_name: TBD
  entitlement_status: confirmed | requested | unavailable | unknown
  license_restrictions: TBD
documentation:
  canonical_urls: []
  login_required: true
  exported_snapshot_allowed: unknown
media:
  base_media_id: TBD
  service_media_ids: []
  holdata: TBD
  checksums: []
installed_state:
  fmids: []
  smpe_csis: []
  target_zones: []
  distribution_zones: []
  hlqs: []
  runtime_libraries: []
  started_tasks: []
  uss_paths: []
runtime_hooks:
  cics: []
  ims: []
  db2: []
  mq: []
  tcpip: []
  security: []
  scheduler: []
classification:
  install_method: unknown_pending_discovery
  confidence: low | medium | high
  blocked_by: []
evidence_refs: []
```

## Entitlement Intake

For each vendor family:

| Vendor | Authoritative intake source | Required evidence |
| --- | --- | --- |
| BMC | BMC Support Central, Product Download Tool, Mainframe Installation, Mainframe Maintenance, product docs | Entitled product list, base media, service media, HOLDDATA, PSI/z/OSMF eligibility, Installation System or generated JCL evidence |
| Broadcom CA | Broadcom Support Portal, My Downloads, TechDocs, Compatibility Matrix, Product Lifecycle, Installation and Maintenance Tools | Entitled product list, product aliases, lifecycle status, compatibility, media, generated JCL, maintenance tooling, common services evidence |
| Rocket | Rocket support portal, Rocket docs portal, product pages, support/download channels | Licensed inventory, media, patches, documentation exports, acquired-name mapping, MVS and USS payload evidence |
| IBM middleware | IBM product docs, IBM Shopz or local software inventory, SMP/E CSIs | FMIDs, service level, HOLDDATA, z/OSMF Software Management or SMP/E evidence |
| Sidequest products | Current custodian, support contact, local installed-state evidence | Custody, license, docs, media, installed state, owner approval |
| OSS | Upstream repository, license file, release metadata, internal mirror | License, version, checksum, dependencies, mirror path, support owner |

## Installed-State Evidence

Installed-state discovery must collect:

- SMP/E CSI, target zone, distribution zone, FMID, and SYSMOD evidence.
- Product HLQs and data set attributes.
- Runtime libraries and sample libraries.
- APF, LINKLIST, LPA, PROGxx, PARMLIB, and PROCLIB references.
- Started tasks and job names.
- Security identities, profiles, permits, and command authorities.
- CICS, IMS, Db2, MQ, TCP/IP, USS, Java, scheduler, and automation hooks.
- Historical install, customization, maintenance, bind, and recovery jobs.
- Product runbooks and owner notes.

Do not collapse installed-state evidence into product definition until it is attributed as vendor base, common component, local override, dead config, or unknown.

## Classification States

Inventory classification states:

- `candidate_from_public_docs`: found in public docs or product pages only.
- `candidate_from_entitlement`: found in entitlement or licensed inventory.
- `candidate_from_installed_state`: found on a z/OS image.
- `evidence_reconciled`: entitlement, docs, media, and installed state are aligned enough for owner review.
- `product_definition_ready`: product owner approved conversion into product definition.
- `blocked`: missing license, media, documentation, owner, or support path.

Install method classification still uses the shared product lifecycle vocabulary:

- `smpe_managed`
- `zosmf_workflow_managed`
- `vendor_jcl_managed`
- `runtime_only`
- `hybrid_zosmf_plus_runtime`
- `hybrid_smpe_plus_runtime`
- `legacy_manual_capture`
- `unknown_pending_discovery`

## Reconciliation Rules

- If entitlement says product exists but no installed state is found, classify as licensed-not-installed or dormant.
- If installed state exists but entitlement is missing, classify as installed-entitlement-gap and block automation.
- If public docs list a product but entitlement does not, keep as candidate only.
- If SMP/E FMIDs exist but runtime hooks are missing, treat install as incomplete or inactive until owner review.
- If runtime hooks exist but SMP/E state is missing, treat as legacy capture until media and install method are proven.
- If multiple products share common components, create separate inventory records for common components if other products depend on them.

## Exit Criteria

This gap is materially reduced when:

- First-wave BMC, Broadcom CA, and Rocket products have inventory records.
- At least two BMC records are reconciled enough for product-definition drafts.
- Entitlement gaps are explicit rather than implicit.
- Installed-state evidence is separated from product-owner approval.
- Product definitions are created only from reconciled inventory records.

## Immediate Actions

1. Create a first-wave inventory table for BMC, Broadcom CA, and Rocket.
2. Mark every product as public-docs-only, entitlement-confirmed, installed-state-confirmed, reconciled, or blocked.
3. Use BMC as the first deep intake path.
4. Add common components as first-class inventory records where other products depend on them.
5. Feed approved inventory into product definition schema examples.
