# 📊 Risk Management — Advanced Concepts
**CyArt Red Teaming — Week 2 | Theory Document 4**
**Submitted by:** Krunal | Enrollment: 221040107040 | GTU-ITR, Mehsana

---

## 📌 Table of Contents
1. [Quantitative vs Qualitative Risk Assessment](#1-quantitative-vs-qualitative-risk-assessment)
2. [ALE Calculation — Ransomware Scenario](#2-ale-calculation--ransomware-scenario)
3. [Business Impact Analysis (BIA)](#3-business-impact-analysis-bia)
4. [5×5 Risk Matrix](#4-5x5-risk-matrix)

---

## 1. Quantitative vs Qualitative Risk Assessment

### Overview

Risk assessment is the process of identifying threats, estimating the likelihood they will occur, and measuring the potential impact on the organization. There are two main approaches:

| Aspect | Quantitative | Qualitative |
|--------|-------------|-------------|
| **Definition** | Assigns numerical monetary values to risks for financial decision-making | Uses descriptive scales (High/Medium/Low) based on expert judgment |
| **Output** | Dollar amounts (e.g., ALE = $2,000/year) | Risk ratings (e.g., High Risk, Medium Risk) |
| **Best For** | Board presentations, budget justification, cyber insurance pricing | Quick assessments, early-stage planning, non-financial risks |
| **Tools** | FAIR model, ALE formula, Monte Carlo simulations | Risk matrices, heat maps, CVSS scoring |
| **Time Required** | High — requires data collection and analysis | Low — can be done in a workshop session |
| **Example Output** | "Ransomware attack costs us $2,000/year on average" | "Ransomware is HIGH likelihood, CRITICAL impact" |
| **Limitation** | Requires accurate historical data; can be time-consuming | Subjective — different analysts may rate the same risk differently |

### When to Use Each

- **Use Quantitative** when you need to justify security budget to the board, compare cost of risk vs cost of control, or apply for cyber insurance.
- **Use Qualitative** when you need a quick risk prioritization, are working with limited data, or are in early planning stages.
- **Best Practice:** Use qualitative first to identify and prioritize risks, then apply quantitative analysis to the top risks for budget justification.

---

## 2. ALE Calculation — Ransomware Scenario

### Key Formulas

| Formula | Meaning |
|---------|---------|
| **SLE = AV × EF** | Single Loss Expectancy = Asset Value × Exposure Factor |
| **ALE = SLE × ARO** | Annualized Loss Expectancy = SLE × Annual Rate of Occurrence |
| **ROSI = (ALE − Cost of Control) / Cost of Control** | Return on Security Investment |

---

### Scenario: Ransomware Attack on a Company Database Server

| Variable | Full Name | Value | Explanation |
|----------|-----------|-------|-------------|
| **AV** | Asset Value | $50,000 | Total value of the database server including data, hardware, and business impact of downtime |
| **EF** | Exposure Factor | 20% (0.20) | Percentage of asset value lost in a single ransomware incident (partial encryption, downtime, recovery costs) |
| **SLE** | Single Loss Expectancy | **$10,000** | AV × EF = $50,000 × 0.20 = $10,000 — Expected loss from ONE ransomware attack |
| **ARO** | Annual Rate of Occurrence | 0.2 | Expected frequency of this attack per year — 0.2 means once every 5 years based on industry data |
| **ALE** | Annualized Loss Expectancy | **$2,000** | SLE × ARO = $10,000 × 0.2 = $2,000 — Average annual financial loss from ransomware risk |

---

### Step-by-Step Calculation

```
Step 1: Calculate SLE
SLE = Asset Value (AV) × Exposure Factor (EF)
SLE = $50,000 × 0.20
SLE = $10,000

Step 2: Calculate ALE
ALE = Single Loss Expectancy (SLE) × Annual Rate of Occurrence (ARO)
ALE = $10,000 × 0.2
ALE = $2,000 per year

Step 3: Business Decision
Annual cost of ransomware risk = $2,000/year
```

> 📸 Screenshot of Google Sheet: See `Theory/screenshots/ale_calculation_sheet.png`

---

### Google Sheets Setup (What to Create)

Create a Google Sheet with this exact layout:

| A (Variable) | B (Value) |
|-------------|---------|
| Asset Value (AV) | $50,000 |
| Exposure Factor (EF) | 20% |
| Single Loss Expectancy (SLE) | =50000*0.2 |
| Annual Rate of Occurrence (ARO) | 0.2 |
| Annualized Loss Expectancy (ALE) | =B3*B4 |

Color the ALE row in **red** to highlight it as the key output.

---

### ROSI — Return on Security Investment

**Scenario:** A ransomware protection solution costs $500/year. It reduces ransomware risk by 80%.

```
ALE Before Control   = $2,000/year
ALE After Control    = $2,000 × (1 - 0.80) = $400/year
Risk Reduction       = $2,000 - $400 = $1,600/year saved

ROSI = (Risk Reduction - Cost of Control) / Cost of Control
ROSI = ($1,600 - $500) / $500
ROSI = $1,100 / $500
ROSI = 2.2 = 220%
```

**Conclusion:** Every $1 invested in this ransomware control returns $2.20 in risk reduction. This is a clear business case to implement the control.

---

### ALE for Multiple Risk Scenarios

| Risk Scenario | AV | EF | SLE | ARO | ALE | Priority |
|--------------|----|----|-----|-----|-----|----------|
| Ransomware Attack | $50,000 | 20% | $10,000 | 0.2 | **$2,000** | HIGH |
| Data Breach (external) | $100,000 | 30% | $30,000 | 0.1 | **$3,000** | CRITICAL |
| DDoS Attack | $20,000 | 10% | $2,000 | 1.0 | **$2,000** | HIGH |
| Phishing (credential theft) | $30,000 | 15% | $4,500 | 0.5 | **$2,250** | HIGH |
| Insider Threat | $80,000 | 25% | $20,000 | 0.05 | **$1,000** | MEDIUM |

---

## 3. Business Impact Analysis (BIA)

### What is BIA?

Business Impact Analysis (BIA) is the process of identifying critical business processes and determining the financial and operational impact if those processes are disrupted. BIA answers: *"If this system goes down, how long can we survive, and what does it cost us?"*

### Key BIA Metrics

| Metric | Full Name | Definition | Ransomware Example |
|--------|-----------|-----------|-------------------|
| **RTO** | Recovery Time Objective | Maximum acceptable time to restore a system after disruption | 4 hours — production database must be back online within 4 hours |
| **RPO** | Recovery Point Objective | Maximum acceptable data loss measured in time | 1 hour — backups every hour; at most 1 hour of transactions can be lost |
| **MTTR** | Mean Time to Recover | Average time taken to fully recover from an incident | 6 hours for ransomware: removal + rebuild + restore + verify |
| **MTBF** | Mean Time Between Failures | Average time between significant security incidents | 24 months — major incident expected every 2 years on average |
| **MTD** | Maximum Tolerable Downtime | Absolute maximum time a business can survive without a system before permanent damage | 48 hours — beyond 2 days, financial and reputational damage becomes unrecoverable |

### BIA for Critical Systems (Mock Organization)

| System | Business Impact | RTO | RPO | Backup Frequency |
|--------|----------------|-----|-----|-----------------|
| Customer Database | CRITICAL — all operations stop | 4 hours | 1 hour | Hourly incremental, daily full |
| Email Server | HIGH — internal comms halted | 8 hours | 4 hours | Every 4 hours |
| Web Application | HIGH — customer-facing services down | 2 hours | 30 min | Every 30 minutes |
| Development Server | MEDIUM — only dev work affected | 24 hours | 24 hours | Daily |
| HR System | LOW — manual workarounds possible | 72 hours | 24 hours | Daily |

---

## 4. 5×5 Risk Matrix

### How to Read the Matrix

- **Risk Score = Likelihood × Impact**
- Scores 1–4: 🟢 **LOW** — Monitor, low priority
- Scores 5–9: 🟡 **MEDIUM** — Plan mitigation, schedule remediation
- Scores 10–14: 🟠 **HIGH** — Immediate action required
- Scores 15–25: 🔴 **CRITICAL** — Emergency response required

> 📸 Screenshot: See `Theory/screenshots/risk_matrix.png`

---

### 5×5 Risk Matrix (Text Representation)

```
         IMPACT →
         1-Very Low  2-Low    3-Medium  4-High   5-Critical
L  5-Almost  5🟡      10🟠     15🔴      20🔴     25🔴
I  Certain
K  4-Likely  4🟢       8🟡     12🟠      16🔴     20🔴
E
L  3-Possible 3🟢      6🟡      9🟡      12🟠    [15🔴★]
I
H  2-Unlikely 2🟢      4🟢      6🟡       8🟡     10🟠
O
O  1-Rare     1🟢      2🟢      3🟢       4🟢      5🟡
D↓

★ = RANSOMWARE plotted here (Likelihood=3, Impact=5) → Score 15 → CRITICAL
```

---

### Risk Register — Top Risks Plotted

| # | Risk | Likelihood | Impact | Score | Rating | Required Action |
|---|------|-----------|--------|-------|--------|----------------|
| 1 | **Ransomware Attack** | 3 — Possible | 5 — Critical | **15** | 🔴 CRITICAL | Offline backups, EDR, patch management, IR plan, MFA |
| 2 | **Phishing / Social Engineering** | 5 — Almost Certain | 3 — Medium | **15** | 🔴 CRITICAL | Email gateway, MFA, security awareness training, anti-phishing policy |
| 3 | **SQL Injection on Web App** | 4 — Likely | 4 — High | **16** | 🔴 CRITICAL | Input validation, WAF, parameterized queries, penetration testing |
| 4 | **Unpatched Software / CVE** | 4 — Likely | 3 — Medium | **12** | 🟠 HIGH | Automated patch management, monthly vuln scans, SLA for critical patches |
| 5 | **Insider Threat** | 2 — Unlikely | 5 — Critical | **10** | 🟠 HIGH | Privileged access management, user behavior analytics, DLP, least privilege |
| 6 | **DDoS Attack** | 3 — Possible | 3 — Medium | **9** | 🟡 MEDIUM | CDN, rate limiting, DDoS mitigation service, incident playbook |
| 7 | **Lost/Stolen Laptop** | 4 — Likely | 2 — Low | **8** | 🟡 MEDIUM | Full disk encryption (BitLocker), MDM remote wipe, MFA for VPN |
| 8 | **Third-Party Vendor Breach** | 2 — Unlikely | 4 — High | **8** | 🟡 MEDIUM | Vendor security assessments, least-privilege API access, contract clauses |

---

### Risk Treatment Options

| Option | When to Use | Example |
|--------|-------------|---------|
| **Mitigate (Reduce)** | When you can reduce likelihood or impact with a control | Deploy MFA to reduce phishing success rate |
| **Transfer** | When financial impact can be shifted to another party | Purchase cyber insurance to cover ransomware losses |
| **Accept** | When cost of mitigation exceeds cost of the risk | Accept risk of a low-score P4 finding with no fix available |
| **Avoid** | When you can eliminate the risk-creating activity | Stop using an unsupported legacy application with known vulnerabilities |

---

### Key Risk Management Takeaways

1. **Quantify before you invest** — Use ALE to prove that a security control is worth its cost before buying it.
2. **Not all risks are equal** — A 5×5 risk matrix helps prioritize where to spend limited security budget.
3. **BIA drives backup strategy** — Your RTO and RPO determine how often backups must run and where they are stored.
4. **Ransomware is a CRITICAL risk** — Likelihood=3, Impact=5, Score=15 places it firmly in the emergency action zone.
5. **ROSI proves business value** — A 220% return on a ransomware control is a compelling case for any CFO.

---

*Document prepared for CyArt Red Teaming Week 2 | Krunal | GTU-ITR Mehsana*
