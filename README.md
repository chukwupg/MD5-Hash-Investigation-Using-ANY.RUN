# MD5 Hash Investigation – Incident Response & Malware Analysis

## Overview

This project documents a hands-on incident response and malware analysis investigation triggered by Intrusion Prevention System (IPS) alerts. The focus of the lab is the validation of MD5 hash-based Indicators of Compromise (IOCs) to determine whether associated files are malicious, suspicious, or benign using sandbox analysis and threat intelligence techniques.

The investigation includes behavioral analysis, network activity inspection, persistence evaluation, and MITRE ATT&CK framework mapping.

---

## Objective

- Validate IPS-generated MD5 hash IOCs  
- Identify malicious, suspicious, and benign activities  
- Analyze malware behavior and execution flow (Process tree)
- Detect persistence and command-and-control (C2) activity  
- Map observed behaviors to the MITRE ATT&CK framework  

---

## Scope of Analysis

- **IOC Type:** MD5 hash values  
- **Sandbox Environment:** Windows 7 (32-bit)   
- **Analysis Platform:** ANY.RUN  
- **Artifacts Analyzed:** Executables, process trees, network traffic, DNS activity, registry changes  

---

## Tools & Technologies

- ANY.RUN Interactive Sandbox  
- ANY.RUN Threat Intelligence   
- MITRE ATT&CK Framework

---

## Key Findings (High-Level)

- Two MD5 hashes were confirmed **malicious**, associated with:
  - **ASYNCRAT** (Remote Access Trojan)
  - **RedLine Stealer** (Information-stealing malware)
- One hash exhibited **suspicious reconnaissance behavior** consistent with early-stage attack activity
- Observed techniques included persistence mechanisms, registry modification, C2 communication, and defense evasion
- Malicious samples demonstrated outbound connections to suspicious IPs, uncommon ports, and obfuscated domains

---

## MITRE ATT&CK Framework (Summary)

Observed tactics across analyzed samples included:

- Persistence  
- Privilege Escalation  
- Discovery  
- Defense Evasion  
- Command and Control (C2)  

---

## Repository Structure

```
md5-hash-investigation/
├── README.md
│
├── report/
│   └── md5-hash-investigation-report.md
│
├── iocs/
│   └── iocs.txt
│
├── screenshots/
│   ├── md5_2fd03624e271ec70349ce56fb30f563b_asyncrat/
│   ├── md5_766b774626947000e67e0b318f558e94_redline/
│   └── md5_c419df63e0121d72411285780c2fc6cc_suspicious/
│
├── mitre-attack/
│   ├── md5_2fd03624e271ec70349ce56fb30f563b_asyncrat/
│   ├── md5_c419df63e0121d72411285780c2fc6cc_suspicious/
│   └── md5_766b774626947000e67e0b318f558e94_redline/
│
└── references/
    ├── md5_2fd03624e271ec70349ce56fb30f563b_asyncrat/
    ├── md5_766b774626947000e67e0b318f558e94_redline/
    └── md5_c419df63e0121d72411285780c2fc6cc_suspicious/

```

---

## Full Technical Report

📄 Detailed Analysis and Findings:
➡️ report/md5-hash-investigation-report.md

The full report contains complete IOC tables, per-hash behavioral analysis, process execution flow, network activity breakdown, registry changes, and detailed MITRE ATT&CK mappings.

## Skills Demonstrated
- Incident response workflow execution
- IOC validation and triage
- Malware behavior analysis
- Network and DNS traffic inspection
- Threat classification and ATT&CK mapping
- Technical documentation and reporting

## Disclaimer

This project was conducted strictly for educational and research purposes. All malware hashes were analyzed in an isolated sandbox environment. No live systems or production networks were affected.
