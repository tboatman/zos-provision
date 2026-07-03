# Rocket First-Wave Documentation Intake

## Purpose

Define the immediate work required to obtain and normalize documentation for the first Rocket Software focus area.

Candidate first-wave families:

- TMON One and mainframe monitoring products.
- Mainstar storage, database, IMS, Db2, and z/OS optimization products.
- Enterprise Suite and Enterprise Developer for Z products.
- ChangeMan-related tools such as Comparex and StarTool where licensed.
- JCL tools such as JCLPREP, JOB/SCAN, and PRO/JCL where licensed.
- DataEdge and mainframe data access products where licensed.
- Rocket acquired, rebranded, or IBM-partnered products already installed locally.

This is a documentation and entitlement intake task. Product names are candidates until confirmed by Rocket support access, licensed inventory, installed SMP/E state, and local ownership.

## Acquisition Flow

Diagram source: [docs/diagrams/rocket-first-wave-documentation-intake-flow.mmd](diagrams/rocket-first-wave-documentation-intake-flow.mmd)

## Official Rocket Entry Points

Use these first:

- Rocket documentation portal: https://docs.rocketsoftware.com/
- Rocket documentation library: https://docs.rocketsoftware.com/bundle
- Rocket product catalog: https://www.rocketsoftware.com/en-us/products
- Rocket support portal: https://my.rocketsoftware.com/

The Rocket product catalog exposes Mainframe Operations navigation including C\Prof, TMON One, PRO/JCL, Enterprise Storage, and EVA, and product families such as Enterprise Suite, DataEdge, Mainstar, TMON One, ChangeMan, Legacy IDMS Utilities, Mainframe Application Testing and Debugging, and Open Source Solutions for z/OS.

The docs portal and support portal may require login or entitlement for product-specific documentation, downloads, patches, or support-only content. Treat Rocket account access as a blocking prerequisite.

## Access Required

Before useful capture can start, obtain:

- Rocket support portal login.
- Rocket documentation portal access.
- Licensed product inventory.
- Product download and patch access.
- Permission to export or internally snapshot Rocket documentation.
- Permission to download base media, service, HOLDDATA, generated JCL, workflows, USS payloads, Java payloads, and installation tooling.

## Target 1: TMON One and Mainframe Monitoring

### Current Official Index Signal

Rocket product catalog entries expose TMON One and related products including TMON for IMS, TMON for MQ, TMON for TCP/IP, TMON for z/OS, TMON Health AI, TMON PA, TMON Stream, and TMON for CICS.

### What To Capture

For each licensed product/version, capture:

- Product page URL.
- Documentation URL.
- Licensed product name, aliases, and local names.
- Installation, upgrade, customization, administration, integration, messages, and maintenance documentation.
- Monitoring architecture, agents, collectors, started tasks, history stores, and data streams.
- CICS, IMS, Db2, MQ, TCP/IP, USS, Java, RACF, scheduler, and automation hooks.
- APF, LINKLIST, LPA, PROCLIB, PARMLIB, and started-task requirements.
- USS, Java, API, AI, analytics, or streaming requirements.
- Verification commands, dashboards, and expected evidence.
- Backout and maintenance instructions.

### Initial Product Codes

Use these temporary product-definition codes until entitlement and media names are confirmed:

```text
rocket-tmon-one
rocket-tmon-zos
rocket-tmon-cics
rocket-tmon-ims
rocket-tmon-mq
rocket-tmon-tcpip
rocket-tmon-stream
rocket-tmon-health-ai
```

## Target 2: Mainstar and Enterprise Storage

### Current Official Index Signal

Rocket product catalog entries expose Mainstar and Enterprise Storage, including Mainstar Catalog RecoveryPlus, Mainstar FastAudit, and database backup/recovery products for IMS.

### What To Capture

For each licensed product/version, capture:

- Product page URL.
- Installation, configuration, administration, recovery, utility, messages, and maintenance documentation.
- Catalog, DFSMShsm, storage, backup, recovery, Db2, IMS, and z/OS optimization scope.
- Started tasks, batch jobs, APF libraries, utility libraries, and scheduler hooks.
- RACF and dataset authority requirements.
- z/OSMF workflow, SMP/E, generated JCL, USS, or vendor tooling availability.
- Verification commands and recoverability evidence.
- Backout and disaster-recovery posture.

### Initial Product Codes

Use these temporary product-definition codes until entitlement and media names are confirmed:

```text
rocket-mainstar-family
rocket-enterprise-storage
rocket-mainstar-catalog-recoveryplus
rocket-mainstar-fastaudit
rocket-mainstar-ims-backup-recovery
```

## Target 3: Enterprise Suite and Developer Tooling

### Current Official Index Signal

Rocket product catalog entries expose Enterprise Suite, Enterprise Analyzer, Enterprise Developer, Enterprise Developer for Z, Enterprise Server, Enterprise Server for .NET, and Enterprise Test Server.

### What To Capture

For each licensed product/version, capture:

- Product page URL.
- Installation, upgrade, customization, administration, IDE, compiler, debugger, application server, and test-server documentation.
- Host-side runtime components and distributed workstation/server components.
- USS, Java, Eclipse, VS Code, REST, certificate, and API requirements.
- CICS, IMS, Db2, batch, TSO, ISPF, RACF, and CI/CD hooks.
- Dataset, load library, source repository, compiler, test, and deployment integration.
- Verification commands and expected evidence.
- Backout and maintenance instructions.

### Initial Product Codes

Use these temporary product-definition codes until entitlement and media names are confirmed:

```text
rocket-enterprise-suite
rocket-enterprise-analyzer
rocket-enterprise-developer
rocket-enterprise-developer-z
rocket-enterprise-server
rocket-enterprise-test-server
```

## Target 4: Change, JCL, and Data Tools

### Current Official Index Signal

Rocket product catalog entries expose ChangeMan-related products such as Comparex and StarTool, JCL products such as JCLPREP and JOB/SCAN, and DataEdge products such as Data Intelligence, Data Replicate and Sync, and Data Virtualization.

### What To Capture

For each licensed product/version, capture:

- Product page URL.
- Installation, customization, administration, conversion, API, security, and maintenance documentation.
- Repository, compare, JCL validation, data access, replication, and virtualization architecture.
- Host-side components, distributed services, connectors, agents, and endpoints.
- Db2, IMS, VSAM, IDMS, CICS, batch, USS, Java, TCP/IP, RACF, and scheduler hooks.
- Data governance, credential, certificate, and audit requirements.
- Verification commands and expected evidence.
- Backout and maintenance instructions.

### Initial Product Codes

Use these temporary product-definition codes until entitlement and media names are confirmed:

```text
rocket-changeman-family
rocket-comparex
rocket-startool
rocket-jcl-tools-family
rocket-jclprep
rocket-jobscan
rocket-projcl
rocket-dataedge-family
rocket-data-virtualization
```

## Shared Rocket Capture Procedure

For each target product/version:

1. Open Rocket product catalog and confirm product family, product name, and public product page.
2. Open Rocket documentation portal or library and record canonical documentation URL.
3. Record whether support login or entitlement is required.
4. Export or internally snapshot installation, customization, upgrade, administration, security, messages, maintenance, and integration sections when permitted.
5. Open Rocket support/download channels and capture base media, service, HOLDDATA, installation tools, generated JCL, z/OSMF workflows, USS payloads, Java payloads, and checksums.
6. Cross-check media and docs against installed SMP/E CSI FMIDs, HLQs, USS paths, started tasks, APF/LINKLIST/LPA, PROCLIB/PARMLIB, and subsystem hooks.
7. Store metadata in the Rocket depot and product definition.

## Output Files To Create Next

After access is available, create or update:

```text
product_definitions/vendors/rocket/product-inventory.yml
product_definitions/vendors/rocket/rocket-tmon-one.yml
product_definitions/vendors/rocket/rocket-mainstar-family.yml
product_definitions/vendors/rocket/rocket-enterprise-suite.yml
product_definitions/vendors/rocket/rocket-change-jcl-data-tools.yml
```

## Blocking Questions

1. Which Rocket products are licensed in the support portal?
2. Which products are installed today, and where are their SMP/E CSIs?
3. Which products have USS, Java, REST, agent, or distributed-service surfaces?
4. Which acquired, rebranded, IBM-partnered, or local names are still used?
5. Who can access Rocket documentation, support, downloads, patches, and support-only content?
6. Which products have z/OSMF workflows or generated JCL?
7. Which products alter monitoring, storage, recovery, testing, JCL, data access, or security posture?
8. Which product should be the first Rocket implementation pilot?

## Rule

For Rocket first-wave targets, the first deliverable is a verified licensed-product and installed-state inventory with documentation URLs, media identifiers, MVS and USS surfaces, distributed dependencies, subsystem hooks, aliases, and enough detail to classify each product.
