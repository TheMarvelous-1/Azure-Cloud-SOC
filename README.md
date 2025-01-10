# Building a SOC + Honeynet in Azure (Live Traffic)
![image](https://github.com/user-attachments/assets/4b0bfa5e-2f8f-4b08-8121-298714382f63)


## Introduction

In this project, I build a mini honeynet in Azure and ingest log sources from various resources into a Log Analytics workspace, which is then used by Microsoft Sentinel to build attack maps, trigger alerts, and create incidents. I measured some security metrics in the insecure environment for 24 hours, apply some security controls to harden the environment, measure metrics for another 24 hours, then show the results below. The metrics we will show are:

- SecurityEvent (Windows Event Logs)
- Syslog (Linux Event Logs)
- SecurityAlert (Log Analytics Alerts Triggered)
- SecurityIncident (Incidents created by Sentinel)
- AzureNetworkAnalytics_CL (Malicious Flows allowed into our honeynet)

## Architecture Before Hardening / Security Controls
![image](https://github.com/user-attachments/assets/048c0f8c-2601-40b1-a287-7fc7ce01e31a)


## Architecture After Hardening / Security Controls
![image](https://github.com/user-attachments/assets/1bf6f5b9-7ed8-4145-a477-1d7e415cf07a)


The architecture of the mini honeynet in Azure consists of the following components:

- Virtual Network (VNet)
- Network Security Group (NSG)
- Virtual Machines (2 windows, 1 linux)
- Log Analytics Workspace
- Azure Key Vault
- Azure Storage Account
- Microsoft Sentinel

For the "BEFORE" metrics, all resources were originally deployed, exposed to the internet. The Virtual Machines had both their Network Security Groups and built-in firewalls wide open, and all other resources are deployed with public endpoints visible to the Internet; aka, no use for Private Endpoints.

For the "AFTER" metrics, Network Security Groups were hardened by blocking ALL traffic with the exception of my admin workstation, and all other resources were protected by their built-in firewalls as well as Private Endpoint

I also took the opportunity to simulate specific attacks via PowerShell scripts or by manually triggering events. The results were observed in Log Analytics Workspace and Sentinel Incident Creation.  

- Linux Brute Force Attempt 
- AAD Brute Force Success 
- Windows Brute Force Success
- Malware Detection (EICAR Test File) 
- Privilege Escalation  


## Attack Maps Before Hardening / Security Controls
![NEW nsg malicious allowed in (Before)](https://github.com/user-attachments/assets/cd700fec-5468-46f5-b0dd-56286bc9d609)
![Syslog ssh auth fail (before)](https://github.com/user-attachments/assets/19960dd6-d064-4bfb-bff0-bb034b83a8fe)
![windows-rdp-auth-fail (before)](https://github.com/user-attachments/assets/ab4db118-4a41-41f7-83e8-3fc379f88d7f)


## Metrics Before Hardening / Security Controls

The following table shows the metrics we measured in our insecure environment for 24 hours:
Start Time 1/5/2025 10:31:57 AM
Stop Time 1/6/2025 10:31:57 AM

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 11546
| Syslog                   | 2366
| SecurityAlert            | 6
| SecurityIncident         | 170
| AzureNetworkAnalytics_CL | 3342

## Attack Maps Before Hardening / Security Controls

```All map queries actually returned no results due to no instances of malicious activity for the 24 hour period after hardening.```

## Metrics After Hardening / Security Controls

The following table shows the metrics we measured in our environment for another 24 hours, but after we have applied security controls:
Start Time 1/8/2025 1:17:39 PM
Stop Time	1/9/2025 1:17:39 PM

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 4037
| Syslog                   | 4
| SecurityAlert            | 0
| SecurityIncident         | 0
| AzureNetworkAnalytics_CL | 0

![image](https://github.com/user-attachments/assets/aa3606e4-8c4b-4c55-81f5-3397b08e786a)


## Conclusion

In this project, a mini honeynet was constructed in Microsoft Azure and log sources were integrated into a Log Analytics workspace. Microsoft Sentinel was employed to trigger alerts and create incidents based on the ingested logs. Additionally, metrics were measured in the insecure environment before security controls were applied, and then again after implementing security measures. It is noteworthy that the number of security events and incidents were drastically reduced after the security controls were applied, demonstrating their effectiveness.

It is worth noting that if the resources within the network were heavily utilized by regular users, it is likely that more security events and alerts may have been generated within the 24-hour period following the implementation of the security controls.
