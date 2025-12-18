# Damage Formula – Mathematical Model

This document presents the **mathematical representation** of the currently known damage calculation model in *Where Winds Meet*.

It is intended for readers who prefer to reason directly with formulas and numbers, or who want to validate the spreadsheet logic independently.

⚠️ **Important**  
All formulas here are based on community testing and CN sources.  
They are **not officially confirmed**, but match observed in-game behavior closely.

---

## 1. Conceptual Structure

At a high level, damage is computed in **three stages**:

1. **Base Damage Construction**  
   Physical and attribute damage are constructed separately.

2. **Hit Type Resolution**  
   One hit type (Normal / Crit / Affinity / Precision / Abrasion) is selected.

3. **Final Modifiers & Mitigation**  
   Penetration, bonuses, and enemy mitigation are applied.

Each stage is described mathematically below.

---

## 2. Base Damage Construction

### 2.1 Physical Damage Component

Let:

- `Pmin`, `Pmax` = minimum and maximum physical attack  
- `PhysPen` = physical penetration  
- `PhysBonus` = physical damage bonus  
- `PhysMult` = skill-specific physical multiplier  

The **expected physical base damage** is:

`PhysBase` =
((`Pmin` + `Pmax`) / 2)
× (1 + `PhysPen` / 200)
× (1 + `PhysBonus`)
× `PhysMult`

Why this matters: Physical attack values appear early in the formula and are then amplified by multiple multipliers.

---

### 2.2 Attribute Damage Component (Main School)

Let:

- `Amin`, `Amax` = min / max attribute attack of the **main attribute type** (your school)  
- `AttrPen` = attribute penetration (your type)  
- `AttrBonus` = attribute damage bonus (your type, as a decimal)  
- `ElemMult` = main element multiplier (often modeled as 1.0 unless a skill specifies otherwise)

A simplified attribute term is:

`AttrBase` =
((`Amin` + `Amax`) / 2)
× `ElemMult`
× (1 + `AttrPen` / 200 + `AttrBonus`)

Key observation: Attribute attack is typically added as its own term and is affected by fewer global multipliers than physical.

---

### 2.3 Combined Base Damage

Before hit type resolution:

`BaseDamage` =
`PhysBase` + `AttrBase` + `FlatDamage`

---

## 3. Hit Type Resolution

Exactly **one hit type** is applied per hit.

Hit types are **mutually exclusive**.

### 3.1 Normal Hit

`Damage` = `BaseDamage`

---

### 3.2 Critical Hit

- Uses **random value between Pmin and Pmax**
- Applies critical multiplier

`CritDamage` =
`BaseDamage` × `CritMultiplier`

(Default CritMultiplier ≈ 1.5)

---

### 3.3 Affinity Hit

- Uses **maximum physical attack**
- Applies affinity multiplier

`AffinityDamage` =
(`Pmax` × `PhysMult` + `AttrBase` + `FlatDamage`)
× `AffinityMultiplier`

(Default `AffinityMultiplier` ≈ 1.35)

---

### 3.4 Precision Hit

- **Forces maximum damage roll**
- Prevents Critical and Affinity hits

`PrecisionDamage` =
`MaxPossibleBaseDamage`

Precision increases **consistency**, not peak damage.

---

### 3.5 Abrasion Hit

- Forces **minimum damage roll**
- Applies small fixed modifier (≈ 1.02)

`AbrasionDamage` =
`MinPossibleBaseDamage` × 1.02

---

## 4. Penetration & External Defense

Enemy mitigation is modeled as **external defense**, distinct from visible armor.

Let:

- `Pen` = penetration
- `Def` = external defense

### 4.1 Effective Damage Multiplier

If penetration does **not exceed defense**:

`DamageMultiplier` = 1 − (`Def` − `Pen`)

If penetration **exceeds defense**, overflow applies at **50% efficiency**:

`Overflow` = `Pen` − `Def`
`DamageMultiplier` = 1 + (`Overflow` / 2)

Example:

- 30 penetration vs 0 defense → +15% damage  
- 30 penetration vs 10 defense → +10% + (20 / 2) = +20%

This explains why:
- Penetration is always valuable against bosses
- Returns diminish after exceeding defense

---

## 5. Final Damage

Putting everything together:

`FinalDamage` =
`HitTypeDamage`
× `DamageMultiplier`

Rounded according to internal game rules.

---

## 6. Full Damage Formula (Consolidated View)

Putting all components together, the expected damage of a single hit can be represented as:

`FinalDamage` =
(`PhysBase` + `AttrBase` + `FlatDamage`)
× `HitTypeMultiplier`
× `FinalDamageModifiers`

Where:

`PhysBase` =
((`Pmin` + `Pmax`) / 2)
× (1 + `PhysPen` / 200)
× (1 + `PhysBonus`)
× `PhysMult`

`AttrBase` =
((`Amin` + `Amax`) / 2)
× `MainElementMult`
× (1 + `AttrPen` / 200 + `AttrBonus`)
× `SchoolBonus`

`HitTypeMultiplier` ∈ {  
`Normal` = 1.0,  
`Critical` = 1.5,  
`Affinity` = 1.35,  
`Precision` = `MaxRoll` / `AvgRoll`,  
`Abrasion` = `MinRoll` / `AvgRoll`  
}

`FinalDamageModifiers` include any remaining global effects applied after hit resolution (e.g. Affinity Damage Bonus, control-condition bonuses, exhaustion bonuses, etc.).

This representation shows the full picture: **physical damage and attribute damage are calculated independently, added together with flat damage, then modified once by hit type and final multipliers**.

---

## 7. Practical Implications

From the math, we can conclude:

- Physical Attack affects **more multipliers** than any other stat
- Penetration scales against **hidden mitigation**
- Attribute Attack contributes, but in a **narrower scope**
- Precision trades peak damage for reliability
- Affinity favors burst but depends on caps and uptime

These conclusions are reflected directly in the spreadsheet results.

---

## 8. Relation to the Spreadsheet

The calculator spreadsheet implements this model by:

- Separating physical and attribute components
- Averaging min/max values where appropriate
- Applying hit-type-specific branches
- Calculating expected damage over probability distributions

This document exists so the spreadsheet is **auditable**, not opaque.

---

## 9. Limitations

- Enemy external defense values are unknown
- Bosses may vary internally
- Some skill multipliers are inferred
- PvP scaling may differ

Future updates may refine coefficients or structure.
