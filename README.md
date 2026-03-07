# SOC Lab: Port Scan Detection using Windows Firewall Logs

This project demonstrates how port scanning activity can be detected using Windows Firewall logs in a controlled home lab environment.

A Kali Linux machine performed an Nmap scan against a Windows 7 system.  
The Windows Firewall logging mechanism recorded repeated connection attempts to multiple ports from a single source IP address.

This pattern is consistent with reconnaissance activity often observed in the early stages of cyber attacks.

---

## Lab Environment

Attacker Machine:  
Kali Linux

Target Machine:  
Windows 7 (Virtual Machine)

Network:  
VirtualBox internal network

Detection Source:  
Windows Firewall logs

Tool Used: 
Nmap

---

# Attack Simulation

The attacker system (Kali Linux) performed a port scan against the Windows target.

Command used:

```bash
nmap -sT -Pn -p 1-200 192.168.100.25
```
# Evidence in Firewall Logs

Windows Firewall logging was enabled to capture network activity.

The firewall logs recorded multiple blocked TCP connection attempts from the Kali Linux machine.

Example entries:
DROP TCP 192.168.100.19 192.168.100.25 52614 139
DROP TCP 192.168.100.19 192.168.100.25 40370 445
DROP TCP 192.168.100.19 192.168.100.25 46934 135
DROP TCP 192.168.100.19 192.168.100.25 46226 49157

These logs clearly show repeated attempts to access different ports on the target machine.

This behavior is typical of network reconnaissance activity.

# Detection Logic

Indicators of port scanning:

- Multiple connection attempts from a single source IP
- Connection attempts targeting multiple ports
- Short time interval between attempts
- Repeated blocked connections in firewall logs

These indicators can be used by SOC analysts to identify suspicious network activity.

# Attack Timeline 

| Time     | Event                                                |
| -------- | ---------------------------------------------------- |
| 18:24:36 | First suspicious connection attempt detected         |
| 18:24:37 | Multiple TCP connection attempts from 192.168.100.19 |
| 18:24:38 | Firewall begins dropping connections                 |
| 18:24:43 | Continued probing of multiple ports                  |

# MITRE ATT&CK Mapping

T1046 - Network Service Discovery

The attacker performed port scanning to identify accessible services on the target system.

# Conclusion

The firewall logs show repeated connection attempts from a single source IP address targeting multiple ports within a short period of time.

This behavior strongly indicates port scanning activity.

In a real production environment, such activity should trigger security monitoring alerts and be investigated by the SOC team to determine whether the activity is malicious.

# Security Recommendations

1. Monitor repeated connection attempts to multiple ports
2. Implement SIEM alerts for port scanning patterns
3. Use IDS/IPS systems to detect reconnaissance activity
4. Block suspicious source IP addresses if necessary
5. Centralize firewall logs for correlation and investigation
