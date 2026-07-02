# BMC AMI Target Documentation Intake

## Purpose

Define the immediate work required to obtain and normalize documentation for the first BMC focus area:

- BMC AMI Ops Monitoring and related AMI Ops Monitor products.
- BMC AMI Ops Automation.
- BMC AMI DevX product family.

This is a documentation and entitlement intake task. Do not start product automation from product names alone.

## Acquisition Flow

Diagram source: [docs/diagrams/bmc-ami-target-documentation-intake-flow.mmd](diagrams/bmc-ami-target-documentation-intake-flow.mmd)

## Official BMC Entry Points

Use these first:

- BMC Documentation home: https://docs.bmc.com/
- Mainframe Ops documentation catalog: https://docs.bmc.com/xwiki/bin/view/Mainframe/Ops/
- Mainframe DevX documentation catalog: https://docs.bmc.com/xwiki/bin/view/Mainframe/DevX/
- BMC Product Downloads: https://www.bmc.com/support/resources/product-downloads.html
- BMC Support Central: https://www.bmc.com/support/support-central.html
- BMC Product Support pages: https://docs.bmc.com/xwiki/bin/view/Standalone/Product-Support/productinfo/

The public documentation indexes identify product families, products, and visible versions. The detailed product pages can require BMC login or registration. Treat BMC account access as a blocking prerequisite, not an implementation detail.

## Access Required

Before useful capture can start, obtain:

- BMC Docs login.
- BMC Support Central login.
- Product Download Tool access.
- Licensed Products access.
- Product Patches access.
- Mainframe Installation access.
- Mainframe Maintenance access.
- Permission to export BMC Docs pages to PDF or HTML for internal evidence.
- Permission to download base media, service, HOLDDATA, generated JCL, workflows, and installation tools.

## Target 1: BMC AMI Ops Monitoring and Monitors

### Current Official Index Signal

The BMC Mainframe Ops catalog exposes `BMC AMI Ops Monitoring` as a solution with visible version `7.1`, and product pages identify the solution releases `7.1.02`, `7.1.01`, and `7.1.00`.

The `7.1.02` solution page points to these component products:

- BMC AMI Ops Monitor for CMF 6.5.
- BMC AMI Ops Monitor for z/OS 6.5.
- BMC AMI Ops Monitor for UNIX System Services 6.5.
- BMC AMI Ops SYSPROG Services 6.5.

The Ops catalog also exposes individual monitor products that may be separately licensed or installed:

- BMC AMI Ops Monitor for CICS.
- BMC AMI Ops Monitor for CMF.
- BMC AMI Ops Monitor for DBCTL.
- BMC AMI Ops Monitor for Db2.
- BMC AMI Ops Monitor for IMS Offline.
- BMC AMI Ops Monitor for IMS Online.
- BMC AMI Ops Monitor for IP.
- BMC AMI Ops Monitor for Java Environments.
- BMC AMI Ops Monitor for MQ.
- BMC AMI Ops Monitor for UNIX System Services.
- BMC AMI Ops Monitor for z/OS.
- BMC AMI Ops Monitoring solution.
- BMC AMI Ops SYSPROG Services.

### What To Capture

For the solution and every installed monitor product, capture:

- Product page URL.
- Visible version list.
- Whether detailed pages require login.
- Installation section.
- Customizing-after-installation section.
- Administering or security section.
- Integrating section.
- Messages library.
- Working with infrastructure section.
- Component relationship to BMC AMI Ops Infrastructure.
- SMP/E FMIDs and zones.
- Product HLQs.
- Started tasks.
- APF, LINKLIST, LPA, PARMLIB, and PROCLIB updates.
- CICS, IMS, Db2, MQ, TCP/IP, USS, Java, and RACF hooks.
- Verification commands and expected output.
- Backout and maintenance instructions.

### Initial Product Codes

Use these temporary product-definition codes until entitlement and media names are confirmed:

```text
bmc-ami-ops-monitoring
bmc-ami-ops-monitor-zos
bmc-ami-ops-monitor-cmf
bmc-ami-ops-monitor-uss
bmc-ami-ops-sysprog-services
```

## Target 2: BMC AMI Ops Automation

### Current Official Index Signal

The BMC Mainframe Ops catalog exposes `BMC AMI Ops Automation` with visible versions `8.4`, `8.3.01`, and `8.3`.

The `8.4` product page states that the documentation space covers BMC AMI Ops Automation, formerly MainView AutoOPERATOR, and product options including:

- BMC AMI Ops Automation Access NV.
- BMC AMI Ops Automation for CICS.
- BMC AMI Ops Automation for IMS.
- BMC AMI Ops Automation for MQ.
- BMC AMI Ops Automation for z/OS.
- BMC AMI Automation TapeSHARE.

### What To Capture

For the base product and each licensed option, capture:

- Product page URL.
- Visible version list.
- Whether detailed pages require login.
- Getting started section.
- Rules documentation.
- Basic automation applications documentation.
- Advanced automation and EXEC/API documentation.
- Installing section.
- Upgrading section.
- Customizing-after-installation section.
- Total Object Manager documentation, if licensed.
- MQ automation documentation, if licensed.
- BiiZ or BMC Impact Integration documentation, if licensed.
- Options reference.
- Solutions reference.
- Infrastructure documentation.
- Messages library.
- MainView AutoOPERATOR legacy-title mapping.
- Started tasks and subsystem IDs.
- Rules repositories, automation libraries, EXEC libraries, and parameter members.
- CICS, IMS, MQ, z/OS, TapeSHARE, console, RACF, and automation hooks.
- Verification commands and expected output.
- Backout and maintenance instructions.

### Initial Product Codes

Use these temporary product-definition codes until entitlement and media names are confirmed:

```text
bmc-ami-ops-automation
bmc-ami-ops-automation-cics
bmc-ami-ops-automation-ims
bmc-ami-ops-automation-mq
bmc-ami-ops-automation-zos
bmc-ami-automation-tapeshare
```

## Target 3: BMC AMI DevX

### Current Official Index Signal

`BMC AMI DevX` is not one install target. The BMC Mainframe DevX catalog is a product-family index. It includes multiple independently versioned products and web components.

Visible products include, but are not limited to:

- BMC AMI Common Enterprise Services.
- BMC AMI DevX Abend-AID.
- BMC AMI DevX Abend-AID Web.
- BMC AMI DevX Abend-AID for CICS.
- BMC AMI DevX Code Coverage.
- BMC AMI DevX Code Debug for CICS.
- BMC AMI DevX Code Debug for TSO and IMS.
- BMC AMI DevX Code Insights.
- BMC AMI DevX Code Pipeline.
- BMC AMI DevX Code Pipeline Web.
- BMC AMI DevX Data Studio.
- BMC AMI DevX DevEnterprise.
- BMC AMI DevX File-AID for Db2.
- BMC AMI DevX File-AID for IMS.
- BMC AMI DevX File-AID/Data Solutions.
- BMC AMI DevX File-AID/MVS.
- BMC AMI DevX File-AID/RDX.
- BMC AMI DevX Mainframe DevOps.
- BMC AMI DevX Performance Test.
- BMC AMI DevX Total Test.
- BMC AMI DevX Workbench for Eclipse.
- BMC AMI DevX Workbench for VS Code.
- BMC AMI DevX Xchange.
- BMC AMI Enterprise Common Components.
- BMC AMI Web Products Installation and Configuration.

Treat DevX as a family requiring a licensed-product inventory before selecting install pilots.

### What To Capture

For each licensed DevX product, capture:

- Product page URL.
- Visible version list.
- Whether detailed pages require login.
- Installation and configuration documentation.
- Upgrade and migration documentation.
- Web product installation documentation, if applicable.
- Common Enterprise Services or Enterprise Common Components dependency.
- Workbench desktop or VS Code dependency.
- Host-side runtime components.
- USS, Java, web, REST, and certificate requirements.
- CICS, IMS, Db2, IDMS, TSO, ISPF, batch, and RACF hooks.
- Product repositories, datasets, load libraries, sample libraries, and parameter libraries.
- Verification commands and expected output.
- Backout and maintenance instructions.

### Initial Product Codes

Use these temporary product-definition codes until entitlement and media names are confirmed:

```text
bmc-ami-devx-family
bmc-ami-devx-common-enterprise-services
bmc-ami-devx-abend-aid
bmc-ami-devx-code-debug
bmc-ami-devx-file-aid
bmc-ami-devx-total-test
bmc-ami-devx-workbench-vscode
bmc-ami-web-products-installation
```

## Document Capture Procedure

For each target product/version:

1. Open the BMC Docs product page from the Ops or DevX catalog.
2. Record the canonical URL, product title, visible versions, and last-modified date.
3. Log in if the page reports that authentication is required.
4. Export the product space or relevant page tree as PDF or HTML when permitted.
5. Capture links to installation, customization, administration, integration, messages, infrastructure, upgrade, and maintenance sections.
6. Capture legacy product names, especially MainView and Compuware names.
7. Open BMC Product Downloads and locate the exact product media for the licensed version.
8. Capture download package identifiers, filenames, checksums, service packages, HOLDDATA, generated JCL, z/OSMF workflows, and installation tools.
9. Cross-check media names against installed SMP/E CSI FMIDs and existing site HLQs.
10. Store the documentation and media metadata in the BMC depot and product definition.

## Output Files To Create Next

After access is available, create or update:

```text
product_definitions/vendors/bmc/product-inventory.yml
product_definitions/vendors/bmc/bmc-ami-ops-monitoring.yml
product_definitions/vendors/bmc/bmc-ami-ops-automation.yml
product_definitions/vendors/bmc/bmc-ami-devx-family.yml
```

Documentation snapshots and metadata should be stored under:

```text
depot/vendors/bmc/products/<product-code>/<version>/documentation/
depot/vendors/bmc/products/<product-code>/<version>/media/
depot/vendors/bmc/products/<product-code>/<version>/service/
depot/vendors/bmc/products/<product-code>/<version>/holdata/
depot/vendors/bmc/products/<product-code>/<version>/vendor-jcl/
depot/vendors/bmc/products/<product-code>/<version>/zosmf-workflows/
```

## Blocking Questions

1. Which exact BMC AMI Ops Monitor products are licensed, not merely visible in the public catalog?
2. Is BMC AMI Ops Monitoring licensed as the solution bundle, individual monitor products, or both?
3. Which BMC AMI Ops Automation options are licensed?
4. Which BMC AMI DevX products are licensed?
5. Who can log in to BMC Docs and Support Central?
6. Who can use the Product Download Tool?
7. Are PDFs or HTML exports allowed for internal use?
8. Which currently installed BMC HLQs and SMP/E CSIs correspond to these products?
9. Are MainView, AutoOPERATOR, Compuware, or other legacy names still used locally?
10. Which product should be the first implementation pilot after documentation intake?

## Rule

For these BMC targets, the first deliverable is not Ansible code. The first deliverable is a verified product inventory with documentation URLs, login/export status, media identifiers, installed-state evidence, and enough install/runtime detail to classify each product.
