# Threat Detection and Security Operations with AWS Cloud and ELK-Stack


## Project Overview

This repository contains a series of hands-on labs demonstrating expertise in cloud deployment, cybersecurity monitoring, and threat detection. The project leverages a SIEM solution built with the ELK Stack (Elasticsearch, Logstash and Kibana), and Grafana on Amazon Web Servicve (AWS Cloud) to centralize, analyze, and visualize security data. Additionally, it integrates Suricata IDS/IPS rules to intercept and block malware traffic, showcasing both detection and prevention capabilities in a simulated cloud environment. The labs simulate real-world security environments where detection, logging, and monitoring are critical.  


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
Suricata was configured for real-time threat detection.
It analyzes traffic against rule sets to detect intrusions, malware, or suspicious activity.

To enhance visibility and analyst efficiency, EveBox was integrated as a GUI for Suricata’s eve.json logs. Suricata generates alerts and event logs in JSON format (eve.json), which EveBox processes and displays in a clean, web-based dashboard.

EveBox provides features such as search, filtering, drill-down investigation, and case management (e.g., escalating or archiving alerts). It also supports Elasticsearch as a backend, enabling scalable event storage and querying.

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
<img width="1065" height="465" alt="Screenshot (541)" src="https://github.com/user-attachments/assets/8710c435-3277-4d44-8d00-967de88622bf" />


<img width="1139" height="581" alt="Screenshot (539)" src="https://github.com/user-attachments/assets/5a8f7d9e-268e-4b12-99cc-ef94c35b7220" />

<img width="920" height="541" alt="Screenshot (540)" src="https://github.com/user-attachments/assets/e71fb846-c874-46c4-ac7d-df6064cbfdca" />

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

<img width="1179" height="554" alt="Screenshot (542)" src="https://github.com/user-attachments/assets/8c653e40-93de-4222-8fee-8eecfa4c0e8b" />


<img width="1238" height="587" alt="Screenshot (546)" src="https://github.com/user-attachments/assets/1c492e2f-7f92-4635-b0d0-e51b3a1bfd3e" />


<img width="1235" height="602" alt="Screenshot (547)" src="https://github.com/user-attachments/assets/9a5e71d7-5410-48f8-afcb-f78f743a04b7" />

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

<img width="1189" height="562" alt="Screenshot (548)" src="https://github.com/user-attachments/assets/7d337729-f8d0-4a5b-b80e-2248856d615b" />

<img width="827" height="642" alt="Screenshot (549)" src="https://github.com/user-attachments/assets/ca1ab220-baaa-4078-b5f9-7dda40cedee4" />



<img width="1071" height="471" alt="Screenshot (550)" src="https://github.com/user-attachments/assets/1b32e497-328e-4736-b52a-2ef60bf93647" />



# 4. Suricata IDS & EveBox GUI Setup

**Objective:**  
Detect and alert on malicious activity in real-time while providing a user-friendly interface for monitoring and investigation.

---

## Installation & Commands

```bash
# Install Suricata IDS
sudo apt-get install suricata -y

# Start Suricata in live mode
sudo suricata -c /etc/suricata/suricata.yaml -i eth0

# Tail alerts
tail -f /var/log/suricat<img width="989" height="209" alt="Screenshot (551)" src="https://github.com/user-attachments/assets/f3003337-3232-43ba-8b6f-043f080f5912" />
a/eve.json
```

<img width="989" height="209" alt="Screenshot (551)" src="https://github.com/user-attachments/assets/86b38183-1ffe-486d-a63b-b65941b0f724" />

<img width="1179" height="520" alt="Screenshot (557)" src="https://github.com/user-attachments/assets/e84d8c65-0833-49d6-8c8b-70d8bf2f1938" />

<img width="1071" height="496" alt="Screenshot (556)" src="https://github.com/user-attachments/assets/f08f6217-166f-499d-9103-25d8df3aab3c" />



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
<img width="1429" height="431" alt="Screenshot (553)" src="https://github.com/user-attachments/assets/25df027f-5417-4219-b863-433400a139fd" />

<img width="1436" height="440" alt="Screenshot (554)" src="https://github.com/user-attachments/assets/c09ad4da-40bf-4eda-8275-30d781105918" />


<img width="1432" height="448" alt="Screenshot (555)" src="https://github.com/user-attachments/assets/970f5acc-c574-4fa7-ae28-69f433cc2871" />

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


