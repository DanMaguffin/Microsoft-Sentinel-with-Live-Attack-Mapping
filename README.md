# Microsoft Azure Sentinel Cyber Attack Mapping
A cybersecurity project to visualize live cyber attacks using Azure Sentinel, PowerShell, and geolocation data.

## Overview
This project extracts Windows Event Viewer metadata, processes it through an API for geolocation, and visualizes attack patterns in Azure Sentinel. It demonstrates skills in SIEM configuration, scripting, and real-time threat monitoring.

## Features
- PowerShell script to extract security event metadata from Windows Event Viewer and forward it to a geolocation API.
- Configured Azure Log Analytics Workspace to process and store custom logs with geographical data.
- Designed an Azure Sentinel workbook to display live cyber attack data by location and severity.

## Tools Used
- Microsoft Azure: Cloud platform hosting Sentinel and Log Analytics.
- Azure Sentinel: SIEM tool for threat visualization.
- PowerShell: Scripted event log extraction and API integration.
- Windows Event Viewer: Source of security logs.
- Geolocation API: Third-party service (e.g., free tier of ip-api.com) for location data.

## Prerequisites
- Azure account with a subscription (free tier available at [azure.com](https://azure.com)).
- PowerShell installed (default on Windows).
- Basic knowledge of Azure portal and JSON.

## Setup Instructions
1. **Set Up Azure Environment:**
   - Log into the Azure portal, create a resource group (e.g., `CyberLab`).
   - Deploy Log Analytics Workspace and Azure Sentinel, connecting them to your resource group.
2. **Configure Event Collection:**
   - In Azure Sentinel, enable Windows Security Events connector.
3. **PowerShell Script:**
   - Create `attack_mapping.ps1`:
     ```powershell
     $events = Get-WinEvent -LogName "Security" -MaxEvents 100
     foreach ($event in $events) {
         $ip = $event.Properties[5].Value  # Example: Source IP field
         $response = Invoke-RestMethod -Uri "http://ip-api.com/json/$ip"
         $log = [PSCustomObject]@{Time=$event.TimeCreated; IP=$ip; Country=$response.country; Severity="Medium"}
         $log | ConvertTo-Json | Out-File "attack_logs.json" -Append
     }
     ```
   - Run the script to generate `attack_logs.json`.
4. **Upload to Azure:**
   - In Log Analytics, use “Custom Logs” to upload `attack_logs.json`.
   - Write a KQL query (e.g., `customLogs_CL | summarize by Country_s`).
5. **Visualize in Sentinel:**
   - Create a workbook in Azure Sentinel, add a map visualization, and link it to your KQL query.

## Outcomes
- Mapped real-time cyber attack attempts with geographical context (e.g., country of origin).
- Enhanced skills in PowerShell scripting, SIEM configuration, and Azure cloud services.
- Produced a visually impactful workbook for threat analysis.

## Future Enhancements
- Automate log uploads with Azure Functions.
- Add severity scoring based on event IDs.

## Contact
Daniel Maguffin | dmaguffin17@gmail.com |
