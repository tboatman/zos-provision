# Broadcom CA Documentation Acquisition Plan

## Purpose

Define how to find, capture, classify, and normalize Broadcom CA mainframe installation and maintenance documentation before implementing Broadcom CA product deployment automation.

The Broadcom CA workstream starts with entitlement, documentation, media, and installed-state discovery.

## Official Entry Points

Use these official Broadcom entry points first:

- Broadcom Support Portal: https://support.broadcom.com/
- Broadcom Mainframe Software support links from the Support Portal:
  - My Entitlements
  - My Downloads
  - Documentation
  - Compatibility Matrix
  - Product Lifecycle
  - Knowledge Base Articles
  - Installation and Maintenance Tools
- Broadcom TechDocs: https://techdocs.broadcom.com/
- Broadcom Mainframe Software product area: https://www.broadcom.com/products/mainframe

Some Broadcom/CA documentation and downloads require login and entitlement. Treat access requirements as part of intake.

For the first Broadcom CA targets, see [Broadcom CA First-Wave Documentation Intake](broadcom-ca-first-wave-documentation-intake.md), covering candidate DevOps, automation, security, database, and common-services product families.

## Acquisition Flow

Diagram source: [docs/diagrams/broadcom-ca-documentation-acquisition-flow.mmd](diagrams/broadcom-ca-documentation-acquisition-flow.mmd)

## Step 1: Build the Broadcom CA Product Inventory

Start from licensed and installed products. Preserve both Broadcom names and local CA-prefixed legacy names.

Inputs:

- Broadcom entitlement list.
- Broadcom Support Portal access.
- Broadcom My Downloads access.
- Broadcom TechDocs product URLs.
- Current SMP/E CSI FMIDs and SYSMODs.
- Current Broadcom/CA HLQs.
- Current started tasks.
- Current APF/LINKLIST/LPA entries.
- Current PROCLIB/PARMLIB references.
- Current CICS, IMS, and Db2 hooks.
- Existing install jobs and historical JES output.
- Existing runbooks.

Output:

```text
product_definitions/vendors/broadcom-ca/product-inventory.yml
```

Required fields:

- Product name.
- Product aliases, including CA-prefixed local names.
- Broadcom category or product family.
- Version and release.
- FMIDs, if known.
- Installed HLQs.
- Runtime started tasks.
- Install method, if known.
- Documentation URL.
- Download package identifiers.
- Owner.
- Status: `candidate`, `installed`, `needs_discovery`, `retired`, or `out_of_scope`.

## Step 2: Capture Product Documentation

For each product/version in scope, capture:

- Installation guide.
- Program directory or SMP/E install instructions.
- Customization guide.
- Configuration guide.
- Upgrade or migration guide.
- Maintenance or PTF application guide.
- Release notes.
- System requirements.
- Compatibility matrix entries.
- Product lifecycle dates.
- Security requirements.
- Product-specific messages and return-code references.
- z/OSMF workflow documentation, if present.
- Installation and maintenance tool instructions, if present.
- Generated JCL instructions, if present.

Store references and snapshots under:

```text
depot/vendors/broadcom-ca/products/<product-code>/<version>/documentation/
```

Every product definition should carry the documentation URL, retrieval date, and whether login was required.

## Step 3: Capture Product Media and Tools

From Broadcom My Downloads and related installation/maintenance tooling, capture:

- Base product media.
- Maintenance media.
- PTFs or cumulative service.
- HOLDDATA or equivalent advisories.
- Installation and maintenance tool artifacts.
- Generated JCL packages.
- z/OSMF workflows, if provided.
- License or entitlement artifacts.
- Checksums or signatures where available.

Store under:

```text
depot/vendors/broadcom-ca/products/<product-code>/<version>/media/
depot/vendors/broadcom-ca/products/<product-code>/<version>/service/
depot/vendors/broadcom-ca/products/<product-code>/<version>/holdata/
depot/vendors/broadcom-ca/products/<product-code>/<version>/vendor-jcl/
depot/vendors/broadcom-ca/products/<product-code>/<version>/zosmf-workflows/
```

## Step 4: Classify the Install Method

Each Broadcom CA product/version must be classified before automation:

- `zosmf_workflow_managed`
- `smpe_managed`
- `vendor_jcl_managed`
- `runtime_only`
- `hybrid_zosmf_plus_runtime`
- `hybrid_smpe_plus_runtime`
- `legacy_manual_capture`
- `unknown_pending_discovery`

Classification rules:

- If Broadcom or the site provides a usable z/OSMF workflow, prefer `zosmf_workflow_managed` or `hybrid_zosmf_plus_runtime`.
- If the package uses normal SMP/E receive/apply/accept maintenance, classify as `smpe_managed` or `hybrid_smpe_plus_runtime`.
- If install is driven by generated or vendor-supplied JCL, classify as `vendor_jcl_managed`.
- If the product is already installed and only runtime hooks need control, classify as `runtime_only`.
- If local CA-era state cannot be reconciled with documentation, classify as `legacy_manual_capture`.

## Step 5: Extract Automation Requirements

For each product, extract:

- Required datasets.
- Required USS paths.
- Required SMP/E zones.
- Required APF libraries.
- Required LINKLIST/LPA changes.
- Required PROCLIB/PARMLIB members.
- Required started tasks.
- Required RACF users, groups, profiles, permits, and classes.
- Required CICS definitions.
- Required IMS definitions.
- Required Db2 objects, plans, packages, collections, grants, and binds.
- Required z/OSMF workflow variables.
- Required jobcard/accounting symbols.
- Required license steps.
- Required recycle, refresh, or IPL actions.
- Verification commands and expected output.
- Backout steps.

## Step 6: Create the Product Definition

Create one product definition per product/version:

```text
product_definitions/vendors/broadcom-ca/<product-code>.yml
```

The product definition should preserve aliases and legacy CA names.

## Step 7: Pilot Selection

Pick two Broadcom CA products first:

- One SMP/E-heavy product.
- One runtime-integration-heavy product touching at least one of CICS, IMS, Db2, RACF, automation, or scheduling.

Good pilot criteria:

- Documentation is available.
- Product media is available.
- Product owner is known.
- Install method can be classified.
- Existing runtime state can be discovered.
- Backout can be described.

Avoid starting with a product whose local CA-era naming, exits, or common-service dependencies are not understood.

## Open Questions for Broadcom CA Intake

1. Which Broadcom CA products are actually licensed and in scope?
2. Who has Broadcom Support Portal and My Downloads access?
3. Which products have z/OSMF workflow assets?
4. Which products are installed today, and where are their SMP/E CSIs?
5. Which common services or shared components are dependencies?
6. Which products hook into CICS, IMS, Db2, MQ, TCP/IP, USS, Java, RACF, automation, or scheduling?
7. Which historical install jobs and JES outputs are available?
8. Which local CA-prefixed names must be retained as aliases?
9. Which Broadcom CA products should be the first two pilots?

## Rule

No Broadcom CA product automation should be written until its documentation, media, install method, runtime hooks, verification probes, backout posture, and legacy naming aliases are captured in a product definition.
