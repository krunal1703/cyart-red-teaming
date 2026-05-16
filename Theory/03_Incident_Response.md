# 🚨 Incident Response Fundamentals
**CyArt Red Teaming — Week 2 | Theory Document 3**
**Submitted by:** Krunal | Enrollment: 221040107040 | GTU-ITR, Mehsana

---

## 📌 Table of Contents
1. [What is Incident Response?](#1-what-is-incident-response)
2. [The 6-Phase IR Lifecycle (SANS)](#2-the-6-phase-ir-lifecycle-sans)
3. [SOC Workflow & Incident Prioritization](#3-soc-workflow--incident-prioritization)
4. [IR Playbooks](#4-ir-playbooks)
5. [IR Flowchart](#5-ir-flowchart)

---

## 1. What is Incident Response?

Incident Response (IR) is a structured, organized approach to addressing and managing the aftermath of a security breach or cyberattack. The goal of IR is to:

- **Minimize damage** — limit the scope of the breach
- **Reduce recovery time** — restore normal operations quickly
- **Reduce cost** — prevent additional financial losses
- **Preserve evidence** — collect forensic artifacts for legal or regulatory purposes
- **Prevent recurrence** — identify root cause and fix it permanently

### Key IR Roles

| Role | Responsibility |
|------|---------------|
| **Incident Manager** | Coordinates the overall response. Makes final decisions on containment and communication. |
| **SOC Analyst (L1)** | First to receive and triage alerts. Determines if it is a true positive or false positive. |
| **SOC Analyst (L2/L3)** | Performs deep investigation, threat hunting, and forensic analysis. |
| **Threat Hunter** | Proactively searches for hidden attackers in the environment using IOCs and behavioral patterns. |
| **Forensic Analyst** | Collects, preserves, and analyzes digital evidence from compromised systems. |
| **Communications Lead** | Handles internal notifications (management, legal) and external communications (customers, regulators). |

---

## 2. The 6-Phase IR Lifecycle (SANS)

> 🔗 Source: SANS Incident Handler's Handbook — https://www.sans.org/white-papers/incident-handlers-handbook

The SANS Institute defines six phases of the Incident Response lifecycle. Each phase must be completed in order for an effective response.

---

### Phase 1 — Preparation

**Goal:** Build all capabilities BEFORE an incident occurs so the team is ready to respond effectively.

**Key Activities:**
- Develop and document an Incident Response Policy and Plan
- Create IR playbooks for common scenarios (phishing, ransomware, data breach)
- Deploy and configure monitoring tools (SIEM, EDR, IDS/IPS)
- Define roles and responsibilities for the IR team
- Establish communication trees (who calls who, escalation paths)
- Run tabletop exercises and simulated attack scenarios to test the plan
- Ensure all endpoints have logging enabled (Windows Event Logs, Sysmon)
- Maintain an updated asset inventory

**Tools:** Wazuh, Velociraptor, Elastic SIEM, Suricata, SANS IR templates

---

### Phase 2 — Identification / Detection

**Goal:** Determine whether a security incident has actually occurred and understand its scope.

**Key Activities:**
- Monitor SIEM dashboards for triggered correlation rules
- Review IDS/IPS alerts for anomalous network traffic
- Analyze endpoint alerts from EDR tools
- Review logs for indicators of compromise (IOCs): unusual logins, PowerShell execution, new scheduled tasks, outbound connections to unknown IPs
- Triage alerts by severity level (P1 Critical → P4 Low)
- Validate true positives vs. false positives
- Document initial findings: What happened? When? Which systems are affected?
- Declare the incident officially and assign an incident ticket number

**Tools:** Elastic SIEM, Wazuh, Suricata alerts, Sigma rules, Windows Event Viewer

**Key Windows Event IDs to Monitor:**

| Event ID | Description | Why It Matters |
|----------|-------------|---------------|
| 4624 | Successful logon | Baseline normal logins; detect anomalies |
| 4625 | Failed logon | Detect brute-force attacks |
| 4688 | Process creation | Detect malicious process launches (PowerShell, cmd) |
| 4698 | Scheduled task created | Detect persistence mechanisms |
| 7045 | New service installed | Detect malware installing as a service |
| 4732 | User added to admin group | Detect privilege escalation |

---

### Phase 3 — Containment

**Goal:** Stop the incident from spreading and limit its impact on the organization.

**Two-Stage Containment:**

**Short-Term Containment (immediate actions):**
- Isolate infected systems from the network (disable NIC or move to quarantine VLAN)
- Block attacker's IP addresses at the firewall level
- Disable compromised user accounts in Active Directory
- Block malicious domains at the DNS level
- Preserve a memory dump and disk image of the infected system BEFORE any changes

**Long-Term Containment (stabilization):**
- Apply emergency patches to prevent re-exploitation
- Change all potentially compromised passwords
- Implement additional monitoring on related systems
- Increase logging verbosity across the environment
- Notify affected business units to pause sensitive operations

**Tools:** CrowdSec, iptables, firewall rules, Active Directory, VirtualBox network isolation

---

### Phase 4 — Eradication

**Goal:** Remove the threat completely from every part of the environment.

**Key Activities:**
- Delete all malware files and dropper scripts
- Remove persistence mechanisms:
  - Windows: Registry Run keys, Scheduled Tasks, Services, Startup folder
  - Linux: Cron jobs, systemd services, .bashrc modifications
- Patch the specific vulnerability that was exploited
- Scan all systems enterprise-wide for the same IOCs to find other infections
- Rebuild or reimage severely compromised systems from scratch
- Verify no backdoors or secondary malware remain using threat hunting queries

**Tools:** Malwarebytes, Velociraptor artifact collection, OpenVAS, manual forensics, EDR full scan

---

### Phase 5 — Recovery

**Goal:** Safely restore systems and services to full normal operation.

**Key Activities:**
- Restore systems from verified, clean backups (confirm backup integrity first)
- Reconnect systems to the network in stages, starting with lowest-risk systems
- Monitor restored systems closely for 24–72 hours for signs of re-infection
- Verify all business services are fully operational
- Confirm the attacker no longer has any access to the environment
- Communicate recovery status to management and affected users
- Gradually lift any emergency restrictions put in place during containment

**Tools:** Backup restoration tools, Wazuh monitoring, network traffic analysis, endpoint scans

---

### Phase 6 — Lessons Learned

**Goal:** Improve security posture and IR capabilities based on what was discovered.

**Key Activities:**
- Conduct a post-incident review meeting within 2 weeks of incident closure
- Document a complete timeline of events (discovery → resolution)
- Identify root cause: How did the attacker get in? What let them stay undetected?
- Answer: What worked well? What failed? What needs to change?
- Update IR playbooks with new techniques and lessons
- Submit IOCs (file hashes, IP addresses, domains) to threat intelligence platforms (MISP, VirusTotal)
- Report findings to management with recommendations and budget requests
- Track remediation items as action items with owners and due dates

**Tools:** Google Docs IR report, MITRE ATT&CK mapping, CISA reporting portal

---

### Summary Table

| Phase | Goal | Duration | Key Output |
|-------|------|----------|-----------|
| 1. Preparation | Be ready before incidents happen | Ongoing | IR Plan, playbooks, trained team |
| 2. Detection | Confirm an incident occurred | Minutes to hours | Incident declaration, scope assessment |
| 3. Containment | Stop the spread | Hours | Isolated systems, blocked attacker |
| 4. Eradication | Remove the threat entirely | Hours to days | Clean environment confirmed |
| 5. Recovery | Restore normal operations | Hours to days | Systems operational, monitoring active |
| 6. Lessons Learned | Improve for next time | 1–2 weeks post-incident | Updated playbooks, management report |

---

## 3. SOC Workflow & Incident Prioritization

### Alert Triage Levels

| Priority | Level | Description | Response Time | Example |
|----------|-------|-------------|--------------|---------|
| **P1** | Critical | Active compromise with ongoing data exfiltration or ransomware executing | Immediate (< 15 min) | Ransomware actively encrypting files on production server |
| **P2** | High | Confirmed intrusion with limited impact; attacker has foothold but limited movement | < 1 hour | Malicious PowerShell detected on employee workstation |
| **P3** | Medium | Suspicious activity requiring investigation; possible breach indicator | < 4 hours | Multiple failed login attempts from foreign IP address |
| **P4** | Low | Informational event; policy violation or very low-risk finding | < 24 hours | User visited blocked website; antivirus detected PUA |

---

### SOC Alert Workflow

```
Alert Triggered (SIEM/IDS/EDR)
           ↓
    L1 Analyst Receives Alert
           ↓
    Initial Triage (True Positive or False Positive?)
           ↓
    False Positive → Document → Close ticket
           ↓
    True Positive → Assign Severity (P1–P4)
           ↓
    P3/P4 → L1 Investigates → Documents findings
    P1/P2 → Escalate to L2/L3 immediately
           ↓
    L2/L3 Deep Investigation + Forensics
           ↓
    Containment → Eradication → Recovery
           ↓
    Lessons Learned → Update playbook
```

---

## 4. IR Playbooks

### Playbook 1 — Phishing Incident Response

| Step | Action | Responsible | Time |
|------|--------|-------------|------|
| **1. Detection** | SIEM alerts on suspicious email OR user reports phishing email to security team | SOC L1 | T+0 |
| **2. Triage** | Analyst reviews email headers, sender reputation, links (VirusTotal, URLScan.io), and attachments | SOC L1 | T+15 min |
| **3. Containment** | Block sender domain at email gateway. Quarantine the email from all mailboxes. Isolate user's machine if attachment was opened. | SOC L2 | T+30 min |
| **4. Investigation** | Check if user clicked link or opened attachment. Review Event ID 4688 for malicious child processes. Check outbound connections from user's machine. | SOC L2/L3 | T+1 hour |
| **5. Eradication** | Remove malware if present. Reset user credentials. Block C2 IPs and domains at firewall. Remove malicious email from all inboxes. | SOC L3 | T+2 hours |
| **6. Recovery** | Restore files if needed. Re-enable user account. Confirm endpoint is clean via EDR full scan. | SOC L2 | T+3 hours |
| **7. Documentation** | Write incident report. Update email gateway rules. Brief affected user on phishing awareness. Submit IOCs to threat intel feeds. | Incident Manager | T+24 hours |

---

### Playbook 2 — Ransomware Incident Response

| Step | Action |
|------|--------|
| **1. Detect** | SIEM alerts on mass file encryption activity (thousands of file write events per second) or EDR flags ransomware process |
| **2. Isolate IMMEDIATELY** | Disconnect infected system from network at the switch level. Do NOT shut down — volatile memory contains evidence |
| **3. Identify Scope** | Determine which systems are affected. Check for lateral movement to file servers, domain controllers |
| **4. Preserve Evidence** | Take memory dump and disk image of infected system before any remediation |
| **5. Notify** | Alert incident manager, CISO, legal team. If customer data affected, notify per regulatory requirements (GDPR 72-hour rule) |
| **6. Identify Ransomware Variant** | Upload sample to ID Ransomware (id-ransomware.malwarehunterteam.com). Check for decryptors |
| **7. Restore from Backup** | Verify backup integrity. Restore from last known clean backup. Test before reconnecting to network |
| **8. Lessons Learned** | Document how attacker got in. Patch the vulnerability. Improve backup strategy |

---

## 5. IR Flowchart

> 📸 See: `Theory/screenshots/ir_flowchart.png`
> Created using draw.io

### Flowchart Description:

```
┌─────────────────┐
│   PREPARATION   │ ← IR Plan, Tools, Training, Playbooks
└────────┬────────┘
         ↓
┌─────────────────┐
│   DETECTION     │ ← SIEM Alert, IDS Alert, User Report
└────────┬────────┘
         ↓
┌─────────────────┐     No (False Positive)
│    ANALYSIS     │ ──────────────────────→ [Document & Close]
└────────┬────────┘
         ↓ Yes (True Positive)
┌─────────────────┐
│  CONTAINMENT    │ ← Isolate, Block, Disable Account
└────────┬────────┘
         ↓
┌─────────────────┐
│  ERADICATION    │ ← Remove Malware, Patch, Scan All Systems
└────────┬────────┘
         ↓
┌─────────────────┐
│    RECOVERY     │ ← Restore Backup, Monitor, Verify
└────────┬────────┘
         ↓
┌─────────────────┐
│ LESSONS LEARNED │ ← Report, Update Playbook, Submit IOCs
└─────────────────┘
```

---

*Document prepared for CyArt Red Teaming Week 2 | Krunal | GTU-ITR Mehsana*
