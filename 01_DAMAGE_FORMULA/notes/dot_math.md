# DOT Damage Formula – Mathematical Model

This document defines the **Damage-over-Time (DOT) damage model** used in *Where Winds Meet*, using the same naming conventions and structural style as the direct-hit mathematical model.

The purpose of this document is to:
- Explicitly define DOT-specific terms
- Show which components are **removed or simplified**
- Allow direct comparison with the direct-hit formula

⚠️ **Important**  
This model is derived from CN documentation and community testing.  
It is not officially confirmed, but matches observed behavior with <1% error.

---

## 1. Conceptual Scope

DOT damage uses a **reduced construction model** compared to direct-hit damage.

Key exclusions:
- No fixed external damage
- No main-attribute extra multiplier
- No main-attribute +80 offset
- Usually no tuning (定音) scaling

The remaining structure mirrors the core physical + attribute construction.

---

## 2. Variable Definitions (DOT Model)

### 2.1 External / Physical (外功)

- `ExtAtk`  
  External / Physical Attack value

- `TargetDef`  
  Target Defense (usually low but non-zero)

- `ExtMult`  
  External Attack Multiplier (skill / panel multiplier)

- `ExtPen`  
  External Penetration

- `ExtDmgBonus`  
  External Damage Bonus

---

### 2.2 Attribute Damage (属性攻击)

DOT damage uses **one unified attribute term**.

- `AttrAtk`  
  Attribute Attack used by the DOT effect

- `AttrPen`  
  Corresponding Attribute Penetration

- `AttrBonus`  
  Corresponding Attribute Damage Bonus

---

### 2.3 Final Multipliers

- `HitMult`  
  Hit-type multiplier  
  `HitMult ∈ {1.0, CritMultiplier, AffinityMultiplier}`

- `AdditiveIncSum`  
  Sum of additive damage increases  
  `(增伤1 + 增伤2 + … + 增伤n)`

---

## 3. DOT Damage Construction

### 3.1 External / Physical DOT Term

`ExtTerm` =
(`ExtMult` × (`ExtAtk` − `TargetDef`))
× (1 + `ExtPen` / 200)
× (1 + `ExtDmgBonus`)

Notes:
- No fixed external damage term
- Uses the same panel multiplier as direct hits
- Represents the physical contribution per tick

---

### 3.2 Attribute DOT Term

AttrTerm =
(`ExtMult` × `AttrAtk`)
× (1 + `AttrPen` / 200)
× (1 + `AttrBonus`)

Notes:
- Reuses `ExtMult`
- No separation into main/off attribute
- No +80 offset
- Linear contribution only

---

### 3.3 Base DOT Damage

`BaseDOT` =
`ExtTerm` + `AttrTerm`

At this stage:
- Damage is fully constructed
- No hit-type multiplier applied
- No global increases applied

---

## 4. Hit-Type Application to DOT

The CN formula applies a hit-type multiplier to DOT:

`DOTAfterHit` =
`BaseDOT` × `HitMult`

Where:
- `HitMult = CritMultiplier` if DOT resolves as Critical
- `HitMult = AffinityMultiplier` if DOT resolves as Affinity
- `HitMult = 1.0` otherwise

⚠️ Crit/Affinity interaction depends on the originating skill and DOT type.  
Many DOT effects have **limited or inconsistent hit-type scaling**.

---

## 5. Final DOT Damage Formula

`FinalDOT` =
`BaseDOT`
× `HitMult`
× `AdditiveIncSum`

Expanded:

`FinalDOT` =
(
(`ExtMult` × (`ExtAtk` − `TargetDef`))
× (1 + `ExtPen` / 200)
× (1 + `ExtDmgBonus`)

(`ExtMult` × `AttrAtk`)
× (1 + `AttrPen` / 200)
× (1 + `AttrBonus`)
)
× `HitMult`
× `AdditiveIncSum`

---

## 6. Comparison to Direct-Hit Terms

| Component                  | Direct Hit | DOT |
|---------------------------|------------|-----|
| `FixedExtDmg`             | ✓          | ✗   |
| `OffAttrTerm`             | ✓          | ✗   |
| `MainAttrTerm`            | ✓          | ✗   |
| `AttrOffset (+80)`        | ✓          | ✗   |
| `ExtMult reuse`           | ✓          | ✓   |
| `HitMult`                 | ✓          | ✓   |
| `AdditiveIncSum`          | ✓          | ✓   |
| `TuningInc`               | ✓          | ✗   |

---

## 7. Practical Implications (From Terms)

From the DOT terms alone:

- `ExtMult` is the dominant scaling lever
- `ExtAtk` contributes, but with fewer amplifiers
- `AttrAtk` is additive and narrow
- `HitMult` has reduced practical impact
- Additive scaling dilutes DOT faster than direct hits

This explains why DOT-heavy effects:
- Scale well early
- Plateau sooner
- Are sensitive to panel multiplier inaccuracies

---

## 8. DOT-Based “Direct” Skills

Some skills that appear instantaneous internally use this DOT model, including:

- 神龙吐火 – 爆燃 DOT
- 太白醉月 – 酒气爆炸
- 九剑流血
- High-value bleed effects
- Thes still need to be checked for the coresponding sources in global

These skills:
- Do not include `FixedExtDmg`
- Do not include `MainAttrTerm`
- Follow the DOT formula despite appearing as direct damage

---

## 9. Spreadsheet Implementation Notes

In the spreadsheet:

- DOT damage uses `BaseDOT` instead of `BaseConstructed`
- `FixedExtDmg`, `OffAttrTerm`, and `MainAttrTerm` are excluded
- Hit-type weighting is simplified
- Tick frequency and uptime are modeled explicitly

This prevents systematic overestimation of DOT scaling.

---

## Summary

DOT damage is constructed as:

`BaseDOT` = `ExtTerm` + `AttrTerm`

And finalized as:

`FinalDOT` = `BaseDOT` × `HitMult` × `AdditiveIncSum`

It is a **structurally simpler model** with fewer amplification paths, which fully explains its observed scaling behavior in PVE.
