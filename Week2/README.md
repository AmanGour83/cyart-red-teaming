# CyArt Internship — Week 2: Advanced Threat Analysis & Incident Response

**Intern:** Aman Gour | **Date:** May 29, 2026

## Lab Environment
| Component | Details |
|---|---|
| Attacker | Kali Linux — 10.0.2.15 |
| Target | Metasploitable2 — 10.0.2.3 |
| Network | NAT 10.0.2.0/24 |

## Tools Used
Metasploit | Wazuh | OpenVAS | DefectDojo | Suricata 8.0.5 | CrowdSec 1.4.6

## Key Evidence
| Tool | Finding | Timestamp |
|---|---|---|
| Metasploit | vsftpd backdoor → root shell (uid=0) | 2026-05-29 00:06:21 |
| Wazuh | Rule 533 Level 7 — port change detected | 2026-05-29 00:06:18 |
| CrowdSec | Decision #29998 — 10.0.2.15 banned 24h | 2026-05-29 00:10:16 |
| Suricata | SID:1000001 [wDrop] — Metasploitable2 blocked | 2026-05-29 00:14:51 |

## MITRE ATT&CK Techniques
| ID | Technique |
|---|---|
| T1190 | Exploit Public-Facing Application |
| T1046 | Network Service Scanning |
| T1059 | Command Scripting Interpreter |
| T1548 | Abuse Elevation Control |
| T1562 | Impair Defenses |
| T1566 | Phishing Simulation |
| T1071 | Application Layer Protocol |