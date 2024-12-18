# Building a SOC + Honeynet in Azure with Live Traffic
![322930551-ca6ef315-dcbf-4176-b90f-c46a6bbf0459](https://github.com/UdoWilliams/Azure-Honeynet-Soc/assets/148285991/aa1cab61-4e55-47b5-850a-8a723880202b)

# Introduction
In this project, I set up a small honeynet within Azure to collect logs from various sources, which were then centralized in a Log Analytics Workspace. Using Microsoft Sentinel, I analyzed the data to create attack maps, generate alerts, and trigger incident responses. Over a 4-hour period, I evaluated security metrics in an unsecured environment, implemented measures to enhance security, and conducted a second 4-hour assessment to measure the improvements. The gathered metrics include:

- SecurityEvent (Windows Event Logs)
- Syslog (Linux Event Logs)
- SecurityAlert (Log Analytics Alerts Triggered)
- SecurityIncident (Incidents created by Sentinel)
- AzureNetworkAnalytics_CL (Malicious Flows allowed into our honeynet)

# Objectives
The main objective of this project was to deploy intentionally vulnerable virtual machines within the Azure infrastructure to attract and analyze cyberattacks. This effort deepened my understanding of attacker tactics and techniques while showcasing my ability to quickly and effectively remediate identified vulnerabilities.

# Technologies, Regulations, and Azure Components Employed:

- Azure Virtual Network (VNet)
- Azure Network Security Group (NSG)
- Virtual Machines (2x Windows, 1x Linux)
- Log Analytics Workspace with Kusto Query Language (KQL) Queries
- Azure Key Vault for Secure Secrets Management
- Azure Storage Account for Data Storage
- Microsoft Sentinel for Security Information and Event Management (SIEM)
- Microsoft Defender for Cloud to Protect Cloud Resources
- Windows Remote Desktop for Remote Access
- Command Line Interface (CLI) for System Management
- PowerShell for Automation and Configuration Management
- NIST SP 800-53 Revision 5 for Security Controls
- NIST SP 800-61 Revision 2 for Incident Handling Guidance

# Methodology
-Setting up the honeynet: I began by deploying several intentionally vulnerable virtual machines in Azure to replicate an insecure environment.

- Monitoring and analysis: I configured Azure to aggregate log data from various resources into a Log Analytics Workspace. Leveraging Microsoft Sentinel, I created attack maps, generated alerts, and managed incidents based on the collected data.

- Security metrics assessment: I monitored the insecure environment over a 4-hour period, collecting key security metrics to establish a baseline for comparison after implementing remediation measures.

- Incident response and remediation: After identifying vulnerabilities and addressing incidents, I began the hardening process by applying security best practices and following Azure-specific recommendations.

- Post-remediation evaluation: I performed a second 4-hour observation of the environment to reassess security metrics and compare the results to the initial baseline.

# Before Hardening / Security Controls Architecture

In![before hardeing](https://github.com/UdoWilliams/Azure-Honeynet-Soc/assets/148285991/88eba60c-82e6-491f-a945-3fb37b3ff44a)
 Azure, I set up a resource group and deployed a Windows virtual machine alongside a Linux virtual machine. To introduce vulnerabilities, I disabled the Windows Firewall on the Windows machine through Remote Desktop Protocol (RDP). I also configured the network security group (NSG) for the Windows machine to accept all inbound traffic using the wildcard *. Similarly, I adjusted the NSG for the Linux machine to allow unrestricted inbound traffic with the wildcard *.`.

During the "BEFORE" phase of the project, all resources were deliberately configured with public exposure to the internet to create an insecure environment and attract potential cyber attackers. The Virtual Machines had their Network Security Groups (NSGs) and internal firewalls configured to allow unrestricted access from any source. Similarly, other resources, including storage accounts and databases, were deployed with public endpoints accessible from the internet, without leveraging Private Endpoints for enhanced security..

# Architecture After Hardening / Security Controls

![after hardening](https://github.com/UdoWilliams/Azure-Honeynet-Soc/assets/148285991/c65a1f49-23d1-440f-9e8a-d145a0dda29e)

- To strengthen our architecture, I carried out a thorough set of hardening measures and implemented strong security controls to improve the overall security posture of the environment. These improvements included:

- Network Security Groups (NSGs): I configured the NSGs to allow inbound and outbound traffic exclusively from my specified public IP address. This limitation ensured that only authorized traffic could access the virtual machines.
  
- Built-in Firewalls: I configured the firewalls on each VM to block unauthorized access, customizing the rules to reduce potential attack surfaces and protect resources.

- Private Endpoints: I substituted public endpoints with Private Endpoints for Azure resources such as storage accounts and databases. This change restricted access to the virtual network, protecting sensitive resources from external threats and unauthorized access.

 By analyzing security metrics before and after applying these hardening measures and security controls, I effectively demonstrated the impact of each step in improving the overall security posture of the Azure environment.

# Attack Maps Before Hardening / Security Controls

The attack maps used pre-built KQL .JSON map files sourced from a workbook. These files organized attack patterns and related data, enabling visualizations that clearly illustrated cyber threats and their impact on the system.

This attack map underscores the dangers of leaving the Network Security Group (NSG) open, which allows unrestricted malicious traffic. It highlights the importance of implementing strong security practices, such as enforcing strict NSG rules, to prevent unauthorized access and reduce potential threats.

![malicious allowed](https://github.com/UdoWilliams/Azure-Honeynet-Soc/assets/148285991/4556bbdb-5950-4559-a7b5-2344349c4a9c)
![linux fail](https://github.com/UdoWilliams/Azure-Honeynet-Soc/assets/148285991/9150c135-b95d-4f8d-a0c6-6ccbb0870c0d)
![windows before](https://github.com/UdoWilliams/Azure-Honeynet-Soc/assets/148285991/aea3adca-c623-4b67-aaf2-f0b5d730ffbc)

# Metrics Before Hardening / Security Controls
The following table shows the metrics we measured in our insecure environment for 4 hours plus:

| Start Time 2024-06-24  9:00:47
| Stop Time 2024-06-24 13:00:48

| Metric                    | Count |
|---------------------------|-------|
| SecurityEvent             | 1988  |
| Syslog                    | 11    |
| SecurityAlert             | 22    |
| SecurityIncident          | 23    |
| AzureNetworkAnalytics_CL  | 2190  |

# Attack Maps After Hardening / Security Controls
All map queries actually returned no results due to no instances of malicious activity for the 4 hour period after hardening.

Metrics After Hardening / Security Controls
The following table shows the metrics we measured in our environment for another 4 hours, but after we have applied security controls:

| Start Time 2024-06-24 13:05:28
| Stop Time 2024-06-24 17:50:28

| Metric                    | Count |
|---------------------------|-------|
| SecurityEvent             | 3894  |
| Syslog                    | 6     |
| SecurityAlert             | 0     |
| SecurityIncident          | 0     |
| AzureNetworkAnalytics_CL  | 0     |

![Screenshot 2024-06-24 130215](https://github.com/UdoWilliams/Azure-Honeynet-Soc/assets/148285991/1d15c9b1-b81f-431a-af12-0eede8d07312)

# Summary
In this project, I established a small honeynet within Microsoft Azure, routing logs to a Log Analytics Workspace for in-depth analysis. Microsoft Sentinel was essential in generating alerts and managing incidents based on the log data. I began by assessing metrics in an insecure environment, then quickly implemented security controls. Follow-up evaluations showed a marked reduction in security events and incidents, highlighting the effectiveness of these measures.

It's worth mentioning that to replicate a typical workday scenario, I accelerated the security remediation of resources within an 8-hour period, instead of leaving the virtual machines exposed for 24 hours. This approach enabled a realistic assessment of the security measures' effectiveness within a limited timeframe.

# KQL Queries

| Metric                                  | Query                                                                                                                                                       |
|-----------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Start/Stop Time                         | range x from 1 to 1 step 1 <br> \| project StartTime = ago(8h), StopTime = now()                                                                            |
| Security Events (Windows VMs)           | SecurityEvent <br> \| where TimeGenerated >= ago(8h) <br> \| count                                                                                           |
| Syslog (Linux VMs)                      | Syslog <br> \| where TimeGenerated >= ago(8h) <br> \| count                                                                                                  |
| SecurityAlert (Microsoft Defender)      | Security Alert <br> \| where DisplayName !startswith "CUSTOM" and DisplayName !startswith "TEST" <br> \| where TimeGenerated >= ago(8h) <br> \| count       |
| Security Incident (Sentinel Incidents)  | SecurityIncident <br> \| where TimeGenerated >= ago(8h) <br> \| count                                                                                        |
| NSG Inbound Malicious Flows Allowed     | AzureNetworkAnalytics_CL <br> \| where FlowType_s == "MaliciousFlow" and AllowedInFlows_d > 0 <br> \| where TimeGenerated >= ago(8h) <br> \| count           |

In conclusion, I established a streamlined honeynet using Microsoft Azure's robust cloud infrastructure. Microsoft Sentinel was employed to trigger alerts and generate incidents based on ingested logs from designated watch lists. Baseline metrics were recorded in the unprotected environment prior to implementing any security controls. Subsequently, a series of security measures were implemented to strengthen the network against potential threats. 

Comparing pre- and post-implementation metrics revealed a significant decrease in security events and incidents, underscoring the effectiveness of the implemented security controls. It's worth noting that had the network resources been actively used by regular users, it could have resulted in a higher volume of security events and alerts within the 4-hour post-implementation timeframe.





  
