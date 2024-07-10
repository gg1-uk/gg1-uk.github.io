---
layout: post
title:  "Sentinel-Log-Ingestion-Resiliency"
author: don
categories: [ sentinel ]
image: assets/images/australia2019/island.jpg
featured: true
hidden: true
beforetoc: "Read/watch time: 3/10 minutes, updated 10-07-24 9:21pm"
toc: true
---

### Survival rule
Often time, we are hearing from our client on topic vis-a-vis know-how addressing security log ingestion capable of surviving intermittent or periodic network connectivity failure. 

Honestly, this is a valid and crucial concerns for client as part of the architectural design consideration and business as usual risk awareness, all in all ensuring there is no loss of security logs in the event of connection failure.

### Endurance test
This case study focuses on the `resiliency features` of the two native software agents purpose-built to handle ingestion of the security logs from on-premises operating systems to Microsoft Sentinel, which attesting the resiliency in term of `retry mechanism` during temporary or prolong network connectivity downtime.

### The test coverage
> An eco-friendly way, a nano box was powered-on alongsides 6-month licensed Windows Server 2019 Standard with Hyper-V virtualized Ubuntu Server 22.04.4 LTS up and running. This test aimed to survive target 3 to 7 days long 24x7 uptimes with minimum carbon emisson and minimize risk of surging power utility bill at home ;)

The test components below.
+ Windows Server 2019 Standard with Azure Connected Machine Agent
+ Ubuntu Server 22.04.4 with Azure Connected Machine Agent
+ Microsoft Sentinel
  + Log Analytics Workspace
  + Syslog via AMA data connector
  + Data Collection Rule for Syslog for Azure Arc Machine
  + Data Collection Rule for Windows Event Log for Azure Arc Machine
+ Internet Connectivity

The test scenarios below.
+ Windows Server 2019 Standard with Azure Connected Machine Agent suffering an 9 hours network connectivity downtime
  + According to Microsoft official <a href="https://learn.microsoft.com/en-us/troubleshoot/azure/azure-monitor/log-analytics/windows-agents/mma-troubleshoot-basics#frequently-asked-questions-faq">guideline</a>, data is buffered for up to 8.5 hours before being discarded, hence, an 9 hours downtime was selected
+ Windows Server 2019 Standard with Azure Connected Machine Agent suffering an 3 days network connectivity downtime
  + What happened if the connectivity downtime more than 8.5 hours as maximum downtime allowed with data less than 1.5 GB, can the logs re-transmission survived?
+ Ubuntu Server 22.04.4 with Azure Connected Machine Agent suffering a 3 days network connectivity downtime
  + It seems how long the logs survive depend on the disk storage size as per <a href="https://learn.microsoft.com/en-us/azure/azure-monitor/agents/azure-monitor-agent-troubleshoot-linux-vm-rsyslog#:~:text=Azure%20Monitor%20Agent%20uses%20local%20persistency%20by%20default
 ">guideline</a> and let's proof it to be trued

![walking](/assets/images/australia2019/island.jpg)

