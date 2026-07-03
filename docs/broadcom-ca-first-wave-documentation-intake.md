# Broadcom CA First-Wave Documentation Intake

## Purpose

Define the immediate work required to obtain and normalize documentation for the first Broadcom CA focus area.

Candidate first-wave families:

- Mainframe DevOps and change management, especially Endevor-class tooling.
- Mainframe automation and operations, especially OPS/MVS-class tooling.
- Mainframe security, especially ACF2 and Top Secret-class tooling.
- Mainframe scheduling and workload automation, if licensed.
- Mainframe database and data management, especially Datacom and IDMS-class tooling.
- Shared Broadcom common services, installation tools, and maintenance tooling.

This is a documentation and entitlement intake task. Product names are candidates until confirmed by Broadcom entitlements, installed SMP/E state, and local ownership.

## Acquisition Flow

Diagram source: [docs/diagrams/broadcom-ca-first-wave-documentation-intake-flow.mmd](diagrams/broadcom-ca-first-wave-documentation-intake-flow.mmd)

## Official Broadcom Entry Points

Use these first:

- Broadcom Support Portal: https://support.broadcom.com/
- Broadcom Mainframe Software All Products: https://support.broadcom.com/web/ecx/all-products?segment=MF
- Broadcom TechDocs: https://techdocs.broadcom.com/
- Broadcom Mainframe Software product area: https://www.broadcom.com/products/mainframe

The Broadcom Support Portal exposes the mainframe software paths for My Entitlements, My Downloads, Documentation, Security Advisories, Communities, Compatibility Matrix, All Products, Learnings, Product Lifecycle, Knowledge Base Articles, and Installation and Maintenance Tools.

TechDocs may block unauthenticated access or require entitlement-backed login for specific product content. Treat Broadcom account access as a blocking prerequisite.

## Access Required

Before useful capture can start, obtain:

- Broadcom Support Portal login.
- My Entitlements access for Mainframe Software.
- My Downloads access for Mainframe Software.
- TechDocs access for Mainframe Software.
- Compatibility Matrix access.
- Product Lifecycle access.
- Knowledge Base access.
- Installation and Maintenance Tools access.
- Permission to export or internally snapshot TechDocs pages.
- Permission to download base media, service, HOLDDATA, generated JCL, z/OSMF workflows, and installation tooling.

## Target 1: Mainframe DevOps and Change Management

### Current Official Index Signal

Broadcom exposes mainframe documentation, all-products, downloads, and lifecycle entry points through the Support Portal. The exact product list must be confirmed from entitlements and installed state rather than assumed from public search.

Candidate products include Endevor-class source, package, promotion, and deployment tooling.

### What To Capture

For each licensed product/version, capture:

- Product page URL.
- Entitlement name and SKU/package name.
- Visible versions and product lifecycle status.
- Whether detailed pages require login.
- Installation guide.
- Upgrade or migration guide.
- Customization guide.
- Security and authorization guide.
- Program directory or SMP/E install instructions.
- Installation and Maintenance Tools output, if applicable.
- z/OSMF workflow documentation, if present.
- Generated JCL package, if present.
- Repository, inventory, package, processor, and promotion model.
- ISPF, batch, USS, REST, webhook, Git, CI/CD, and API surfaces.
- RACF users, groups, dataset profiles, FACILITY profiles, started-task profiles, and certificates.
- CICS, IMS, Db2, JES, USS, scheduler, and automation hooks.
- Verification commands and expected output.
- Backout and maintenance instructions.

### Initial Product Codes

Use these temporary product-definition codes until entitlement and media names are confirmed:

```text
broadcom-ca-devops-family
broadcom-ca-endevor-family
broadcom-ca-change-management
```

## Target 2: Automation, Operations, and Scheduling

### Current Official Index Signal

Broadcom mainframe support entry points expose the needed source systems, but the exact automation and scheduling products must be resolved from My Entitlements, My Downloads, installed HLQs, started tasks, and SMP/E CSIs.

Candidate products include OPS/MVS-class automation, workload automation, console automation, system automation, and operational integration tooling.

### What To Capture

For each licensed product/version, capture:

- Product page URL.
- Entitlement name and download package identifiers.
- Installation, upgrade, customization, administration, and messages documentation.
- Automation rule, policy, script, REXX, OPS, console, scheduler, and event models.
- Started tasks, subsystem IDs, PROCLIB members, PARMLIB members, and runtime datasets.
- APF, LINKLIST, LPA, JES, console command, OPERCMDS, and automation authority requirements.
- CICS, IMS, Db2, MQ, TCP/IP, USS, scheduler, and security hooks.
- z/OSMF workflow or generated JCL availability.
- Verification commands, operator commands, and expected messages.
- Backout and maintenance instructions.

### Initial Product Codes

Use these temporary product-definition codes until entitlement and media names are confirmed:

```text
broadcom-ca-automation-family
broadcom-ca-opsmvs-family
broadcom-ca-workload-automation-family
```

## Target 3: Security

### Current Official Index Signal

Broadcom mainframe estates commonly include CA-prefixed or Broadcom-branded security products. Because security product automation changes authority and clone-local authentication posture, this track requires stricter intake before any execution design.

Candidate products include ACF2-class and Top Secret-class security tooling.

### What To Capture

For each licensed product/version, capture:

- Product page URL.
- Security product identity, aliases, and local naming.
- Installation, upgrade, migration, administration, command, and messages documentation.
- System exits, started tasks, PARMLIB, PROCLIB, APF, LPA, and LINKLIST requirements.
- Dataset rule, resource rule, facility, class, certificate, keyring, and OMVS implications.
- Started-task user mapping.
- Clone-local identity model requirements.
- RACF coexistence or migration constraints, if any.
- Audit, compliance, report, and drift-detection requirements.
- Verification commands and expected output.
- Backout and emergency-access procedures.

### Initial Product Codes

Use these temporary product-definition codes until entitlement and media names are confirmed:

```text
broadcom-ca-security-family
broadcom-ca-acf2-family
broadcom-ca-top-secret-family
```

## Target 4: Database and Data Management

### Current Official Index Signal

Candidate products include Datacom-class and IDMS-class systems, plus related tools and utilities. These products may require deeper database lifecycle modeling than a simple product install.

### What To Capture

For each licensed product/version, capture:

- Product page URL.
- Installation, upgrade, migration, customization, administration, utility, and messages documentation.
- Database catalog/dictionary/repository model.
- Started tasks, batch jobs, runtime libraries, and utility libraries.
- CICS, IMS, Db2, batch, scheduler, RACF, and automation hooks.
- Backup, recovery, maintenance, and data migration procedures.
- z/OSMF workflow, SMP/E, generated JCL, and vendor tooling availability.
- Verification commands and expected output.
- Backout and recoverability posture.

### Initial Product Codes

Use these temporary product-definition codes until entitlement and media names are confirmed:

```text
broadcom-ca-database-family
broadcom-ca-datacom-family
broadcom-ca-idms-family
```

## Shared Broadcom Capture Procedure

For each target product/version:

1. Open Broadcom Support Portal and confirm Mainframe Software segment access.
2. Capture the entitlement name, product name, product aliases, and local CA-prefixed names.
3. Open TechDocs and record the canonical product/version documentation URL.
4. Record whether login or entitlement is required.
5. Export or internally snapshot installation, customization, upgrade, administration, security, messages, maintenance, and integration sections when permitted.
6. Open My Downloads and capture base media, service, HOLDDATA, installation tools, generated JCL, workflows, and checksums.
7. Check Compatibility Matrix and Product Lifecycle entries.
8. Cross-check media and docs against installed SMP/E CSI FMIDs, HLQs, started tasks, APF/LINKLIST/LPA, PROCLIB/PARMLIB, and subsystem hooks.
9. Store metadata in the Broadcom CA depot and product definition.

## Output Files To Create Next

After access is available, create or update:

```text
product_definitions/vendors/broadcom-ca/product-inventory.yml
product_definitions/vendors/broadcom-ca/broadcom-ca-devops-family.yml
product_definitions/vendors/broadcom-ca/broadcom-ca-automation-family.yml
product_definitions/vendors/broadcom-ca/broadcom-ca-security-family.yml
product_definitions/vendors/broadcom-ca/broadcom-ca-database-family.yml
```

## Blocking Questions

1. Which Broadcom CA products are licensed in My Entitlements?
2. Which legacy CA product names are still used locally?
3. Which products are installed today, and where are their SMP/E CSIs?
4. Who can access TechDocs, My Downloads, Compatibility Matrix, Product Lifecycle, and Installation and Maintenance Tools?
5. Which products have z/OSMF workflows or generated JCL?
6. Which products alter security authority, started tasks, exits, subsystem hooks, or clone-local identity?
7. Which Broadcom common services are dependencies?
8. Which product should be the first Broadcom CA implementation pilot?

## Rule

For Broadcom CA first-wave targets, the first deliverable is a verified entitlement and installed-state inventory with documentation URLs, media identifiers, lifecycle status, compatibility posture, install/runtime hooks, legacy aliases, and enough detail to classify each product.
