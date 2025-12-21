# Damage Calculation Overview

This document provides a **top-down explanation** of how damage is calculated in *Where Winds Meet*, based on the mathematical model used throughout this project.

Rather than listing individual stats or formulas, this section explains **how damage is built in layers**, what each layer does, and why certain stats scale far better than others once all layers are considered.

If you want to understand *why the spreadsheet behaves the way it does*, start here.

---

## Damage Is Calculated in Three Layers

Every direct hit in the game follows the same high-level structure:

1. **Base Damage Construction**  
2. **Hit Type Resolution**  
3. **Final Modifiers & Mitigation**

Each layer serves a different purpose and amplifies different stats.

Understanding which stats matter in which layer is the key to correct optimization.

---

## 1️⃣ Base Damage Construction  
*“How much damage does this attack fundamentally contain?”*

In the first layer, the game **constructs raw damage** by adding together multiple independent components.

At this stage:
- No crits are applied
- No hit-type multipliers are applied
- No global damage increases are applied

Damage is simply **built**, not evaluated.

---

### Components of Base Damage

Base damage is the **sum** of three parts:

- **External / Physical damage**
- **Off-attribute damage** (non-current weapon attributes)
- **Main attribute damage** (current weapon attribute)

These components are **added together first**, then treated as a single value in later layers.

This is critical:  
> **Nothing multiplies anything else at this stage — everything is summed.**

---

### Why Physical Attack Scales So Well

The **external / physical component** is the most important contributor because it:

- Uses **Physical Attack**
- Uses **skill (panel) multipliers**
- Applies **penetration**
- Applies **physical damage bonuses**
- Can include **flat damage**
- Shares its multiplier with off-attribute damage

In other words, Physical Attack participates in **more scaling mechanisms** than any other stat.

Because later layers multiply the *entire constructed damage*, anything that dominates this layer gets amplified everywhere else.

---

### Why Attribute Attack Falls Behind

Attribute Attack contributes in two narrower ways:

- **Off-attribute damage**  
  - Reuses the physical multiplier  
  - Scales reasonably, but only as an extension of physical structure

- **Main attribute damage**  
  - Uses its own multiplier  
  - Receives a fixed +80 offset  
  - Does *not* scale with Physical Attack  
  - Does *not* reuse the external multiplier

As a result:
- Attribute damage looks strong when examined in isolation
- But it forms a **smaller share of total damage**
- And therefore benefits less from later multipliers

This is why attribute-focused builds tend to fall behind as gear and buffs improve.

---

## 2️⃣ Hit Type Resolution  
*“What kind of hit did you roll?”*

Once base damage is constructed, the game resolves the **hit type**.

This layer does **not** rebuild damage.  
It simply evaluates the damage that already exists.

---

### What Hit Type Resolution Changes

Hit type resolution affects two things:

#### 1. Attack Roll Selection
Depending on hit type:
- Normal / Precision → random or averaged roll
- Critical / Affinity → max-roll behavior
- Abrasion → min-roll behavior

The formula does not change — only the input value does.

#### 2. Hit Multiplier Application
- Critical → Crit multiplier
- Affinity → Affinity multiplier
- All other hit types → multiplier = **1.0**

Crit chance and affinity chance are **not part of the formula**.  
They only determine which hit type is resolved.

---

### Why This Favors Physical Damage

Hit multipliers apply to the **entire constructed damage**:

(Physical + Off-Attribute + Main Attribute)

Since Physical Attack dominates base construction, it also benefits most from:
- Critical hits
- Affinity hits
- Any hit-type-based burst

This reinforces physical scaling rather than correcting it.

---

## 3️⃣ Final Modifiers & Mitigation  
*“How much of this hit actually lands?”*

The final layer applies **global modifiers** that affect all damage equally.

---

### What Is Applied Here

- **Additive damage increases**  
  (weapon buffs, inner ways, dungeon talents, exhaustion bonuses, etc.)

- **Special damage multipliers**  
  (rare effects that multiply separately)

- **Tuning / Dingyin bonuses**  
  (always applied as `(1 + value)`)

Most damage increases are **added together first**, then applied once.

---

### Why Attribute Damage Suffers Here

By the time damage reaches this layer:

- Attribute damage is already a smaller portion of the total
- Additive stacking causes heavy dilution
- Physical-fed components still dominate the base

As a result:
- Attribute bonuses scale poorly in high-buff environments
- Physical-heavy builds retain efficiency under stacking

---

## Putting the Layers Together

[ Base Damage Construction ]
├─ Physical core
├─ Off-attribute extension
└─ Main attribute add-on
↓
[ Hit Type Resolution ]
├─ Attack roll selection
└─ Crit / Affinity multiplier
↓
[ Final Modifiers ]
├─ Additive increases
├─ Special multipliers
└─ Tuning

Each layer amplifies the output of the previous one — it never replaces it.

---

## Relation to the Spreadsheet

The spreadsheet mirrors this structure exactly:

- Damage components are calculated **separately**
- Hit types are applied **after construction**
- Additive bonuses are **collapsed into a single bucket**
- Expected damage is calculated via **probability weighting**

Nothing in the spreadsheet is arbitrary — it is a direct implementation of this layered model.

---

## Key Takeaways

- **Physical Attack** scales best because it participates in the most layers  
- **Attribute Attack** adds damage but lacks global amplification  
- **Hit types amplify existing structure**, they do not rebalance it  
- **Additive stacking dilutes narrow bonuses** over time  

Once you understand these layers, stat priorities become predictable rather than debatable.
