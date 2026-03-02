# 🔄 Logging & Data Flow Design Plan

## 1️⃣ Objective

This document defines how telemetry will flow through the SOC lab environment. The architecture is designed to simulate enterprise-style log centralisation and support future detection engineering in Phase 1.

The data flow model prioritises:

- Authentication visibility
- Process-level monitoring
- Network connection telemetry
- Centralised correlation within the SIEM

---

## 2️⃣ High-Level Log Flow

Planned log movement within the lab:

Kali (Attack Simulation) → Windows / Linux Endpoint (Telemetry Generated) → Log Forwarder (Universal Forwarder – Planned) → Splunk Indexer (SIEM) → Search, Correlation, Detection Engineering.


---

## 3️⃣ Log Sources & Flow Mapping

| Log Source | Origin System | Collection Method | Destination |
|------------|---------------|------------------|-------------|
| Linux auth.log | Linux Endpoint | Splunk Forwarder (planned) | Splunk Index |
| Windows Security Log | Windows Endpoint | Splunk Forwarder (planned) | Splunk Index |
| Sysmon Process Logs | Windows Endpoint | Splunk Forwarder (planned) | Splunk Index |
| Sysmon Network Logs | Windows Endpoint | Splunk Forwarder (planned) | Splunk Index |

All logs are forwarded from endpoints to the SIEM using an agent-based model.

---

## 4️⃣ Planned SIEM Ingestion Ports

| Purpose | Port | Direction |
|----------|------|-----------|
| Splunk Web Interface | 8000 | Analyst Access |
| Log Forwarder Receiver | 9997 | Endpoint → SIEM |
| Management Port | 8089 | Internal Control |

Ports are documented in advance to support clean Phase 1 implementation.

---

## 5️⃣ Indexing Strategy (Planned)

Logs will be separated logically within Splunk to support structured searching.

Planned Index Structure:

- index=auth_logs
- index=sysmon_logs
- index=windows_security

Logical separation supports:

- Faster investigations
- Cleaner detection queries
- Controlled search scope

---

## 6️⃣ Data Retention Assumptions

Since this is a home lab environment:

- Log retention will be limited to available disk capacity.
- No formal archival strategy is implemented in Phase 0.
- Retention tuning will be considered during SIEM optimisation in Phase 1.

---

## 7️⃣ Design Rationale

This data flow model mirrors real-world SOC architecture where:

- Endpoints generate high-value telemetry.
- Logs are centralised in a SIEM.
- Detection engineering is built on structured ingestion.
- Analysts perform investigation through indexed search.

The lab prioritises clarity of log flow over infrastructure complexity to maximise learning and detection capability.
