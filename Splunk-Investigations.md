# 🔍 Splunk Cloud: Scenario-Based Investigations, Alerts & Dashboards  

## 📖 Overview
This is the follow up project to my initial Splunk Cloud setup with Universal Forwarders on both a Windows & Linux machine. In this project, I aim to carry simulate real-world security scenarios, perform further SPL queries, and create alerts and dashboards, to develop a deeper understanding of Splunk and to gain hands-on experience. This will also serve me well as I soon intend to take the Blue Team Level 1 certifcation exam. 

## 🎯 Goals
- Simulate security events for log analysis and threat detection.
- Perform more advanced SPL searches to hands-on experience and development.
- Create real-time alerts for security incidents.  
- Develop dashboards for proactive log monitoring and visualisation.
- Utilise the MITRE ATTACK Framework to align with real-world adversary tactics.

### Learning & Practical Applications  
- Building scenario-based investigations for real-world security analysis.  
- Learning SPL techniques for anomaly detection & log correlation.  
- Automating security monitoring with alerts & scheduled reports.  
- Designing Splunk dashboards for continuous security insights.

### 🔍 MITRE ATT&CK Framework Integration  
This project also allows me to gain essential experience using MITRE ATT&CK. By structuring our tests and detections based on MITRE techniques, I can  ensure our Splunk implementation mirrors industry best practices for security monitoring.  

| **Tactic** | **MITRE Technique** | **Simulated Test** | **Expected Detection** |
|------------|------------------|------------------|------------------|
| **Credential Access** | `T1003.008 - OS Credential Dumping` | Unprivileged user attempts to read `/etc/shadow` | `index=linux_logs sourcetype=linux:audit file IN ("/etc/passwd", "/etc/shadow")` |
| **Defense Evasion** | `T1078 - Valid Accounts` | Privilege escalation via `sudo` group addition | `index=linux_logs sourcetype=linux:auth "usermod -aG sudo"` |
| **Persistence** | `T1098 - Account Manipulation` | New admin account creation | `index=linux_logs sourcetype=linux:auth EventCode=4720` |
| **Initial Access** | `T1110 - Credential Brute Force` | Multiple failed SSH login attempts | `index=linux_logs sourcetype=linux:auth "Invalid user"` |
| **Execution** | `T1059 - Command and Scripting Interpreter` | Running a suspicious script | `index=linux_logs sourcetype=syslog "malware.sh"` |

## Project walk-through
This section provides a step-by-step breakdown of the process followed in this follow-up Splunk project. It demonstrates my enthusiasm for learning industry-relevant tools and developing the skills essential for an aspiring cybersecurity professional. Additionally, this serves as a learning resource that I can refer back to as I continue expanding my expertise.

### 1. Carrying out scenario-based Security Events
Generating security incidents to later analyse in Splunk. Overview of each security event and commands used.

### 1.1 Linux Endpoint
- Unprivileged user attempts to read /etc/passwd (ID: T1003.008)

### 1.2 Configure AuditD

For this test, I had to configure and enable AuditD. This audit logs, I could send changes to files such as /etc/passwd and /shadow from our UF to Splunk Cloud. On our Linux machine as splunkadmin:
- Install and Enable AuditD
```
sudo apt update && sudo apt install auditd -y
sudo systemctl start auditd  
sudo systemctl enable auditd
```

- Verify AuditD is Running
```
sudo systemctl status auditd
```

-  Add Audit Rules for Unauthorised Access to /etc/passwd and /shadow
```
sudo auditctl -w /etc/passwd -p r -k passwd-access
sudo auditctl -w /etc/shadow -p r -k shadow-access
```

- Check Active Audit rules
```
sudo auditctl -l

-w /etc/passwd -p r -k passwd-access
-w /etc/shadow -p r -k shadow-access
```

- Persist Audit Rules (append to bottom of file)
```
sudo vi /etc/audit/rules.d/audit.rules

-w /etc/passwd -p r -k passwd-access
-w /etc/shadow -p r -k shadow-access
```

- Apply the changes
```
sudo systemctl restart auditd
```

- Forward AuditD Logs to Splunk
```
sudo nano /opt/splunkforwarder/etc/system/local/inputs.conf

[monitor:///var/log/audit/audit.log]
index = linux_logs
sourcetype = linux:audit
```

- Restart splunk UF
```
sudo systemctl restart SplunkForwarder
```

### 1.3 Carry out unauthorised access
Here I simulated access attempts





### 2. SPL Searches & Threat Hunting
Carrying out SPL searches to identify the generated security events in Splunk.

### 3. Creating Alerts & Reports in Splunk
Hands-on experience creating alerts and reports in Splunk.

### 4. Building Splunk Dashboards
Creating splunk dashboards for proactive threat response.

### 5. SPL Glossary
Summary of commands used including command details.

### 6. Overview
Overview of the project and lessons learned.
