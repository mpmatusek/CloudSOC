# Building a SOC + Honeynet in Azure (Live Traffic)
![image](https://github.com/mpmatusek/CloudSOC/assets/167713753/7573fb2d-89b8-43e9-bdc5-ee57fb1929f6)

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

<h3>Components:</h3>
<ul>
  <li>Virtual Network (VNet)</li>
  <li>Network Security Group (NSG)</li>
  <li>Virtual Machines (2 Windows, 1 Linux)</li>
  <li>Log Analytics Workspace</li>
  <li>Azure Key Vault</li>
  <li>Azure Storage Account</li>
  <li>Microsoft Sentinel</li>
</ul>

<h3>Before Metrics:</h3>
<p>
For the "BEFORE" metrics, all resources were originally deployed, exposed to the internet. The Virtual Machines had both their Network Security Groups and built-in firewalls wide open, and all other resources are deployed with public endpoints visible to the Internet; aka, no use for Private Endpoints.
</p>

<h3>After Metrics:</h3>
<p>
For the "AFTER" metrics, Network Security Groups were hardened by blocking ALL traffic with the exception of my admin workstation, and all other resources were protected by their built-in firewalls as well as Private Endpoint.
</p>

## Attack Maps Before Hardening / Security Controls
<h3>NSG Malicious Traffic - World Map</h3>
(https://github.com/mpmatusek/CloudSOC/assets/167713753/af9fbc7e-37e6-4ca4-b8f2-c174e5205bf2)

<h3>Windows Malicious Traffic - World Map</h3>
(https://github.com/mpmatusek/CloudSOC/assets/167713753/bfcf277b-97ba-44a0-a8df-f5d1ce81fea7)

<h3>Linux Malicious Traffic - World Map</h3>
(https://github.com/mpmatusek/CloudSOC/assets/167713753/ea52e63b-8238-4b02-9dad-fdc36eb81ec6)

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

In this project, we established a miniature honeynet within Microsoft Azure, integrating various log sources into a Log Analytics workspace. Leveraging Microsoft Sentinel, we configured the system to trigger alerts and generate incidents based on the ingested logs.

Furthermore, we conducted metric assessments within the vulnerable environment both before and after the implementation of security measures. Notably, the results revealed a significant reduction in the frequency of security events and incidents post-implementation, underscoring the efficacy of the applied security controls.

It's important to acknowledge that had the network resources been actively utilized by regular users, it's plausible that a greater volume of security events and alerts would have been observed within the 24-hour period following the deployment of the security controls.
