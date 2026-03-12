
# 🖥 VM Specifications

## 1️⃣ Objective

This document defines the planned virtual machines for the SOC home lab, including roles and resource allocation. The setup is designed to be realistic for SOC practice while remaining manageable on a personal host system.

---

## VM Plan

| VM Name | Role | Operating System | CPU | RAM | Storage | Network |
|--------|------|------------------|-----|-----|---------|---------|
| Kali-Attacker | Adversary Simulation | Kali Linux | 2vCPU | 4 GB | 40 GB | Internal Network (SOC_LAB) |
| Win-Endpoint | Monitored Endpoint | Windows 10 / 11 | 2vCPU | 4 GB | 60 GB | Internal Network (SOC_LAB) |
| Splunk-SIEM | Log Collection & Analysis | Ubuntu Server + Splunk Enterprise | 2vCPU | 4 GB | 60 GB | Internal Network (SOC_LAB) |

---

## 2️⃣ Notes

- Resource allocation may be adjusted based on host performance.
- Splunk is given higher RAM to ensure stable indexing and search performance.
- All VMs remain on an isolated internal network to support controlled and safe attack simulation.
