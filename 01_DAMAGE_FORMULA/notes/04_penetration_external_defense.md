# Penetration & External Defense (Summary)

This note provides a **high-level explanation** of how `penetration` appears to influence damage in *Where Winds Meet*, based on currently available CN sources, community calculators, and direct testing.

A deeper and more technical discussion — including formulas, edge cases, and reconciliation of conflicting data — is provided in **Section 02**.

---

## External Defense (Conceptual)

In community discussions, the term **“external defense”** or **“resistance”** is often used to describe a form of **damage mitigation that is not clearly exposed in the UI**.

What can be stated with confidence:

- It is **not identical** to the visible `Physical Defense` stat shown on enemies.
- Bosses appear to apply some form of mitigation **even when no explicit defense value is displayed**.
- Normal enemies may or may not have meaningful mitigation.
- Exact values and implementation details are **not directly observable**.

At present, it is safer to treat `external defense` as a **conceptual placeholder** for *unknown or opaque mitigation behavior*, rather than as a clearly defined stat.

---

## How `penetration` Appears to Work (Observed Behavior)

Across multiple independent sources and tests, `penetration` consistently increases damage in a way that is:

- broadly **linear** within practical stat ranges,
- effective against bosses and normal enemies alike,
- and **not nullified** by low or near-zero displayed defense.

However, the **exact conversion rate** from `penetration` to damage increase is **not fully agreed upon**.

### What different sources report

- Multiple CN explanations and player tests suggest that:
  - **~3–5% total damage increase per 10 `penetration`** is a reasonable real-world estimate.
- Spreadsheet-based implementations derived from CN formulas frequently show:
  - **~3.7–4.0% per 10 `penetration`** under controlled conditions.
- Some video explanations from reputable CN creators report **higher per-point values**, which do not perfectly align with spreadsheet results.

All of these sources appear internally consistent within their own assumptions — but **do not yet fully agree with each other**.

---

## What Is Still Unclear

The following points remain **open questions**:

- How (or if) enemy `resistance` interacts with `penetration`.
- Whether `penetration` is partially offset, dampened, or redistributed internally.
- Why some explanations arrive at higher per-point values than formula-based calculators.
- Whether additional hidden factors affect `penetration`’s effective contribution in combat.

As a result, `penetration` should not currently be described as a fixed-percentage stat with a universally agreed conversion rate.

---

## Practical Interim Understanding

Until these discrepancies are resolved, the most defensible summary is:

- `penetration` **scales damage linearly within tested ranges**.
- A working estimate of **~3–5% damage increase per 10 `penetration`** is reasonable for practical comparisons.
- The **exact conversion depends on factors that are not yet fully understood**.
- This section should be treated as **provisional** and updated as new evidence becomes available.

---

## Status & Next Steps

This topic is **actively under investigation**.

Further clarification is expected through:
- direct communication with CN content creators,
- comparison against original spreadsheets used in CN theorycrafting,
- and additional controlled testing.

Any confirmed revisions to `penetration`’s behavior will be documented explicitly and reflected throughout the damage model.
