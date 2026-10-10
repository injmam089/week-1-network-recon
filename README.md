# Week 1 — Local Network Reconnaissance & Host Discovery

![Domain](https://img.shields.io/badge/Domain-Cybersecurity-blue)
![Tool](https://img.shields.io/badge/Tool-Nmap-red)
![Platform](https://img.shields.io/badge/Platform-Windows%2011-blue)
![Week](https://img.shields.io/badge/Week%201-Completed-success)

## Internship

**GetIntern.in — 8-Week Practical Cybersecurity Track**

---

## 1. Objective

The objective of this Week 1 task was to perform basic reconnaissance
of an authorized local network using **Nmap**.

The exercise focused on:

- Discovering active hosts on the local network
- Identifying reachable systems
- Performing basic TCP port scanning
- Detecting running services and service versions
- Understanding open, closed, and filtered ports
- Documenting reconnaissance findings
- Maintaining an ethical and authorized testing scope

---

## 2. Tools & Environment

| Category | Details |
|----------|---------|
| Operating System | Windows 11 |
| Command Line | Windows PowerShell |
| Network Scanner | Nmap 7.99.1 |
| Target Network | `192.168.1.0/24` |
| Scan Type | Host Discovery + TCP Service Enumeration |

---

## 3. Authorized Scope

All reconnaissance activities were performed only against my
authorized local network:

```text
192.168.1.0/24
```

No external systems, public IP addresses, or unauthorized networks
were intentionally targeted.

---

## 4. Methodology

The reconnaissance process was performed in the following stages.

### Step 1 — Host Discovery

Nmap was used to identify active hosts on the local network.

```bash
nmap -sn 192.168.1.0/24
```

The `-sn` option performs host discovery without performing a
full TCP port scan.

### Step 2 — Service Enumeration

Selected active hosts were scanned for TCP ports and service
information using Nmap service detection.

Example:

```bash
nmap -sV 192.168.1.1
```

The `-sV` option attempts to identify services and their versions
on discovered open ports.

### Step 3 — Filtered Host Verification

For `192.168.1.3`, an additional scan was performed using:

```bash
nmap -Pn -sV 192.168.1.3
```

The `-Pn` option skips host discovery and treats the target as
available for scanning.

---

## 5. Host Discovery Results

The host discovery scan identified **6 active hosts** on the
authorized local network.

| Host | Status |
|------|--------|
| `192.168.1.1` | Active |
| `192.168.1.2` | Active |
| `192.168.1.3` | Active |
| `192.168.1.7` | Active |
| `192.168.1.10` | Active |
| `192.168.1.15` | Active |

### Discovery Output

The complete host discovery output is stored in:

`scans/host-discovery.txt`

---

# 6. Service Enumeration Results

## 6.1 — 192.168.1.1

The authorized host `192.168.1.1` was scanned using Nmap service
detection.

### Open TCP Ports

| Port | State | Service | Version |
|------|-------|---------|---------|
| `53/tcp` | Open | DNS / domain | dnsmasq 2.52 |
| `80/tcp` | Open | HTTP | Boa HTTPd 0.94.13 |
| `5555/tcp` | Open | UPnP | Version not identified |

### Observation

The host exposes DNS, HTTP, and UPnP-related services.

Nmap identified:

- `dnsmasq 2.52` on port 53
- `Boa HTTPd 0.94.13` on port 80
- A UPnP-related service on port 5555

Nmap could not determine an exact service version for port 5555.

No exploitation or authentication attempts were performed.

### Evidence

`scans/192.168.1.1-services.txt`

---

## 6.2 — 192.168.1.2

The host was identified as active.

The default Nmap scan of 1000 TCP ports reported all scanned
ports as **closed**.

### Observation

No open TCP service was identified within the default
1000-port scan.

A closed port indicates that the host responded but no application
was accepting connections on the scanned port.

### Evidence

`scans/192.168.1.2-services.txt`

---

## 6.3 — 192.168.1.3

The host required additional verification because it did not respond
to the initial ping-based host discovery.

A second scan was performed using:

```bash
nmap -Pn -sV 192.168.1.3
```

### Observation

All 1000 scanned TCP ports were reported as **filtered**.

This may indicate that the host or an intermediate network security
control is filtering or dropping the scan traffic.

The result does not confirm that the host has no services. It indicates
that the scanned ports did not provide a response to the probes.

### Evidence

- `scans/192.168.1.3-services.txt`
- `scans/192.168.1.3-services-pn.txt`

---

## 6.4 — 192.168.1.7

The host was identified as active.

All 1000 scanned TCP ports were reported as **closed**.

### Observation

No open TCP service was identified within the default
1000-port scan.

### Evidence

`scans/192.168.1.7-services.txt`

---

## 6.5 — 192.168.1.10

A service enumeration scan was performed against the host.

The complete Nmap output is stored in:

`scans/192.168.1.10-services.txt`

The raw scan output is retained as evidence for the
reconnaissance activity.

> Note: The README does not make assumptions about services on this
> host beyond what is contained in the raw Nmap evidence.

---

## 6.6 — 192.168.1.15

A service enumeration scan identified multiple open TCP ports.

### Observed Open Ports

| Port | State | Service |
|------|-------|---------|
| `5000/tcp` | Open | RTSP |
| `7000/tcp` | Open | RTSP |
| `49152/tcp` | Open | Unknown |

Nmap also returned service fingerprints containing RTSP/AirTunes-related
information.

### Observation

The host exposes multiple network services, including RTSP-related
services and an additional unidentified service.

Service fingerprinting should be interpreted carefully when Nmap
cannot conclusively identify an exact application or version.

No exploitation was performed.

### Evidence

`scans/192.168.1.15-services.txt`

---

# 7. Summary of Findings

| Host | Host Status | TCP Scan Result | Key Finding |
|------|-------------|-----------------|-------------|
| `192.168.1.1` | Active | Open ports found | DNS, HTTP, and UPnP-related services |
| `192.168.1.2` | Active | Ports closed | No open TCP service in default scan |
| `192.168.1.3` | Active | Ports filtered | Possible filtering/firewall behavior |
| `192.168.1.7` | Active | Ports closed | No open TCP service in default scan |
| `192.168.1.10` | Active | Service scan performed | See raw Nmap evidence |
| `192.168.1.15` | Active | Open ports found | RTSP and additional service detected |

---

# 8. Security Observations

The reconnaissance exercise produced the following observations:

1. **Six active hosts** were identified on the authorized
   `192.168.1.0/24` network.

2. `192.168.1.1` exposed DNS, HTTP, and UPnP-related services.

3. `192.168.1.2` and `192.168.1.7` had no open TCP ports within
   the default Nmap 1000-port scan.

4. `192.168.1.3` returned filtered results, indicating possible
   firewall or packet-filtering behavior.

5. `192.168.1.15` exposed RTSP-related services and an additional
   unidentified service.

6. An open port does not automatically indicate a vulnerability.
   Service exposure should be evaluated separately.

7. A **closed** port generally indicates that the target was
   reachable but no application was accepting connections on
   that port.

8. A **filtered** port indicates that Nmap could not determine
   whether the port was open because packet filtering prevented
   a response.

---

# 9. Evidence

All Nmap scan results are stored in the `scans/` directory.

## Host Discovery

- `scans/host-discovery.txt`

## Service Enumeration

- `scans/192.168.1.1-services.txt`
- `scans/192.168.1.2-services.txt`
- `scans/192.168.1.3-services.txt`
- `scans/192.168.1.3-services-pn.txt`
- `scans/192.168.1.7-services.txt`
- `scans/192.168.1.10-services.txt`
- `scans/192.168.1.15-services.txt`

These files contain the raw Nmap outputs generated during
the practical exercise.

---

# 10. Limitations

This exercise was intentionally limited to basic reconnaissance.

- The host discovery scan covered the authorized `/24` network.
- Service enumeration primarily examined Nmap's default 1000 TCP ports.
- UDP scanning was not performed.
- No vulnerability exploitation was performed.
- No password attacks or credential testing were performed.
- No unauthorized access was attempted.
- Service version identification may not always be conclusive.

Therefore, the absence of a detected open port does not necessarily
mean that a host has no network services.

---

# 11. Ethical & Safety Statement

This activity was conducted for cybersecurity learning and internship
practice on an authorized local network.

The assessment was limited to reconnaissance and service identification.

**No exploitation, credential attacks, brute-force attempts, or
unauthorized access were performed.**

The findings should not be interpreted as a vulnerability assessment
or penetration test.

---

# 12. Skills Demonstrated

Through this exercise, I practiced:

- Network reconnaissance
- Host discovery
- TCP port scanning
- Nmap service detection
- Service/version identification
- Interpretation of open, closed, and filtered ports
- Basic network security analysis
- Evidence collection
- Technical documentation
- Ethical cybersecurity practices

---

# 13. Conclusion

The Week 1 reconnaissance exercise successfully identified active
systems on the authorized local network and performed basic TCP
service enumeration.

The exercise provided practical experience with Nmap and demonstrated
how reconnaissance data can be collected, interpreted, documented,
and preserved as technical evidence.

The activity was performed within the defined authorized scope and
did not involve exploitation or unauthorized access.

---

## Week 1 — Local Network Reconnaissance: Completed ✅
