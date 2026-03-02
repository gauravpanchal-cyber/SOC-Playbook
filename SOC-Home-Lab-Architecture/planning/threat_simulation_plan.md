
# 🎯 Threat Simulation Plan

## 1️⃣ Objective

This document defines the planned adversarial simulations used to generate telemetry for detection and investigation practice.

Each simulation aligns with defined SOC monitoring use cases.

---

## 2️⃣Planned Attack Scenarios

| Attack Scenario | Tool Used | Target | Expected Log Artifact | Related Use Case |
|----------------|-----------|--------|----------------------|------------------|
| SSH Brute Force | Hydra | Windows Endpoint | Multiple failed login entries in auth.log | UC-01 |
| RDP Brute Force | Hydra / Manual Attempts | Windows Endpoint | Event ID 4625 (Failed Logon) | UC-02 |
| Port Scanning | Nmap | Endpoint | Multiple connection attempts / network logs | UC-03 |
| Suspicious PowerShell Execution | Manual Execution | Windows Endpoint | Sysmon Event ID 1 (Process Creation) | UC-04 |
| Privilege Escalation Attempt | RunAs / Local Admin Actions | Windows Endpoint | Event ID 4672 | UC-05 |
| Suspicious Outbound Connection | Netcat / Test Connection | Endpoint | Sysmon Event ID 3 | UC-06 |

---

## 3️⃣ Design Philosophy

Attacks are intentionally controlled and repeatable to allow:

- Detection rule development
- Log correlation testing
- Incident investigation walkthrough creation

Actual execution and detection engineering are implemented in Phase 1 and Phase 2.
