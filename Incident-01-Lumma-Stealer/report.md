# Incident 01 – Lumma Stealer Malware Investigation

## Executive Summary
During routine SOC investigation, an unusual VPN login alert originating from IP address
37.19.201.132 (Singapore) was initially considered legitimate, as the user was reportedly
traveling. However, user confirmation revealed no such login attempt was made.

Further investigation identified a suspicious executable file associated with the activity.
Threat intelligence analysis confirmed the file as **Lumma Stealer**, a known information-stealing
malware operating under a Malware-as-a-Service (MaaS) model.

---

## Initial Alert & Context
- Alert Type: Unusual VPN Login
- User: susan.martin@probablyfine.thm
- Source IP: 37.19.201.132
- Country: Singapore
- ASN: 212238 (Datacamp Limited)

Initial assumptions classified the activity as low-risk due to user travel. However, direct
user verification disproved this assumption, escalating the investigation.

---

## Malware Identification
- File Name: zY9sqWs.exe
- File Type: PE32 Windows Executable
- SHA-256: b8e02f2bc0ffb42e8cf28e37a26d8d825f639079bf6d948f8debab6440ee5630
- File Size: 369,664 bytes

Multiple antivirus vendors flagged the file as malicious. Microsoft identified it as:

**Trojan:Win32/LummaStealer.PM!MTB**

This detection confirms the malware belongs to the Lumma Stealer family, designed to steal
credentials, browser data, and sensitive information.

---

## Threat Intelligence & Vendor Detections
More than 50 security vendors identified the file as malicious, including:
- Microsoft
- Kaspersky
- ESET
- CrowdStrike
- Malwarebytes

The malware is linked to known Lumma Stealer campaigns observed globally.

---

## Network Communication Analysis
Dynamic and static analysis revealed that the malware contacted multiple external domains,
indicating command-and-control (C2) and payload delivery behavior.

Observed contacted domains include:
- hardswarehub.today
- gadgethgfub.icu
- joyfulhezart.tech

These domains show high detection ratios and are associated with malicious infrastructure.

---

## SSL Certificate & Infrastructure Analysis
Analysis of HTTPS certificates linked to the contacted domains revealed:
- Large Subject Alternative Name (SAN) lists
- Shared certificates across multiple suspicious domains
- Indicators of infrastructure reuse

This suggests the domains are part of a larger coordinated malicious campaign rather than
isolated infrastructure.

---

## YARA Detection
The malware sample matched a YARA rule authored by **kevoreilly**, a known malware researcher.

Key YARA condition:
uint16(0) == 0x5a4d and any of them

This condition confirms the file is a valid Windows PE executable and matches known Lumma
Stealer patterns.

---

## Associated Threat Actors
Threat intelligence reports associate Lumma Stealer operations with organized criminal
affiliates. Research indicates collaboration with:
- GhostSocks (residential SOCKS5 proxy infrastructure)

This enables attackers to route malicious traffic through compromised systems, increasing
anonymity and evasion.

---

## Impact Assessment
Potential impacts of Lumma Stealer infection include:
- Credential theft
- Session hijacking
- Financial fraud
- Unauthorized access to enterprise systems

The malware poses high risk to enterprise environments.

---

## Recommendations
- Immediate password resets for affected accounts
- Enforce MFA re-registration
- Block identified IOCs at network perimeter
- Monitor for Lumma Stealer indicators
- User awareness training on phishing and malware delivery

---

## Conclusion
This investigation confirms the incident was not a false positive but a verified Lumma
Stealer infection attempt. The malware leveraged malicious infrastructure, proxy services,
and obfuscation techniques to evade detection.

Prompt investigation and threat intelligence correlation prevented further compromise.
