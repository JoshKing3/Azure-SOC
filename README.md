# Azure-SOC

# Azure SOC & Honeynet (with live traffic)
![Azure SOC vpd](https://github.com/JoshKing3/Azure-SOC/assets/165087957/098ed1c5-ef54-4fe4-8afd-0d5e94001842)


## Introduction

In this project, I configured a cloud-based SOC in Azure, using Microsoft Sentinel as our SIEM. Set up logging, alerts, and practiced incident response against live malicious traffic.

This small honeynet in Azure ingests log sources from different resources into a Log Analytics workspace, which Microsoft Sentinel then uses to create attack maps, trigger alerts, and create incidents. I documented security metrics in the insecure environment for 24 hours, hardened the environment by applying security controls, recorded the metrics for another 24 hours, and then produced the results below. 


Resources, Regulations, and Technologies Used:

- Azure Virtual Network (VNet)
- Azure Network Security Group (NSG)
- Virtual Machines (2x Windows 10 Pro, 1x Linux Server)
- Azure Key Vault
- Azure Storage Account
- Microsoft Sentinel (SIEM)
- Microsoft Defender for Cloud
- Log Analytics Workspace with Kusto Query Language (KQL) Queries
- NIST SP 800-53 Revision 4

The metrics we will show are:

- SecurityEvent (Windows Event Logs)
- Syslog (Linux Event Logs)
- SecurityAlert (Log Analytics Alerts Triggered)
- SecurityIncident (Incidents created by Sentinel)
- AzureNetworkAnalytics_CL (Malicious Flows allowed into our honeynet)

## Architecture Before Hardening / Security Controls
![Architecture Before Hardening _ Security Controls](https://github.com/JoshKing3/Azure-SOC/assets/165087957/4aedd8c0-96ec-48cd-8c5b-c582ec354075)

For the "BEFORE" metrics, all resources were initially deployed and exposed to the internet. The Virtual Machines had their Network Security Groups(NSG) and built-in firewalls configured with unrestricted access, it was wide open to the Internet, while all other resources were deployed with public endpoints accessible from the Internet, rendering Private Endpoints unnecessary.

## Architecture After Hardening / Security Controls
![Architecture After Hardening _ Security Controls](https://github.com/JoshKing3/Azure-SOC/assets/165087957/014c864c-44cc-4482-a5b4-8334d7b5571f)

For the "AFTER" metrics, I hardened the Network Security Groups by blocking ALL traffic except for my admin workstation, and all other resources were safe due to their built-in firewalls and Private Endpoint configured. 

## Analytics Rule in Sentinel
<img width="887" alt="CustomRules" src="https://github.com/JoshKing3/Azure-SOC/assets/165087957/523bf6a0-9325-4a91-98c6-cb26813d06c0">



## Attack Maps Before Hardening / Security Controls
<img width="673" alt="nsg-malicious-allowed-in" src="https://github.com/JoshKing3/Azure-SOC/assets/165087957/866518a5-2917-4c05-95fd-bd2831778765">
<img width="650" alt="image" src="https://github.com/JoshKing3/Azure-SOC/assets/165087957/700ddaf9-7b16-4e8c-be04-19955d67f3ff">
<img width="609" alt="windows-rdp-auth-fail" src="https://github.com/JoshKing3/Azure-SOC/assets/165087957/9bf97a8b-ead1-46a8-8750-fc73c4154640">


## Metrics Before Hardening / Security Controls

This table shows the metrics that were measured in our insecure environment for 24 hours:

|Start Time 2024-04-16 17:29:51
|Stop Time 2024-04-17 17:29:51

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 20180
| Syslog                   | 280
| SecurityAlert            | 3
| SecurityIncident         | 77
| AzureNetworkAnalytics_CL | 863

## Attack Maps After Hardening / Implementing Security Controls

```All map queries returned no results because there were no instances of malicious activity detected during the 24-hour period following the hardening process.```

<img width="212" alt="No_results" src="https://github.com/JoshKing3/Azure-SOC/assets/165087957/9dc30192-d2e7-4e4e-b550-8e3aa0834643">


## Metrics After Hardening / Security Controls

This table shows the metrics measured in our environment for another 24 hours, but after we have applied security controls:

|Start Time 2024-04-18 20:02:51
|Stop Time	2024-04-19 20:02:51

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 3487
| Syslog                   | 13
| SecurityAlert            | 0
| SecurityIncident         | 0
| AzureNetworkAnalytics_CL | 0

## Results 

| Metric                   | Change after securing the environment
| ------------------------ | -----
| SecurityEvent            | -82.72%
| Syslog                   | -95.36%
| SecurityAlert            | -100.00%
| SecurityIncident         | -100.00%
| AzureNetworkAnalytics_CL | -100.00%

## Conclusion

In this project, a small honeynet was set up in Azure, and log sources were integrated into a Log Analytics workspace. Microsoft Sentinel was utilized to generate alerts and incidents based on the ingested logs. Furthermore, metrics were assessed in the insecure environment before implementing security controls and compared to measurements taken after the security measures were implemented. Notably, there was a significant reduction in the number of security events and incidents after security controls were implemented, showcasing their effectiveness.

It's important to consider that if the network resources were heavily used by regular users, there might have been more security events and alerts generated within the 24-hour period following the implementation of the security controls.
