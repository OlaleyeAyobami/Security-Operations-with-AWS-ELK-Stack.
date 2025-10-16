# Threat-Detection-and-Security-Operations-with-AWS-and-ELK-Stack


## Project Overview

This repository contains a series of hands-on labs demonstrating expertise in **cloud deployment, cybersecurity monitoring, and threat detection**.  
The labs simulate real-world security environments where detection, logging, and monitoring are critical.  

It showcases **hands-on proficiency with modern cybersecurity tools, practical configurations, and threat detection techniques**, highlighting my skills in **cloud operations, security monitoring, and incident response**.

---

## Key Objectives

- Deploy and configure **AWS EC2 instances** as test environments.  
- Implement centralized logging and monitoring with the **ELK Stack**.  
- Capture network activity and detect anomalies using **Zeek**.  
- Conduct real-time intrusion detection with **Suricata IDS**.  
- Visualize alerts and logs using **Grafana dashboards**.  
- Integrate **Filebeat** to ship logs efficiently from multiple sources.  

---

## Lab Walkthrough

### 1. AWS EC2 Deployment
EC2 instances were deployed as isolated test environments to simulate a real-world cloud infrastructure.  
I configured networking, security groups, and instance settings to ensure safe and controlled environments for security testing.  
This demonstrates **cloud administration skills** and the ability to create **scalable, secure environments** for cybersecurity operations.

---

### 2. ELK Stack (Elasticsearch, Logstash, Kibana)
The ELK Stack was implemented to **centralize logs from multiple sources**.  
- **Elasticsearch** stores and indexes data efficiently.  
- **Logstash** processes and normalizes logs.  
- **Kibana** provides interactive dashboards for analysis.  

This setup shows my ability to implement **robust monitoring solutions** and manage **large-scale log data** for actionable insights.

---

### 3. Zeek (Network Traffic Analysis)
Zeek was used to **capture and analyze network traffic in real time**.  
By detecting anomalies and suspicious activities, I could identify potential threats and generate detailed logs for further analysis.  

This highlights skills in **network monitoring, anomaly detection, and threat intelligence**.

---

### 4. Suricata IDS (Intrusion Detection System)
Suricata was configured for **real-time threat detection**.  
It analyzes traffic against rule sets to detect intrusions, malware, or suspicious activity.  

This demonstrates my capability to implement **active defense mechanisms** and understand **security event correlation**.

---

### 5. Grafana Dashboards
Grafana was used to **visualize logs, alerts, and system metrics** from ELK, Zeek, and Suricata.  
Interactive dashboards were created for trend analysis and incident monitoring, showing my ability to **communicate complex security data in an accessible, visual format**.

---

### 6. Filebeat Integration
Filebeat was configured to **efficiently ship logs from multiple sources to Elasticsearch**.  
This ensures a reliable and scalable log collection pipeline, demonstrating expertise in **data ingestion, automation, and centralized monitoring** for security operations.

---


# 1. AWS EC2 Deployment

**Objective:**  
Deploy cloud-hosted VMs to act as monitoring nodes and simulate enterprise network assets.

---

## Architecture & Setup

- **OS:** Ubuntu 22.04 LTS  
- **Instance Type:** t2.micro (cost-effective lab testing)  
- **Security Groups:** SSH (22), HTTP (80), HTTPS (443), custom rules for IDS testing  
- **IAM Role:** EC2 Full Access (for monitoring and log shipping scripts)  

---

## Commands Example

```bash
# Launch an EC2 instance
aws ec2 run-instances \
  --image-id ami-xxxxxxxx \
  --count 1 \
  --instance-type t2.micro \
  --key-name MyKeyPair \
  --security-group-ids sg-xxxxxxxx \
  --subnet-id subnet-xxxxxxxx
```


## 2. ELK Stack Configuration

**Objective:**  
Centralize logs for easy search, analytics, and visualization.

---

## Components Deployed

- **Elasticsearch:** Stores structured log data.  
- **Logstash:** Processes logs from multiple sources.  
- **Kibana:** Dashboard for visualization and analytics.  

---

## Commands Example

```bash
# Install Elasticsearch
sudo apt-get update && sudo apt-get install elasticsearch -y

# Start Elasticsearch service
sudo systemctl enable elasticsearch
sudo systemctl start elasticsearch

# Verify service
curl -X GET "localhost:9200/"

```

# 3. Zeek Network Monitoring

**Objective:**  
Monitor network traffic and detect anomalous behavior.

---

## Configuration Example

```yaml
module: zeek
capture_loss:
  enabled: true
  var.paths: ["/opt/zeek/logs/current/capture_loss.log"]
connection:
  enabled: true
  var.paths: ["/opt/zeek/logs/current/conn.log"]
dce_rpc:
  enabled: true
  var.paths: ["/opt/zeek/logs/current/dce_rpc.log"]
```

# 4. Suricata IDS Setup

**Objective:**  
Detect and alert on malicious activity in real-time.

---

## Installation & Commands

```bash
# Install Suricata IDS
sudo apt-get install suricata -y

# Start Suricata in live mode
sudo suricata -c /etc/suricata/suricata.yaml -i eth0

# Tail alerts
tail -f /var/log/suricata/eve.json
```
# 5. Grafana Monitoring Dashboard

**Objective:**  
Visualize network events, IDS alerts, and system logs for security situational awareness.

---

## Steps to Setup

```bash
# Start Grafana
sudo systemctl start grafana-server
sudo systemctl enable grafana-server

# Access Grafana dashboard
http://<EC2-Public-IP>:3000
```

# 6. Filebeat Integration

**Objective:**  
Ship logs reliably to ELK Stack from multiple sources.

---

## Configuration Example

```yaml
filebeat.inputs:
  - type: log
    enabled: true
    paths:
      - /opt/zeek/logs/current/*.log
      - /var/log/suricata/eve.json

output.elasticsearch:
  hosts: ["http://localhost:9200"]
```
# 7. Threat Detection & Logging

**Integrated Security Workflow:**

- Zeek monitors network traffic and logs anomalies.  
- Suricata generates alerts for policy violations and attacks.  
- Filebeat ships all logs to Elasticsearch.  
- Kibana and Grafana visualize alerts and trends.  

**Outcome:**  
Provides full visibility of network and host events, supporting incident response and threat hunting.

---

# 8. Security Highlights & Challenges

- Detected simulated malware traffic using Suricata.  
- Captured network anomalies in Zeek logs.  
- Deployed centralized logging to simplify security monitoring.  
- Overcame log formatting and ingestion challenges by customizing Filebeat and Logstash pipelines.  

---

# Conclusion

This project demonstrates end-to-end capabilities in cloud security, SIEM implementation, EDR monitoring, and threat detection.  

It highlights hands-on proficiency in deploying secure cloud environments, integrating multiple monitoring and detection tools, and delivering scalable, maintainable, and actionable security operations workflows.  

These labs showcase practical expertise for roles in cybersecurity engineering, cloud security, SOC operations, and incident response.


