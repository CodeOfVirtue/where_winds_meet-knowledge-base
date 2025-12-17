# Damage Formula – Overview

This section provides a **high-level explanation** of how damage is calculated in *Where Winds Meet*, based on community research, translated CN sources, and spreadsheet modeling.

The goal here is **conceptual understanding**, not exact simulation.
For practical usage, see the spreadsheet guide.
For deeper mechanics (penetration, external defense), see Section 02.

---

## Scope of the Model

The damage formula modeled in this repository answers one primary question:

> *How does changing stats or gear affect expected average damage per hit?*

It does **not** attempt to model:
- Skill rotations or uptime
- Endurance management
- Player execution or positioning
- Boss-specific mechanics or immunity phases
- PvP-specific rule variations

As such, results should be interpreted as **comparative**, not absolute.

---

## High-Level Structure of Damage Calculation

Damage can be understood as occurring in three layers:

1. **Base Damage Construction**
2. **Hit Type Resolution**
3. **Final Modifiers & Mitigation**

Each layer contributes independently to the final outcome.

---

## Base Damage Construction (Conceptual)

At a high level:

- **Physical Attack** contributes broadly across the damage formula:
  - Min / Max Physical Attack
  - Physical Attack Multiplier
  - Flat Damage
  - Physical Damage Bonuses
  - Physical Penetration

- **Attribute Attack** (Bellstrike, Stonesplit, etc.) contributes more narrowly:
  - Applied primarily through the Main Element Multiplier
  - Affected by Attribute Penetration and Attribute Damage Bonus
  - Scales strongly in isolation, but impacts fewer parts of the total formula

This difference in **breadth of application** is a key reason why Physical Attack tends to scale better as total stats increase.

---

## Hit Type Resolution (Summary)

Every attack resolves into **exactly one hit type**:
- Normal
- Critical
- Affinity
- Precision
- Abrasion

These hit types are **mutually exclusive**.
For a detailed explanation, see:
- [`hit_types_explained.md`](./hit_types_explained.md)

---

## Penetration & Mitigation (Summary)

Penetration interacts with a **hidden mitigation layer** often referred to as *external defense*.

- This is **not** the visible Physical Defense stat
- Bosses appear to have this mitigation even when no defense value is shown
- Excess penetration still increases damage, but at reduced efficiency

This topic is complex and discussed in detail in:
- [`../../02_PENETRATION_EXTERNAL_DEFENSE/README.md`](../../02_PENETRATION_EXTERNAL_DEFENSE/README.md)

---

## Why This Matters

Understanding this structure explains why:
- Some stats look strong locally but underperform globally
- Physical Attack remains highly valuable even at high gear levels
- Set bonuses and stat caps change relative value over time

This overview serves as the foundation for later sections on stat priority and gear sets.
