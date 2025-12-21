# Damage Formula – Mathematical Model (Revised Draft Excerpt)

This document presents a mathematical representation of the currently known **direct-hit** damage model in *Where Winds Meet*, based on community testing and CN sources.

It is intended for readers who prefer formulas and want to audit the calculator logic independently.

> ⚠️ Important: This model is community-derived. It is not officially confirmed, but it matches CN documentation and observed behavior closely.

---

## 1. Conceptual Structure

At a high level, **direct-hit damage** is computed in these stages:

1) **Base Damage Construction**  
   Damage is constructed as a sum of multiple components (external/physical + attribute terms + fixed damage).

2) **Hit Type Resolution**  
   A hit type is resolved (e.g., Normal / Crit / Affinity / Precision / Abrasion).  
   This affects:
   - which attack roll is used (min / max / random),
   - which multiplier is applied (Crit/Affinity multiplier vs `1.0`).

3) **Final Damage Modifiers**  
   General “damage increases” are applied (mostly additive with each other), plus special multipliers.

This document focuses on **Stage 1 + Stage 3**, and clarifies how **Stage 2** plugs into the formula.

---

## 2. Variable Definitions (Naming Consistency)

To keep naming consistent with the rest of the repo, we’ll use:

### 2.1 External / Physical (外功)
- `ExtAtk` = External Attack value used for this hit (see Hit Types section)
- `TargetDef` = Target Defense used in the formula (CN sources show bosses can have small non-zero values)
- `ExtMult` = External Attack Multiplier (skill-specific / “panel multiplier”)
- `FixedExtDmg` = Fixed External Damage term (固伤 / 固定外功伤害)
- `ExtPen` = External Penetration (外功穿透)
- `ExtDmgBonus` = External Damage Bonus (外功伤害加成)

### 2.2 Attribute (属性攻击)
There are **two attribute-related terms** in the CN formula:

**A) Non-current weapon attribute attack (外系/非当前武器属性攻击)**
- `OffAttrAtk` = Non-current weapon attribute attack
- `OffAttrPen` = Corresponding attribute penetration for that attribute type
- `OffAttrBonus` = Corresponding attribute damage bonus for that attribute type

**B) Current weapon attribute attack (当前武器属性攻击)**
- `MainAttrAtk` = Current weapon attribute attack
- `MainAttrMult` = Current weapon attribute multiplier (当前武器属性倍率)
- `MainAttrPen` = Corresponding attribute penetration for the current attribute type
- `MainAttrBonus` = Corresponding attribute damage bonus for the current attribute type
- `AttrOffset` = constant offset shown in CN post:  
  `AttrOffset = 80` (noted as World Level 9 + Martial Arts Level 70 context)

### 2.3 Final multipliers
- `HitMult` = Crit/Affinity multiplier bucket (会意/会心倍率), else `1.0`
- `AdditiveIncSum` = sum of “damage increase” sources (增伤1 + 增伤2 + … + 增伤n)
- `SpecialInc` = special damage increase multiplier (特殊增伤)
- `TuningInc` = “Dingyin” / tuning increase term (定音增伤) applied as `(1 + TuningInc)`

---

## 3. Direct-Hit Damage Construction (CN Core Formula)

### 3.1 External / Physical core term
`ExtTerm` =
(`ExtMult` × (`ExtAtk` − `TargetDef`) + `FixedExtDmg`)
× (1 + `ExtPen` / 200)
× (1 + `ExtDmgBonus`)

### 3.2 Off-attribute term (non-current weapon attribute)
`OffAttrTerm` =
(`ExtMult` × `OffAttrAtk`)
× (1 + `OffAttrPen` / 200)
× (1 + `OffAttrBonus`)

> Note: CN formula reuses `ExtMult` here. This is one reason why “panel multipliers” matter a lot.

### 3.3 Main attribute term (current weapon attribute)
`MainAttrTerm` =
(`MainAttrMult` × (`MainAttrAtk` + `AttrOffset`))
× (1 + `MainAttrPen` / 200)
× (1 + `MainAttrBonus`)

### 3.4 Base constructed damage (before hit type and global increases)
`BaseConstructed` = `ExtTerm` + `OffAttrTerm` + `MainAttrTerm`

---

## 4. How Hit Types Plug Into This Model (Critical Clarification)

The CN formula writes a single multiplier term:

`HitMult` = (会意/会心倍率)

This **does not** mean the formula contains *crit chance* or *affinity chance*.  
It means:

- If the resolved hit type is **Critical** → `HitMult = CritMultiplier` (commonly ~`1.5` baseline)
- If the resolved hit type is **Affinity** → `HitMult = AffinityMultiplier` (commonly ~`1.35` baseline)
- Otherwise (Normal / Precision / Abrasion, etc.) → `HitMult = 1.0`

### 4.1 Which variables change with hit type?
Hit type resolution primarily affects:

**A) The attack roll used for `ExtAtk`**
- Normal / Precision-normal: `ExtAtk` is a random roll in `[Pmin, Pmax]` (or modeled as average)
- Crit / Affinity: often forces **max-roll behavior** in practice (community model)
- Abrasion: forces **min-roll behavior** in practice

So: **the formula stays the same**, but the *input value* `ExtAtk` and the *multiplier* `HitMult` change based on the bucket you rolled.

**B) Whether `HitMult` is `1.0`, `CritMult`, or `AffinityMult`**
- You “replace with 1.0” when the hit is not Crit/Affinity.

This is why a “single consolidated formula” is still valid, but only **conditionally** on the resolved hit type.

---

## 5. Final Damage Application (Global Modifiers)

The CN formula applies additional multipliers after construction:

`FinalDamage` =
`BaseConstructed`
× `HitMult`
× `AdditiveIncSum`
× `SpecialInc`
× (1 + `TuningInc`)

### 5.1 Additive increase note (PS1 from CN post)
CN states most “增伤” sources are **added together** (not multiplied separately), except rare special cases.

So:
- If you have multiple “Damage Increase” sources, you generally build them into one combined term:
  - `AdditiveIncSum = (Inc1 + Inc2 + ... + IncN)`
  - (implementation details depend on whether these are stored as raw percentages or already as multiplier-form)

---

## 6. Full Damage Formula (Consolidated View)

`FinalDamage` =
(
  `ExtTerm`
  + `OffAttrTerm`
  + `MainAttrTerm`
)
× `HitMult`
× `AdditiveIncSum`
× `SpecialInc`
× (1 + `TuningInc`)

Where:

`ExtTerm` =
(`ExtMult` × (`ExtAtk` − `TargetDef`) + `FixedExtDmg`)
× (1 + `ExtPen` / 200)
× (1 + `ExtDmgBonus`)

`OffAttrTerm` =
(`ExtMult` × `OffAttrAtk`)
× (1 + `OffAttrPen` / 200)
× (1 + `OffAttrBonus`)

`MainAttrTerm` =
(`MainAttrMult` × (`MainAttrAtk` + `AttrOffset`))
× (1 + `MainAttrPen` / 200)
× (1 + `MainAttrBonus`)

And:

`HitMult` ∈ { `1.0`, `CritMultiplier`, `AffinityMultiplier` } depending on resolved hit type  
`ExtAtk` is the roll-dependent value (min/max/random) chosen by hit resolution

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
