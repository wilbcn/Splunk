# ☁️ Leveraging Splunk Cloud for log aggregation and analysis

## 📖 Overview
This documentation outlines the process of configuring **Splunk Cloud** to receive and analyze log data from both **Windows** and **Linux** instances using **Universal Forwarders (UF)**. The goal of this project is to develop hands-on experience with **log aggregation, security monitoring, and Splunk Processing Language (SPL)** within my **HomeLab setup**.  

## 🎯 Goals
- **Ingest and analyse system logs** from Windows and Linux endpoints.  
- **Perform scenario-based investigations** to detect and respond to security events.  
- **Utilise Splunk’s Searching & Reporting app** to query, visualize, and analyze data.  
- **Integrate AWS CloudWatch logs** to explore cloud-based security monitoring. 

### Learning & Practical Applications  
- Hands-on experience with **Splunk Universal Forwarders**.  
- Understanding **SPL queries** for event correlation and threat detection.  
- Investigating **custom scenario-based log data** for security analysis.  
- Expanding knowledge in **cloud security services (AWS CloudWatch & CloudTrail)**.  

## Project walk-through
This section provides a step-by-step breakdown of the steps taken in order to carry out this fundemental project with Splunk. The Splunk Cloud instance will act as the Indexer and the Search Head in our setup.

### 1. Configure Universal Forwader on Windows End-Point
In this section, I explain how I configured a Universal Forwarder on our EC2 instance running windows. 
- On my windows VM, I set up a new account on Splunk and launched the Splunk Cloud Platform as an administrator.
- Once logged in, I went to **Universal Forwarder** on the left hand side, and downloaded the customised universal forwarder credentials package. This package ensures a secure and authenticated connection between our end points, and the splunk cloud platform.

![image](https://github.com/user-attachments/assets/1b30f6e3-c4a0-44b5-be9f-5b63e4e95782)

![image](https://github.com/user-attachments/assets/8062bb13-c00b-4ff9-918e-7f9e5017325e)

- Then I headed to [Splunk Universal Forwarder Download](https://www.splunk.com/en_us/download/universal-forwarder.html) and got the free download of the Universal Forwarder. I selected this one as it matches our Windows EC2 architecture.

![image](https://github.com/user-attachments/assets/2e675de7-5e09-4d73-94c2-420cc7b980e2)

- Both files are now prepared and ready on my desktop.

![image](https://github.com/user-attachments/assets/a8f3e39c-18d9-4fc7-991d-8cc5b6b44f3a)

- I Ran the installer file (.msi), and configured it with the following settings.
    - Splunk Cloud instance
    - Install UF as a local system (So Splunk UF has access to all required system logs and events etc.)
    - Enable all log sources, so we gain maximum visiblity into the system for our security analysis later.
    - Create an admin account that will manage the UF.
    - Then hit install.

- Now I opened the command prompt as administator, and installed the credentials package to enable a secure connection to our cloud deployment.

![image](https://github.com/user-attachments/assets/9da572a6-60e9-499d-b854-d696184beb6c)

- Then I restarted the splunk forwarder. Restarting the UF is necessary to apply configuration changes and establish the connection to Splunk Cloud.

![image](https://github.com/user-attachments/assets/10e6a383-57fc-41db-a43b-eeb102c26706)

- Now back on the Cloud platform, I ran a query to test for successful data integration from our windows machine. 

![image](https://github.com/user-attachments/assets/3c7ba20c-0e11-4c9c-a7d2-05f45bf403dc)

- Look at all the data coming in! Successful data ingestion verifies proper configuration of the UF and demonstrates Splunk Cloud’s capability to collect, index, and search data from remote sources in near-real-time.
- I have now configured an AWS EC2 Windows instance to simulate an endpoint used by an end-user for daily tasks. The Splunk Universal Forwarder (UF) is installed on this instance to continuously collect and forward log data—including system events and user activity—directly to Splunk Cloud.

### 2. Configure Universal Forwader on Linux End-Point

### 3. Carry out basic searches
