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


## Enabling Wazuh Archives

By default, Wazuh primarily focuses on events that match rules and generate alerts. For this lab, **archives were enabled so that Wazuh retains all received telemetry**, including events that do not trigger an alert.

This is useful in a SOC lab because an event may still be valuable during an investigation even if it did not initially match a detection rule. Retaining the raw telemetry also allows historical events to be searched, suspicious behaviour to be investigated, and new detection rules to be developed from previously collected data.

### Enable Archives on the Wazuh Manager

The Wazuh manager configuration file was modified:

```text
/var/ossec/etc/ossec.conf
```

The following settings were changed to `yes`:

```xml
<logall>yes</logall>
<logall_json>yes</logall_json>
```

- `logall` enables Wazuh to archive all received events.
- `logall_json` stores the archived events in JSON format, making them suitable for indexing and searching.

After making changes to `ossec.conf`, the Wazuh manager must be restarted for the configuration to take effect:

```bash
systemctl restart wazuh-manager.service
```

### Configure Filebeat

Although Wazuh is now archiving the events, Filebeat also needs to be configured to send the archived data to the Wazuh indexer so that it can be searched through the dashboard.

The Filebeat configuration file is located at:

```text
/etc/filebeat/filebeat.yml
```

I opened the configuration:

```bash
nano /etc/filebeat/filebeat.yml
```

Archive indexing was enabled by changing:

```yaml
archives:
  enabled: true
```

Filebeat was then restarted to apply the change:

```bash
systemctl restart filebeat
```

### Create the Archives Index Pattern

In the Wazuh web interface, I navigated to:

**Dashboard Management → Index Patterns**

There was no existing index pattern for the Wazuh archives, so I created:

```text
wazuh-archives-*
```
<img width="1240" height="532" alt="image" src="https://github.com/user-attachments/assets/5f17d3e6-b3dd-4d6d-a24d-91877200a9fb" />

<img width="1039" height="521" alt="image" src="https://github.com/user-attachments/assets/a75c6043-d11a-4c9e-80be-cf60f2eb7b0f" />

This makes the archived telemetry available for searching and analysis through the Wazuh dashboard.

### Result

Wazuh archives are now enabled and Filebeat is configured to index the archived events. The `wazuh-archives-*` index pattern was successfully created and the raw telemetry can now be accessed through the web interface.

<img width="2539" height="1262" alt="image" src="https://github.com/user-attachments/assets/3b8df5e5-ff5c-42d6-ab91-96dd0fe5f17f" />
