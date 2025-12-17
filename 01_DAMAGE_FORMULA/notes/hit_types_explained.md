# Hit Types Explained

This document explains how different hit types work in *Where Winds Meet* and how they affect damage outcomes.

---

## Core Rule: Mutual Exclusivity

Each attack can result in **only one hit type**.

A hit **cannot** be:
- Precision *and* Critical
- Critical *and* Affinity
- Affinity *and* Precision

The game first determines **which hit type occurs**, then applies the corresponding damage calculation.

---

## Hit Types Overview

### Normal Hit
- Uses an average roll between minimum and maximum values
- No special multipliers applied

---

### Critical Hit
- Applies a Critical Damage multiplier
- Default base multiplier is approximately **1.5×**
- Uses standard damage construction, then multiplies

---

### Affinity Hit
- Uses **maximum attack value**
- Applies an Affinity Damage multiplier
- Default base multiplier is approximately **1.35×**

Affinity hits favor builds that emphasize maximum attack scaling.

---

### Precision Hit
- Forces a **maximum roll**
- **Prevents Critical and Affinity hits**
- Strong for consistency, but suppresses crit-based scaling

Precision increases reliability at the cost of peak potential.

---

### Abrasion Hit
- Forces a **minimum roll**
- Represents the lowest damage outcome
- Typically undesirable but unavoidable in probability-based systems

---

## Practical Implications

Because hit types are mutually exclusive:
- Increasing Precision can reduce effective Critical/Affinity value
- High Critical Rate does nothing on Precision hits
- Hit-type balance is a **trade-off**, not a stack

This interaction is one reason why stat optimization is non-trivial.
