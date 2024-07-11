---
layout: post
title:  "Sentinel-Log-Ingestion-Resiliency"
author: don
categories: [ sentinel ]
image: assets/images/australia2019/island.jpg
featured: true
hidden: true
beforetoc: "Often time, we are hearing from our client on topic vis-a-vis know-how addressing security log ingestion capable of surviving intermittent or periodic network connectivity failure."
toc: true
---

<p style="font-size:12px;">Read: 3mins, watch: 10mins, updated 11-07-24 8:19pm live.</p>

### Ingestion Survivability
This is a valid and crucial concerns for client as part of the architectural resiliency design and risk considerations, all in all ensuring there is no loss of security logs in the event of connection failure.

### Endurance test
This case study focuses on the `resiliency features` of the two native software agents purpose-built to handle ingestion of the security logs from on-premises operating systems to Microsoft Sentinel, which attesting the robustness of `retry mechanism` during temporary or prolong network connectivity downtime.

### Greener lab
> An eco-friendly way, a nano box was powered-on alongsides 6-month licensed Windows Server 2019 Standard with Hyper-V virtualized Ubuntu Server 22.04.4 LTS up and running. This test aimed to survive target 3 to 7 days long 24x7 uptimes with minimum carbon emisson and minimize risk of surging power utility bill at home ;)

### Gears up
4.1 Windows Server 2019 Standard with Azure Connected Machine Agent<br>
4.2 Ubuntu Server 22.04.4 with Azure Connected Machine Agent<br>
4.3 Microsoft Sentinel<br>
  + Log Analytics Workspace
  + Syslog via AMA data connector
  + Data Collection Rule for Syslog for Azure Arc Machine
  + Data Collection Rule for Windows Event Log for Azure Arc Machine
4.4 Internet Connectivity<br>

### Coverage
5.1 Windows Server 2019 Standard with Azure Connected Machine Agent suffering more than 9 hours network connectivity downtime<br>
  + According to Microsoft official <a href="https://learn.microsoft.com/en-us/troubleshoot/azure/azure-monitor/log-analytics/windows-agents/mma-troubleshoot-basics#frequently-asked-questions-faq">guideline</a>, data is buffered for up to 8.5 hours and maximum size of less than 1.5 GB before being discarded, hence, more than 9 hours downtime was identified and tested, can the logs re-transmission prevail?
    
5.2 Ubuntu Server 22.04.4 with Azure Connected Machine Agent suffering 3 days network connectivity downtime<br>
  + Let's see how long the logs survive depend on the disk storage size as per Microsoft official <a href="https://learn.microsoft.com/en-us/azure/azure-monitor/agents/azure-monitor-agent-troubleshoot-linux-vm-rsyslog#:~:text=Azure%20Monitor%20Agent%20uses%20local%20persistency%20by%20default
 ">guideline</a> and let's prove it to be trued

### Result
<table class="blueTable">
<thead>
<tr align="center">
<th>Downtime Hour</th>
<th>Windows 2019</th>
<th>Ubuntu 20.04.4</th>
<th>Remark</th>
</tr>
</thead>
<tfoot>
<tr>
<td colspan="4" align="left">
<div class="links"><a class="active" href="javascript:alert('ETA 17 July 2024!');">Test in progress</a></div>
</td>
</tr>
</tfoot>
<tbody align="center">
<tr>
<td>19</td>
<td></td>
<td></td>
<td></td>
</tr>
<tr>
<td>38</td>
<td></td>
<td></td>
<td></td>
</tr>
<tr>
<td>72</td>
<td></td>
<td></td>
<td></td>
</tr>
<tr>
<td>168</td>
<td></td>
<td></td>
<td></td>
</tr>
</tbody>
</table><br>

### Garage
<table class="blueTable">
<thead>
<tr align="center">
<th>Microsoft Sentinel</th>
<th>Windows 2019</th>
<th>Ubuntu 20.04.4</th>
<th>Remark</th>
</tr>
</thead>
<tfoot>
<tr>
<td colspan="4" align="left">
<div class="links"><a class="active" href="javascript:alert('ETA 17 July 2024!');">Test in progress</a></div>
</td>
</tr>
</tfoot>
<tbody align="center">
<tr>
<td>Windows Agent Install Script<img src="/assets/images/logingest/AzureArcAgentInstallationWindows.png"></td>
<td>Windows Scheduler Event Write Every 5mins<img src="/assets/images/logingest/Scheduler5minWrite.png"></td>
<td></td>
<td></td>
</tr>
<tr>
<td>Linux Agent Install Script<img src="/assets/images/logingest/AzureArcAgentInstallationLinux.png"></td>
<td></td>
<td></td>
<td></td>
</tr>
<tr>
<td>Azure Arc Machines Overview<img src="/assets/images/logingest/AzureArcMachinesView.png"></td>
<td></td>
<td></td>
<td></td>
</tr>
<tr>
<td>Windows Data Collection Rule<img src="/assets/images/logingest/AzureDCRWindows.png"></td>
<td></td>
<td></td>
<td></td>
</tr>
<tr>
<td>Linux Data Collection Rule<img src="/assets/images/logingest/AzureDCRLinux.png"></td>
<td></td>
<td></td>
<td></td>
</tr>  
</tbody>
</table><br>

### The step-by-step guide
If there is a demand, this can be developed.
