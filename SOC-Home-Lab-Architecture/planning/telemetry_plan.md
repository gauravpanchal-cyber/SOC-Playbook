# 📡 Telemetry & Log Source Planning

## 1️⃣ Objective

This document defines the telemetry sources required to support the SOC monitoring use cases established in Phase 0.

The lab prioritises endpoint-based telemetry to maximise detection visibility across authentication, process activity, and network connections.

---

## 2️⃣ Primary Telemetry Sources

| Log Source | System | Purpose | Detection Use Case |
|------------|--------|----------|--------------------|
| auth.log | Linux Endpoint | SSH authentication monitoring | UC-01 |
| Windows Security Log | Windows Endpoint | Logon & privilege monitoring | UC-02, UC-05 |
| Sysmon – Process Creation | Windows Endpoint | Process execution visibility | UC-04 |
| Sysmon – Network Connections | Windows Endpoint | Outbound traffic monitoring | UC-03, UC-06 |

---

## 3️⃣ Critical Log Fields of Interest

### 🔐 Authentication Monitoring

**Linux auth.log**
- source IP
- username
- authentication status (failed/success)
- timestamp

**Windows Security Log**
- Event ID 4624 (Successful logon)
- Event ID 4625 (Failed logon)
- Event ID 4672 (Special privileges assigned)
- Account Name
- Source Network Address

---

### 🖥 Process Execution Visibility (Sysmon Event ID 1)

- Image (process path)
- CommandLine
- ParentImage
- User
- Process ID
- Timestamp

Used for detecting:
- Encoded PowerShell
- Suspicious command-line execution
- Living-off-the-land techniques

---

### 🌐 Network Connection Monitoring (Sysmon Event ID 3)

- Destination IP
- Destination Port
- Source IP
- Process Image
- Protocol

Used for detecting:
- Port scanning activity
- Suspicious outbound communication
- Potential command-and-control behaviour

---

## 4️⃣ Telemetry Coverage Strategy

The lab intentionally focuses on:

- High-signal logs (authentication + Sysmon)
- Behavioural visibility rather than signature-based alerts
- Logs that support investigation workflows, not just detection

This ensures future phases (Detection Engineering & Incident Case Files) are built on rich, structured telemetry.

---

## 5️⃣ Future Telemetry Expansion

Planned expansion in later phases may include:

- Firewall logs
- Suricata IDS alerts
- PowerShell Script Block Logging
- Domain Controller security logs
- Cloud authentication logs

These enhancements will increase detection complexity and enterprise realism.
