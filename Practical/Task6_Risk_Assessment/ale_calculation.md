# 📊 Task 6 — Risk Assessment (ALE Calculation)
**CyArt Red Teaming — Week 2 | Practical Task 6**

---

## 📌 Objective
Calculate Annualized Loss Expectancy (ALE) for a ransomware scenario using Google Sheets. Create a 5×5 Risk Matrix.

---

## ✅ ALE Calculation — Ransomware Scenario

| Variable | Full Name | Value | Formula |
|----------|-----------|-------|---------|
| **AV** | Asset Value | $50,000 | Direct estimate of server + data value |
| **EF** | Exposure Factor | 20% (0.20) | % of asset lost in one incident |
| **SLE** | Single Loss Expectancy | **$10,000** | = AV × EF = $50,000 × 0.20 |
| **ARO** | Annual Rate of Occurrence | 0.2 | Once every 5 years based on industry data |
| **ALE** | Annualized Loss Expectancy | **$2,000** | = SLE × ARO = $10,000 × 0.2 |

> 📸 Screenshot of Google Sheet: `Task6_Risk_Assessment/screenshots/ale_google_sheet.png`

---

## ✅ ROSI Calculation

```
Security Control Cost = $500/year
Control Effectiveness = 80% risk reduction

ALE Before = $2,000/year
ALE After  = $2,000 × (1 - 0.80) = $400/year
Savings    = $2,000 - $400 = $1,600/year

ROSI = ($1,600 - $500) / $500 = 220%
```

**Conclusion:** $500/year security control gives 220% return. Justified to implement.

---

## ✅ 5×5 Risk Matrix

Created in Google Sheets with color coding:
- 🟢 Green = Score 1–4 (Low)
- 🟡 Yellow = Score 5–9 (Medium)
- 🟠 Orange = Score 10–14 (High)
- 🔴 Red = Score 15–25 (Critical)

**Ransomware plotted at: Likelihood=3, Impact=5 → Score=15 → CRITICAL (Red)**

> 📸 Screenshot: `Task6_Risk_Assessment/screenshots/risk_matrix.png`

---

## Google Sheets Formula Reference

```
Cell B3 (SLE): =B1*B2          → $50,000 × 0.20 = $10,000
Cell B5 (ALE): =B3*B4          → $10,000 × 0.2  = $2,000
Risk Matrix cell: =ROW_VALUE*COL_VALUE (then use conditional formatting for colors)
```
