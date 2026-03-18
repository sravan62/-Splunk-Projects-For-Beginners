# SSH Log Analysis & Threat Detection Using Splunk

##  Introduction

SSH (Secure Shell) logs are critical for monitoring remote access, authentication attempts, and system activity. By analyzing SSH logs, security analysts can detect suspicious behavior such as brute force attacks, unauthorized logins, privilege escalation, and lateral movement.

Using Splunk, we can efficiently ingest, search, and analyze SSH logs to uncover anomalies and gain actionable security insights.

---

##  Prerequisites

Before starting the analysis, make sure:

- Splunk is installed and running  
- SSH log sources are available (e.g., `/var/log/auth.log` or `/var/log/secure`)  
- Basic understanding of SSH logs (login attempts, users, IPs, etc.)  

---

##  Uploading SSH Log Data into Splunk

### 1. Prepare SSH Log Files

- Collect SSH log files (e.g., `.log` or `.txt` format)  
- Ensure logs include:
  - Timestamp  
  - Source IP  
  - Username  
  - Authentication status (success/failure)  
  - Port number  
  - SSH service messages  
- Store the files in a location accessible to Splunk  

---

### 2. Add Data to Splunk

- Log in to the Splunk Web interface  
- Go to **Settings → Add Data**  
- Choose **Upload**  

---

### 3. Select the Log File

- Click **Select File**  
- Upload your SSH log file  

---

### 4. Configure Source Type

- Assign:
  - Example: `ssh_logs`  
- Or create a custom source type  

---

### 5. Review Configuration

Verify:

- Index (e.g., `main`)  
- Host  
- Source Type  

---

### 6. Upload the Data

- Click **Review → Submit**  

---

### 7. Validate Data Ingestion

```
index=main sourcetype=ssh_logs
```

### Steps to Analyze SSH Log Files in Splunk SIEM

## 1. Search for SSH Events
```
index=main sourcetype=ssh_logs
```

## 2. Identify Top Source IPs (Potential Attackers)
```
index=main sourcetype=ssh_logs
| stats count by src_ip
| sort -count
```
This helps identify:
- High login activity
- Possible brute force attackers

## 3. Detect Failed Login Attempts
```
index=main sourcetype=ssh_logs "Failed password"
| stats count by src_ip, user
| sort -count
```
This helps detect:
- Brute force attacks
- Password guessing

## 4. Detect Successful Logins
```
index=main sourcetype=ssh_logs "Accepted password"
| stats count by user, src_ip
```
This helps:
- Track valid access
- Identify unusual login sources

## 5. Detect Brute Force Attacks
```
index=main sourcetype=ssh_logs "Failed password"
| stats count by src_ip
| where count > 10
```
This helps identify:
- Repeated login failures from same IP
- Automated attack behavior

## 6. Detect Multiple User Attempts from Same IP
```
index=main sourcetype=ssh_logs "Failed password"
| stats dc(user) as unique_users by src_ip
| where unique_users > 5
```
This helps detect:
- Credential stuffing attacks
- Username enumeration

## 7. Detect Logins at Unusual Times
```
index=main sourcetype=ssh_logs "Accepted password"
| eval hour=strftime(_time,"%H")
| where hour<6 OR hour>22
```
This helps detect:
- Suspicious off-hours access

## 8. Detect Root Login Attempts
```
index=main sourcetype=ssh_logs "root"
```
This helps identify:
- Privileged account targeting
- High-risk access attempts

## 9. Detect Possible Lateral Movement
```
index=main sourcetype=ssh_logs "Accepted password"
| stats count by src_ip, dest_host
```
This helps detect:
- Movement between systems
- Internal compromise

### Conclusion

Analyzing SSH logs in Splunk SIEM enables detection of brute force attacks, unauthorized access, and suspicious login behavior. By leveraging SPL queries, security analysts can monitor authentication patterns, identify anomalies, and respond to threats effectively.
This approach is essential for securing remote access systems and preventing unauthorized intrusions.



