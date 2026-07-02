# Vendor Adapter Skeletons

## Purpose

Define the skeleton process overlays for vendor-dependent product deployment while keeping the high-level lifecycle shared across BMC, Broadcom CA, and Rocket Software products.

The intent is not to guess product-specific install instructions. The intent is to reserve the right extension points now so each vendor and product family can be onboarded without bending the generic architecture.

## Shared Lifecycle Phases

Every vendor adapter should implement these phases:

1. **Discover**
   - Identify installed products, releases, FMIDs, libraries, started tasks, runtime hooks, and local configuration.

2. **Intake**
   - Register product media, service media, z/OSMF workflows, generated JCL, HOLDDATA, checksums, release notes, and licenses in the internal vendor software depot.

3. **Classify**
   - Assign install method: `zosmf_workflow_managed`, `smpe_managed`, `vendor_jcl_managed`, `runtime_only`, `hybrid_zosmf_plus_runtime`, `hybrid_smpe_plus_runtime`, `legacy_manual_capture`, or `unknown_pending_discovery`.

4. **Plan**
   - Resolve dependencies, required approvals, affected subsystems, affected middleware regions, runtime hooks, backout posture, and expected evidence.

5. **Stage**
   - Stage media, workflows, variable files, JCL, SQL, bind control files, parameter members, security definitions, and runtime overlays.

6. **Check**
   - Run non-invasive validation such as SMP/E CHECK, z/OSMF workflow validation, generated JCL validation, authority checks, catalog queries, and dataset existence checks.

7. **Apply**
   - Execute z/OSMF workflows, SMP/E APPLY, vendor install JCL, runtime configuration, bind jobs, utility jobs, or customization steps.

8. **Verify**
   - Verify product state through CSI queries, catalog queries, job output, started task status, product commands, middleware state, and expected messages.

9. **Recover**
   - Restore prior runtime members, back out library references, restore SMP/E where possible, rebind prior packages, re-enable prior started task procedures, or mark manual recovery steps.

10. **Accept**
    - Run SMP/E ACCEPT or equivalent permanence step only after soak validation and approval.

11. **Evidence**
    - Publish product manifest, rendered artifacts, workflow output, JES output, before/after snapshots, approvals, and verification output.

Diagram source: [docs/diagrams/vendor-adapter-skeleton.mmd](diagrams/vendor-adapter-skeleton.mmd)

## Vendor Adapter Contract

Each vendor adapter should provide:

- Vendor identifier: `bmc`, `broadcom-ca`, or `rocket`.
- Product family identifier.
- Product code and product name normalization rules.
- Media naming and checksum rules.
- z/OSMF workflow discovery and variable mapping rules.
- SMP/E CSI and zone discovery rules.
- Vendor JCL discovery and symbol substitution rules.
- License and entitlement input model.
- Product library classification rules.
- Runtime hook classification rules.
- Security definition templates.
- Middleware integration templates for CICS, IMS, and Db2.
- Verification probes.
- Backout classification.
- Evidence extraction rules.

## BMC Adapter Skeleton

### Discovery

- Locate BMC product HLQs, common libraries, runtime libraries, sample libraries, and product parameter datasets.
- Identify BMC-related SMP/E CSIs, target zones, distribution zones, FMIDs, and SYSMOD status.
- Inventory BMC started tasks, STC user IDs, PROCLIB members, PARMLIB references, APF entries, LINKLIST entries, exits, and scheduler jobs.
- Detect BMC hooks in CICS, IMS, Db2, MQ, TCP/IP, Java, and USS where applicable.
- Capture existing BMC-generated install or customization JCL.

### Intake

- Store BMC base media, service media, HOLDDATA, generated JCL, z/OSMF workflow assets if present, release notes, license artifacts, and checksums under `depot/vendors/bmc`.
- Normalize product codes, FMIDs, and common-component dependencies.

### Install Overlay

- Prefer z/OSMF workflow execution when usable workflow assets exist and APIs are approved.
- Use SMP/E adapter for SMP/E-packaged products and service.
- Use vendor JCL adapter for generated customization or install jobs not represented by z/OSMF.
- Split base install from runtime hooks.

### Runtime Overlay

- Model BMC runtime libraries, product parameter members, started tasks, APF/LINKLIST/LPA changes, CICS/IMS/Db2 hooks, and product-specific operator commands.
- Keep BMC common infrastructure dependencies separate from product-specific runtime configuration.

### Verification

- Verify CSI state, target libraries, started task status, product command output, CICS/IMS/Db2 hook state, license status where observable, and expected messages.

## Broadcom CA Adapter Skeleton

### Discovery

- Locate Broadcom CA product HLQs, legacy CA-prefixed product libraries, common services libraries, runtime libraries, sample libraries, and parameter datasets.
- Identify SMP/E CSIs, target zones, distribution zones, FMIDs, SYSMOD status, and any product-family common components.
- Inventory started tasks, STC user IDs, PROCLIB members, PARMLIB references, APF entries, LINKLIST entries, exits, ISPF assets, CLIST/REXX assets, scheduler jobs, and security definitions.
- Detect hooks into CICS, IMS, Db2, MQ, TCP/IP, Java, USS, security products, automation products, and scheduler products where applicable.
- Capture legacy install jobs, generated JCL, z/OSMF workflows if available, and site-specific customization jobs.

### Intake

- Store base media, maintenance media, HOLDDATA, generated JCL, z/OSMF workflow assets, release notes, license/entitlement artifacts, and checksums under `depot/vendors/broadcom-ca`.
- Preserve both current Broadcom naming and locally used CA-prefixed product names as aliases.

### Install Overlay

- Prefer z/OSMF workflow execution where provided or site-authored.
- Use SMP/E adapter for SMP/E-packaged products and service.
- Use vendor JCL adapter for legacy generated jobs, common services customization, or product-specific setup jobs.
- Treat shared Broadcom/CA common services as dependency products with their own product definitions.

### Runtime Overlay

- Model common services, product-specific started tasks, APF/LINKLIST/LPA changes, PARMLIB and PROCLIB changes, exits, panels, skeletons, CLIST/REXX assets, and middleware hooks.
- Preserve local legacy names, aliases, and procedure conventions instead of forcing renames during discovery.

### Verification

- Verify CSI state, common service state, product command output, started task status, middleware hooks, security access, ISPF asset availability, and expected job messages.

## Rocket Adapter Skeleton

### Discovery

- Locate Rocket product HLQs, runtime libraries, sample libraries, USS components, product configuration datasets, and any IBM-partnered or inherited product components.
- Identify SMP/E CSIs, target zones, distribution zones, FMIDs, SYSMOD status, generated JCL, z/OSMF workflow assets if present, and product-specific installers.
- Inventory started tasks, STC user IDs, PROCLIB members, PARMLIB references, APF entries, LINKLIST entries, exits, USS paths, Java paths, scheduler jobs, and security definitions.
- Detect hooks into CICS, IMS, Db2, MQ, TCP/IP, Java, USS, terminal access, data integration, and development tooling where applicable.

### Intake

- Store Rocket base media, maintenance media, generated JCL, z/OSMF workflows, USS payloads, release notes, license/entitlement artifacts, and checksums under `depot/vendors/rocket`.
- Normalize product family names where Rocket products are IBM-partnered, acquired, rebranded, or locally known by legacy names.

### Install Overlay

- Prefer z/OSMF workflow execution where workflow assets and APIs are available.
- Use SMP/E adapter for SMP/E-packaged products and service.
- Use vendor JCL adapter for generated install jobs, USS extraction jobs, Java setup jobs, or product-specific runtime customization.
- Treat USS components and MVS components as separate install surfaces under one product definition.

### Runtime Overlay

- Model MVS libraries, USS directories, started tasks, APF/LINKLIST/LPA changes, Java or LE requirements, product parameter files, middleware hooks, and external connectivity endpoints.
- Keep USS file ownership, tagging, permissions, and mount dependencies explicit.

### Verification

- Verify CSI state, MVS library state, USS file state, started task status, product command output, middleware hooks, connectivity probes, license status where observable, and expected messages.

## Product-Specific Adapter Files

Each onboarded product should eventually get a product-specific adapter file:

```text
product_definitions/
  vendors/
    bmc/
      <product-code>.yml
    broadcom-ca/
      <product-code>.yml
    rocket/
      <product-code>.yml
```

Each product-specific file should fill in:

- Product aliases.
- Install method.
- Required artifacts.
- Required common components.
- z/OSMF workflow names and variables.
- SMP/E FMIDs and target zones.
- Vendor JCL members and symbolic variables.
- Runtime hooks.
- Verification probes.
- Backout steps.
- Evidence expectations.

## Skeleton Rule

Do not implement a product directly in playbook logic. First classify it under the generic lifecycle, then add a vendor adapter overlay, then add a product-specific definition.
