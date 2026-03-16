# 📊 Log Sources & Collection Plan

## 1️⃣ Objective

This document defines the primary log sources required to support the SOC monitoring use cases established in Phase 0.

The focus is on high-value endpoint telemetry that enables authentication monitoring, process visibility, and network activity analysis.

---

## 2️⃣ Planned Log Sources

| Log Source | System | Purpose | Detection Use Case |
|------------|--------|----------|--------------------|
| Windows Security Log | Windows Endpoint | Logon & privilege monitoring | UC-02, UC-05 |
| Sysmon – Process Creation | Windows Endpoint | Process execution visibility | UC-04 |
| Sysmon – Network Connections | Windows Endpoint | Outbound traffic monitoring | UC-03, UC-06 |


---

## 3️⃣ Log Forwarding Concept

All endpoint logs will be forwarded to the SIEM (Splunk) using an agent-based model.

Planned Flow:

Endpoint → Splunk Forwarder → Splunk Indexer

Detailed SIEM implementation is handled in [Phase 1](Phases/phase-1-siem-deployment).
