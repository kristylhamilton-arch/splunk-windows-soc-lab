# Windows SOC Lab – Splunk Enterprise

## Project Overview

This project demonstrates a home SOC/SIEM environment built using Splunk Enterprise and Windows 11. The lab was designed to collect Windows Security Event Logs, identify suspicious authentication activity, create detection logic using SPL, and investigate triggered security alerts.

## Objective

Build a home SOC/SIEM environment capable of:

- Collecting Windows Security Event Logs
- Monitoring authentication activity
- Detecting repeated failed login attempts
- Creating automated security alerts
- Investigating authentication events using Splunk

## Lab Environment

- Windows 11
- Splunk Enterprise 10.4.2
- Windows Security Event Logs
- Local Windows test accounts

## What I Configured

- Installed and configured Splunk Enterprise
- Configured ingestion of the Windows Security Event Log
- Investigated Windows Event IDs 4624 and 4625
- Generated controlled failed-login activity
- Used SPL to identify repeated authentication failures
- Created a scheduled detection for multiple failed login attempts
- Successfully triggered and investigated the alert

## Detection

Windows Security Event ID **4625** represents a failed account logon.

I created the following SPL query to identify accounts with multiple failed authentication attempts:

```spl
index=* source="WinEventLog:Security" EventCode=4625
| stats count by Account_Name
| where count >= 5

## Detection Result
The detection identified repeated failed authentication attempts using Windows Security Event ID 4625. The SPL query grouped failed logins by account and flagged accounts with five or more failures. The scheduled detection successfully identified 10 failed authentication events during the five-minute monitoring window, confirming that the detection logic worked as intended.
