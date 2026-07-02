# BMC Documentation Acquisition Plan

## Purpose

Define how to find, capture, classify, and normalize BMC installation and maintenance documentation before implementing BMC product deployment automation.

The BMC workstream starts with documentation and media discovery, not playbook development.

## Official Entry Points

Use these official BMC entry points first:

- BMC Documentation home: https://docs.bmc.com/
- BMC Mainframe documentation catalog: https://docs.bmc.com/xwiki/bin/view/Mainframe/
- BMC Mainframe Ops documentation catalog: https://docs.bmc.com/xwiki/bin/view/Mainframe/Ops/
- BMC Mainframe DevX documentation catalog: https://docs.bmc.com/xwiki/bin/view/Mainframe/DevX/
- BMC Product Support pages: https://docs.bmc.com/xwiki/bin/view/Standalone/Product-Support/productinfo/
- BMC Product Downloads: https://www.bmc.com/support/resources/product-downloads.html
- BMC Support Central: https://www.bmc.com/support/support-central.html

The BMC Mainframe documentation catalog is the primary product/version map. The Product Download Tool is the primary source for product media, patches, and installation tools.

For the first BMC targets, see [BMC AMI Target Documentation Intake](bmc-ami-target-documentation-intake.md), covering BMC AMI Ops Monitoring and Monitor products, BMC AMI Ops Automation, and the BMC AMI DevX product family.

## Acquisition Flow

Diagram source: [docs/diagrams/bmc-documentation-acquisition-flow.mmd](diagrams/bmc-documentation-acquisition-flow.mmd)

## Step 1: Build the BMC Product Inventory

Start from what the site owns and what is installed, not from the full BMC catalog.

Inputs:

- BMC entitlement list.
- Product Download Tool access.
- Current SMP/E CSI FMIDs and SYSMODs.
- Current BMC HLQs.
- Current started tasks.
- Current APF/LINKLIST/LPA entries.
- Current PROCLIB/PARMLIB references.
- Current CICS, IMS, and Db2 hooks.
- Existing install jobs and historical JES output.
- Existing runbooks.

Output:

```text
product_definitions/vendors/bmc/product-inventory.yml
```

Required fields:

- Product name.
- Product aliases.
- BMC category: Cloud, Data for Db2, Data for IMS, DevX, Ops, Security, Storage, Common, or other.
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
- Customization guide.
- Configuration guide.
- Upgrade or migration guide.
- Maintenance or PTF application guide.
- Release notes.
- System requirements.
- Compatibility matrix.
- Security requirements.
- Product-specific messages and return-code references.
- z/OSMF workflow documentation, if present.
- SMP/E instructions, if present.
- Generated JCL instructions, if present.

Store references and snapshots under:

```text
depot/vendors/bmc/products/<product-code>/<version>/documentation/
```

Do not rely on browser bookmarks alone. Every product definition should carry the documentation URL and the retrieval date.

## Step 3: Capture Product Media and Tools

From BMC Product Downloads, capture:

- Base product media.
- Maintenance media.
- PTFs or cumulative service.
- HOLDDATA or equivalent advisories.
- Installation tools.
- Generated JCL packages.
- z/OSMF workflows, if provided.
- License or entitlement artifacts.
- Checksums or signatures where available.

Store under:

```text
depot/vendors/bmc/products/<product-code>/<version>/media/
depot/vendors/bmc/products/<product-code>/<version>/service/
depot/vendors/bmc/products/<product-code>/<version>/holdata/
depot/vendors/bmc/products/<product-code>/<version>/vendor-jcl/
depot/vendors/bmc/products/<product-code>/<version>/zosmf-workflows/
```

## Step 4: Classify the Install Method

Each BMC product/version must be classified before automation:

- `zosmf_workflow_managed`
- `smpe_managed`
- `vendor_jcl_managed`
- `runtime_only`
- `hybrid_zosmf_plus_runtime`
- `hybrid_smpe_plus_runtime`
- `legacy_manual_capture`
- `unknown_pending_discovery`

Classification rules:

- If BMC or the site provides a usable z/OSMF workflow, prefer `zosmf_workflow_managed` or `hybrid_zosmf_plus_runtime`.
- If the package contains standard SMP/E instructions and service flow, classify as `smpe_managed` or `hybrid_smpe_plus_runtime`.
- If install is driven by generated or vendor-supplied JCL, classify as `vendor_jcl_managed`.
- If the product is already installed and only runtime hooks need control, classify as `runtime_only`.
- If current state cannot be reconciled with documentation, classify as `legacy_manual_capture`.

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
product_definitions/vendors/bmc/<product-code>.yml
```

The product definition should be reviewed before any playbook uses it.

## Step 7: Pilot Selection

Pick two BMC products first:

- One SMP/E-heavy product.
- One runtime-integration-heavy product touching at least one of CICS, IMS, or Db2.

Good pilot criteria:

- Documentation is available.
- Product media is available.
- Product owner is known.
- Install method can be classified.
- Existing runtime state can be discovered.
- Backout can be described.

Avoid starting with the largest BMC suite or the product with the most undocumented local changes.

## Open Questions for BMC Intake

1. Which BMC products are actually licensed and in scope?
2. Who has BMC Product Download Tool access?
3. Which products have z/OSMF workflow assets?
4. Which products are installed today, and where are their SMP/E CSIs?
5. Which BMC common components are shared across products?
6. Which products hook into CICS, IMS, Db2, MQ, TCP/IP, USS, Java, or RACF?
7. Which historical install jobs and JES outputs are available?
8. Which BMC products should be the first two pilots?

## Rule

No BMC product automation should be written until its documentation, media, install method, runtime hooks, verification probes, and backout posture are captured in a product definition.
