# Building a SOC + Honeynet in Azure (Live Traffic)
![CLOUD HONEYNET SOC](https://github.com/user-attachments/assets/63c400d5-8fa5-4254-8f14-44d49914b976)


## Introduction

In this project, I set up a mini honeynet in Azure, ingesting log data from various resources into a Log Analytics workspace. Microsoft Sentinel was then used to create attack maps, trigger alerts, and generate incidents. I collected security metrics from the insecure environment over a 24-hour period, applied security controls to harden the environment, and measured the metrics again for another 24 hours. The results are shown below, focusing on the following metrics:

- SecurityEvent (Windows Event Logs)
- Syslog (Linux Event Logs)
- SecurityAlert (Log Analytics Alerts Triggered)
- SecurityIncident (Incidents created by Sentinel)
- AzureNetworkAnalytics_CL (Malicious Flows allowed into our honeynet)

## Architecture Before Hardening / Security Controls
![BEFORE HARDENING](https://github.com/user-attachments/assets/83c57250-51de-4a59-8a23-9c9b0782bbe1)


## Architecture After Hardening / Security Controls
![AFTER HARDENING](https://github.com/user-attachments/assets/a7de23e3-b79f-4729-9606-2f3a7f6af243)


The architecture of the mini honeynet in Azure consists of the following components:

- Virtual Network (VNet)
- Network Security Group (NSG)
- Virtual Machines (2 windows, 1 linux)
- Log Analytics Workspace
- Azure Key Vault
- Azure Storage Account
- Microsoft Sentinel

For the "BEFORE" metrics, all resources were initially deployed and exposed to the internet. The Virtual Machines had both their Network Security Groups and built-in firewalls left wide open, and all other resources were deployed with public endpoints visible to the internet.

For the "AFTER" metrics, the Network Security Groups were hardened to block all traffic except from my admin workstation. Additionally, all resources were secured using both their built-in firewalls and Private Endpoints for added protection.

## Attack Maps Before Hardening / Security Controls
NSG TRAFFIC ALLOWED
![NSG ALLOWED BEFORE](https://github.com/user-attachments/assets/03488cbf-960f-401a-8a84-1d75ec07a4b4)<br>
LINUX SSH FAILED LOGINS
![LINUX SSH BEFORE](https://github.com/user-attachments/assets/9a65d3ba-063a-4bb4-9c9b-a5c1c70aa91d)<br>
WINDOWS RDP FAILED LOGINS
![WIN RDP FAIL BEFORE](https://github.com/user-attachments/assets/92ae9e8b-756d-43ad-9ef1-8ddebdc9da26)<br>

## Metrics Before Hardening / Security Controls

The following table shows the metrics I measured in my insecure environment for 24 hours:</br>
Start Time 2023-09-14  18:26:49</br>
Stop Time  2023-09-15  18:26:49

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 35650
| Syslog                   | 15004
| SecurityAlert            | 230
| SecurityIncident         | 226
| AzureNetworkAnalytics_CL | 1281

## Attack Maps After Hardening / Security Controls
WINDOWS RDP FAILED LOGINS
![Screenshot 2024-09-15 150417](https://github.com/user-attachments/assets/c30eccad-be5b-4dad-a8fe-71a63331856e)</br>

```No maps were generated from the remaining logs due to the absence of malicious activity during the 24-hour period.```

## Metrics After Hardening / Security Controls

The following table shows the metrics we measured in our environment for another 24 hours, but after we have applied security controls:</br>
Start Time 2023-09-16  22:45:25</br>
Stop Time	 2023-09-17  22:45:25

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 5991
| Syslog                   | 0
| SecurityAlert            | 0
| SecurityIncident         | 0
| AzureNetworkAnalytics_CL | 0

## Conclusion

In this project, a mini honeynet was built in Microsoft Azure, with log sources integrated into a Log Analytics workspace. Microsoft Sentinel was used to trigger alerts and create incidents based on the ingested logs. Security metrics were first measured in the unsecured environment, then remeasured after implementing security controls. The results showed a significant reduction in security events and incidents following the application of these controls, highlighting their effectiveness.
It is worth noting that if the resources within the network were heavily utilized by regular users, it is likely that more security events and alerts may have been generated within the 24-hour period following the implementation of the security controls.
