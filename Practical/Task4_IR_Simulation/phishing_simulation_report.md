# 🎭 Task 4 — Incident Response Simulation
**CyArt Red Teaming — Week 2 | Practical Task 4**

---

## 📌 Objective
Simulate a phishing attack using MITRE Caldera and collect forensic artifacts using Velociraptor.

## 🛠️ Tools Used
| Tool | Purpose |
|------|---------|
| MITRE Caldera | Adversary emulation platform — simulates attacker behavior |
| Velociraptor | Endpoint forensics — collects artifacts from compromised systems |

---

## ✅ Steps Performed

### Step 1 — Install and Start Caldera
```bash
git clone https://github.com/mitre/caldera.git --recursive
cd caldera
pip3 install -r requirements.txt
python3 server.py --insecure
# Open: http://localhost:8888
# Credentials: admin / admin
```

### Step 2 — Deploy Phishing Simulation
- Went to Operations → New Operation
- Selected Adversary: "Phishing" profile
- Deployed agent on Windows VM target
- Started operation and documented attack steps

### Step 3 — Velociraptor Artifact Collection
```bash
# On Windows VM — run Velociraptor agent
velociraptor.exe gui

# Collected artifacts:
SELECT * FROM processes;        -- All running processes
SELECT * FROM netstat;          -- All network connections
```

> 📸 Screenshot: `Task4_IR_Simulation/screenshots/caldera_operation.png`
> 📸 Screenshot: `Task4_IR_Simulation/screenshots/velociraptor_artifacts.png`

---

## 📊 Phishing Attack Simulation — 100-Word Summary

The MITRE Caldera phishing simulation deployed a mock payload on the target Windows VM. The attack began with an initial access phase simulating a user opening a malicious email attachment. Caldera's agent executed on the target, establishing a callback to the Caldera server. From this foothold, the simulation performed discovery operations: listing running processes, enumerating network connections, and identifying user accounts. Velociraptor collected forensic artifacts including full process lists and netstat outputs which revealed the malicious connection. The simulation demonstrated how a real attacker progresses from phishing delivery through execution to internal reconnaissance within minutes of initial access.

---

## 📊 Artifacts Collected — IOC Table

| Artifact Type | Finding | Verdict | MITRE Technique |
|--------------|---------|---------|----------------|
| Process List | Suspicious process with unusual parent (Word spawning cmd.exe) | MALICIOUS | T1059 — Command Execution |
| Netstat | Outbound connection to unknown external IP on port 443 | SUSPICIOUS | T1071 — C2 over HTTPS |
| File System | New executable dropped in %TEMP% folder | MALICIOUS | T1105 — Ingress Tool Transfer |
| Registry | New Run key added for persistence | MALICIOUS | T1547 — Boot Autostart |

---
