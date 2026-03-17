# DNS Log Analysis & Threat Detection Using Splunk

## Introduction

DNS (Domain Name System) logs play a key role in monitoring network activity and uncovering potential security threats. By analyzing DNS traffic, security analysts can identify suspicious behavior such as unusual domain queries, malware communication, and data exfiltration attempts.

Using Splunk, we can efficiently ingest, search, and analyze DNS logs to detect anomalies and gain actionable security insights.

## Prerequisites

Before starting the analysis, make sure:

- Splunk is installed and running  
- DNS log sources are configured to send data to Splunk  
- Basic understanding of DNS log fields (IP, query, response, etc.)

## Uploading DNS Log Data into Splunk

### 1. Prepare DNS Log Files

- Collect sample DNS log files (e.g., `.log` or `.txt` format)  
- Ensure logs include:
  - Source IP  
  - Destination IP  
  - Domain Name (Query)  
  - Query Type  
  - Response Code  
- Store the files in a location accessible to Splunk  

### 2. Add Data to Splunk

- Log in to the Splunk Web interface  
- Go to **Settings → Add Data**  
- Choose **Upload** as the data input method  

### 3. Select the Log File

- Click **Select File**  
- Upload the prepared DNS log file  

### 4. Configure Source Type

- Assign the correct source type:
  - Example: `dns`  
- Or create a custom source type if needed  

### 5. Review Configuration

Verify the following settings:

- Index  
- Host  
- Source Type  

Ensure they correctly match your log format  

### 6. Upload the Data

- Click **Review**  
- Confirm all configurations  
- Click **Submit** to ingest the logs into Splunk  

### 7. Validate Data Ingestion

- Go to the **Search & Reporting** section  
- Run the following query:

```
index=<your_dns_index> sourcetype=<your_dns_sourcetype>
```

## Steps to Analyze DNS Log Files in Splunk SIEM

### 1. Search for DNS Events

Open the Splunk interface and navigate to the **Search & Reporting** section.

Enter the following query to retrieve DNS logs:

```
index=main sourcetype=dns_logs
```
### 2. Identify Top DNS Clients

Find which systems are generating the highest number of DNS requests.

```
index=main sourcetype=dns_logs
| stats count by src_ip
| sort -count
```

**This helps identify:**

- High-traffic hosts
- Potentially infected machines generating excessive DNS requests

### 3. Analyze Top Queried Domains 

Identify the most frequently requested domains in the network.

```
index=main sourcetype=dns_logs
| stats count by query
| sort -count
```

**This helps:**

- Understand user behavior
- Detect suspicious or unusual domains

### 4. Detect Suspicious Queries

Search for malformed or unusual DNS queries.

```
index=main sourcetype=dns_logs query="*\x00*"
```

**These queries may indicate:**

- Malformed packets
- Scanning activity
- Potential exploitation attempts

### 5. Analyze Broadcast Traffic

Identify DNS or NetBIOS broadcast traffic in the network.

```
index=main sourcetype=dns_logs dest_ip=192.168.202.255
```

**This helps detect:**

- Network discovery activity
- Internal scanning behavior
- Misconfigured systems

**Conclusion**

Analyzing DNS logs in Splunk SIEM enables the identification of unusual network activity, detection of suspicious queries, and effective monitoring of DNS traffic patterns. This approach plays a crucial role in uncovering potential threats such as malware communication, network scanning, and system misconfigurations.
