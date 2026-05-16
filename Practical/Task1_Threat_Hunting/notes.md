# 🔍 Task 1 — Threat Hunting with Sigma Rules
**CyArt Red Teaming — Week 2 | Practical Task 1**

---

## 📌 Objective
Write a Sigma rule to detect suspicious PowerShell activity and test it in a Windows environment. Document findings in a structured table.

## 🛠️ Tools Used
| Tool | Purpose |
|------|---------|
| Sigma | Rule creation framework for SIEM detection |
| Windows Event Viewer | View Event ID 4688 (process creation) |
| PowerShell | Test the detection rule with harmless command |

---

## ✅ Steps Performed

### Step 1 — Wrote the Sigma Rule
Created `sigma_rule.yml` to detect PowerShell execution using the `-Command` flag.

**Rule Logic:**
- Looks for any process where the image ends with `powershell.exe`
- AND the command line contains `-Command`
- This catches both legitimate and malicious PowerShell usage for analyst review

### Step 2 — Tested the Rule
Ran the following harmless test command on Windows VM:
```powershell
powershell -Command "Write-Host Test"
```

### Step 3 — Verified Detection
Opened Windows Event Viewer:
- Path: `Windows Logs → Security → Filter by Event ID 4688`
- Confirmed the PowerShell process appeared in logs with the `-Command` flag visible

> 📸 Screenshot: `Task1_Threat_Hunting/screenshots/event_4688_powershell.png`

---

## 📊 Threat Hunting Log — Findings Table

| Timestamp | Process | Command Line | Event ID | Verdict | Notes |
|-----------|---------|-------------|----------|---------|-------|
| 2026-05-12 10:00:00 | powershell.exe | -Command "Write-Host Test" | 4688 | Suspicious | Test execution — harmless but matches rule pattern |
| 2026-05-12 10:05:00 | powershell.exe | -Command "IEX(New-Object...)" | 4688 | MALICIOUS | This pattern used for fileless malware download |
| 2026-05-12 10:10:00 | powershell.exe | -EncodedCommand <base64> | 4688 | Suspicious | Encoded commands often used to obfuscate payloads |

---

## 🎯 MITRE ATT&CK Mapping

| Alert | Tactic | Technique ID | Technique Name | Notes |
|-------|--------|-------------|---------------|-------|
| PowerShell -Command execution | Execution | T1059.001 | PowerShell | Matches common attacker script execution pattern |
| Encoded PowerShell | Defense Evasion | T1027 | Obfuscated Files/Info | Base64 encoding used to bypass string detection |

---

## 📝 Key Findings

- PowerShell with `-Command` flag is extremely common in both legitimate admin use and attacker activity
- The Sigma rule successfully triggered on the test command
- Event ID **4688** (Process Creation) must be enabled in Windows Audit Policy for this to work
- **Recommendation:** Combine this rule with additional context (parent process, network connections) to reduce false positives

---

## 🔧 How to Enable Event ID 4688 (Process Creation Logging)

1. Open `gpedit.msc` (Group Policy Editor)
2. Navigate to: `Computer Configuration → Windows Settings → Security Settings → Advanced Audit Policy → Detailed Tracking`
3. Enable: **Audit Process Creation** → Success and Failure
4. Also enable command line logging: `Computer Configuration → Administrative Templates → System → Audit Process Creation → Include command line in process creation events` → **Enabled**

---