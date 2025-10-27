# Network Security Home Lab

This repository is my **Network Security Home Lab**, documenting hands-on exercises where I secure an Ubuntu server and explore its network environment.  

Each lab is a **chapter**, focusing on specific tools, commands, and techniques for analyzing and strengthening system security.  

The purpose of this repo is:
- To practice **network security fundamentals** on Ubuntu servers.
- To demonstrate **hands-on technical skills** with industry tools (Nmap, netstat, tcpdump, ufw, etc.).
- To build a **portfolio** that can be shared with employers as proof of my Network Security and system administration knowledge.

---

## Chapters
- [Exploring Ubuntu Home Lab](Exploring_Ubuntu_Home_Lab.md)
- [Exploring UFW Firewall](Exploring_UFW_Firewall.md)
- [Exploring Snort IDPS](Exploring_Snort_IDPS.md)
---

## Tools Covered
- **ip/ifconfig** → network interfaces & IP addresses  
- **netstat / ss** → open ports  
- **lsof** → active connections  
- **nmap** → scanning, services, versions, vulnerabilities  
- **tcpdump** → packet capture & traffic analysis  
- **ufw** → firewall rules
- **UFW (Uncomplicated Firewall)** → enable/disable firewall, manage rules, allow/deny traffic, block IPs, allow specific IP/port access.  
- **ss (Socket Statistics)** → check open/listening TCP & UDP ports.  
- **lsof (List Open Files)** → identify which service is using a given port.  
- **tail** → monitor log files in real time (`/var/log/ufw.log`).  
- **grep** → filter logs for specific patterns (e.g., `DENY`, `ALLOW`).
- **snort** → installing and configuring Snort.

---

