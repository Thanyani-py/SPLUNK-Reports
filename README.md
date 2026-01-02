Project: Incident Handling with Splunk
Platform: TryHackMe (SOC Level 1 Path)

Scenario: Wayne Enterprises Web Server Defacement Investigation

Environment: Splunk SIEM, Windows Event Logs, Suricata IDS, Web Traffic Logs

📌 Project Objective
The goal of this investigation was to reconstruct the attack lifecycle that led to the defacement of the imreallynotbatman.com website. I utilized Splunk to ingest and analyze host-centric and network-centric logs to identify indicators of compromise (IOCs) and attribute the attack to specific threat actor infrastructure.

🛡️ Investigative Workflow (Cyber Kill Chain Mapping)
1. Reconnaissance
Activity: Identified automated scanning originating from IP 40.80.148.42.

Tools Identified: Correlated Suricata IDS alerts to identify the use of the Acunetix vulnerability scanner.

Vulnerability: Discovered attempts to exploit CVE-2014-6271 (Shellshock) against the server's Joomla CMS.

2. Exploitation & Installation
Brute Force Detection: Leveraged Splunk's stats and dc() (distinct count) functions to identify a massive brute-force campaign against the /joomla/administrator/index.php login page from IP 23.22.63.114.

Successful Logon: Pinpointed the exact moment of compromise by filtering for successful POST requests and identifying the password batman being used for the admin account.

Malware Deployment: Traced the upload and execution of a malicious file, 3791.exe, using Sysmon logs and network traffic correlation.

3. Command & Control (C2) & Actions on Objectives
C2 Identification: Used OSINT (ThreatMiner and VirusTotal) to verify that the attacker's IP was linked to the domain prankglassinebracket.jumpingcrab.com.

Impact Analysis: Confirmed the final stage of the attack where the threat actor successfully defaced the web server's home page.

💻 Key Splunk Queries Used
Traffic Spike Analysis: index=botsv1 imreallynotbatman.com | timechart span=1h count.

Brute Force Triage: index=botsv1 sourcetype=stream:http uri="/joomla/administrator/index.php" http_method=POST | stats count by src_ip.

IDS Alert Correlation: index=botsv1 sourcetype=suricata signature_id=20146271.

📝 Key Lessons Learned
The Value of Log Correlation: Combining IDS alerts with web traffic logs was essential to distinguishing between background noise and a targeted attack.

Proactive Defenses: Recommended the implementation of account lockout policies and Multi-Factor Authentication (MFA) to mitigate the brute-force risk identified during the investigation.
