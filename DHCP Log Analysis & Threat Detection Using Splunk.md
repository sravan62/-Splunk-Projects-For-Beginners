# DHCP Log Analysis & Threat Detection Using Splunk

##  Introduction
DHCP (Dynamic Host Configuration Protocol) logs are essential for tracking IP address assignments and monitoring devices joining a network. By analyzing DHCP logs, security analysts can detect suspicious activities such as:

- Unauthorized device connections
- IP conflicts
- Rogue DHCP servers
- Abnormal network behavior

Using **Splunk**, we can ingest, search, and analyze DHCP logs to identify anomalies and gain actionable security insights.

---

##  Prerequisites
Before starting the analysis, ensure:

- Splunk is installed and running
- DHCP log sources are available (log files or live logs)
- Basic understanding of DHCP concepts (IP leasing, MAC address, DHCP server)

---

##  Uploading DHCP Log Data into Splunk

### 1. Prepare DHCP Log Files
Collect DHCP log files (e.g., `.log` or `.txt` format) and ensure they include:

- Timestamp
- Client IP address
- MAC address
- Assigned IP address
- DHCP Server IP
- Lease time

### 2. Add Data to Splunk
1. Log in to Splunk Web
2. Go to `Settings → Add Data`
3. Choose `Upload`

### 3. Select the Log File
- Click **Select File**
- Upload your DHCP log file

### 4. Configure Source Type
- Assign a source type, e.g., `dhcp_logs`
- Or create a custom source type

### 5. Review Configuration
Verify the following:

- **Index** (e.g., `main`)
- **Host**
- **Source Type**

### 6. Upload the Data
- Click **Review → Submit**

### 7. Validate Data Ingestion
Use the following Splunk search query:

```
index=main sourcetype=dhcp_logs
```
### Steps to Analyze DHCP Log Files in Splunk SIEM
1. Search for DHCP Events
```
index=main sourcetype=dhcp_logs
```
3. Identify Devices on the Network
```
index=main sourcetype=dhcp_logs
| stats count by mac_address, assigned_ip
```
- Tracks devices connecting to the network
- Helps identify unknown or suspicious devices

3. Track IP Address Assignments
```
index=main sourcetype=dhcp_logs
| stats count by assigned_ip, mac_address
```
- Maps devices to IP addresses
- Monitors asset activity

4. Detect IP Conflicts
```
index=main sourcetype=dhcp_logs
| stats dc(mac_address) as unique_macs by assigned_ip
| where unique_macs > 1
```
- Detects multiple MAC addresses assigned the same IP
- Useful to identify spoofing or misconfiguration

5. Detect Rogue Devices (High Activity)
```
index=main sourcetype=dhcp_logs
| stats count by mac_address
| where count > 10
```
- Identifies devices requesting IPs frequently
- Can indicate DHCP starvation attacks

6. Detect Rogue DHCP Servers
```
index=main sourcetype=dhcp_logs
| stats count by server_ip
```
- Identifies unauthorized DHCP servers in the network

7. Track Activity Trends
```
index=main sourcetype=dhcp_logs
| timechart count
```
- Visualizes network activity over time
- Detects spikes or unusual patterns
