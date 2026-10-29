# ApexPlanet Cybersecurity Internship — Task 2
## Network Security and Scanning

### 1. Objective
The objective of this task is to understand network reconnaissance, host discovery, port scanning, service detection, vulnerability assessment, packet analysis, and basic firewall configuration in an authorised lab environment.

### 2. Lab Environment
- **Attacker machine:** Kali Linux
- **Target machine:** Metasploitable 2
- **Target IP address:** `192.168.56.101`
- **Tools:** Nmap, Wireshark, Greenbone/OpenVAS
- **Network:** Isolated VirtualBox lab network

All scanning activities were performed against the intentionally vulnerable lab machine.

### 3. Network Reconnaissance
Network reconnaissance collects information about a target before security testing.

Common commands:

```bash
whois example.com
nslookup example.com
ip addr
```

- `whois` retrieves available domain-registration information.
- `nslookup` queries DNS records.
- `ip addr` displays network interfaces and IP addresses on the local Linux machine.

**Note:** Public-domain reconnaissance should only be performed when permitted.

### 4. Host Discovery
Host discovery determines whether a target machine is reachable.

```bash
nmap -sn 192.168.56.101
```

The `-sn` option performs host discovery without a port scan.

**Observation:** The target machine was detected as up during the lab scans.

### 5. TCP Port Scanning
TCP scanning identifies open ports and services that may be accessible on the target.

```bash
nmap 192.168.56.101
```

A more detailed TCP scan was also performed:

```bash
nmap -sS -p- 192.168.56.101
```

The results showed multiple open TCP ports. The full scan reported 23 open TCP ports and 977 filtered TCP ports.

### 6. Service and Version Detection
Service detection helps identify the applications and versions running on open ports.

```bash
nmap -sV 192.168.56.101
```

Examples observed during the scan included:

| Port | Service | Observed Version/Details |
|---|---|---|
| 21 | FTP | vsftpd 2.3.4 |
| 22 | SSH | OpenSSH 4.7p1 |
| 23 | Telnet | telnetd |
| 25 | SMTP | Postfix smtpd |
| 53 | DNS | BIND 9.4.2 |
| 80 | HTTP | Apache 2.2.8 |
| 139, 445 | SMB | Samba |
| 3306 | MySQL | MySQL 5.0.51a |
| 5432 | PostgreSQL | PostgreSQL 8.3.x |
| 5900 | VNC | VNC protocol 3.3 |
| 6667 | IRC | UnrealIRCd |
| 8180 | HTTP | Apache Tomcat/Coyote |

These are scanner-reported service identifications. Version detection alone does not prove that a service is exploitable.

### 7. UDP Scanning
UDP scanning checks for services that communicate using the User Datagram Protocol.

```bash
nmap -sU --top-ports 100 192.168.56.101
```

UDP scans may take longer than TCP scans. Some ports can be reported as `open|filtered` when Nmap cannot determine their state conclusively.

The exact results should be taken from the saved UDP scan report.

### 8. OS Detection
Operating-system detection attempts to estimate the target's operating system from network responses.

```bash
sudo nmap -O 192.168.56.101
```

**Observation:** OS detection was inconclusive in the recorded full scan. Therefore, no specific operating-system identification is claimed from that result.

### 9. Wireshark Packet Analysis
Wireshark was used to inspect network packets and understand communication between hosts.

Useful display filters:

```text
http
dns
ftp
tcp
ip.addr == 192.168.56.101
```

- **HTTP:** Helps inspect web requests and responses.
- **DNS:** Helps inspect domain-name queries and responses.
- **FTP:** Helps inspect FTP control traffic.
- **TCP:** Displays TCP packets.
- **IP address filter:** Limits displayed packets to traffic involving the target.

Packet analysis can reveal protocols, endpoints, connection behaviour, and potentially sensitive information when protocols are unencrypted.

Only include packet-capture findings that were actually observed in the saved captures.

### 10. Vulnerability Assessment with OpenVAS/Greenbone
OpenVAS/Greenbone was selected for vulnerability assessment. During setup, the scanner manager encountered service/database initialisation problems, and a completed vulnerability scan report was not generated.

**Status: Pending.**

A successful vulnerability assessment requires a functioning scanner, updated vulnerability feeds where applicable, a configured target, and a completed scan report. No vulnerability findings are claimed in this section.

### 11. Basic Firewall Configuration
A firewall controls network traffic according to configured rules.

Common Linux firewall commands include:

```bash
sudo iptables -L -n -v
sudo ufw status verbose
```

- `iptables -L -n -v` lists existing iptables rules and counters.
- `ufw status verbose` displays UFW status and rules when UFW is installed.

Firewall rules should follow the principle of least privilege: allow only required traffic and restrict unnecessary services.

Record the actual commands, rules, and verification results used in the lab. Do not claim a port was blocked unless the effective rule and its result were verified.

### 12. Key Findings
1. The target was reachable during scanning.
2. Multiple TCP services were exposed.
3. Several services reported legacy software versions that warrant further investigation.
4. Telnet is an insecure protocol for sensitive remote administration because it does not provide encryption.
5. Unnecessary database and remote-access services should be restricted to authorised hosts.
6. OS detection was inconclusive.
7. OpenVAS setup remained incomplete, so a vulnerability report is still pending.

### 13. Security Recommendations
- Disable unused services and close unnecessary ports.
- Replace legacy software with supported versions.
- Use SSH instead of Telnet for remote administration.
- Restrict database access to trusted systems.
- Apply security updates regularly.
- Use firewall rules based on least privilege.
- Perform authenticated vulnerability assessments where appropriate.
- Review network traffic for suspicious activity.
- Retest after applying security fixes.

### 14. Evidence and Deliverables
Add the actual evidence files to this repository as they become available:

- Nmap basic scan output
- Nmap service/version scan output
- Nmap full TCP scan output
- Nmap UDP scan output
- Wireshark screenshots or capture files
- Firewall configuration and verification evidence
- OpenVAS report, once successfully generated
- Demonstration video link

### 15. Conclusion
This task provided practical experience with network reconnaissance, host discovery, TCP and UDP scanning, service identification, packet analysis, and firewall concepts. The scan results demonstrated why exposed services and legacy software require security review. The OpenVAS assessment remains pending because of setup issues, and the report should be updated after a successful scan.

**Disclaimer:** This work was performed in an authorised, isolated virtual lab for educational purposes.
