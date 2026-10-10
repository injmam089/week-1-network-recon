\# Week 1 - Local Network Reconnaissance \& Host Discovery



\## Internship

GetIntern.in - 8-Week Practical Cybersecurity Track



\## Objective

Perform basic reconnaissance of my authorized local network and identify active hosts using Nmap.



\## Tools Used

\- Nmap 7.99.1

\- Windows PowerShell

\- Windows 11



\## Target Network

192.168.1.0/24



\## Host Discovery Command

nmap -sn 192.168.1.0/24



\## Result

The scan identified 6 active hosts on the authorized local network.



\### Discovered Hosts

\- 192.168.1.1

\- 192.168.1.2

\- 192.168.1.3

\- 192.168.1.7

\- 192.168.1.10

\- 192.168.1.15



\## Output

The Nmap scan output is stored in:



scans/host-discovery.txt



\## Ethical Scope

This reconnaissance was performed only on my authorized local network for cybersecurity learning and internship practice.



\## Service Enumeration — 192.168.1.1



The authorized host 192.168.1.1 was scanned using Nmap service detection.



\### Open TCP Ports



| Port | State | Service | Version |

|------|-------|---------|---------|

| 53/tcp | open | domain | dnsmasq 2.52 |

| 80/tcp | open | http | Boa HTTPd 0.94.13 |

| 5555/tcp | open | upnp | Version not identified |



\### Observation



The host exposes DNS, HTTP, and UPnP-related services. Nmap identified

Boa HTTPd 0.94.13 on port 80. Port 5555 returned UPnP-related data,

but Nmap could not determine an exact service version.



No exploitation was performed. This activity was limited to

authorized reconnaissance and service identification.



\## Additional Host Scan Results



\### 192.168.1.2



Nmap identified the host as active. All 1000 scanned TCP ports were

reported as closed. No open TCP service was identified during the scan.



\### 192.168.1.3



The host responded to the scan, but all 1000 scanned TCP ports were

reported as filtered (no response). This indicates that the host or

network controls may be filtering the scan traffic.



A second scan using `-Pn` was performed because the host did not respond

to the initial ping-based detection. The result still showed all 1000

TCP ports as filtered.



\### 192.168.1.7



Nmap identified the host as active. All 1000 scanned TCP ports were

reported as closed. No open TCP service was identified during the scan.



\### 192.168.1.10



A service enumeration scan was performed and the output was saved as:



`scans/192.168.1.10-services.txt`



\### 192.168.1.15



A service enumeration scan was performed and the output was saved as:



`scans/192.168.1.15-services.txt`



\## Security Observations



\- Six active hosts were identified on the authorized local network.

\- 192.168.1.1 exposed DNS, HTTP and UPnP-related services.

\- 192.168.1.2 and 192.168.1.7 had no open TCP ports among the

&#x20; default 1000-port scan.

\- 192.168.1.3 showed filtered TCP ports, indicating possible

&#x20; firewall or packet-filtering controls.

\- Service enumeration was performed only for reconnaissance purposes.

\- No exploitation, credential attacks, or unauthorized access attempts

&#x20; were performed.



\## Evidence



All Nmap results are stored in the `scans/` directory:



\- `scans/host-discovery.txt`

\- `scans/192.168.1.1-services.txt`

\- `scans/192.168.1.2-services.txt`

\- `scans/192.168.1.3-services.txt`

\- `scans/192.168.1.3-services-pn.txt`

\- `scans/192.168.1.7-services.txt`

\- `scans/192.168.1.10-services.txt`

\- `scans/192.168.1.15-services.txt`



\## Conclusion



The Week 1 reconnaissance exercise successfully identified active

hosts on the authorized local network and performed basic TCP service

enumeration.



The exercise demonstrated practical use of Nmap for host discovery,

port scanning, service detection, and basic interpretation of closed

and filtered ports.



All activities were limited to the authorized local network and no

exploitation was performed.

