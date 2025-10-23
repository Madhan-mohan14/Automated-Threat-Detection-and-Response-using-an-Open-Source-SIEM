## Automated Threat Detection and Response using an Open-Source SIEM

A Capstone Project demonstrating a complete, automated security monitoring and response cycle using the open-source Wazuh platform.

## Project by:

MADHAN MOHAN NAIDU .M (22BCY10182)

REHAN RAJESH (22BCY10196)

V.A.AASWIN (22BCY10257)

INSAF SADIK A S (22BCY10282)

## Project Overview
This project implements a robust, cost-effective Security Information and Event Management (SIEM) system in a controlled virtual environment. The core of the system is built on Wazuh, an open-source platform that combines SIEM and Endpoint Detection and Response (EDR) capabilities.

The system is proven to detect a simulated SSH brute-force attack in real-time, correlate the data to identify a sustained threat, and automatically execute an Active Response to block the attacker's IP address, neutralizing the threat without human intervention.

For a complete and detailed explanation of the project, including the initial research, strategic pivot from the ELK stack, and in-depth analysis, please see the full Project Report PDF.

## Key Features
Real-time Threat Detection: Actively monitors system logs for suspicious activity.

**Log Correlation:** Intelligently correlates multiple low-level alerts (like authentication failed) into a single, high-severity attack alert.

**Automated Active Response:** Automatically blocks the attacker's IP address by triggering a firewall rule upon threat confirmation.

**File Integrity Monitoring (FIM):** Detects unauthorized changes to critical system files like /etc/passwd.

**Cost-Effective & Scalable:** Built entirely on open-source tools, providing an enterprise-grade solution without high licensing fees.

## Technical Architecture
The entire lab was built within Oracle VirtualBox using an isolated NAT Network named WazuhLabNet.

## How It Works: The Attack & Response Cycle
This flow demonstrates the project's primary objective:

**1. The Attack**
An SSH brute-force attack is launched from the Kali VM against the Ubuntu server using Hydra.

`![hydra attack](https://github.com/user-attachments/assets/00d47f3d-51a4-4f33-a53a-a4fe72bd341d)'

**2. Detection (Rule 5760)**
The Wazuh Manager instantly detects the individual failed login attempts, generating multiple Rule 5760 (sshd: authentication failed) alerts.


**3. Correlation (Rule 5720)**
After 6 failed attempts, the Manager's correlation engine escalates the event, triggering a single, high-severity Rule 5720 (sshd: Multiple authentication failures) alert. This confirms a sustained attack is in progress.


**4. Automated Response (Rule 651)**
This is the critical step. The Rule 5720 alert automatically triggers our Active Response configuration. The Wazuh server executes the firewall-drop script, blocking the attacker's IP (10.0.2.5). The system then generates a Rule 651 alert, providing a final confirmation that the automated response was successfully executed.



## Configuration & Key Features
Active Response Configuration
To enable the automated response, the following block was added to the Wazuh Manager's ossec.conf file, instructing it to run the firewall-drop command when Rule 5720 is triggered.

<ossec_config>
  ...
  <active-response>
    <command>firewall-drop</command>
    <location>local</location>
    <rules_id>5720</rules_id>
    <timeout>600</timeout>
  </active-response>
  ...
</ossec_config>

## File Integrity Monitoring (FIM)
The system also demonstrates powerful EDR capabilities by monitoring critical files. When a test file was created (passlist.txt) or a sensitive file was modified (/etc/passwd), the system generated immediate alerts.

Rule 554: File added to the system.

Rule 550: Integrity checksum changed.


## Technologies Used
**SIEM/EDR:** Wazuh 4.7.4 (Manager, Indexer, Dashboard)

**Hypervisor:** Oracle VirtualBox

**Server OS:** Ubuntu Server 22.04 LTS

**Endpoint OS:** Kali Linux

**Attack Tool:** THC-Hydra

## Future Scope
The next phase of this project would be to evolve the system into a full Security Orchestration, Automation, and Response (SOAR) platform by integrating TheHive (for incident management) and Cortex (for threat intelligence analysis), building upon our initial research with the ELK stack, VirusTotal, and Shodan.

## Acknowledgements
We would like to express our sincere gratitude to our project supervisor, Dr. Hariharasitaraman.S, for his invaluable guidance and support throughout this project.
