<p align="left">
  <img src="..\..\..\MnMLogo.png" alt="MachineEnglish logo" width="48" />&nbsp;&nbsp;&nbsp;<strong>MachineEnglish(ME) - a language of intent</strong>
</p>
> **Credit:** Sourced from [MukeshAdhvaryu/SDLC-Governance](https://github.com/MukeshAdhvaryu/SDLC-Governance) and adopted for this project.

> **Path Authority:** Repository file/folder paths are maintained only in [DIP.md](../../../DIP.md) and [DIP_Internal.md](../../../DIP_Internal.md). Do not rely on path literals in this document.

---

# DFR Finished Record

Consolidated register of resolved drift fix reports. Each entry is a closed drift -
the gap was found, a root cause was identified, and a fix was applied and verified.

Active (open/in-progress) drifts live as individual files in `ME/Architecture/Drift-Reports/`
using `DFR-Template.md`. When a drift is resolved, its summary is appended here and the
individual file is retired (kept for reference but no longer the working document).

---

## Inventory

| ID | Title | Affected area | Date fixed |
|----|-------|--------------|------------|
| [DFR-NNN](#dfr-nnn) | One-sentence title | Spc-AreaName.md / component | YYYY-MM-DD |

---

## Entries

Entries are appended in resolution order. Do not edit past entries.

---

### DFR-NNN

**Date fixed:** YYYY-MM-DD
**Affected area:** Spc-AreaName.md (section N.N) / Component name
**Backing decision:** ADR-NNN / MD-NNN / Sem rule AREA-NNN

#### Gap found

State the observed divergence between the intended design and what was present.
Which component, which behaviour, which document or decision was violated?
Be precise - a vague gap description is not useful as a historical record.

#### Root cause

Why did this drift occur? Common causes:
- Decision was made after the code or doc was written and was never backported.
- A refactoring changed behaviour without checking the design contract.
- The design was ambiguous and two contributors interpreted it differently.
- Placeholder content was created before the backing decision was accepted.

#### Fix applied

What was changed to resolve the drift?

- Which files were edited.
- What was added, removed, or corrected.
- Cross-reference to commit SHA or PR if available.

---

## Template usage notes

> **Remove this section from the live document.**

### When to add an entry

Add an entry when a DFR individual file transitions to `Resolved`. Copy the Drift,
Root Cause, and Decision content from the individual file into this compact format.
The individual file can then be archived or deleted - this register is the permanent record.

### Entry format rules

- One `###` heading per DFR, using the DFR ID as the anchor.
- Three sub-sections only: **Gap found**, **Root cause**, **Fix applied**.
- Do not duplicate the full prose from the individual DFR file - summarise to the
  essential facts a future reader needs to understand what changed and why.
- Keep each sub-section to a paragraph or a short bullet list. Long narrative belongs
  in the individual DFR file (archived), not here.

### Inventory table

The inventory table at the top is the quick-reference index. Every entry in the
`## Entries` section must have a matching row in `## Inventory`. Keep both in sync.

### Numbering

DFR IDs are assigned sequentially across the full register (both active and finished).
Do not reuse an ID. Do not renumber resolved entries.
