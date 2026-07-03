# Rocket Public Documentation Collection

## Purpose

Record Rocket Software documentation and product sources that are publicly reachable now for the first Rocket scope. This is a collection ledger, not a mirror of vendor manuals.

Do not commit copied Rocket manuals, exported HTML, or PDFs into this repository until the project has an explicit entitlement, retention, and redistribution decision. Store source URLs, product evidence, access status, and retrieval notes here first.

Retrieval pass: 2026-07-03.

## Collection Flow

Diagram source: [docs/diagrams/rocket-public-documentation-collection-flow.mmd](diagrams/rocket-public-documentation-collection-flow.mmd)

## Collection Rules

- Prefer official Rocket URLs over third-party mirrors.
- Capture public product pages, visible resource links, support-login links, documentation-portal links, and product-family relationships.
- Mark product technical details as gated when the product page points to Rocket documentation or support login but the content is not visible publicly.
- Do not commit Rocket brochures, datasheets, manuals, or exported files into git until the retention and redistribution decision is made.
- Re-check every URL before implementation, because Rocket has recently absorbed and rebranded product families.

## Public Source Inventory

| Source | URL | Current collection status | Use |
| --- | --- | --- | --- |
| Rocket documentation portal | https://docs.rocketsoftware.com/ | Public shell; login/search context likely required for useful product manuals | Canonical documentation lookup after product/version is known |
| Rocket documentation library | https://docs.rocketsoftware.com/bundle | Public shell; no useful product list returned in this pass | Documentation search and filtering after login |
| Rocket Products A-Z | https://www.rocketsoftware.com/en-us/products | Public product catalog | Product-family discovery and public product pages |
| Rocket support portal | https://my.rocketsoftware.com/ | Public login entry; unauthenticated page not useful | Downloads, patches, cases, support-only docs |
| Rocket product documentation link | https://docs.rocketsoftware.com/ | Public navigation target from product pages | Product-specific technical documentation after login/search |

## Publicly Reachable Rocket Product Pages

| Product area | Public URL | Publicly visible evidence | Access status |
| --- | --- | --- | --- |
| TMON One | https://www.rocketsoftware.com/en-us/products/tmon-one | Mainframe capacity planning and management; products listed include TMON for z/OS, Db2, CICS, IMS, MQ, TCP/IP, and PA; related observability pieces include TMON Health AI, TMON Stream, Rocket Data Gateway, NaviPlex, and NaviGraph | Product page public; install/configuration docs require docs portal or support access |
| PRO/JCL | https://www.rocketsoftware.com/en-us/products/projcl | JCL management, REXX APIs, ISPF/Eclipse/BMC AMI DevX Workbench/IDz/VS Code surfaces, Zowe-conformant RESTful web services, scheduler compatibility including Zeke, ESP, and Control-M | Product page public; technical docs require docs portal or support access |
| Enterprise Storage | https://www.rocketsoftware.com/en-us/products/enterprise-storage | Mainframe enterprise storage family; resilience, backup, recovery, compliance, cost reduction; page references Mainstar Catalog RecoveryPlus through customer evidence | Product page public; product-specific storage docs require docs portal or support access |
| ChangeMan ZMF | https://www.rocketsoftware.com/en-us/products/changeman/changeman-zmf | Mainframe change management for z/OS; formerly Micro Focus; supports IDE, DevOps tools, TSO/ISPF, packages, native Git integration, Zowe CLI plugin, VS Code ZMF Explorer, Advanced Authentication Connector for IBM z/OS | Product page public; documentation link points to Rocket docs portal |
| Enterprise Developer for Z | https://www.rocketsoftware.com/en-us/products/enterprise-suite/enterprise-developer-for-z | Graphical IDE for IBM zSystems; COBOL and PL/I support; CICS, IMS, JES2 compatibility; remote z/OS development from Visual Studio or Eclipse; debugging, unit test, code coverage, source-control integration | Product page public; documentation link points to Rocket docs portal |
| Mainstar Database Backup and Recovery for IMS | https://www.rocketsoftware.com/en-us/products/mainstar/database-backup-and-recovery-ims | IMS backup, recovery, disaster recovery; integrates storage processor fast replication with IMS backup/recovery; system-level backup profiles and retention settings | Product page public; detailed installation and operations docs require docs portal or support access |

## Public Product Catalog Evidence

The Rocket Products A-Z page exposes useful first-wave candidates:

- Mainframe Operations: C\Prof, TMON One, PRO/JCL, Enterprise Storage, EVA.
- ChangeMan: ChangeMan, ChangeMan SSM, ChangeMan ZMF, ChangeMan ZMF Client Pack, Comparex.
- Mainstar: Clone and Rename for IMS, Database Backup and Recovery for IMS.
- Enterprise Suite: Enterprise Analyzer, Enterprise Developer, Enterprise Developer for Z, Enterprise Server, Enterprise Server for .NET, Enterprise Test Server.
- DataEdge: Data Intelligence, Data Replicate and Sync, Data Virtualization.
- OpenTech and storage-related products such as CopyExport Manager and DASD Backup Supervisor.

## Technical Signals To Preserve

Rocket first-wave products have several surfaces that matter for Ansible and z/OSMF design:

- TMON products touch z/OS, CICS, Db2, IMS, MQ, TCP/IP, observability streams, and analytics surfaces.
- PRO/JCL has ISPF, IDE, REST, Zowe, scheduler, REXX, and CI/CD integration points.
- ChangeMan ZMF has package, repository, Git, Zowe CLI, VS Code, TSO/ISPF, authentication, and promotion-governance surfaces.
- Enterprise Developer for Z has workstation IDE, remote z/OS, compiler/debugger/test, CICS, IMS, JES2, and source-control surfaces.
- Mainstar and Enterprise Storage products can affect recovery, storage processors, backup profiles, disaster-recovery posture, and scheduler hooks.

## Immediate Gaps

- Rocket documentation portal did not expose useful product-specific manuals in the unauthenticated browser pass.
- Rocket support portal access is required for downloads, patches, media, maintenance, and support-only docs.
- Product versions, FMIDs, install methods, z/OSMF workflow availability, SMP/E packaging, and maintenance paths are not public from the product pages.
- Rebranded OpenText/Micro Focus and acquired product lineage must be normalized against installed state before implementation.

## Next Collection Pass

1. Log in to Rocket documentation and support portals.
2. Search by exact product names from the public catalog and local licensed inventory.
3. Capture canonical product/version documentation URLs.
4. Capture install, customization, upgrade, security, messages, maintenance, and integration docs where export is permitted.
5. Capture media package names, checksums, z/OSMF workflows, generated JCL, SMP/E FMIDs, HOLDDATA, and patch details from support/download channels.
6. Convert each collected product into a Rocket product-definition source map.
