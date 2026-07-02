# Rocket Documentation Acquisition Plan

## Purpose

Define how to find, capture, classify, and normalize Rocket Software mainframe installation and maintenance documentation before implementing Rocket product deployment automation.

The Rocket workstream starts with entitlement, documentation, media, installed-state, and USS/MVS surface discovery.

## Official Entry Points

Use these official Rocket entry points first:

- Rocket documentation portal: https://docs.rocketsoftware.com/
- Rocket documentation library: https://docs.rocketsoftware.com/bundle
- Rocket product catalog: https://www.rocketsoftware.com/
- Rocket support portal: https://my.rocketsoftware.com/
- Rocket product documentation link from the Rocket site navigation.

Some Rocket documentation, support, and download functions require support-portal login and entitlement. Treat access requirements as part of intake.

## Acquisition Flow

Diagram source: [docs/diagrams/rocket-documentation-acquisition-flow.mmd](diagrams/rocket-documentation-acquisition-flow.mmd)

## Step 1: Build the Rocket Product Inventory

Start from licensed and installed products. Rocket products may include MVS libraries, USS content, Java components, acquired/rebranded products, or IBM-partnered components.

Inputs:

- Rocket entitlement list.
- Rocket support portal access.
- Rocket documentation portal product URLs.
- Current SMP/E CSI FMIDs and SYSMODs.
- Current Rocket HLQs.
- Current USS installation paths.
- Current started tasks.
- Current APF/LINKLIST/LPA entries.
- Current PROCLIB/PARMLIB references.
- Current CICS, IMS, and Db2 hooks.
- Existing install jobs and historical JES output.
- Existing runbooks.

Output:

```text
product_definitions/vendors/rocket/product-inventory.yml
```

Required fields:

- Product name.
- Product aliases, including acquired, rebranded, or local names.
- Rocket category or product family.
- Version and release.
- FMIDs, if known.
- Installed HLQs.
- Installed USS paths.
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
- Compatibility information.
- Security requirements.
- USS file ownership, permission, and tagging requirements.
- Java, LE, TCP/IP, or external connectivity requirements.
- Product-specific messages and return-code references.
- z/OSMF workflow documentation, if present.
- Generated JCL instructions, if present.

Store references and snapshots under:

```text
depot/vendors/rocket/products/<product-code>/<version>/documentation/
```

Every product definition should carry the documentation URL, retrieval date, and whether login was required.

## Step 3: Capture Product Media and Tools

From Rocket support/download channels, capture:

- Base product media.
- Maintenance media.
- PTFs or cumulative service.
- HOLDDATA or equivalent advisories.
- Installation tools.
- Generated JCL packages.
- USS payloads.
- Java payloads.
- z/OSMF workflows, if provided.
- License or entitlement artifacts.
- Checksums or signatures where available.

Store under:

```text
depot/vendors/rocket/products/<product-code>/<version>/media/
depot/vendors/rocket/products/<product-code>/<version>/service/
depot/vendors/rocket/products/<product-code>/<version>/holdata/
depot/vendors/rocket/products/<product-code>/<version>/vendor-jcl/
depot/vendors/rocket/products/<product-code>/<version>/uss/
depot/vendors/rocket/products/<product-code>/<version>/zosmf-workflows/
```

## Step 4: Classify the Install Method

Each Rocket product/version must be classified before automation:

- `zosmf_workflow_managed`
- `smpe_managed`
- `vendor_jcl_managed`
- `runtime_only`
- `hybrid_zosmf_plus_runtime`
- `hybrid_smpe_plus_runtime`
- `legacy_manual_capture`
- `unknown_pending_discovery`

Classification rules:

- If Rocket or the site provides a usable z/OSMF workflow, prefer `zosmf_workflow_managed` or `hybrid_zosmf_plus_runtime`.
- If the package uses normal SMP/E receive/apply/accept maintenance, classify as `smpe_managed` or `hybrid_smpe_plus_runtime`.
- If install is driven by generated or vendor-supplied JCL, classify as `vendor_jcl_managed`.
- If USS extraction, Java setup, or mixed MVS/USS runtime work is material, preserve that as product metadata even if SMP/E is also used.
- If the product is already installed and only runtime hooks need control, classify as `runtime_only`.
- If acquired or rebranded product state cannot be reconciled with documentation, classify as `legacy_manual_capture`.

## Step 5: Extract Automation Requirements

For each product, extract:

- Required datasets.
- Required USS paths, ownership, permissions, and tagging.
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
- Required Java, LE, TCP/IP, or external endpoint setup.
- Required recycle, refresh, or IPL actions.
- Verification commands and expected output.
- Backout steps.

## Step 6: Create the Product Definition

Create one product definition per product/version:

```text
product_definitions/vendors/rocket/<product-code>.yml
```

The product definition should identify MVS and USS install surfaces separately.

## Step 7: Pilot Selection

Pick two Rocket products first:

- One SMP/E-heavy or MVS-library-heavy product.
- One mixed runtime product with USS, Java, connectivity, or middleware integration.

Good pilot criteria:

- Documentation is available.
- Product media is available.
- Product owner is known.
- Install method can be classified.
- MVS and USS surfaces can be discovered.
- Backout can be described.

Avoid starting with a product whose acquired/rebranded lineage, USS layout, or external connectivity dependencies are not understood.

## Open Questions for Rocket Intake

1. Which Rocket products are actually licensed and in scope?
2. Who has Rocket support portal and download access?
3. Which products have z/OSMF workflow assets?
4. Which products are installed today, and where are their SMP/E CSIs?
5. Which products have USS, Java, TCP/IP, or external connectivity surfaces?
6. Which products hook into CICS, IMS, Db2, MQ, TCP/IP, USS, Java, or RACF?
7. Which historical install jobs and JES outputs are available?
8. Which acquired, rebranded, or local names must be retained as aliases?
9. Which Rocket products should be the first two pilots?

## Rule

No Rocket product automation should be written until its documentation, media, install method, MVS and USS surfaces, runtime hooks, verification probes, backout posture, and product aliases are captured in a product definition.
