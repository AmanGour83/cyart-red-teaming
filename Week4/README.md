# Week 4 — Advanced Red Team Operations

**CyArt Cybersecurity Internship**  
**Intern:** Aman Gour  
**Institution:** SVIT Hyderabad  
**Date:** June 14, 2026  
**Classification:** Confidential — Internal Use Only

---

## Overview

This folder contains all documentation, notes, and evidence for Week 4 of the CyArt Cybersecurity Internship. The week focused on advanced red team techniques including Command and Control infrastructure, cloud environment attacks, adversary emulation, payload evasion, and a full capstone adversary simulation.

All activities were conducted in an **isolated VirtualBox lab environment** with explicit authorization as part of the CyArt internship program. No real production systems were targeted.

---

## Lab Environment

| Component | IP Address | Role |
|-----------|-----------|------|
| Kali Linux (Attacker) | 10.0.2.15 | Red Team Operator |
| Metasploitable 2 (Target) | 10.0.2.3 | Vulnerable Target |
| Network | 10.0.2.0/24 | VirtualBox NAT |

---

## Theory Topics Covered

| # | Topic | MITRE ATT&CK Reference |
|---|-------|------------------------|
| 1 | Advanced Command and Control (C2) Frameworks | T1071 |
| 2 | Cloud Environment Attacks | T1580, T1078.004, T1537 |
| 3 | Adversary Emulation and Threat Simulation | T1583, T1566 |
| 4 | Advanced Reporting and Debriefing | PTES Standard |
| 5 | Native Tool Abuse (Living-Off-the-Land) | T1059, T1055, T1003 |

---

## Practical Labs

| Lab | Title | Tools Used | Key MITRE TTP |
|-----|-------|-----------|----------------|
| 1 | Advanced C2 Setup | PoshC2 v9.0, Metasploit | T1071.001 |
| 2 | Cloud Attack Lab | AWS CLI, LocalStack 3.0.0 | T1580, T1537 |
| 3 | Adversary Emulation (APT29) | Metasploit, SSH | T1566, T1136, T1053.003 |
| 4 | Advanced Evasion Lab | msfvenom, shikata_ga_nai | T1027, T1059 |
| 5 | Cloud Privilege Abuse | AWS CLI, STS | T1548, T1078.004 |
| 6 | Automated Attack Orchestration | Metasploit | T1190, T1003 |
| 7 | Living-Off-the-Land Lab | Native Linux tools | T1059, T1082 |
| 8 | Comprehensive Reporting | Google Docs | PTES |
| 9 | Capstone: Full Adversary Simulation | Nmap, Metasploit | T1046, T1190, T1003 |

---

## Key Findings Summary

| Finding ID | TTP | CVSS Score | Tool | Remediation |
|------------|-----|-----------|------|-------------|
| FID001 | T1071 — C2 Communication | 8.5 | PoshC2 | Network monitoring, HTTPS inspection |
| FID002 | T1580 — Cloud Recon | 7.5 | AWS CLI | Block public S3 ACLs via SCP |
| FID003 | T1210 — Service Exploit | 9.0 | Metasploit | Patch Samba, network segmentation |
| FID004 | T1027 — Obfuscated Payload | 8.0 | msfvenom | Deploy behavioral EDR |
| FID005 | T1548 — Privilege Escalation | 9.0 | IAM/STS | Enforce least-privilege IAM |
| FID006 | T1053.003 — Crontab Persistence | 8.5 | Crontab | File integrity monitoring |
| FID007 | T1003 — Credential Dumping | 9.0 | Native tools | SHA-512 hashing, restrict shadow |

---

## Folder Structure

```
Week 4/
├── README.md                          # This file
├── Week4_RedTeam_Report_AmanGour.docx # Full report (PDF export for submission)
├── screenshots/
│   ├── lab1_poshc2_server.png         # PoshC2 C2 server running
│   ├── lab1_session_root.png          # Root shell via Metasploit
│   ├── lab2_s3_bucket.png             # S3 bucket creation and public ACL
│   ├── lab2_iam_escalation.png        # IAM role with AdministratorAccess
│   ├── lab2_exfiltration.png          # Data exfiltrated from S3
│   ├── lab3_apt29_session.png         # APT29 emulation root shell
│   ├── lab3_crontab_persistence.png   # Crontab backdoor entry
│   ├── lab4_msfvenom_payload.png      # Obfuscated payload creation
│   ├── lab4_meterpreter_session.png   # Meterpreter session opened
│   ├── lab5_iam_privesc.png           # STS assume-role escalation
│   ├── lab6_orchestration.png         # Automated attack chain
│   ├── lab7_lotl_suid.png             # SUID binaries and root accounts
│   ├── lab8_findings_table.png        # Findings documentation
│   ├── lab9_nmap_scan.png             # Capstone nmap recon
│   ├── lab9_shadow_dump.png           # /etc/shadow credential dump
│   └── lab9_persistence.png           # apt29backdoor in /etc/passwd
└── notes/
    └── week4_findings.txt             # Raw findings notes
```

---

## Tools & Versions

| Tool | Version | Purpose |
|------|---------|---------|
| PoshC2 | v9.0 | C2 Framework |
| Metasploit Framework | Latest | Exploitation & Post-exploitation |
| msfvenom | Built-in | Payload Generation & Encoding |
| Nmap | 7.99 | Network Reconnaissance |
| AWS CLI | 1.45.29 | Cloud Attack Simulation |
| LocalStack | 3.0.0 | AWS Environment Simulation |
| Docker | Latest | LocalStack container runtime |

---

## Attack Chain Summary (Capstone)

```
Recon (Nmap)
    ↓
Cloud Recon (S3 Enumeration)
    ↓
Initial Access (Samba Exploit → Root Shell)
    ↓
Persistence (Crontab + Backdoor User)
    ↓
Credential Access (/etc/shadow Dump)
    ↓
Data Exfiltration (/etc/passwd, config files)
    ↓
Cloud Privilege Escalation (IAM Role Abuse → Admin)
```

---

## Disclaimer

> All activities documented in this repository were performed in a controlled, isolated lab environment as part of the CyArt Cybersecurity Internship program at SVIT Hyderabad. All targets were intentionally vulnerable systems (Metasploitable 2) owned and operated within the intern's personal VirtualBox environment. No unauthorized systems were accessed. This documentation is intended for educational purposes only.

---

*CyArt Cybersecurity Internship | Week 4 | Aman Gour | June 2026*
