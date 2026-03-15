# 🌐 Network Segmentation & Design Plan

## 1️⃣ Network Architecture Overview

The SOC lab environment is deployed within an isolated virtual network to safely simulate adversarial activity and monitoring workflows.

All virtual machines are connected to a dedicated **Internal Network** within VirtualBox named:

SOC_LAB

This ensures:

- No exposure to the host’s external network.
- Controlled attack simulation.
- Clean log generation without environmental noise.
- Safe brute-force and scanning activity.

---

## 2️⃣ Network Mode Selection

| Component | Network Mode | Purpose |
|------------|-------------|----------|
| Kali-Attacker | Internal Network (SOC_LAB) | Generates simulated attack traffic |
| Windows Endpoint | Internal Network (SOC_LAB) | Produces authentication & endpoint telemetry |
| Splunk-SIEM | Internal Network (SOC_LAB) | Receives and analyses log data |

Optional future expansion:
- Add secondary NAT adapter to Splunk for updates
- Introduce segmented subnets for advanced simulation

---

## 3️⃣ IP Addressing Scheme

Planned internal subnet:

192.168.100.0/24

| VM Name | Role | Planned Static IP |
|----------|------|------------------|
| Kali-Attacker | Adversary Simulation | 192.168.100.10 |
| Win-Endpoint | Monitored Endpoint | 192.168.100.20 |
| Splunk-SIEM | Log Collection | 192.168.100.30 |

Static addressing is selected to:
- Ensure consistent log correlation
- Simplify SIEM configuration
- Support repeatable attack simulations

---

## 4️⃣ Traffic Flow Boundaries

Planned traffic patterns:

Kali → Endpoint (Attack Simulation) → Splunk (Log Forwarding)

No direct external exposure

This enforces a simplified enterprise-style monitoring model:
- Attack originates internally
- Endpoint generates telemetry
- SIEM centralises visibility

---

## 5️⃣ Isolation Model

The lab intentionally avoids bridged networking during Phase 0 to prevent:

- Accidental brute-force attempts against external networks
- Host exposure risks
- Uncontrolled inbound/outbound traffic

Isolation supports controlled security experimentation.

---

## 6️⃣ Future Network Enhancements (Phase Expansion)

The architecture allows future integration of:

- Network IDS (Suricata)
- Simulated firewall logs
- Separate attacker subnet
- Domain controller environment
- Cloud log ingestion simulation

These components will be introduced in later phases to increase detection complexity.
