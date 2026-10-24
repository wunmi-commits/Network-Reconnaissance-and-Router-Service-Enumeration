# Network-Reconnaissance-and-Router-Service-Enumeration
A hands-on cybersecurity project focused on network reconnaissance and router service enumeration using Kali Linux, Nmap, Netcat, and SNMP tools. Practised identifying open ports, investigating network services, and using Linux commands such as `find` and `grep` in an authorized lab environment.
## Network Reconnaissance and Router Service Enumeration

## Project Overview
This project documents my hands-on cybersecurity investigation in the Operation Takeover lab. The objective was to identify exposed network services on a target router, investigate potential management interfaces, and determine the next steps required to access the router in an authorized lab environment.

## Objectives
- Perform network reconnaissance using Nmap.
- Identify open ports and running services.
- Investigate SSH and FRRouting services.
- Explore SNMP enumeration and understand unsuccessful responses.
- Practise Linux file-search commands using `find` and `grep`.
- Document findings and recommend further investigation.

## Tools Used
- Kali Linux
- Nmap
- Netcat (`nc`)
- SSH client
- SNMP utilities
- Linux commands: `find`, `grep`, `cat`

## Investigation and Findings

### 1. Network reconnaissance
I scanned the authorized lab target to identify exposed services.

Command:

```bash
nmap -sV -p 22,179,2623 10.128.130.198
```

Services identified during the investigation:

| Port | Service | Observation |
|---|---|---|
| 22/TCP | SSH | OpenSSH 8.2p1 |
| 179/TCP | BGP | Border Gateway Protocol service |
| 2623/TCP | FRRouting | Version 10.0; password prompt observed |
| 161/UDP | SNMP | Reported open by Nmap; queries did not return useful responses |
## Evidence Screenshots


### 2. Investigating the FRRouting service
I connected to port 2623 using Netcat.

```bash
nc -nv 10.128.130.198 2623
```

The service returned an FRRouting 10.0 greeting and a `User Access Verification` password prompt.

**Finding:** The service was reachable, but authentication was not successful. No router access was confirmed.

### 3. SNMP enumeration
I attempted SNMP enumeration with community strings such as `public` and `pubLic`, using SNMP utilities.

The requests did not return useful responses.

**Finding:** The SNMP service required further investigation. An Nmap UDP result alone did not confirm that SNMP enumeration was successful.

### 4. Linux file-search practice
I practised searching for files by name:

```bash
find / -type f -name "flag.txt" 2>/dev/null
```

I also practised searching file contents:

```bash
grep -Rin "flag" /root 2>/dev/null
```

These commands search the filesystem on which they are executed. Running them on Kali searches Kali, not the remote router. To inspect the router's `/root` directory, authenticated access to the router would first be required.

## Challenges Encountered
- SNMP queries returned no useful responses.
- The FRRouting service required authentication.
- I did not establish a successful SSH or router-management session.
- I learned to distinguish local Kali files from files on a remote target.

## Security Lessons Learned
1. Open ports indicate exposed services, not successful access.
2. Service enumeration helps determine what to investigate next.
3. A password prompt does not prove that a service accepts SSH.
4. UDP scan results require careful interpretation.
5. Linux file-search commands operate on the local or remote filesystem where they run.
6. Accurate reporting should distinguish confirmed findings from assumptions.

## Conclusion
This lab improved my understanding of network reconnaissance, service identification, SNMP investigation, router management interfaces, and Linux file-search commands.

The router-access objective remains incomplete. Further investigation is required to identify the intended authentication or access path before attempting to retrieve the lab flag.

## Scope and Authorization
All testing was conducted as part of an authorized cybersecurity training lab. The techniques documented here should only be used on systems for which testing permission has been granted.
