# Building a SOC + Honeynet in Azure (Live Traffic)
![Cloud Honeynet / SOC](https://i.imgur.com/ZWxe03e.jpg)

## Introduction

<p>In this project, I set up a small network security system in Microsoft Azure, a powerful cloud platform. I gathered logs from different sources and stored them in one central place called Log Analytics. This helps Microsoft Sentinel, a smart security tool, to spot potential threats, send alerts, and take action quickly.</p>

<p>I split the project into two main parts. First, I looked at how secure the system was without any extra protection for 24 hours. Then, I added some security measures and watched for another 24 hours to see if they made a difference. Here are the things I looked at:</p>

<ul>
  <li><b>SecurityEvent</b>: These are like a diary of what's happening on Windows computers. They help us see if anything unusual is going on.</li>

  <li><b>Syslog</b>: This is similar but for Linux systems. It helps us understand what's happening on those computers.</li>

  <li><b>SecurityAlert</b>: These are warning signs that something might be wrong. They help us catch problems early.</li>

  <li><b>SecurityIncident</b>: These are real issues that have been spotted and dealt with. They show how well our system responds to threats.</li>

  <li><b>AzureNetworkAnalytics_CL</b>: This tells us about any suspicious activity in the network. It's like our first line of defense against hackers trying to get in.</li>
</ul>

<p>By doing this, we can see how effective our security measures are and make improvements where needed.</p>

## Architecture Before Hardening / Security Controls
![Unhardened Azure Environment'](https://github.com/mpmatusek/CloudSOC/assets/167713753/ee68271d-3e25-41d8-9dbe-03d253a1fa9c)

## Architecture After Hardening / Security Controls
![Hardened Azure Environment](https://github.com/mpmatusek/CloudSOC/assets/167713753/0cf0845c-a2c7-4cdc-8610-6871f4fd5381)

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

## Attack Maps Before Hardening / Security Controls
![NSG Allowed Inbound Malicious Flows](https://i.imgur.com/1qvswSX.png)<br>
![Linux Syslog Auth Failures](https://i.imgur.com/G1YgZt6.png)<br>
![Windows RDP/SMB Auth Failures](https://i.imgur.com/ESr9Dlv.png)<br>

## Metrics Before Hardening / Security Controls

The following table shows the metrics we measured in our insecure environment for 24 hours:
Start Time 2024-05-08 22:34
Stop Time 2024-05-09 22:34

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 18777
| Syslog                   | 15915
| SecurityAlert            | 5
| SecurityIncident         | 223
| AzureNetworkAnalytics_CL | 2227

## Attack Maps Before Hardening / Security Controls

```All map queries actually returned no results due to no instances of malicious activity for the 24 hour period after hardening.```

## Metrics After Hardening / Security Controls

The following table shows the metrics we measured in our environment for another 24 hours, but after we have applied security controls:
Start Time 2024-05-11 09:34
Stop Time	2024-05-12 09:34

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 9781
| Syslog                   | 7
| SecurityAlert            | 0
| SecurityIncident         | 0
| AzureNetworkAnalytics_CL | 0

## Conclusion

In this project, a mini honeynet was constructed in Microsoft Azure and log sources were integrated into a Log Analytics workspace. Microsoft Sentinel was employed to trigger alerts and create incidents based on the ingested logs. Additionally, metrics were measured in the insecure environment before security controls were applied, and then again after implementing security measures. It is noteworthy that the number of security events and incidents were drastically reduced after the security controls were applied, demonstrating their effectiveness.

It is worth noting that if the resources within the network were heavily utilized by regular users, it is likely that more security events and alerts may have been generated within the 24-hour period following the implementation of the security controls.
