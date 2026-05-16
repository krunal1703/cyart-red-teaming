# 🗺️ Task 5 — Suricata Alerts MITRE ATT&CK Mapping
**CyArt Red Teaming — Week 2 | Task 5 Sub-file**

## Suricata Rule Used
```
drop ip 192.168.1.100 any -> any any (msg:"Block Malicious IP - CyArt Week2"; sid:1000001; rev:1;)
```

## MITRE ATT&CK Alert Mapping

| Alert | Source IP | Destination | Tactic | Technique ID | Technique Name | Notes |
|-------|----------|-------------|--------|-------------|---------------|-------|
| Block Malicious IP | 192.168.1.100 | Any | Command & Control | T1071 | Application Layer Protocol | Outbound traffic from known malicious IP blocked by Suricata drop rule |
| Suspicious HTTP Traffic | 192.168.1.100 | 443/TCP | Command & Control | T1071.001 | Web Protocols | HTTPS C2 beacon traffic pattern detected |
| Port Scan Detected | 192.168.1.100 | Multiple | Discovery | T1046 | Network Service Discovery | Sequential port scanning from malicious IP |

## Why This Matters
Mapping Suricata alerts directly to MITRE ATT&CK techniques allows SOC analysts to:
1. Understand the **attacker's goal** (not just the alert name)
2. **Correlate** multiple alerts into a single attack campaign
3. Build **detection coverage** to ensure all tactics in an attack chain are monitored
4. **Communicate** findings to non-technical stakeholders using standardized language
