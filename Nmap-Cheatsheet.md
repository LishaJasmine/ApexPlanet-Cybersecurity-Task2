# Nmap Cheatsheet — Network Security Lab

Nmap is a network-scanning tool used to discover hosts, identify open ports, detect services, and gather information about networked systems.

**Lab target:** `192.168.56.101`  
**Environment:** Authorised, isolated Metasploitable 2 virtual machine.

## 1. Check Network Information

```bash
ip addr
```
Displays local network interfaces and IP addresses.

```bash
ip route
```
Displays the routing table.

## 2. Discover a Host

```bash
nmap -sn 192.168.56.101
```
Checks whether the target is reachable without performing a port scan.

## 3. Basic Port Scan

```bash
nmap 192.168.56.101
```
Scans common TCP ports on the target.

## 4. Scan Specific Ports

```bash
nmap -p 22,80,443 192.168.56.101
```
Scans TCP ports 22, 80, and 443.

## 5. Scan All TCP Ports

```bash
nmap -p- 192.168.56.101
```
Scans all TCP ports from 1 through 65535.

## 6. TCP SYN Scan

```bash
sudo nmap -sS 192.168.56.101
```
Performs a TCP SYN scan. This generally requires elevated privileges for raw-packet scanning.

## 7. Service and Version Detection

```bash
nmap -sV 192.168.56.101
```
Attempts to identify the services and software versions running on open ports.

## 8. Operating-System Detection

```bash
sudo nmap -O 192.168.56.101
```
Attempts to estimate the target's operating system. Results may be inconclusive.

## 9. Combined Scan

```bash
sudo nmap -sS -sV -O 192.168.56.101
```
Combines TCP SYN scanning, service/version detection, and OS detection.

## 10. UDP Scan

```bash
sudo nmap -sU --top-ports 100 192.168.56.101
```
Scans the 100 most common UDP ports. UDP scanning may take longer, and some results may be inconclusive.

## 11. Save Scan Results

Save results in normal text format:

```bash
nmap -sV 192.168.56.101 -oN nmap_versions.txt
```

Save results in XML format:

```bash
nmap -sV 192.168.56.101 -oX nmap_results.xml
```

Save results in all major formats:

```bash
nmap -sV 192.168.56.101 -oA nmap_results
```

The `-oA` option creates normal, XML, and grepable output files.

## 12. Useful Nmap Options

| Option | Meaning |
|---|---|
| `-sn` | Host discovery without a port scan |
| `-sS` | TCP SYN scan |
| `-sT` | TCP connect scan |
| `-sU` | UDP scan |
| `-sV` | Service/version detection |
| `-O` | OS detection |
| `-p 80` | Scan a specific port |
| `-p 1-1000` | Scan a specified port range |
| `-p-` | Scan all TCP ports |
| `-Pn` | Skip host discovery and treat the target as online |
| `-T3` | Use a moderate scan timing template |
| `-oN file.txt` | Save normal text output |
| `-oX file.xml` | Save XML output |
| `-oA filename` | Save output in multiple formats |
| `--top-ports 100` | Scan the 100 most common ports |

## 13. Understanding Common Port States

- **Open:** A service is accepting connections on the port.
- **Closed:** The host is reachable, but no service is listening on that port.
- **Filtered:** A firewall or other network behaviour prevents Nmap from determining whether the port is open.
- **Open|filtered:** Nmap cannot confidently distinguish between an open port and a filtered port.

## 14. Important Security Notes

- Scan only systems for which you have explicit authorisation.
- Keep lab targets isolated from public networks.
- A detected service version does not automatically prove a vulnerability.
- Confirm important findings with reliable evidence and appropriate vulnerability checks.
- Save scan outputs so that findings can be reviewed and reproduced.
