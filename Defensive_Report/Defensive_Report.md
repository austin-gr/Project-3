# Blue Team: Summary of Operations

## Table of Contents
- Network Topology
- Description of Targets
- Monitoring the Targets
- Patterns of Traffic & Behavior
- Suggestions for Going Further

### Network Topology

The following machines were identified on the network:
- Capstone
  - **Operating System**: Ubuntu (Linux)
  - **Purpose**: Vulnerable Web Server, Filebeat and Metricbeat are installed and will forward logs to ELK machine
  - **IP Address**: 192.168.1.105
- ELK
  - **Operating System**: Ubuntu (Linux)
  - **Purpose**: ELK monitoring stack; monitor logs for network activity
  - **IP Address**: 192.168.1.100
- Kali
  - **Operating System**: Debian Kali (Linux)
  - **Purpose**: Penetration Testing
  - **IP Address**: 192.168.1.90
- Target 1
  - **Operating System**: Debian GNU/Linux 8
  - **Purpose**: Vulnerable WordPress Host
  - **IP Address**: 192.168.1.110

![DefensiveReport_Alerts2.png](/Images/DefensiveReport_Alerts_2.png)

### Description of Targets

The target of this attack was: `Target 1` (192.168.1.110).

Target 1 is an Apache web server and has SSH enabled, so ports 80 and 22 are possible ports of entry for attackers. As such, the following alerts have been implemented:

### Monitoring the Targets

Traffic to these services should be carefully monitored. To this end, we have implemented the alerts below:

#### Excessive HTTP Errors

Excessive HTTP Errors is implemented as follows:
  - **Metric**: count() GROUPED OVER top 5 'http.response.status_code' FOR THE LAST 5 minutes
  - **Threshold**: IS ABOVE 400
  - **Vulnerability Mitigated**: Enumeration / Brute Force
  - **Reliability**: This alert is highly reliable. Filtering out error codes 400 and above filters out the normal activity/successful responses you would see normally. Anything above error code 400 is a cause for concern.

#### HTTP Request Size Monitor
HTTP Request Size Monitor is implemented as follows:
  - **Metric**: sum() of http.request.bytes OVER all documents FOR THE LAST 1 minute
  - **Threshold**: IS ABOVE 3500
  - **Vulnerability Mitigated**: Code injection in HTTP requests or DDoS
  - **Reliability**: This alert has the potential to create false positives, therefore rate it as having medium reliability. It is possible there is a large number of legitimate HTTP traffic that happens, so it isn't a "highly" reliable alert as of now.

#### CPU Usage Monitor
CPU Usage Monitor is implemented as follows:
  - **Metric**: max() OF system.process.cpu.total.pct OVER all documents FOR THE LAST 5 minutes
  - **Threshold**: IS ABOVE 0.5
  - **Vulnerability Mitigated**: Malicious software, malware/viruses that are running and taking up space
  - **Reliability**: This alert is highly reliable. This alert can serve as a helper, in the fact that it can show you where your CPU can improve its usage even without malicious programs running.
