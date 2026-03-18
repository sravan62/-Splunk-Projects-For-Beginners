#  FTP Log Analysis & Threat Detection Using Splunk

##  Introduction

FTP (File Transfer Protocol) logs are crucial for monitoring file transfers, user activity, and potential security threats within a network. By analyzing FTP logs, security analysts can detect suspicious behaviors such as unauthorized access, malware file transfers, brute force attempts, and abnormal file operations.

Using Splunk, we can efficiently ingest, search, and analyze FTP logs to uncover anomalies and gain actionable security insights.

---

##  Prerequisites

Before starting the analysis, make sure:

- Splunk is installed and running  
- FTP log sources are available (log files or live logs)  
- Basic understanding of FTP log fields (IP, commands, status codes, etc.)  

---

##  Uploading FTP Log Data into Splunk

### 1. Prepare FTP Log Files

- Collect sample FTP log files (e.g., `.log` or `.txt` format)  
- Ensure logs include:
  - Source IP  
  - Destination IP  
  - Username  
  - FTP Commands (RETR, STOR, DELE, PORT, PASV)  
  - File paths / URLs  
  - Status codes  
  - Server responses  
- Store the files in a location accessible to Splunk  

---

### 2. Add Data to Splunk

- Log in to the Splunk Web interface  
- Go to **Settings → Add Data**  
- Choose **Upload** as the data input method  

---

### 3. Select the Log File

- Click **Select File**  
- Upload the prepared FTP log file  

---

### 4. Configure Source Type

- Assign the correct source type:
  - Example: `ftp_logs`  
- Or create a custom source type if needed  

---

### 5. Review Configuration

Verify the following settings:

- Index (e.g., `main`)  
- Host  
- Source Type  

Ensure they correctly match your log format  

---

### 6. Upload the Data

- Click **Review**  
- Confirm all configurations  
- Click **Submit** to ingest the logs into Splunk  

---

### 7. Validate Data Ingestion

- Go to the **Search & Reporting** section and run:

```
index=main sourcetype=ftp_logs
```
## Steps to Analyze FTP Log Files in Splunk SIEM

### 1. Search for FTP Events

Open Splunk and navigate to the **Search & Reporting** section.

Run:

```
index=main sourcetype=ftp_logs
```
### 2. Identify Top FTP Clients (Source IPs)

Find which systems are generating the most FTP activity.
```
index=main sourcetype=ftp_logs
| stats count by src_ip
| sort -count
```
This helps identify:
- High-traffic clients
- Potential attackers or compromised systems

### 3. Analyze FTP Commands Usage

Identify the most frequently used FTP commands.
```
index=main sourcetype=ftp_logs
| stats count by command
| sort -count
```
This helps:
- Understand user behavior
- Detect abnormal command usage

### 4. Detect Suspicious File Downloads

Search for executable file downloads.
```
index=main sourcetype=ftp_logs command=RETR
| search file="*.exe"
```
These may indicate:
- Malware downloads
- Unauthorized file access

### 5. Detect Suspicious File Uploads

Identify file upload attempts.
```
index=main sourcetype=ftp_logs command=STOR
| stats count by src_ip, file
```
This helps detect:
- Unauthorized uploads
- Hidden or suspicious files (e.g., .ftpduBnga4)

### 6. Detect Unauthorized or Anonymous Access
```
index=main sourcetype=ftp_logs user=anonymous
| stats count by src_ip
```
This helps identify:
- Weak authentication configurations
- Unauthorized access attempts

### 7. Analyze Failed Operations

Detect failed FTP operations using status codes.
```
index=main sourcetype=ftp_logs status_code=550
| stats count by src_ip
| sort -count
```
This helps detect:
- Brute force attempts
- Unauthorized file access
- Attack behavior

### 8. Detect Access to Sensitive Directories
```
index=main sourcetype=ftp_logs
| search file="*.ssh*" OR file="*.cache*" OR file="*dept*"
```
This helps detect:
- Directory traversal attempts
- Access to sensitive system paths

### 9. Analyze FTP Mode Usage (PORT & PASV)
```
index=main sourcetype=ftp_logs command=PASV OR command=PORT
| stats count by src_ip
```
This helps identify:
- Abnormal FTP session behavior
- Possible exploitation attempts

# Conclusion

Analyzing FTP logs in Splunk SIEM enables the detection of suspicious file transfers, unauthorized access, and abnormal command usage. This approach helps security analysts monitor FTP activity, identify anomalies, and respond to potential threats effectively.
By leveraging Splunk’s powerful search capabilities, organizations can enhance visibility into FTP traffic and strengthen their overall security posture.
