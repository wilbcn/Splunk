# 🔍 Splunk Cloud: Detecting Unauthorised Admin Account Creation

## 📖 Overview
This project is a continuation of my Splunk series, aimed at gaining hands-on experience with the Splunk platform and learning how to use the Splunk Processing Language (SPL) effectively to investigate and report on security events. 
In this follow-up, I explore and continue learning key Splunk features such as alerts** and dashboards, while aligning all detection and mitigation strategies with the MITRE ATT&CK framework to mirror real-world security operations.
This projects focuses on unauthorised administrator account creation and privilege escalation on a Windows machine — a critical persistence tactic used by adversaries (MITRE Technique: `T1098 - Account Manipulation`).

To carry out this project, I configured universal forwaders on some EC2 instances.
[Splunk Cloud Setup](https://github.com/wilbcn/Splunk/blob/main/Splunk-Cloud-HomeLab.md)

## 🎯 Goals  
✅ Detect the creation of new administrator accounts on a Windows system.  
✅ Gain hands-on experience with SPL to perform targeted threat investigations.  
✅ Build real-time alerts to notify of privilege escalation events.  
✅ Design a Splunk dashboard to visualize account manipulation activity.  
✅ Align all detection and mitigation techniques with the MITRE ATT&CK framework.  

---

### 🔍 MITRE ATT&CK Framework Integration  
This project strengthens my understanding of adversary behavior by simulating account manipulation techniques and aligning detection strategies with the MITRE ATT&CK framework. By leveraging SPL, alerts, and dashboards, I aim to replicate how a SOC analyst would monitor for and respond to suspicious privilege escalation attempts.

| **Tactic**            | **MITRE Technique**                | **Simulated Test**                              | **Expected Detection** |
|-----------------------|------------------------------------|-------------------------------------------------|-------------------------|
| **Persistence**       | `T1098 - Account Manipulation`     | Create a new admin account in Windows           | `index=* sourcetype="WinEventLog:Security" EventCode=4720` |
| **Detection Strategy**| `DS0028 - Account Creation Monitoring` | Alert on the creation of privileged user accounts | `index=* sourcetype="WinEventLog:Security" EventCode=4720 OR EventCode=4732` |
| **Mitigation Strategy**| `M1018 - User Account Management` | Implement strict user provisioning policies & monitor admin group membership | GPO enforcement, privileged group auditing |

---

## Project walk-through  
This section provides a step-by-step breakdown of the work carried out in this Splunk project. It demonstrates my ability to simulate adversarial tactics, detect privilege escalation using SPL, and take action through real-time alerts and visual dashboards. As with previous projects, this serves both as a practical security learning experience and a documented workflow I can refer to in the future.

### 1. Simulating Scenario-Based Security Events in Windows

