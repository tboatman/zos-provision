# zos-provision

Planning and architecture documentation for provisioning and maintaining a z/OS platform through Ansible: CICS/IMS/Db2 regions, platform maintenance and disconnected VM clones, and third-party vendor software (BMC, Broadcom CA, Rocket Software) lifecycle management.

This repository is currently **documentation only**. Per [zos-cics-ims-db2-vendor-ansible-architecture.md](docs/zos-cics-ims-db2-vendor-ansible-architecture.md)'s own implementation readiness checklist, code should not begin until the product inventory, environment model, and approval gates described in these docs are reviewed and approved.

## Where to start

- [z/OS CICS, IMS, Db2, and Vendor Product Deployment Through Ansible](docs/zos-cics-ims-db2-vendor-ansible-architecture.md) — the overall system architecture: target architecture, repository layout, data model, role boundaries, execution flows, and delivery phases. Start here.
- [Third-Party Vendor Software Lifecycle Strategy](docs/third-party-software-lifecycle-strategy.md) — the vendor-product track (BMC, Broadcom CA, Rocket) that plugs into the architecture above.
- [Platform Maintenance and Disconnected Clone Strategy](docs/platform-maintenance-and-disconnected-clones.md) — the substrate layer (z/OS maintenance, internal SMP/E repository, RACF, disconnected VM clones) that the other two depend on.
- [IBM Ansible Documentation Gathering](docs/ibm-ansible-documentation-gathering.md) — the Ansible and IBM-platform source map for z/OS, z/OSMF, CICS, IMS, Db2, MQ, System Automation, Z HMC, and z/VM.
- [z/VM Ansible Documentation Sources](docs/zvm-ansible-documentation-sources.md) — focused source ledger for z/VM Ansible, SMAPI, Feilong/zthin, and disconnected VM guest clone automation.
- [OSS Tooling Discovery](docs/oss-tooling-discovery.md) — open source tooling candidates for control-node helpers, z/OS USS tooling, z/VM clone-factory support, validation harnesses, and quarantine-only legacy/lab paths.
- [Top-to-Bottom Gap Assessment and Remediation Plan](docs/top-to-bottom-gap-assessment.md) — the cross-document gap register and sequenced plan for closing design, evidence, schema, product, z/VM, and governance gaps before implementation.

## Vendor documentation intake

Each vendor family (BMC, Broadcom CA, Rocket) has a parallel set of intake documents under `docs/`, following the same naming pattern:

- `<vendor>-documentation-acquisition-plan.md` — the intake process for that vendor.
- `<vendor>-first-wave-documentation-intake.md` — the first set of product families targeted for intake.
- `<vendor>-public-documentation-collection.md` — results of collecting publicly reachable documentation.
- `<vendor>-noncanonical-install-documentation-search.md` — results of searching non-canonical (unofficial) sources as breadcrumbs only.
- `<vendor>-alternative-documentation-sources.md` — a map of official-but-non-obvious source classes (support portals, static doc trees, parameter reference databases, etc.) beyond the primary product catalog.

See [Vendor Adapter Skeletons](docs/vendor-adapter-skeletons.md) for how vendor-specific findings turn into shared Ansible adapter overlays.

## IBM and Ansible documentation intake

The IBM-platform automation substrate is tracked separately from third-party vendor intake:

- [IBM Ansible Documentation Gathering](docs/ibm-ansible-documentation-gathering.md) — collection and product-documentation intake for Ansible for IBM Z, z/OS, IBM subsystems, and z/VM.
- [z/VM Ansible Documentation Sources](docs/zvm-ansible-documentation-sources.md) — canonical and non-canonical source discovery for z/VM automation where Galaxy-published coverage is not enough.
- [OSS Tooling Discovery](docs/oss-tooling-discovery.md) — governed evaluation of relevant open source tools such as Feilong, Zowe, zopen community packages, Galasa, and last-resort legacy tooling.

## Sidequests

Products that are not part of the first vendor-family track (e.g. [IOF Sidequest](docs/iof-sidequest.md)) are tracked as standalone custody and installed-state investigations before being promoted into the normal product-definition lifecycle.

## Diagrams

All flowcharts are maintained as standalone Mermaid `.mmd` source files under [docs/diagrams/](docs/diagrams/) and linked from the Markdown documents that use them, rather than embedded inline.
