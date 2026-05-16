# 🔐 Security Frameworks In Depth
**CyArt Red Teaming — Week 2 | Theory Document 2**
**Submitted by:** Krunal | Enrollment: 221040107040 | GTU-ITR, Mehsana

---

## 📌 Table of Contents
1. [NIST Cybersecurity Framework (CSF)](#1-nist-cybersecurity-framework-csf)
2. [ISO 27001 Controls](#2-iso-27001-controls)
3. [WannaCry — NIST CSF Mapping](#3-wannacry-ransomware--nist-csf-mapping)

---

## 1. NIST Cybersecurity Framework (CSF)

### What is NIST CSF?

The NIST Cybersecurity Framework was developed by the National Institute of Standards and Technology in 2014 and updated to Version 2.0 in 2024. It provides a risk-based, flexible approach to managing cybersecurity risk applicable to any organization regardless of size, sector, or maturity level.

> 🔗 Source: https://www.nist.gov/cyberframework

---

### The 5 Core Functions

| Function | Goal | Key Activities | Real-World Example |
|----------|------|---------------|-------------------|
| **IDENTIFY** | Understand the business context and cybersecurity risks | Asset Management, Risk Assessment, Governance, Supply Chain Risk Management | Create a complete inventory of all servers, applications, databases, and data assets. Identify which are critical to business operations and what threats they face. |
| **PROTECT** | Implement safeguards to ensure delivery of critical services | Access Control, Encryption, Security Awareness Training, Secure Configuration, Data Protection | Enable MFA for all user accounts. Encrypt sensitive data at rest and in transit using AES-256. Conduct quarterly phishing awareness training for all employees. |
| **DETECT** | Identify the occurrence of a cybersecurity event quickly | Security Monitoring, Anomaly Detection, SIEM Alerting, Log Analysis, IDS/IPS | Deploy Wazuh SIEM to alert on suspicious login attempts, PowerShell executions, privilege escalations, and lateral movement patterns across the network. |
| **RESPOND** | Take action regarding a detected cybersecurity incident | Incident Response Planning, Communication, Containment, Mitigation, Forensic Analysis | Execute the IR playbook: isolate the infected host, notify stakeholders, block attacker IP via firewall, preserve disk image for forensics, update threat intel feeds. |
| **RECOVER** | Restore capabilities impaired by a cybersecurity incident | Recovery Planning, Backup Restoration, Lessons Learned, Communication, Improvement | Restore affected systems from clean, verified offline backups. Monitor restored systems for 72 hours. Conduct post-incident review and update security controls. |

---

### NIST CSF Implementation Tiers

| Tier | Name | Description | Maturity Level |
|------|------|-------------|---------------|
| **Tier 1** | Partial | Ad-hoc and reactive cybersecurity. No formal risk management process. Security practices are inconsistent and not organization-wide. | Low |
| **Tier 2** | Risk-Informed | Risk management practices exist and are approved by management but are not consistently applied across the organization. Some awareness of cybersecurity risk exists. | Medium-Low |
| **Tier 3** | Repeatable | Formally approved, documented, and regularly updated risk management policies. Consistent organization-wide implementation. Security integrated into budgeting. | Medium-High |
| **Tier 4** | Adaptive | Organization continuously improves based on lessons learned, threat intelligence, and predictive indicators. Cybersecurity is embedded in organizational culture. | High |

---

### Applying NIST CSF to a Mock Organization

**Scenario:** A 200-person fintech company storing customer financial data.

| NIST Function | Current Gap | Recommended Action | Priority |
|--------------|-------------|-------------------|----------|
| Identify | No formal asset inventory exists | Deploy asset discovery tool, create CMDB | HIGH |
| Protect | Only 40% of accounts have MFA | Enforce MFA for all accounts within 30 days | CRITICAL |
| Detect | No centralized logging or SIEM | Deploy Wazuh or Elastic SIEM | HIGH |
| Respond | No documented incident response plan | Create and test IR playbook within 60 days | HIGH |
| Recover | Backups not tested in 12 months | Test backup restoration quarterly | MEDIUM |

---

## 2. ISO 27001 Controls

### What is ISO 27001?

ISO 27001 is the international standard for Information Security Management Systems (ISMS). Published by the International Organization for Standardization, it defines requirements for establishing, implementing, maintaining, and continually improving an ISMS. Annex A contains 114 controls across 14 domains.

> 🔗 Source: https://advisera.com/27001academy/what-is-iso-27001

---

### 5 Key Controls Mapped to Ransomware Defense

| Control | Domain | Title | How It Helps Against Ransomware |
|---------|--------|-------|--------------------------------|
| **A.12.3.1** | Operations Security | Information Backup | **Most critical ransomware defense.** Requires regular backups of all critical data stored securely in an offline or off-site location. If ransomware encrypts all files, verified clean backups allow full restoration without paying the ransom. The 3-2-1 rule (3 copies, 2 media types, 1 offsite) ensures resilience. |
| **A.12.4.1** | Operations Security | Event Logging | Requires all user activities, security events, and exceptions to be logged and retained. Ransomware leaves detectable traces — mass file encryption events, suspicious process creation, outbound C2 connections — that appear in logs. SIEM correlation of these events enables early detection before full encryption completes. |
| **A.14.2.5** | System Acquisition & Development | Secure Development Principles | Requires security to be built into software from the design phase. Internally developed applications with proper input validation, authentication, and error handling reduce the vulnerabilities that ransomware uses for initial access (e.g., web shell uploads, SQL injection to execute OS commands). |
| **A.8.2.1** | Asset Management | Classification of Information | Requires all information assets to be classified (e.g., Public, Internal, Confidential, Secret). Helps organizations identify which data is most valuable and therefore most critical to protect with frequent encrypted backups. Ransomware actors specifically target high-value classified data for double-extortion attacks. |
| **A.6.1.2** | Organization of Information Security | Segregation of Duties | Prevents a single user account from having unrestricted access to all systems. If one account is compromised by ransomware, network segmentation and access segregation prevent the malware from encrypting all servers simultaneously, significantly limiting the blast radius of the attack. |

---

### ISO 27001 vs NIST CSF — Comparison

| Aspect | NIST CSF | ISO 27001 |
|--------|----------|-----------|
| Origin | US Government (NIST) | International Standard (ISO/IEC) |
| Type | Framework / Guideline | Standard with Certification |
| Certification | No formal certification | Yes — audited by accredited body |
| Focus | Risk-based cybersecurity management | Information security management system |
| Best For | US organizations, flexible risk management | Global compliance, formal certification needed |
| Controls | Mapped to subcategories | 114 controls in Annex A |
| Update Cycle | CSF 2.0 released 2024 | ISO 27001:2022 is current version |

---

## 3. WannaCry Ransomware — NIST CSF Mapping

### Background

WannaCry struck in May 2017 and infected over **200,000 computers in 150 countries within 24 hours** — one of the fastest-spreading cyberattacks in history. It exploited **EternalBlue (MS17-010)**, a Windows SMB vulnerability originally developed by the NSA and leaked by the Shadow Brokers group. Microsoft had released patch MS17-010 in March 2017 — two months before the attack — but millions of systems remained unpatched.

**Major Victims:** UK National Health Service (NHS), Telefonica (Spain), FedEx (USA), Deutsche Bahn (Germany), Russian Interior Ministry.

> 🔗 Source: https://www.cisa.gov/news-events/cybersecurity-advisories

---

### WannaCry Mapped to NIST CSF

| NIST CSF Function | What Went Wrong | What Should Have Been Done | ISO 27001 Control |
|------------------|----------------|--------------------------|------------------|
| **IDENTIFY** | Organizations had not inventoried which Windows systems were running SMBv1 or were missing the MS17-010 patch. Without knowing vulnerable assets, they could not prioritize remediation. | Maintain a real-time asset inventory including OS versions and patch status. Perform regular vulnerability scans (at minimum monthly) to identify unpatched systems. | A.8.1.1 — Inventory of Assets |
| **PROTECT** | The MS17-010 patch was available for 2 months before WannaCry struck. The majority of affected organizations had not applied it. SMBv1 — an outdated protocol — was still enabled by default on millions of systems. | Apply critical security patches within 24–72 hours of release. Disable legacy protocols (SMBv1) via Group Policy. Implement network segmentation to prevent lateral spread via SMB (TCP port 445). | A.12.6.1 — Management of Technical Vulnerabilities |
| **DETECT** | Most organizations had no IDS/IPS monitoring SMB traffic on internal networks. WannaCry spread undetected from machine to machine for hours before alerts were raised. | Deploy network IDS (Suricata/Snort) with EternalBlue detection signatures. Enable Windows Event logging (Event ID 4624, 4625, 7045) and forward to SIEM. Alert on mass file extension changes (ransomware indicator). | A.12.4.1 — Event Logging |
| **RESPOND** | Many organizations were slow to isolate infected systems from the network. Each infected machine continued scanning and infecting new targets via SMB while teams debated the response. UK NHS took hours to formally declare an incident. | Execute IR playbook immediately upon ransomware detection: isolate affected hosts from the network, block TCP port 445 at all firewall levels, activate incident response team, notify CISA/NCSC, preserve disk images for forensics. | A.16.1.5 — Response to Information Security Incidents |
| **RECOVER** | Organizations without tested, offline backups faced weeks or months of downtime. UK NHS cancelled over **19,000 patient appointments** and diverted ambulances due to system unavailability. | Implement the 3-2-1 backup rule: 3 copies, 2 different media types, 1 stored offline/offsite. Test backup restoration quarterly. Establish Recovery Time Objective (RTO) and Recovery Point Objective (RPO) targets. Have a tested DR (Disaster Recovery) plan. | A.12.3.1 — Information Backup |

---

### WannaCry Kill Switch — Notable Fact

Security researcher Marcus Hutchins accidentally stopped WannaCry's global spread by registering an unregistered domain (`iuqerfsodp9ifjaposdfjhgosurijfaewrwergwea.com`) that the malware checked before encrypting files. This acted as a kill switch built in by the attackers — when the domain resolved successfully, WannaCry stopped spreading. This demonstrates the importance of reverse engineering malware to find kill switches and behavioral triggers.

---

### Key Takeaways

1. **Patch immediately** — a 2-month-old patch would have stopped WannaCry entirely.
2. **Disable legacy protocols** — SMBv1 had no business being enabled on modern systems.
3. **Segment your network** — ransomware spreads laterally; segmentation contains the damage.
4. **Test your backups** — untested backups are not backups; they are hopes.
5. **Have an IR plan** — slow response allowed WannaCry to spread further inside networks.

---

*Document prepared for CyArt Red Teaming Week 2 | Krunal | GTU-ITR Mehsana*
