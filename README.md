# Cloud-Based Honeypot with Log Analytics & Attack Visualization
 Goal: The goal for this lab is to simulate a real-world cyber threat detection workflow by deploying a honeypot in Microsoft Azure, capturing failed login attempts, forwarding security logs to Azure Sentinel, enriching those logs with geolocation data, and visualizing attacker activity through custom dashboards. This hands-on lab demonstrates core concepts in cloud security, SIEM operations, and threat intelligence.

## Technology Used
Cloud & Virtualization
Microsoft Azure – Cloud platform used to host and manage virtual infrastructure

Azure Virtual Machines – Used to deploy the honeypot (Windows 10 VM)
Azure Network Security Groups (NSGs) – To control inbound/outbound traffic
Azure Log Analytics Workspace (LAW) – Centralized log repository
Azure Sentinel – Cloud-native SIEM tool for log analysis, alerting, and visualization

Operating Systems
Windows 10 – Operating system used on the honeypot VM

Security Tools & Techniques
Event Viewer (Windows Logs) – To view failed login attempts (Event ID 4625)
Windows Firewall – Disabled to simulate a vulnerable host
Sentinel Watchlists – Used for enriching logs with geolocation data

Data & Log Management
Kusto Query Language (KQL) – Used to query logs in Azure Sentinel and Log Analytics
Data Collection Rules (DCR) – To forward logs from VMs to LAW/Sentinel
ipv4_lookup Function – For enriching IP logs with geolocation info

Visualization & Reporting
Azure Sentinel Workbooks – Used to create visualizations (attack map)
Custom JSON Configuration – For building interactive map-based dashboards

## Steps and Documentation
1: Deploy the Honeypot (Azure Virtual Machine)
2: Simulate Failed Logins & Inspect Logs
3: Forward Logs to Sentinel via Log Analytics
4: Enrich Logs with GeoIP Data
5: Visualize Attacks with an Interactive Map

## Step 1 Set Up Azure VM as the Honeypot
Deploy a Windows 10 virtual machine in Azure to serve as a honeypot for detecting unauthorized access attempts. Configure the Network Security Group (NSG) to allow all inbound traffic, simulating an exposed and vulnerable system. After deployment, disable the Windows Firewall on the VM to further reduce its defenses and attract attack traffic. This setup creates a realistic target for threat detection and log analysis in later steps.
![sentinel_vm](https://github.com/user-attachments/assets/3881ac72-04ef-42ba-b284-ada56735dab0)
![SENTINEL_DANGERFWRULE](https://github.com/user-attachments/assets/553fc357-36a8-4d59-9779-02e7ed8ad7c8)
![sentinel_wfoff](https://github.com/user-attachments/assets/04bbb523-2f82-4a4d-bd25-335676b3e1b6)

## Step 2 Simulate Failed Login & Inspect Logs
Attempt multiple failed login attempts to the honeypot VM using a fake username to generate security events. Log into the VM and open Event Viewer to examine the Security logs, specifically identifying Event ID 4625, which indicates failed logon attempts. This step demonstrates how suspicious activity is recorded locally before being forwarded to a centralized log repository.
![sentinel_logfailuretest](https://github.com/user-attachments/assets/2a282845-14e6-4350-b4ce-c69aa374af4e)
![LAW-SENTINEL](https://github.com/user-attachments/assets/88dfd352-3759-4cca-b31b-10504ef01768)

## Step 3 Forward Logs to Sentinel via Log Analystics
Create a Log Analytics Workspace (LAW) and deploy Azure Sentinel to begin centralizing and analyzing security logs. Connect the Windows VM to the workspace by configuring the “Windows Security Events via AMA” data connector and creating a Data Collection Rule (DCR). Once logs are flowing into the workspace, use Kusto Query Language (KQL) to search for specific security events—such as failed login attempts (Event ID 4625)—within both Log Analytics and Sentinel.
![LAW-SENTINEL](https://github.com/user-attachments/assets/4cab2e2c-f451-493b-8a4e-0eafa68da256)
![SENTINEL-WSE](https://github.com/user-attachments/assets/0ed278be-b61f-4be8-93bf-647dbe3f5617)
![SENTINEL-DCRULE](https://github.com/user-attachments/assets/de0c2bef-9311-478f-9144-e3815e466538)
![SENTINEL-SELOGS](https://github.com/user-attachments/assets/5bcbf863-82c5-46a4-aed7-f009d1e6f925)

## Step 4 Enrich Logs with GeoIP Data
Enhance the SecurityEvent logs in Sentinel by importing external geolocation data. Since logs only display IP addresses by default, download and import the geoip-summarized.csv file as a Sentinel Watchlist to map IP addresses to geographic regions. Configure the watchlist settings and use a KQL query with the ipv4_lookup function to join IPs with location data. 
![sentinel-wlitems](https://github.com/user-attachments/assets/74b31bf2-9a95-4ae5-85c2-80762e704427)
![sentinel-enrichedlogs](https://github.com/user-attachments/assets/adc51572-f9c7-4a17-98b5-8e7228e83df8)

## Step 5 Visualize Attacks with and Interactive map using Sentinel Workbooks
In Azure Sentinel, create a new Workbook to visualize attack data geographically. Remove default components and add a custom Query element. Use the advanced editor to paste a JSON configuration (map.json) that defines the attack map visualization.
![SENTINEL-VMATTACKMAP](https://github.com/user-attachments/assets/fc3729fe-69bb-4fa0-a392-525c7a9031c0)

## Summary
This project offers a practical introduction to cloud-based security monitoring by setting up a honeypot environment in Microsoft Azure. Throughout the process, I learned how to deploy and configure Azure resources such as virtual machines and network security groups, creating an intentionally vulnerable system to attract and observe attack attempts. By simulating failed login attempts, I gained experience inspecting Windows security event logs and identifying relevant event IDs. I then centralized log collection using Azure Log Analytics Workspace and Azure Sentinel, learning to write Kusto Query Language (KQL) queries to analyze security events effectively. Additionally, I enriched raw log data with geolocation information by importing external data through Sentinel Watchlists, which provided valuable context to track the geographic origin of attacks. Finally, I created interactive dashboards using Azure Sentinel Workbooks to visualize attacker activity and geographic trends. This project enhanced my skills in cloud infrastructure management, security operations, log analytics, and data visualization.

