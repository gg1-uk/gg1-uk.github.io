---
layout: post
title:  "Sentinel-Log-Ingestion-Resiliency"
author: don
categories: [ sentinel ]
image: assets/images/australia2019/island.jpg
featured: true
hidden: true
beforetoc: "Security logs ingestion from on-premises to Microsoft Sentinel."
toc: true
---

Often time, we are hearing from our client on topic vis-a-vis know-how addressing security log ingestion capable of surviving intermittent or periodic network connectivity failure.
<p style="font-size:16px; font-style:italic;">Read: 3mins, watch: 10mins, updated 18-Jul-24 12:25pm live.</p>

### Ingestion Survivability
This is a valid and crucial concerns for client as part of the architectural resiliency design and risk considerations, all in all ensuring there is no loss of security logs in the event of network connection failure.

### Endurance test
This case study focuses on the `resiliency features` of the two native software agents purpose-built to handle ingestion of the security logs from on-premises operating systems to Microsoft Sentinel, which attesting the robustness of `retry mechanism` during temporary or prolong network connectivity downtime.

### Greener lab
> An eco-friendly way, a nano box was powered-on alongsides 6-month licensed Windows Server 2019 Standard with Hyper-V virtualized Ubuntu Server 22.04.4 LTS up and running. This test aimed to survive target 3 to 7 days long 24x7 uptimes with minimum carbon emisson and minimize risk of surging power utility bill at home ;)

### Gears up
4.1 Windows Server 2019 Standard with Azure Connected Machine Agent<br>
4.2 Ubuntu Server 22.04.4 with Azure Connected Machine Agent<br>
4.3 Internet Connectivity<br>
4.4 Microsoft Sentinel<br>
  + Log Analytics Workspace
  + Syslog via AMA data connector
  + Data Collection Rule for Syslog for Azure Arc Machine
  + Data Collection Rule for Windows Event Log for Azure Arc Machine  

### Coverage
5.1 Windows Server 2019 Standard with Azure Connected Machine Agent suffering more than 9 hours network connectivity downtime.<br>
  + According to Microsoft official <a href="https://learn.microsoft.com/en-us/troubleshoot/azure/azure-monitor/log-analytics/windows-agents/mma-troubleshoot-basics#frequently-asked-questions-faq">guideline</a>, data is buffered for up to 8.5 hours and maximum size of less than 1.5 GB before being discarded, hence, more than 9 hours downtime was identified and tested, can the logs re-transmission prevail?
    
5.2 Ubuntu Server 22.04.4 with Azure Connected Machine Agent suffering 3 days network connectivity downtime.<br>
  + Let's see how long the logs survive depend on the disk storage size as per Microsoft official <a href="https://learn.microsoft.com/en-us/azure/azure-monitor/agents/azure-monitor-agent-troubleshoot-linux-vm-rsyslog#:~:text=Azure%20Monitor%20Agent%20uses%20local%20persistency%20by%20default
 ">guideline</a> and let's prove it to be trued.

### Result
<table class="blueTable">
<thead>
<tr align="center">
<th>Network Downtime Hour</th>
<th>Windows Server 2019</th>
<th>Ubuntu Server 22.04.4</th>
<th>Remark  (GMT+8)</th>
</tr>
</thead>
<tfoot>
<tr>
<td colspan="4" align="left">
<div class="links"><a class="active" href="javascript:alert('2nd phase ETA 27 July 2024!');">Testing in progress</a></div>
</td>
</tr>
</tfoot>
<tbody align="center">
<tr>
<td>187 (7 days 19 hours)</td>
<td><p style="font-size:12px;">Recovered last 48 hours Windows Event</p><img src="/assets/images/logingest/EventSixDaysMissing.png"></td>
<td><p style="font-size:12px;">Missing two Syslog record</p><img src="/assets/images/logingest/SyslogTwoRecordsMissing.png"></td>
<td><p style="font-size:12px;"><br>Blocked software agent network connetivity on 10/Jul/24 3:30PM<br><br>Resumed software agent network connectivity on 18/Jul/24 9:35AM<br><br>Windows Event first resumed ingestion on 18/Jul/24 10:45AM<br><br>Syslog first resumed ingestion on 10/Jul/24 6:00PM</p></td>
</tr>
<tr>
<td>48 (2 days)</td>
<td></td>
<td></td>
<td><p style="font-size:12px;"><br>Modified Event and Syslog message body with timestamp<br><br>Disconnect network cable on 19/Jul/24 12:48PM</p></td>
</tr>
</tbody>
</table><br>

### Garage
<table class="blueTable">
<thead>
<tr align="center">
<th>Microsoft Sentinel</th>
<th>Windows Server 2019</th>
<th>Ubuntu Server 22.04.4</th>
<th>Remark</th>
</tr>
</thead>
<tfoot>
<tr>
<td colspan="4" align="left">
<div class="links"><a class="active" href="javascript:alert('Garage always in innovating mode!');">Upgrading in progress</a></div>
</td>
</tr>
</tfoot>
<tbody align="center">
<tr>
<td><p style="font-size:12px;">Windows Agent Install Script</p><img src="/assets/images/logingest/AzureArcAgentInstallationWindows.png"></td>
<td><p style="font-size:12px;">Windows Scheduler Event Write Every 5mins</p><img src="/assets/images/logingest/Scheduler5minWrite.png"></td>
<td><p style="font-size:12px;">Hyper-V Linux Virtual Machine Installation</p><img src="/assets/images/logingest/HyperVLinuxInstallation.png"></td>
<td><p style="font-size:12px;">Windows Sentinel Last Ingested Event</p><img src="/assets/images/logingest/KQLWindowsEvents.png"></td>
</tr>
<tr>
<td><p style="font-size:12px;">Linux Agent Install Script</p><img src="/assets/images/logingest/AzureArcAgentInstallationLinux.png"></td>
<td><p style="font-size:12px;">Windows Event</p><img src="/assets/images/logingest/WindowsEvent.png"></td>
<td><p style="font-size:12px;">Linux Cronjob Hourly Write</p><img src="/assets/images/logingest/LinuxCronjob1hourWrite.png"></td>
<td><p style="font-size:12px;">Linux Sentinel Last Ingested Syslog</p><img src="/assets/images/logingest/KQLSyslogs.png"></td>
<td></td>
</tr>
<tr>
<td><p style="font-size:12px;">Azure Arc Machines Overview</p><img src="/assets/images/logingest/AzureArcMachinesView.png"></td>
<td><p style="font-size:12px;">Windows Azure Agent Installation</p><img src="/assets/images/logingest/WindowsAzureAgentInstallation.png"></td>
<td><p style="font-size:12px;">Linux Syslog</p><img src="/assets/images/logingest/LinuxSyslog.png"></td>
<td><p style="font-size:12px;">Windows Last Ingested Event</p><img src="/assets/images/logingest/WindowsEventLive.png"></td>
</tr>
<tr>
<td><p style="font-size:12px;">Windows Data Collection Rule</p><img src="/assets/images/logingest/AzureDCRWindows.png"></td>
<td><p style="font-size:12px;">Windows Wireshark Detecting Outbound Azure Agent Traffic</p><img src="/assets/images/logingest/WindowsWiresharkDetectOutboundAzureTraffic.png"></td>
<td><p style="font-size:12px;">Linux Agent Installation and Device Login</p><img src="/assets/images/logingest/LinuxAzureAgentInstallation.png"></td>
<td><p style="font-size:12px;">Linux Last Ingested Syslog</p><img src="/assets/images/logingest/LinuxSyslogLive.png"></td>
</tr>
<tr>
<td><p style="font-size:12px;">Linux Data Collection Rule</p><img src="/assets/images/logingest/AzureDCRLinux.png"></td>
<td><p style="font-size:12px;">Windows Hosts File Prevent Outbound Azure Agent Traffic</p><img src="/assets/images/logingest/WindowsHostsFileBlockAccess.png"></td>
<td><p style="font-size:12px;">Linux Host File Prevent Outbound Azure Agent Traffic</p><img src="/assets/images/logingest/LinuxHostsFileBlockAccess.png"></td>
<td></td>
</tr>  
</tbody>
</table><br>

### Step by step
Development in progress.
