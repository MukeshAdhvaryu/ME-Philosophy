<div style="display:inline-flex; align-items:center; gap:14px; border:1px solid rgba(164,175,186,0.35); border-radius:18px; padding:10px 16px; margin:0 0 12px 0; background:linear-gradient(135deg, rgba(255,255,255,0.06), rgba(255,255,255,0.02)); box-shadow: 0 8px 24px rgba(0,0,0,0.18);">
  <img src="..\..\..\MnMLogo.png" alt="MachineEnglish logo" width="48" />
  <span style="font-size: 2em; font-style: italic; font-weight: 700;">MachineEnglish(ME) - a language of intent</span>
</div>

> **Credit:** Sourced from [MukeshAdhvaryu/SDLC-Governance](https://github.com/MukeshAdhvaryu/SDLC-Governance) and adopted for this project.

# DIP - Document Index Page

Public-facing index for repository documents, user-visible source files, and public
entities. Internal implementation details live in `DIP_Internal.md`.

> This is a template DIP. It demonstrates the format and column structure.
> In your project, each entry should be a markdown link: `[filename.md](relative/path/to/file.md)`.

- Generated: YYYY-MM-DD
- Columns: `Name`, `Document Type`, `Folder`, `File Type`, `Description`.

---

## Table of Contents

- [Project Root](#project-root)
- [Architecture/ADRs](#architectureadrs)
- [Architecture/Major-Decisions](#architecturemajor-decisions)
- [Architecture/Policies](#architecturepolicies)
- [Architecture/Templates](#architecturetemplates)
- [Architecture/Drift-Reports](#architecturedrift-reports)
- [Architecture/Roadmap](#architectureroadmap)
- [Architecture/Specs](#architecturespecs)

---

## Project Root

| Name | Document Type | Folder | File Type | Description |
|------|---------------|--------|-----------|-------------|
| [README.md](README.md) | Project README | `/` | `.md` | Entry point: what the project is, current status, build instructions, and links to canonical documents. |
| [DIP.md](DIP.md) | Document Index | `/` | `.md` | Public-facing catalogue of all repository documents and source entities. |

---

## Architecture/ADRs

| Name | Document Type | Folder | File Type | Description |
|------|---------------|--------|-----------|-------------|
| ADR-001-Title.md | ADR | `Architecture/ADRs` | `.md` | One-sentence description of what this decision covers. |

---

## Architecture/Major-Decisions

| Name | Document Type | Folder | File Type | Description |
|------|---------------|--------|-----------|-------------|
| MD-001-Title.md | Major Decision | `Architecture/Major-Decisions` | `.md` | One-sentence description of the design decision. |

---

## Architecture/Policies

| Name | Document Type | Folder | File Type | Description |
|------|---------------|--------|-----------|-------------|
| [BEST-ENGINEERING-POLICY.md](Architecture/Policies/BEST-ENGINEERING-POLICY.md) | Policy | `Architecture/Policies` | `.md` | Engineering standards: constants, nesting depth, SOLID, MVP-to-production naming. |
| [DOCUMENTATION-POLICY.md](Architecture/Policies/DOCUMENTATION-POLICY.md) | Policy | `Architecture/Policies` | `.md` | Five-tier documentation strategy, README standards, DIP format, and cross-reference rules. |
| [DRIFT-RESOLUTION-POLICY.md](Architecture/Policies/DRIFT-RESOLUTION-POLICY.md) | Policy | `Architecture/Policies` | `.md` | Format and closure criteria for Drift Fix Reports. |
| [NAMING-CONVENTION-POLICY.md](Architecture/Policies/NAMING-CONVENTION-POLICY.md) | Policy | `Architecture/Policies` | `.md` | Positive capability naming, interface prefix conventions, and namespace/folder alignment rules. |
| [SOLIDE-DESIGN-POLICY.md](Architecture/Policies/SOLIDE-DESIGN-POLICY.md) | Policy | `Architecture/Policies` | `.md` | SOLIDE design philosophy: an honest successor to SOLID with Explicit Extension as the sixth principle. |
| [TESTING-STANDARDS-POLICY.md](Architecture/Policies/TESTING-STANDARDS-POLICY.md) | Policy | `Architecture/Policies` | `.md` | Contract-first testing standard, test categories, anti-patterns, and review criteria. |
| [WORK-DONE-POLICY.md](Architecture/Policies/WORK-DONE-POLICY.md) | Policy | `Architecture/Policies` | `.md` | Decision flow and mandatory status table format for ADRs, MDs, DFRs, and Roadmaps. |

---

## Architecture/Templates

| Name | Document Type | Folder | File Type | Description |
|------|---------------|--------|-----------|-------------|
| [ADR-Template.md](Architecture/Templates/ADR-Template.md) | Template | `Architecture/Templates` | `.md` | Starter template for Architecture Decision Records. |
| [DFR-Template.md](Architecture/Templates/DFR-Template.md) | Template | `Architecture/Templates` | `.md` | Starter template for Drift Fix Reports. |
| [DIP-Template.md](Architecture/Templates/DIP-Template.md) | Template | `Architecture/Templates` | `.md` | Starter template for Document Index Pages. |
| [MD-Template.md](Architecture/Templates/MD-Template.md) | Template | `Architecture/Templates` | `.md` | Starter template for Major Decisions. |
| [RDMP-Template.md](Architecture/Templates/RDMP-Template.md) | Template | `Architecture/Templates` | `.md` | Starter template for Roadmaps. |
| [Semantic-Rule-Template.md](Architecture/Templates/Semantic-Rule-Template.md) | Template | `Architecture/Templates` | `.md` | Guide for writing machine-checkable semantic rules. |
| [Specification-Template.md](Architecture/Templates/Specification-Template.md) | Template | `Architecture/Templates` | `.md` | Starter template for feature specifications. |

---

## Architecture/Drift-Reports

| Name | Document Type | Folder | File Type | Description |
|------|---------------|--------|-----------|-------------|
| DFR-NNN-Title.md | Drift Report | `Architecture/Drift-Reports` | `.md` | One-sentence description of the drift and its resolution state. |

---

## Architecture/Roadmap

| Name | Document Type | Folder | File Type | Description |
|------|---------------|--------|-----------|-------------|
| RDMP-AreaName.md | Roadmap | `Architecture/Roadmap` | `.md` | Delivery tracker for this area. |

---

## Architecture/Specs

| Name | Document Type | Folder | File Type | Description |
|------|---------------|--------|-----------|-------------|
| Spc-AreaName.md | Spec | `Architecture/Specs` | `.md` | Specification for domain rules and contracts in this area. |
