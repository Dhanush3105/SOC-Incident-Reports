# Incident 01 – Lumma Stealer Infection

## Incident Summary
An alert was generated for an unusual VPN login originating from an external IP address. Subsequent investigation revealed the presence of a malicious Windows executable identified as **Lumma Stealer**, an infostealer malware commonly used to exfiltrate credentials and sensitive browser data.

## Initial Alert
- Alert Type: Unusual VPN Login
- User: susan.martin@probablyfine.thm
- Source IP: 37.19.201.132 (Singapore)
- Context: User denied performing the login

## Investigation Overview
The investigation involved:
- IP reputation analysis
- Malware hash analysis
- File behavior inspection
- Contacted domains analysis
- Threat intelligence correlation
- MITRE ATT&CK mapping

## Malware Identification
- File Name: zY9sqWs.exe
- File Type: Windows PE32 Executable
- SHA-256: b8e02f2bc0ffb42e8cf28e37a26d8d825f639079bf6d948f8debab6440ee5630

Multiple security vendors identified the file as Lumma Stealer.

## Detection Verdict
- Malware Family: Lumma Stealer
- Malware Type: Infostealer
- Risk Level: High

## Conclusion
This incident was confirmed as a credential-stealing malware infection. Immediate containment actions such as credential resets, endpoint isolation, and IOC blocking were recommended.
