# Penetration & External Defense (Summary)

This note provides a **brief explanation** of how penetration is modeled in the damage formula.
A deeper discussion is available in Section 02.

---

## External Defense (Conceptual)

“External defense” refers to a **hidden damage mitigation layer** inferred from CN sources and testing.

Key points:
- It is **not** the same as the visible Physical Defense stat
- Bosses appear to have this mitigation even when no defense value is shown
- Normal enemies may or may not have meaningful values

Because it is hidden, exact values are unknown.

---

## How Penetration Interacts

Penetration reduces the effect of this mitigation.

- If penetration is **lower than** external defense:
  - Damage is reduced
- If penetration **exceeds** external defense:
  - Excess penetration still increases damage
  - Excess benefit is applied at **50% efficiency**

Example (simplified):
- 30 penetration vs 0 external defense → ~15% bonus damage

---

## Why This Matters

This behavior explains why:
- Penetration remains valuable against bosses
- Penetration is not a simple flat damage multiplier
- Physical penetration often scales better than expected

For a full explanation, examples, and caveats, see:
- [`../../02_PENETRATION_EXTERNAL_DEFENSE/README.md`](../../02_PENETRATION_EXTERNAL_DEFENSE/README.md)
