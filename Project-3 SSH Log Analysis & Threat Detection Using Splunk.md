# SSH Log Analysis & Threat Detection Using Splunk

## 📌 Introduction

SSH (Secure Shell) logs are critical for monitoring remote access, authentication attempts, and system activity. By analyzing SSH logs, security analysts can detect suspicious behavior such as brute force attacks, unauthorized logins, privilege escalation, and lateral movement.

Using Splunk, we can efficiently ingest, search, and analyze SSH logs to uncover anomalies and gain actionable security insights.

---

## ⚙️ Prerequisites

Before starting the analysis, make sure:

- Splunk is installed and running  
- SSH log sources are available (e.g., `/var/log/auth.log` or `/var/log/secure`)  
- Basic understanding of SSH logs (login attempts, users, IPs, etc.)  

---

## 📥 Uploading SSH Log Data into Splunk

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

```spl
index=main sourcetype=ssh_logs
