# 🛡️ Week 2 — Red Teaming & Security Operations

**Program:** CyArt Red Teaming Internship
**Submitted by:** Krunal Patel

---

## Week 2 Overview

This week covers both **Theoretical Knowledge** and **Practical Application** in the domain of Red Teaming, Threat Hunting, Incident Response, and Vulnerability Management.

## Part 1 — Theoretical Knowledge

### 1. Advanced Threat Analysis
| Topic | Status |
|-------|--------|
| MITRE ATT&CK — T1566, T1059, T1190 mapped | Done |
| ATT&CK Navigator — SVG exported | Done |
| STRIDE Threat Model — Web App diagram | Done |
| SolarWinds Case Study — 5 key points | Done |

### 2. Security Frameworks
| Topic | Status |
|-------|--------|
| NIST CSF — All 5 functions documented | Done |
| ISO 27001 — 3 controls mapped to ransomware | Done |
| WannaCry — Mapped to NIST CSF | Done |

### 3. Incident Response Fundamentals
| Topic | Status |
|-------|--------|
| SANS IR Lifecycle — 6 phases documented | Done |
| Phishing IR Playbook — 7 steps written | Done |
| IR Flowchart — draw.io PNG exported | Done |

### 4. Risk Management
| Topic | Status |
|-------|--------|
| ALE Calculation — $2,000/year (ransomware) | Done |
| ROSI Calculation — 220% return | Done |
| 5×5 Risk Matrix — Color coded + ransomware marked | Done |

---

## 🛠️ Part 2 — Practical Tasks

| Task | Tool(s) Used | Status |
|------|-------------|--------|
| Task 1 — Threat Hunting + Sigma Rule | Elastic Security, Sigma | Done |
| Task 2 — Malware Analysis | REMnux, Hybrid Analysis | Done |
| Task 3 — Vulnerability Management | OpenVAS, DefectDojo | Done |
| Task 4 — IR Simulation | MITRE Caldera, Velociraptor | Done |
| Task 5 — Network Defense | Suricata, CrowdSec | Done |
| Task 6 — Risk Assessment (ALE) | Google Sheets | Done |
| Task 7 — IR Report | SANS Template, draw.io | Done |
| Task 8 — Capstone (Full IR Cycle) | Metasploit, Wazuh, CrowdSec | Done |

---

## 💻 Lab Environment

| Component | Details |
|-----------|---------|
| Host OS | Windows 11 |
| VM Software | VirtualBox |
| Attacker Machine | Kali Linux (kali/kali) |
| Target Machine | Metasploitable 2 (msfadmin/msfadmin) |
| Malware Analysis | REMnux VM |
| Network Mode | Host-Only Adapter (isolated, no internet exposure) |

---

## 🔑 Key Findings & Learnings

- **MITRE ATT&CK** maps real-world attacker behavior to specific techniques, enabling defenders to build targeted detection rules.
- **STRIDE** helps identify threats systematically at every component boundary in an architecture.
- **SolarWinds** proved that even trusted software update pipelines can be weaponized — supply chain security is critical.
- **NIST CSF** provides a complete lifecycle (Identify → Protect → Detect → Respond → Recover) applicable to any organization size.
- **ISO 27001 A.12.3.1** (Backup) is the single most effective ransomware defense — verified offline backups eliminate the need to pay ransom.
- **ALE = $2,000/year** for ransomware shows that a $500 security control with 80% effectiveness gives 220% ROSI — a clear business case.
- **Suricata** can block malicious IPs in real-time when rules are correctly configured and tested.
- **Wazuh** provides enterprise-grade threat detection in a free, open-source package — ideal for small SOC teams.

---

## 📎 References

| Resource | Link |
|----------|------|
| MITRE ATT&CK | https://attack.mitre.org |
| ATT&CK Navigator | https://mitre-attack.github.io/attack-navigator |
| OWASP Threat Dragon | https://owasp.org/www-project-threat-dragon |
| NIST Cybersecurity Framework | https://www.nist.gov/cyberframework |
| ISO 27001 Controls | https://advisera.com/27001academy |
| SANS IR Handbook | https://www.sans.org/white-papers/incident-handlers-handbook |
| CISA WannaCry Advisory | https://www.cisa.gov/news-events/cybersecurity-advisories |
| Hybrid Analysis | https://www.hybrid-analysis.com |
| Sigma Rules | https://github.com/SigmaHQ/sigma |
| draw.io | https://draw.io |