# IOC Investigation Report
This lab documents the investigation of MD5 hash values generated from Intrusion Prevention System (IPS) alerts as part of an incident response and digital forensic analysis process.

---

## Objective

The objective of this investigation was to validate identified Indicators of Compromise (IOCs) by determining whether the associated files were malicious, suspicious, or benign.

Malware analysis tools and threat intelligence resources were used to validate these hashes, and the findings informed further analysis of malicious behavior, attack techniques, and potential threat attribution.

---

**Sandbox Environment:** Windows 7 (32-bit) on ANY.RUN  
**IOC Type:** MD5 hashes of suspicious files

---

## Part 1: Validating Suspicious Hashes

| S/N | MD5 Hash Value | Verdict | Associated Filename(s) |
| --- | --- | --- | --- |
| 1 | 2fd03624e271ec70349ce56fb30f563b | Malicious | wireframe.exe, NvidiaGPU.exe |
| 2 | c419df63e0121d72411285780c2fc6cc | Suspicious | Updreg.exe |
| 3 | 3acf52e5a62d50bdcedcb89174bf5492 | Benign | No analysis found |
| 4 | 766b774626947000e67e0b318f558e94 | Malicious | gh2st.exe |
| 5 | 422a6ca28a7e4d8e5e498523c6f049f4 | Benign | No analysis found |
| 6 | b497845beb135740e6caed03a2020036 | Benign | No analysis found |

### Summary of Findings

After investigating the MD5 hashes on ANY.RUN:

- Two hashes (S/N 1 and 4) were confirmed to be **malicious**
- One hash (S/N 2) returned *“No threat detected”* but exhibited suspicious behavior and requires further investigation
- Three hashes (S/N 3, 5, and 6) had no available reports at the time of analysis and remain inconclusive

---

## Part 2: Investigating Malicious Activity

---

### MD5: **2fd03624e271ec70349ce56fb30f563b**

| Attribute | Value |
| --- | --- |
| SHA256 | 9C83A89EA0E56D5AF9AA37D2DABED20B2412DB8C9694A13128EA173A73557487 |
| Threat | ASYNCRAT (Remote Access Trojan) |
| Dangers | Registry autorun modification; persistent RAT activity |
| Verdict | Malicious |

#### Behavioral Analysis

The process tree indicates a combination of legitimate Windows processes and malware-related executables facilitating infection and persistence.

**Execution sequence:**
- svchost.exe  
- wireframe.exe  
- cmd.exe  
- timeout.exe  
- NvidiaGPU.exe  

The malware established 14 outbound network connections. Although most were whitelisted—possibly as a masquerading technique—one connection was identified as malicious.

Three DNS requests were observed. One resolved to the malicious IP address **49.13.77.253**, confirming command-and-control (C2) activity associated with the domain:

- **tasteless-minister.auto.playit.gg**

Registry autorun values were modified, and **NvidiaGPU.exe** was executed from a registry key, likely as an evasion technique. Command execution via **cmd.exe** further confirms persistent remote administration consistent with ASYNCRAT behavior.

---

#### MITRE ATT&CK Mapping

- **Tactics:** 5  
- **Techniques:** 6  
- **Events:** 25  

**Observed Tactics:**
- Execution  
- Persistence  
- Privilege Escalation  
- Discovery  
- Command and Control (C2)

![ASYNCRAT MITRE Mapping](attachment:10f23ca7-9e2b-4e13-91e6-f64803956c91:image.png)

---

### MD5: **c419df63e0121d72411285780c2fc6cc**

| Attribute | Value |
| --- | --- |
| SHA256 | F47F854D327C589D174D3BB5B55D5C05F5ACA73DF52A6BEF47596B9010190291 |
| Verdict | Suspicious |

#### Behavioral Analysis

No explicit malicious indicators were identified. However, the file exhibited reconnaissance-related behavior, including registry queries and system information discovery.

Although not linked to confirmed malicious activity, these behaviors align with early-stage attack reconnaissance and justify the *suspicious* classification.

---

#### MITRE ATT&CK Mapping

- **Tactics:** 1  
- **Techniques:** 2  
- **Events:** 2  

**Observed Tactic:**
- Discovery  

This activity suggests reconnaissance behavior indicative of a potential pre-attack phase.

![Suspicious Hash MITRE Mapping](attachment:df44f268-d1b9-45ce-83ea-7a04ce3ef9fe:image.png)

---

### MD5: **766b774626947000e67e0b318f558e94**

| Attribute | Value |
| --- | --- |
| SHA256 | 88DD2037D0C43ABACEBAD866DF3F8CCD2EE7D64B01405AA6756A3A1C2FAC28FA |
| Threat | RedLine Stealer |
| Danger | REDLINE detected (YARA) |
| Verdict | Malicious |

#### Behavioral Analysis

The process tree shows both legitimate Windows processes and malware-related executables involved in execution:

- gh2st.exe  
- conhost.exe  
- slui.exe  

The malware initiated 37 outbound network connections across multiple IP addresses and uncommon ports in various geographic locations. Seven connections were flagged as malicious.

Thirteen DNS queries were observed (2 requests and 11 responses). While none were explicitly flagged, several domains appeared obfuscated, suggesting attempts to conceal malicious traffic.

---

#### MITRE ATT&CK Mapping

- **Tactics:** 3  
- **Techniques:** 5  
- **Events:** 40  

**Observed Tactics:**
- Defense Evasion  
- Discovery  
- Command and Control (C2)

![RedLine MITRE Mapping](attachment:63f0526e-f973-49a9-a87b-5f32749abc3d:image.png)

---

## Conclusion

This investigation successfully validated IPS-generated MD5 hash IOCs and identified two confirmed malware samples **(ASYNCRAT** and **RedLine)** both demonstrating persistence, command-and-control communication, and reconnaissance behavior.

One hash exhibited early-stage reconnaissance activity, while others remain inconclusive due to limited available intelligence information on ANY.RUN. These findings highlight the importance of hash-based IOC validation, behavioral malware analysis, and MITRE ATT&CK mapping in effective incident response and threat hunting workflows.
