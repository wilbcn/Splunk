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
Generating security incidents to later analyse in Splunk. Overview of each security event and commands used.

### 1.1 Technique Overview
A new administrator account is created, that also has a suspicious name. [T1098 - Account Manipulation](https://attack.mitre.org/techniques/T1098/)

### 1.2 Simulating the security event - New Admin creation
- In control panel, I navigated to User Accounts, User Accounts, Manage Accounts, and Add a user account
- I then created a new user account, opting for an 'IT-Looking' username, which a real attacker may use in order to blend in.
- Going into the settings for our new user, I then changed the account type to Administrator.

<img width="1125" alt="image" src="https://github.com/user-attachments/assets/04c2bc4c-9ec2-4b2e-a856-dd34a26b0187" />

### 1.3 Initial SPL search
Now that _suspicious_ admin has been created, I ran an initial query in Splunk to verify we were generating logs for this event.

```
index=* sourcetype="WinEventLog:Security" (EventCode=4720 OR EventCode=4732)
```

![image](https://github.com/user-attachments/assets/ebc94f68-2b76-45e8-97e3-028c6a433ee2)

By expanding on these logs, we can see that the account `Administrator` has created a new user, called `system_admin`, at `03/24/2025 07:09:46 PM`

![image](https://github.com/user-attachments/assets/dc8580d1-5463-49d1-b845-ac365c3dcdc4)

This is a security concern for multiple reasons: 
- The name `system_admin` follows a suspicious naming pattern that an attacker might use to blend in.
- The account was created outside of regular working hours, which could indicate unauthorised activity.

Expanding on the next log, we are able to see than an account has been added to a `security-enabled local group`

![image](https://github.com/user-attachments/assets/92bb5224-c58d-4cd4-94f4-371f4bb0b758)

While this security event was logged in Splunk, `Event ID `4732` did not contain the resolved username of the account added to the Administrators group. 

### 1.4 Verifying in Windows Event Viewer
To confirm which user was added to the Administrators group, I used the Event Viewer directly on the Windows Machine. [Acknowledgement](https://www.blumira.com/blog/event-id-4732)

In Event Viewer, I applied a custom filter to show only Event ID `4732` from the last 24 hours:

<img width="546" alt="image" src="https://github.com/user-attachments/assets/906e3464-e97a-4464-a5b4-1f253e7512d0" />

This allowed me to confirm that `system_admin`, the newly created user account, was also added to the group `Administrators`.

<img width="1437" alt="image" src="https://github.com/user-attachments/assets/658a5430-38ee-4a0c-8d07-76bd56d6cd30" />

### 2. Place holder


