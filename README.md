# Azure-SOC
# Building a SOC + Honeynet in Azure (Live Traffic)
  <img width="678" alt="image" src="https://github.com/user-attachments/assets/2c745492-0bee-4f22-a9a5-3cdc89b689b4">

## Introduction

In this project, I build a mini honeynet in Azure and ingest log sources from various resources into a Log Analytics workspace. 
This data was then utilized by Microsoft Sentinel to create attack maps, trigger alerts, and generate incidents. Initially,
I monitored security metrics in the unsecured environment for 24 hours. After applying security controls to strengthen the environment,
I collected metrics for another 24-hour period to assess the improvements. The key metrics are presented below:


- SecurityEvent (Windows Event Logs)
- Syslog (Linux Event Logs)
- SecurityAlert (Log Analytics Alerts Triggered)
- SecurityIncident (Incidents created by Sentinel)
- AzureNetworkAnalytics_CL (Malicious Flows allowed into our honeynet)

## Architecture Before Hardening / Security Controls
<img width="680" alt="image" src="https://github.com/user-attachments/assets/497eeefd-b241-4f99-b1d1-e22642d32546">

## Architecture After Hardening / Security Controls
<img width="680" alt="image" src="https://github.com/user-attachments/assets/706c90e1-4ab0-4d5f-908d-b738a14762db">


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
<img width="857" alt="image" src="https://github.com/user-attachments/assets/f7249f2a-0474-4d1b-ad1c-6365df8586d9">

********
<img width="887" alt="image" src="https://github.com/user-attachments/assets/8a3fdafc-95bf-4e6c-8a7f-04c988055bde">

********
<img width="863" alt="image" src="https://github.com/user-attachments/assets/7867f12a-9577-4cbd-8ffe-710bd29d8684">

*******

## Metrics Before Hardening / Security Controls

The following table shows the metrics we measured in our insecure environment for 24 hours:

Start Time 2024-09-23 14:41:24

Stop Time 2024-09- 24 14:41:24

| Metric                             | Count
| ---------------------------------- | -----
| SecurityEvent                      | 54,119
| Syslog                             | 6,533
| SecurityAlert                      | 15
| SecurityIncident                   | 179
| NSG Inbound Malicious Flows Allowed| 3,502

## Attack Maps Before Hardening / Security Controls

```All map queries actually returned no results due to no instances of malicious activity for the 24 hour period after hardening.```

## Metrics After Hardening / Security Controls

The following table shows the metrics we measured in our environment for another 24 hours, but after we have applied security controls:

Start Time 2024-09-25 15:37: 40

Stop Time	2024-09- 26 15:37: 40

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 8778
| Syslog                   | 25
| SecurityAlert            | 0
| SecurityIncident         | 0
| AzureNetworkAnalytics_CL | 0

## Conclusion

In this project, a honeynet environment was deployed in Microsoft Azure, with log sources integrated into a Log Analytics workspace for centralized monitoring.
Microsoft Sentinel was configured to generate alerts and incidents based on real-time log ingestion. Baseline metrics were initially collected in the unsecured environment, followed by a second measurement after security controls were implemented. The data revealed a substantial decrease in security events and incidents post-implementation, demonstrating the efficacy of the applied controls.

It is important to note that if network resources had been subject to higher utilization by regular users during this period, a greater volume of security events and alerts could have been observed within the 24-hour window following the deployment of security measures.
