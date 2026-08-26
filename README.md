# Wazuh-Project


# Wazuh SIEM Lab

## Overview

This project documents the deployment and configuration of a Wazuh-based security monitoring lab. The environment will be used to collect endpoint telemetry, create and test detections, investigate security events, and simulate activity that may be encountered by a SOC analyst.

## Wazuh Server

The Wazuh server was deployed on an Ubuntu Server 24.04 LTS virtual machine.

### Initial Configuration

- Installed Ubuntu Server 24.04 LTS
- Enabled OpenSSH during installation for remote administration
- Installed Wazuh

The Ubuntu VM will act as the central Wazuh server for the lab, receiving and analysing telemetry from endpoints added later in the project.


## Accessing the Wazuh Dashboard

After installation, the Wazuh web interface was accessed through a browser using the IP address of the Wazuh server.

The dashboard provides the main interface for monitoring the environment, viewing security alerts, investigating events, managing agents, and configuring Wazuh.

<img width="2559" height="1116" alt="image" src="https://github.com/user-attachments/assets/599fa94b-eaa1-43a7-aa89-fa3951b8c37a" />
