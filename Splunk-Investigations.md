# 🔍 Splunk Cloud: Scenario-Based Investigations, Alerts & Dashboards  

## 📖 Overview
This is a follow up project to my initial Splunk Cloud setup with Universal Forwarders on both a Windows & Linux machine. In this project, I aim to simulate real-world security scenarios, perform further SPL queries, and create alerts and dashboards, to develop a deeper understanding of Splunk and to gain hands-on experience. To accompany this documentation, I will refer to the MITRE ATTACK framework, referencing real world examples for prevention and detection strategies. 

I will then leverage the knowledge gained to assist me in the Blue Team Level 1 certifcation exam. 

## 🎯 Goals
- Simulate security events for log analysis and threat detection.
- Perform more advanced SPL searches to hands-on experience and development.
- Create real-time alerts for security incidents.  
- Utilise the MITRE ATTACK Framework to align with real-world adversary tactics.
- Develop dashboards for proactive log monitoring and visualisation.

### 🔍 MITRE ATT&CK Framework Integration  
This project also allowed me to gain essential experience using MITRE ATT&CK. By structuring our tests and detections based on MITRE techniques, I can ensure our Splunk implementation mirrors industry best practices for security monitoring. Below are the MITRE techniques referred to in this project, which I carried out during the practical tests. 

| **Tactic** | **MITRE Technique** | **Simulated Test** | **Expected Detection** |
|------------|------------------|------------------|------------------|
| **Credential Access** | `T1003.008 - OS Credential Dumping` | Unprivileged user attempts to read `/etc/shadow` | `index=linux_logs sourcetype=linux:audit file IN ("/etc/passwd", "/etc/shadow")` |
| **Persistence** | `T1098 - Account Manipulation` | New admin account creation | `index=linux_logs sourcetype=linux:auth EventCode=4720` |

## Project walk-through
This section provides a step-by-step breakdown of the process followed in this follow-up Splunk project. It demonstrates my enthusiasm for learning industry-relevant tools and developing the skills essential for an aspiring cybersecurity professional. Additionally, this serves as a learning resource that I can refer back to as I continue expanding my expertise.

### 1. Carrying out scenario-based Security Events in Linux
Generating security incidents to later analyse in Splunk. Overview of each security event and commands used.

### 1.1 OS Credential Dumping
- Unprivileged user attempts to read `/etc/passwd` (ID: T1003.008)

### 1.2 Configure AuditD

For this test, I had to configure and enable AuditD. This audit logs, I could send changes to files such as `/etc/passwd` and `/etc/shadow` from our UF to Splunk Cloud. On our Linux machine as splunkadmin:
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

### 1.3 Simulating unauthorised access
Here I simulated access attempts from our non-admin user account to `/etc/shadow`. This is a system file in Linux that stores encrypted user passwords and is accessible only to the root user. Attempted access could be a potential security risk, and something we can simulate and take proactive action against in this project.

SPL Query: index=linux_logs sourcetype=linux:audit "shadow-access" success=no

![image](https://github.com/user-attachments/assets/e7b714da-e936-41f6-97aa-25f829549742)

- By expanding on these log entries, we are able to identify valuable information about this event.

| **Field**      | **Value**                           | **Explanation** |
|---------------|-----------------------------------|----------------|
| **`_time`**   | `2025-03-18T12:32:06.728+00:00`   | Timestamp of the event. |
| **`UID`**     | `badguy`                          | The user attempting access. |
| **`key`**     | `shadow-access`                   | AuditD rule matched (attempt to access `/etc/shadow`). |
| **`success`** | `no`                              | Access attempt **failed**. |
| **`exit`**    | `-13`                             | **Permission Denied** (`EACCES` error). |

- This confirms that `badguy` attempted to access `/etc/shadow` but was denied due to insufficient permissions. Referring to the MITRE ATT&CK Framework (T1003.008 - /etc/shadow Credential Dumping), we can enhance our detection capabilities by implementing Detection Strategy `DS0022`, on File Access.

![image](https://github.com/user-attachments/assets/930f0466-d531-4f53-b8b4-5c43176f980c)

### 1.4 Creating the alert
Now to create the alert in Splunk. After performing the SPL query, we can click save-as alert.

![image](https://github.com/user-attachments/assets/c8d4ada7-4c75-43fa-ba7c-b44d16b51490)

The alert was then created with an appropriate title and description. The cron job was set to every 5 minutes. I left the expiration at 24 hours as we are only testing. The trigger conditions were set so we would receive an alert each time the conditions were met. 

![image](https://github.com/user-attachments/assets/a62dd5ba-e742-49ee-bc7d-2a5ccd2893c4)

The alert was then configured to send to my email address (left blank for privacy reasons). With an appropriate subject and priority level.

![image](https://github.com/user-attachments/assets/eb505393-b72e-49eb-8dac-09b4521421ff)

![image](https://github.com/user-attachments/assets/718d4327-c957-46a3-b5eb-3d37e482ca73)

### 1.5 Triggering the alert.
Back in my Linux machine, I once again attempted to access `/etc/shadow` from our non-admin user. This successfully generated an alert, and I was notified via email!

![image](https://github.com/user-attachments/assets/8bbb2b95-19ea-4d31-ae95-fd5386a06db6)

### 2. Carrying out scenario-based Security Events in Linux pt.2

### 2.1 Account Manipulation


