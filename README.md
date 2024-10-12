# Azure-SOC
# Building a SOC + Honeynet in Azure (Live Traffic)
![Cloud Honeynet / SOC](https://i.imgur.com/ZWxe03e.jpg)

## Introduction

In this project, I build a mini honeynet in Azure and ingest log sources from various resources into a Log Analytics workspace. This data was then utilized by Microsoft Sentinel to create attack maps, trigger alerts, and generate incidents. Initially, I monitored security metrics in the unsecured environment for 24 hours. After applying security controls to strengthen the environment, I collected metrics for another 24-hour period to assess the improvements. The key metrics are presented below:


- SecurityEvent (Windows Event Logs)
- Syslog (Linux Event Logs)
- SecurityAlert (Log Analytics Alerts Triggered)
- SecurityIncident (Incidents created by Sentinel)
- AzureNetworkAnalytics_CL (Malicious Flows allowed into our honeynet)

## Architecture Before Hardening / Security Controls
![Architecture Diagram](https://i.imgur.com/aBDwnKb.jpg)

## Architecture After Hardening / Security Controls
![Architecture Diagram](https://i.imgur.com/YQNa9Pp.jpg)

The architecture of the mini honeynet in Azure consists of the following components:

- Virtual Network (VNet)
- Network Security Group (NSG)
- Virtual Machines (2 windows, 1 linux)
- Log Analytics Workspace
- Azure Key Vault
- Azure Storage Account
- Microsoft Sentinel

For the "BEFORE" metrics, all resources were initially deployed with full exposure to the internet. The Virtual Machines had both Network Security Groups (NSGs) and built-in firewalls configured to allow unrestricted access, and other resources were deployed with public endpoints accessible over the internet, without utilizing Private Endpoints.

For the "AFTER" metrics, the environment was secured by hardening the NSGs, blocking all traffic except from my admin workstation. Additionally, all other resources were protected by built-in firewalls and secured using Private Endpoints.

## Attack Maps Before Hardening / Security Controls
![NSG Allowed Inbound Malicious Flows](https://i.imgur.com/1qvswSX.png)<br>
![Linux Syslog Auth Failures](https://i.imgur.com/G1YgZt6.png)<br>
![Windows RDP/SMB Auth Failures](https://i.imgur.com/ESr9Dlv.png)<br>

## Metrics Before Hardening / Security Controls

The following table shows the metrics we measured in our insecure environment for 24 hours:
Start Time 2023-03-15 17:04:29
Stop Time 2023-03-16 17:04:29

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 19470
| Syslog                   | 3028
| SecurityAlert            | 10
| SecurityIncident         | 348
| AzureNetworkAnalytics_CL | 843

## Attack Maps Before Hardening / Security Controls

```All map queries actually returned no results due to no instances of malicious activity for the 24 hour period after hardening.```

## Metrics After Hardening / Security Controls

The following table shows the metrics we measured in our environment for another 24 hours, but after we have applied security controls:
Start Time 2023-03-18 15:37
Stop Time	2023-03-19 15:37

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 8778
| Syslog                   | 25
| SecurityAlert            | 0
| SecurityIncident         | 0
| AzureNetworkAnalytics_CL | 0

## Conclusion

In this project, a honeynet environment was deployed in Microsoft Azure, with log sources integrated into a Log Analytics workspace for centralized monitoring. Microsoft Sentinel was configured to generate alerts and incidents based on real-time log ingestion. Baseline metrics were initially collected in the unsecured environment, followed by a second measurement after security controls were implemented. The data revealed a substantial decrease in security events and incidents post-implementation, demonstrating the efficacy of the applied controls.

It is important to note that if network resources had been subject to higher utilization by regular users during this period, a greater volume of security events and alerts could have been observed within the 24-hour window following the deployment of security measures.
