# BMC First-Wave Documentation Intake

## Purpose

Define the immediate work required to obtain and normalize documentation for the first BMC focus area:

- BMC AMI Ops Monitoring and related AMI Ops Monitor products.
- BMC AMI Ops Automation.
- BMC AMI DevX product family.
- BMC Control-M suite, including Control-M for Mainframe and INCONTROL components.

This is a documentation and entitlement intake task. Do not start product automation from product names alone.

## Acquisition Flow

Diagram source: [docs/diagrams/bmc-first-wave-documentation-intake-flow.mmd](diagrams/bmc-first-wave-documentation-intake-flow.mmd)

## Official BMC Entry Points

Use these first:

- BMC Documentation home: https://docs.bmc.com/
- Mainframe Ops documentation catalog: https://docs.bmc.com/xwiki/bin/view/Mainframe/Ops/
- Mainframe DevX documentation catalog: https://docs.bmc.com/xwiki/bin/view/Mainframe/DevX/
- BMC Product Downloads: https://www.bmc.com/support/resources/product-downloads.html
- BMC Support Central: https://www.bmc.com/support/support-central.html
- BMC Product Support pages: https://docs.bmc.com/xwiki/bin/view/Standalone/Product-Support/productinfo/
- Control-M documentation: https://documents.bmc.com/supportu/9.0.21.300/en-US/Documentation/home.htm
- Control-M for Mainframe and INCONTROL documentation: https://documents.bmc.com/supportu/INC/9.0.21/en-US/home.htm

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

## Target 4: BMC Control-M Suite

### Current Official Index Signal

The BMC Documentation home links Control-M as a workload automation solution and separately links Control-M for Mainframe. The current Control-M documentation entry point is hosted on `documents.bmc.com` and exposes version `9.0.21.300`. The Control-M for Mainframe link opens the INCONTROL documentation space, also marked version `9.0.21.300`.

Treat Control-M as a suite, not a single z/OS product. It spans distributed Control-M services, agents, plug-ins, Automation API, and mainframe INCONTROL components.

Visible Control-M documentation areas include:

- Get started.
- Architecture.
- Planning workflows.
- Creating jobs, folders, events, resources, calendars, and variables.
- Monitoring workflows, jobs, services, alerts, and viewpoints.
- Reports.
- Administration.
- Installation.
- Security.
- Agents.
- Authentication.
- Authorizations.
- System settings.
- Site standards.
- Migration.
- Plug-ins.
- Application Integrator.
- Control-M integrations.
- Managed File Transfer.
- Control-M for SAP.
- Automation API installation, tutorials, code reference, and services.
- Conversion tooling.

Visible Control-M for Mainframe and INCONTROL areas include:

- Installing INCONTROL.
- Control-M for z/OS.
- Control-M/Restart.
- Control-M Analyzer.
- Control-M Assist.
- Control-O.
- COSMOS.
- INCONTROL administration.
- Utilities.
- Security.
- Messages.
- KeyStroke Language.
- Control-M JCL Verify.
- Control-D and Control-V.
- Control-M/Tape.
- Conversion from existing workflows.

### What To Capture

For the Control-M suite and each licensed mainframe component, capture:

- Product page URL.
- Documentation version and publication date.
- Licensed component list.
- Whether detailed pages require login.
- Distributed Control-M architecture and installation requirements.
- Control-M for z/OS and INCONTROL installation requirements.
- Mainframe agent, server, gateway, and communication topology.
- Authentication, authorization, and security model.
- RACF profiles, started tasks, users, groups, permits, certificates, and keyrings.
- TCP/IP ports, DNS, TLS, firewall, and endpoint requirements.
- Db2, CICS, IMS, MQ, SAP, file transfer, and other plug-in dependencies if licensed.
- Agent deployment and maintenance procedures.
- Control-M/Restart, Control-M Analyzer, Control-M Assist, Control-O, COSMOS, JCL Verify, Control-D, Control-V, and Control-M/Tape documentation if licensed.
- Job definition, calendar, variable, resource, event, and site-standard migration requirements.
- Automation API installation and credential requirements.
- Conversion tooling and source-data extraction requirements.
- Product media, service, patches, HOLDDATA, generated JCL, and installation tools.
- Verification commands, health checks, workflow checks, and expected evidence.
- Backout and maintenance instructions.

### Initial Product Codes

Use these temporary product-definition codes until entitlement and media names are confirmed:

```text
bmc-control-m-suite
bmc-control-m-mainframe
bmc-incontrol
bmc-control-m-zos
bmc-control-m-restart
bmc-control-m-analyzer
bmc-control-m-assist
bmc-control-o
bmc-control-m-jcl-verify
bmc-control-d-v
bmc-control-m-tape
bmc-control-m-automation-api
```

## Document Capture Procedure

For each target product/version:

1. Open the BMC Docs product page from the Ops, DevX, Control-M, or INCONTROL catalog.
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
product_definitions/vendors/bmc/bmc-control-m-suite.yml
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
5. Which BMC Control-M, Control-M for Mainframe, and INCONTROL components are licensed?
6. Is Control-M in scope as a full distributed-plus-mainframe suite or only the z/OS/INCONTROL side?
7. Who can log in to BMC Docs and Support Central?
8. Who can use the Product Download Tool?
9. Are PDFs or HTML exports allowed for internal use?
10. Which currently installed BMC HLQs and SMP/E CSIs correspond to these products?
11. Are MainView, AutoOPERATOR, Compuware, INCONTROL, Control-D, Control-O, or other legacy names still used locally?
12. Which product should be the first implementation pilot after documentation intake?

## Rule

For these BMC targets, the first deliverable is not Ansible code. The first deliverable is a verified product inventory with documentation URLs, login/export status, media identifiers, installed-state evidence, and enough install/runtime detail to classify each product.
