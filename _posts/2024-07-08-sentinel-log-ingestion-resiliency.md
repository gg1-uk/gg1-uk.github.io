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
<p style="font-size:16px; font-style:italic;">Read: 3mins, watch: 10mins, updated 4-Aug-24 9:13pm live.</p>

### Ingestion Survivability
This case study focuses on the `resiliency features` testing of the two native software agents purpose-built to handle the security logs ingestion from on-premises operating systems to Microsoft Sentinel, which attesting its robustness of `retry mechanism` during temporary or prolong network connectivity downtime.

In general, this is a valid and crucial concern ensuring there is no loss of security logs in the event of network connection failure during the ingestion. It serves as a guiding principle for a robust and resilient architectural design.

### Overall Architecture
Overview of Sentinel log ingestion architecture.
<img src="/assets/images/logingest/Sentinel_Log_Ingestion_Resiliency_Final.png"><br>
<p style="font-size:12px;">Download original diagram in draw.io format <a href="/assets/images/logingest/Sentinel_Log_Ingestion_Resiliency_Final.drawio">here</a>.</p>

### Greener lab
> An eco-friendly way, a nano box was powered-on alongsides 6-month licensed Windows Server 2019 Standard with Hyper-V virtualized Ubuntu Server 22.04.4 LTS up and running. This test aimed to survive target 3 to 7 days long 24x7 uptimes with minimum carbon emisson and minimize risk of surging power utility bill at home ;)
<img src="/assets/images/logingest/thegreenerlab.png">

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
<div class="links"><a class="active" href="javascript:alert('\n1st and 2nd phases of tests completed as of 18 and 22 July 2024!\n\nPhase 3 test completed on 23 July 2024!'
);">Survivability Tests Completed</a></div>
</td>
</tr>
</tfoot>
<tbody align="center">
<tr>
<td>Phase 1 Test<br><br>187 (7 days 19 hours)</td>
<td><p style="font-size:12px;">Recovered last 48 hours Windows Event</p><img src="/assets/images/logingest/EventSixDaysMissing.png"></td>
<td><p style="font-size:12px;">Missing two Syslog record</p><img src="/assets/images/logingest/SyslogTwoRecordsMissing.png"></td>
<td><p style="font-size:12px;"><br>Blocked software agent network connetivity on 10/Jul/24 3:30PM<br><br>Resumed software agent network connectivity on 18/Jul/24 9:35AM<br><br>Windows Event first resumed ingestion on 18/Jul/24 10:45AM<br><br>Syslog first resumed ingestion on 10/Jul/24 6:00PM</p></td>
</tr>
<tr>
<td>Phase 2 Test<br><br>74 (3 days 2 hours)</td>
<td><p style="font-size:12px;">Recovered last 48 hours Windows Event</p><img src="/assets/images/logingest/RecoveredWindowsEvents.png"></td>
<td><p style="font-size:12px;">Missing one Syslog record</p><img src="/assets/images/logingest/SyslogMissingOneRecord.png"></td>
<td><p style="font-size:12px;"><br>Modified Event and Syslog message body with timestamp<br><br>Disconnected network cable on 19/Jul/24 12:48PM<br><br>Reconnected network cable on 22/Jul/24 14:58PM<br><br>Windows Event first resumed ingestion on 20/Jul/24 19:29:15<br><br>Syslog first resumed ingestion on Fri Jul 19 12:49:01 PM +08 2024</p></td>
</tr>
<tr>
<td>Phase 3 Test<br><br>10 (10 hours 36 minutes)</td>
<td><p style="font-size:12px;">Resumed from last ingested Event, 100% recovered</p><img src="/assets/images/logingest/RestartedAgentIngestionResult.png"></td>
<td><p style="font-size:12px;">Resumed from last ingested Syslog, 100% recovered</p><img src="/assets/images/logingest/RecoveredWindowsEvents.png"></td>
<td><p style="font-size:12px;"><br>Disconnected network cable on 22/Jul/24 21:55PM<br><br>Reconnected network cable on 23/Jul/24 08:31AM<br><br>Windows Event first resumed ingestion on 21/Jul/24 07:34:01AM - 100% recovered<br><br>Syslog first resumed ingestion on Mon Jul 22 09:55:01 PM +08 2024 - 100% recovered</p></td>
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
<th>Remark (GMT+8)</th>
</tr>
</thead>
<tfoot>
<tr>
<td colspan="4" align="left">
<div class="links"><a class="active" href="javascript:alert('\nGarage always in innovating mode!');">Upgrading In Progress</a></div>
</td>
</tr>
</tfoot>
<tbody align="center">
<tr>
<td><p style="font-size:12px;">Windows Agent Install Script</p><img src="/assets/images/logingest/AzureArcAgentInstallationWindows.png"></td>
<td><p style="font-size:12px;">Windows Scheduler Event Write Every 5mins</p><img src="/assets/images/logingest/Scheduler5minWrite.png"></td>
<td><p style="font-size:12px;">Hyper-V Linux Virtual Machine Installation</p><img src="/assets/images/logingest/HyperVLinuxInstallation.png"></td>
<td><p style="font-size:12px;">Sentinel last ingested Windows Event</p><img src="/assets/images/logingest/KQLWindowsEvents.png"></td>
</tr>
<tr>
<td><p style="font-size:12px;">Linux Agent Install Script</p><img src="/assets/images/logingest/AzureArcAgentInstallationLinux.png"></td>
<td><p style="font-size:12px;">Windows Event</p><img src="/assets/images/logingest/WindowsEvent.png"></td>
<td><p style="font-size:12px;">Linux Cronjob Hourly Write</p><img src="/assets/images/logingest/LinuxCronjob1hourWrite.png"></td>
<td><p style="font-size:12px;">Sentinel last ingested Linux Syslog</p><img src="/assets/images/logingest/KQLSyslogs.png"></td>
<td></td>
</tr>
<tr>
<td><p style="font-size:12px;">Azure Arc Machines Overview</p><img src="/assets/images/logingest/AzureArcMachinesView.png"></td>
<td><p style="font-size:12px;">Windows Azure Agent Installation</p><img src="/assets/images/logingest/WindowsAzureAgentInstallation.png"></td>
<td><p style="font-size:12px;">Linux Syslog</p><img src="/assets/images/logingest/LinuxSyslog.png"></td>
<td><p style="font-size:12px;">Last ingested Windows Event</p><img src="/assets/images/logingest/WindowsEventLive.png"></td>
</tr>
<tr>
<td><p style="font-size:12px;">Windows Data Collection Rule</p><img src="/assets/images/logingest/AzureDCRWindows.png"></td>
<td><p style="font-size:12px;">Windows Wireshark Detecting Outbound Azure Agent Traffic</p><img src="/assets/images/logingest/WindowsWiresharkDetectOutboundAzureTraffic.png"></td>
<td><p style="font-size:12px;">Linux Agent Installation and Device Login</p><img src="/assets/images/logingest/LinuxAzureAgentInstallation.png"></td>
<td><p style="font-size:12px;">Last ingested Linux Syslog</p><img src="/assets/images/logingest/LinuxSyslogLive.png"></td>
</tr>
<tr>
<td><p style="font-size:12px;">Linux Data Collection Rule</p><img src="/assets/images/logingest/AzureDCRLinux.png"></td>
<td><p style="font-size:12px;">Windows Hosts File Prevent Outbound Azure Agent Traffic</p><img src="/assets/images/logingest/WindowsHostsFileBlockAccess.png"></td>
<td><p style="font-size:12px;">Linux Host File Prevent Outbound Azure Agent Traffic</p><img src="/assets/images/logingest/LinuxHostsFileBlockAccess.png"></td>
<td><p style="font-size:12px;"><br>Sentinel Phase 2 Test<br><br>2.1 Last ingested Windows Event on 19/Jul/24 12:48:19<br><br>2.2 Last ingested Linux Syslog on Fri Jul 19 12:47:01 PM +08 2024<br><br>Sentinel Phase 3 Test<br><br>3.1 Last ingested Windows Event on 21/Jul/24 07:33:59<br><br>3.2 Last ingested Linux Syslog on Mon Jul 22 09:54:01 PM +08 2024</p></td>
</tr>  
</tbody>
</table><br>

### Conclusion
The three phases of tests spread across two weeks, which conducted from 10 July to 23 July in 2024 produced pattern of results below.

8.1 Windows Azure Connected Machine Agent is capable to recover a maximum of last 24 hours Windows Event once network connectivity resumed and retry counter/polling interval reset every 8.5 hours.
  + A manual restart of Azure Connected Machine Agent is able to resume Windows Event reingestion rather than waiting for the retry counter/polling interval reset and reattempt
  + It has proven works and managed to resumed log retransmission from where it last stopped as per Phase 3 test result, which fully reingested as long as the Windows Event timestamp falls between last 24 hours

8.2 Linux Azure Connected Machine Agent is capable to recover almost 99.99% Syslog records (with 1 missing Syslog record during the one minute interval when network connection failure hits - refer to Phase 2 test result for detail).
  + How long the Syslog messages can be kept in queue (for the whole period of network connectivity failure) depends on Syslog server's storage capacity, it's recommended to perform a sanity check for any missing Syslogs record(s) at least for the first one minute internal when network connection downtime hits and perform a manual reingestion if necessary
  + Despite Phase 3 test result proven and recovered 100% of all Syslog records, it's still recommended to check for any missing Syslog record(s) during the network connectivity downtime for the first one minute

All in all, it's strongly recommended to have the redundance and resilience capabilities in the network connectivity implementation between on-premises and Sentinel. However, in the event of all failures (touch wood), we are prepared and aware what to lookout for symptoms and action to achieve a 100% log reingestion recovery plan.
