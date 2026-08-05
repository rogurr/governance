# RFC: ODP Repository Naming Policy

This RFC defines the naming policy for repositories in the Open Device Partnership (ODP) GitHub organization. The
policy communicates each repository's scope and purpose. For repositories temporarily hosted by ODP, it also
communicates whether the repository is a fork or is intended for handoff.

## Change Log

This text can be modified over time. Add a change log entry for every change made to the RFC.

- 2026-08-05: Initial RFC created.

## Motivation

ODP repositories have been created at different times by different projects without a shared naming convention.
Current names use inconsistent prefixes, separators, abbreviations, and ordering. Some identify an ODP project, some
identify a technology, target, or vendor, and others do not communicate how the repository relates to ODP or an
upstream project.

This inconsistency makes repository names difficult to interpret without opening each repository. It also makes it
harder for contributors to browse or search the organization by project, distinguish project repositories from
organization-wide repositories, and identify temporary forks or work intended for transfer to an external owner.

A shared convention gives each label in a repository name one meaning and a predictable position. It allows every name
to answer the following questions:

1. Which ODP project or organization-wide grouping is this repository associated with?
2. What distinguishes this repository within that scope?
3. Is this repository a temporary fork or handoff pending removal from ODP?

## Technology Background

ODP hosts repositories for individual projects, organization-wide functions, platform demonstrations, and temporary
collaboration with external projects or vendors. GitHub presents these repositories together at the organization
level, where the repository name is the primary information available before a contributor opens it. A predictable
name therefore helps contributors identify relevant repositories while browsing, searching, or following links from
outside ODP.

Git repository names do not define code architecture, package names, or ownership. An ODP repository may contain one
or more modules, tools, applications, or build targets without those choices changing the naming convention.

An external repository referenced as a Git submodule is not an ODP repository. Its canonical name and the path
recorded in `.gitmodules` remain under the control of the consuming repository and are outside this convention. This
distinguishes unchanged external dependencies from external code temporarily hosted in an ODP repository.

This RFC uses the following terms for naming cases:

- **Ancillary repository** - A repository whose exact name is established by GitHub or an organization-wide
  convention, such as `OpenDevicePartnership.github.io`.
- **Temporary fork** - An ODP-hosted repository whose canonical source originated outside ODP and which is retained
  only while ODP changes are being upstreamed to that source.
- **Handoff repository** - An ODP-authored repository hosted temporarily by ODP and intended to be transferred to an
  identified external owner.

## Goals

1. Make ODP repository names consistent and interpretable across the organization.
2. Identify the ODP project or organization-wide grouping associated with each repository.
3. Make a repository's distinguishing purpose discoverable from its name when the scope alone is insufficient.
4. Identify temporary forks and handoff repositories so contributors do not mistake them for repositories ODP intends
   to maintain.
5. Support contributors searching by project, capability, upstream repository, hardware target, or vendor and item
   name.

## Requirements

The naming policy has the following requirements:

- Repository names use the pattern `<scope>[-<purpose>[-<temporary_status>]]`.
- Labels use lowercase ASCII letters and digits, with underscores between words.
- Hyphens separate labels and do not appear within a label.
- `<scope>` is required and indicates the ODP project or reserved organization-wide grouping associated with the
  repository.
  - Approved project scope labels are derived from the ODP project names maintained by the
    [governing board](https://opendevicepartnership.org/projects) and normalized according to this policy. For example,
    `Patina` and `Embedded Controller` become `patina` and `embedded_controller`.
  - Reserved organization-wide scope labels are defined by this RFC and must not be introduced by individual
    repositories.
    - `common` is used for repositories associated with multiple projects or not clearly associated with a single
      project.
    - `governance` is used for the primary ODP governance repository and repositories that support organization-wide
      governance through discussion or documentation.
    - `platform` is used for repositories that integrate a bootable demonstration for a hardware or emulated target.
  - The repository that represents the primary project may use the scope label by itself.
  - Additional organization-wide scope labels require an update to this RFC.
  - The scope label identifies the ODP project or organization-wide grouping associated with a repository and does
    not assign repository ownership.
- `<purpose>` is the tool, module, platform target, or other purpose that distinguishes the repository within its
  scope. This label is required unless the repository represents the primary project or is an ancillary repository.
  - The `<purpose>` label is omitted for the repository that represents the primary project.
  - All other non-ancillary repositories include a `<purpose>` label.
  - The label describes why the repository exists or what it is for, not its implementation language, packaging format,
    or repository type.
  - When a vendor name is needed, include it in the same `<purpose>` label as the item it identifies, separated with an
    underscore; for example, `bq40z50_ti`.
  - Project maintainers approve purpose labels within their scope.
- `<temporary_status>` follows `<purpose>`, and indicates why a repository is hosted temporarily by ODP.
  - The label uses the following pre-defined names:
    - `fork` represents a fork of an external repository that contains ODP changes, is classified as temporary, and
      must be removed from ODP as soon as practical after its changes or contents have been upstreamed. A fork will
      also require the `<purpose>` label to the upstream repository's name.
    - `handoff` represents a reposory created by ODP, but is intended to be moved to an external organization's
      repository as soon as practical and removed from ODP.
  - If ODP intends to maintain a repository, the repository must not include a `<temporary_status>` label.
  - An external vendor name used within the `<purpose>` label does not imply a temporary status.
- New ODP repositories must use this policy when they are created; existing repositories need a plan for adoption.
- Ancillary repositories retain the names required by GitHub or an applicable organization-wide convention.

## Unresolved Questions

No unresolved questions are currently known for the naming grammar itself. Review of this RFC may identify additional
reserved organization-wide scopes or naming cases that the proposed labels do not represent. The following related
decisions are outside the scope of this RFC:

- The final list of repositories to rename and their approved new names.
- Whether existing repositories should be combined, divided, transferred, archived, or removed.
- The schedule, tooling, and ownership for repository renames.
- Enforcement mechanisms and deadlines for adopting the policy.
- Repository contents, layout, documentation, build, CI, release, and ownership policy.

## Prior Art

Existing ODP repository names demonstrate several naming approaches. Some use an `odp-` prefix, such as
`odp-platform-qemu-arm-virt`; some use a project prefix, such as `patina-apps`; and others use an unscoped capability,
device, or upstream project name, such as `cortex-m-stack`, `bq40z50`, or `edk2`. These names are the direct prior art
for this proposal, but no single existing approach consistently identifies scope, purpose, and temporary status.

The proposed convention retains useful project and purpose terms from existing names while assigning them predictable
positions. The examples in the Guide-Level Explanation section show how representative existing names map to the
proposed convention.

## Alternatives

### Keep Existing Names

ODP could leave repository naming to individual projects. This avoids migration work but preserves the current
inconsistency and requires contributors to open repositories to determine their relationship to ODP.

### Use Only Scope and Purpose

The convention could omit `<temporary_status>` and use only `<scope>-<purpose>`. This is simpler, but temporary forks
and handoff repositories would be indistinguishable from repositories ODP intends to maintain.

### Use Vendor Names as Scope

Repositories associated with a hardware vendor could use that vendor as `<scope>`. This would make vendor searches
direct, but it would obscure the ODP project responsible for the work and could incorrectly imply vendor ownership.
Vendor and item names therefore belong in `<purpose>` when they help distinguish a repository.

### Encode the Source or Owner in Temporary Status

The temporary label could include the upstream source or intended owner, such as `edk2_fork` or `ti_handoff`. This
duplicates information already carried by `<purpose>` and makes the third label describe both identity and status.
Using the literal flags `fork` and `handoff` gives `<temporary_status>` one meaning.

### Use Hyphens for All Word Boundaries

Repository names could use hyphens both between labels and between words within labels. This is familiar, but it makes
the label boundaries ambiguous. Using hyphens between labels and underscores within labels keeps the name parseable.

## Rust Code Design

Not applicable.

## Guide-Level Explanation

Choose labels from left to right:

1. Select the ODP project most directly associated with the repository as `<scope>` or a reserved organization-wide
   label when applicable.
2. Add `<purpose>` unless the repository represents the primary project. Choose terms that a contributor is likely to
   search for, such as a capability, upstream repository, hardware target, or vendor and item name.
3. Add `<temporary_status>` only when the repository is a temporary fork or handoff. Omit it for repositories that ODP
   intends to maintain.

The following examples illustrate how selected current repository names could follow this convention. They do not
mandate the names to use going forward.

| Current Name | New Name | Notes |
| --- | --- | --- |
| OpenDevicePartnership.github.io | OpenDevicePartnership.github.io | Unchanged ancillary repository |
| governance | governance | Primary repository for the reserved `governance` scope |
| documentation | governance-documentation | Organization-wide documentation supporting ODP governance |
| cortex-m-stack | common-cortex_m_stack | ODP-owned Cortex-M support code not specific to one project |
| odp-utilities | common-utilities | ODP-owned tools used by multiple projects |
| pico-de-gallo | common-pico_de_gallo | Standalone ODP tool not associated with one project |
| odp-platform-qemu-arm-virt | platform-qemu_arm_virt | Arm-specific emulation platform that boots to a Windows desktop |
| odp-platform-radxa-orion-o6 | platform-radxa_orion_o6 | Platform demonstration that boots a Radxa Orion O6 to a Windows desktop |
| edk2 | platform-edk2-fork | Temporary EDK II fork for platform builds; changes are intended for upstreaming |
| odp-embedded-controller | embedded_controller | Primary Embedded Controller project repository |
| is31fl3743b | embedded_controller-is31fl3743b_lumissil-handoff | ODP-authored driver intended for handoff to Lumissil |
| bq40z50 | embedded_controller-bq40z50_ti-handoff | ODP-authored driver intended for handoff to TI |
| zephyr | embedded_controller-zephyr-fork | Temporary Zephyr fork for EC repositories; changes are intended for upstreaming |
| patina-apps | patina-apps | Applications specific to the Patina project |
| patina-qemu | patina-qemu | QEMU emulation repository specific to the Patina project |
