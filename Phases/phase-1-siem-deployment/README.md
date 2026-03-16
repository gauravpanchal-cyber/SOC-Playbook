# Phase 1 — SIEM Deployment (Splunk)

## 1. Objective

The objective of this phase is to deploy a **functional SIEM platform using Splunk Enterprise** and configure it to collect logs from a Windows endpoint.

This phase establishes the core **log collection pipeline** required for threat detection and SOC monitoring.

Key goals:

- Install **Splunk Enterprise SIEM**
- Configure **log receiving**
- Deploy **Splunk Universal Forwarder**
- Collect **Windows Event Logs**
- Deploy **Sysmon for enhanced telemetry**
- Validate **log ingestion in Splunk**

> Lab architecture and infrastructure details are documented in **[Phase 0](../phase-0-architecture/README.md)**.

---

# 2. Prerequisites

The following infrastructure is defined in Phase 0.

| Component | Description |
|----------|-------------|
| SIEM Server | Ubuntu VM running Splunk Enterprise |
| Windows Endpoint | Windows VM acting as monitored host |
| Internal Network | 192.168.100.0/24 |
| Splunk Server IP | **192.168.100.30** |

### Verify VM Network Connectivity

Before installing Splunk, confirm that all VMs can communicate with each other on the internal network.
Example command:

```bash
ping 192.168.100.30
```

### Network Configuration



Successful responses confirm that the internal lab network is functioning correctly.

The SIEM VM uses two network adapters:

| Adapter | Purpose |
|-------|--------|
| Adapter 1 | Internal lab network communication |
| Adapter 2 (NAT) | Internet access for downloading packages |

This configuration allows the VM to:

- Communicate with other lab machines
- Access the internet to install Splunk and dependencies

---

# 3. Splunk Installation

## Download Splunk Enterprise

Splunk Enterprise requires a Splunk account to download.

Steps:

1. Go to Official [Splunk](https://www.splunk.com/) website and create a Splunk account
2. Navigate to **Trials & Downloads**
3. Select **Splunk Enterprise**
4. Under Linux downloads choose the **.tgz package**

Download page:

https://www.splunk.com/en_us/download/splunk-enterprise.html

On the SIEM Ubuntu VM, install required utilities:

```bash
sudo apt install wget curl net-tools -y
```
Download Splunk using the wget command provided on the Splunk website after logging in.

Example:
```
wget -O splunk-10.2.1-c892b66d163d-linux-amd64.tgz "https://download.splunk.com/products/splunk/releases/10.2.1/linux/splunk-10.2.1-c892b66d163d-linux-amd64.tgz"
```
Extract the downloaded package:
```
tar -xvzf splunk-10.2.1-c892b66d163d-linux-amd64.tgz
```
Move Splunk to the /opt directory:
```
sudo mv splunk /opt/
```

---

## Start Splunk Service

Start Splunk using:

```bash
sudo /opt/splunk/bin/splunk start --run-as-root
```
> Note: Running without --run-as-root produced an error indicating the command was deprecated.
Adding the `--run-as-root` flag allows Splunk to start successfully.

During the first launch:

* License agreement must be accepted
* Administrator credentials must be created

Example prompt:

```
Create administrator username
Create administrator password
```

These credentials are required to access the **Splunk Web Dashboard**.

---

## Access Splunk Web Interface

Splunk Web runs on port **8000**.

Access the dashboard using:

```
http://192.168.100.30:8000
```

Login using the administrator credentials created during installation.

---

# 4. Splunk Configuration

## Enable Data Receiving

To allow endpoints to send logs to Splunk, a receiving port must be configured.

Navigation path:

```
Settings → Forwarding and Receiving → Configure Receiving → New Receiving Port
```

Configured port:

```
9997
```

This port allows **Splunk Universal Forwarders** to send logs to the SIEM.

---

## Index Creation

Three indexes are created to organize log ingestion.

| Index Name | Purpose |
|-----------|--------|
| windows | Windows Event Logs |
| sysmon | Sysmon telemetry |
| system | System related logs |

Using a dedicated index improves:

* Log organization
* Search efficiency
* Detection rule management

---

# Splunk Add-ons Installed

Several Splunk add-ons are installed to enhance log parsing and detection capabilities.

| Add-on | Purpose |
|------|------|
| Splunk Add-on for Windows | Parses Windows Event Logs |
| Splunk Security Essentials | Provides detection use cases and MITRE ATT&CK mapping |
| Sysmon Add-on (if installed) | Improves Sysmon field extraction |
---

# 5. Forwarder Setup (Windows Endpoint)

## Install Splunk Universal Forwarder

Download the Windows Universal Forwarder:

[https://www.splunk.com/en_us/download/universal-forwarder.html](https://www.splunk.com/en_us/download/universal-forwarder.html)

Install the forwarder on the **Windows endpoint VM**.

During installation specify:

```
Receiving Indexer
192.168.100.30:9997
```

---

## Configure Windows Log Monitoring

Windows logs are monitored using the **inputs.conf** file.

Location:

```
C:\Program Files\SplunkUniversalForwarder\etc\system\local\inputs.conf
```

Example configuration:

```
[WinEventLog://Application]
index = windows
disabled = false

[WinEventLog://Security]
index = windows
disabled = false

[WinEventLog://System]
index = windows
disabled = false

[WinEventLog://Microsoft-Windows-Sysmon/Operational]
index = sysmon
disabled = false
renderXml = true
```

This configuration sends Windows Event Logs to the **windows** and **sysmon** indexes in Splunk. This inputs.conf file is available to download from [inputs.conf](Phases/phase-1-siem-deployment/configs/inputs.conf) file.

---

# 6. Sysmon Installation

Sysmon provides enhanced endpoint telemetry including:

* Process creation
* Network connections
* File modifications

---

## Download Sysmon

Official source:

[https://learn.microsoft.com/en-us/sysinternals/downloads/sysmon](https://learn.microsoft.com/en-us/sysinternals/downloads/sysmon)

---

## Download Sysmon Configuration

The **SwiftOnSecurity Sysmon configuration** is used.

[https://github.com/SwiftOnSecurity/sysmon-config](https://github.com/SwiftOnSecurity/sysmon-config)

---

## Install Sysmon via PowerShell

Go to the folder where Sysmon is downloaded, open **PowerShell as Administrator**, and run:

```powershell
sysmon64.exe -accepteula -i sysmonconfig.xml
```

---

## Verify Sysmon Service

Confirm that Sysmon is running.

```powershell
Get-Service sysmon
```

Or verify via run:

```
services.msc
```

---

## Verify Sysmon Events

Sysmon logs appear in Event Viewer.

```
Event Viewer > Applications and Services Logs > Microsoft > Windows > Sysmon > Operational
```

---

# 7. Log Validation

To verify log ingestion in Splunk:

1. Open the Splunk dashboard  
2. Navigate to **Search & Reporting**
3. Use **SPL (Search Processing Language)** queries to analyze logs

---

## Verify Windows Event Logs

Search query:

```
index=windows
```

---

## Verify Sysmon Events

Example query:

```
index=sysmon EventCode=1
```

EventCode 1 represents **Process Creation Events**.

---

## Example Security Event

A failed login event can be generated on Windows.

Relevant Event ID:

```
4625
```
> reference to screenshot is pending
Search query:

```
EventCode=4625
```
> reference to screenshot is pending

This confirms that authentication events are captured by the SIEM.

---


# 8. Outcome

At the end of Phase 1:

* Splunk Enterprise SIEM is successfully deployed
* Windows endpoint logs are forwarded to Splunk
* Sysmon telemetry is collected
* Logs are searchable within Splunk

This phase establishes the **centralized log collection pipeline** required for security monitoring.

---

# Evidence

Screenshots documenting the process are stored in:

```
./evidence/
```

Examples include:

* Splunk installation
* Splunk web dashboard
* Data receiving configuration
* Universal forwarder setup
* Sysmon installation
* Windows log ingestion
* Sysmon search queries

---

# Next Phase

Phase 2 will focus on **Threat Simulation and Detection Engineering**, where attack techniques will be executed and detected using the telemetry collected in this phase.





