# Threat-Hunting-Log-Analysis-
Investigating suspicious authentication and process logs to reconstruct a multi-stage cyber attack

In this investigation, I acted as aSecurity Analyst to analyze system logs for user John. 
The goal was to identify malicious behavious, reconstruct the timeline, and provide remediation steps.

**##TASK 1: SUSPICIOUS ACTIVITY IDENTIFIED**
Following the povided logs, I identified the following red flags:
*Impossible Travel* A login from Abuja was followed by attempts from Germany only 7 minutes later.
*Brute Force Attempts* Multiple LOGIN_FAILED events followed by a LOGIN_SUCCESS from the same German IP
*Obfustcated PowerShell* The start of the powershell.exe with -nop -w hidden flags (used to hide activity.)
*Discovery Coomands* Execution of whoami and ipconfig, typical of an attacker mapping the network.
*Data Exfiltration*An outbound connection followed by a 12MB data tranfer to an unknown external IP

**##TASK 2: THREAT HUNTING HYPOTHESIS##**
If an account experiences multiple falied logins from a foreign IP followed by successful login and the executiojn of hidden system processes, it may indicate a successful accout takeover and subsequent data exfiltration

**##TASK 3: ATTACK TIMELINE##**
*Initial Activty* User John logs in normally from Abuja (102.89.34.12)
*Suspicious Behaviour* Failed login attempts from a German IP (185.203.116.45)
*Compromise* Successful login from the German IP, indicating a compromised account.
*Post-compromise actions* The attacker runs hidden PowerShell and reconnaissance commands (whoami, ipconfig).
*Final Objective* Establishing an outbound connection and exfiltrating 12MB of data to 45.77.12.90

**##TASK 4: INDICATORS OF COMPROMISE (1OCs)##**
*IP Address (Attacker)* 185.203.116.45
*IP Address (Exfiltration Destination)* 45.77.12.90
*Suspicious Command* powershell.exe -nop -w hidden
*Tool Usage* whoami & ipconfig

**##TASK 5: ANALYST ACTIONS##**
*Immediate* Isolate the affected host (192.168.1.10) from the network to stop the data transfe. Reset credentials for user John
*Investigation* Review VPN and firewall logs to see if other accounts were targeted by the same German IP
*Prevention* Enforce multi-factor authentication for all users and implement Geo-blocking for unauthorised regions.

**##THREAT HUNTING EXTENSION##**
To ensure the attacker is fully removed, Iwould investigate:
*Persistence Logs* Check for new scheduled tasks or registry key changes.
*DNS Logs* Look for suspicious domain lookups associated with the exfiltration IP
*Account Logs* Check if any new "Admin" accounts were creayed during the session.
