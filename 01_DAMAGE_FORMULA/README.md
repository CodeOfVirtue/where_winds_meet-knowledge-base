# Damage Formula (Section 01)

This section documents the **currently known damage calculation model** for *Where Winds Meet*, based on community research, translated CN sources, and spreadsheet modeling.

The goal of this section is to provide a **clear, structured understanding** of how damage is constructed, modified, and resolved in combat — and why certain stats and gear choices consistently outperform others.

⚠️ **Disclaimer**  
Some mechanics described here are inferred from testing and CN documentation.  
While the model is believed to be accurate for current versions, it is **not officially confirmed** by the developers.

---

## 📌 What You’ll Find in This Section

### 1️⃣ High-Level Damage Formula Overview  
📄 [`notes/overview.md`](./notes/overview.md)

A **top-down explanation** of how damage is calculated in layers:

- Base damage construction  
- Hit type resolution  
- Final modifiers and mitigation  

This is the best starting point if you want to understand:
- Why Physical Attack scales so well
- Why Attribute Attack looks good locally but falls behind globally
- How the spreadsheet’s logic is structured

---

### 2️⃣ Hit Types Explained (Crit, Affinity, Precision, Abrasion)  
📄 [`notes/hit_types_explained.md`](./notes/hit_types_explained.md)

Explains how hit types work and, critically, why they are **mutually exclusive**.

Covers:
- Normal, Critical, Affinity, Precision, and Abrasion hits
- Why Precision prevents Critical/Affinity hits
- How hit type choice affects consistency vs peak damage

Essential reading for anyone optimizing:
- Crit vs Precision
- Affinity-heavy builds
- Expected damage vs burst damage

---

### 3️⃣ Penetration & External Defense (Summary)  
📄 [`notes/penetration_external_defense.md`](./notes/penetration_external_defense.md)

Introduces the concept of **external defense** — a hidden mitigation layer inferred from CN sources and testing.

Covers:
- Why bosses still mitigate damage without visible defense stats
- How penetration reduces this mitigation
- Why excess penetration still grants damage at reduced efficiency (½ scaling)

This section explains the origin of commonly cited rules such as:
> “30 penetration against 0 defense = ~15% bonus damage”

A deeper dive is available in **Section 02**.

---

### 4️⃣ Damage Calculation Spreadsheets  
📂 `spreadsheets/`

Contains the **damage calculator spreadsheets** used throughout this project.

These allow you to:
- Compare current gear vs new gear
- See average damage changes from stat swaps
- Test how different stats affect outcomes under the same conditions

Detailed usage instructions are documented separately.

---

### 5️⃣ Source Links & Translations  
📂 `sources/`

Primary and secondary references used to build this model, including:
- CN forum posts and articles
- Translated explanations
- Video breakdowns
- Original spreadsheet sources where accessible

This section exists for transparency, validation, and further research.

---

## How to Use This Section

**Recommended order for new readers:**

1. Overview → understand the structure  
2. Hit Types → understand damage variability  
3. Penetration Summary → understand mitigation  
4. Spreadsheet → apply the knowledge  

This section intentionally avoids class-specific or set-specific conclusions — those are covered in later sections.

---

## What This Section Does *Not* Cover

- Gear set comparisons (see Section 03)
- Class-specific rotations
- Endurance management
- PvP-specific scaling
- Healer or tank optimization

Those topics build on the foundation established here.
