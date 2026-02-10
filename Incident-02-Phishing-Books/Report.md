
## **1. Executive Summary**

A phishing incident targeting a university faculty member was investigated after the user reported repeated unsolicited MFA approval requests. Manual analysis of the reported email revealed a credential-harvesting phishing campaign impersonating a university library service. The attacker leveraged email authentication misconfigurations, typosquatting, HTML attachment delivery, JavaScript obfuscation, and IDN homograph techniques. Threat intelligence analysis linked the campaign infrastructure to the known threat actor group **Cobalt Dickens (Silent Librarian)**, which primarily targets academic and research institutions.

---

## **2. Initial Detection & Alert Source**

* The incident was **not detected by SIEM or email security controls**
* Alert originated from a **user report** (**Dr. Isabella**)
* User reported **multiple MFA approval prompts** without initiating login attempts
* SOC advised **immediate password reset and MFA review**
* Manual forensic email analysis was initiated

**Key Lesson:**
**User-reported alerts can be the first indicator of phishing when automated controls fail.**

---

## **3. Email Header Analysis – Filter Bypass**

**Finding:**

* **DMARC = none**
* **SPF = none**
* **DKIM = none**

**Explanation:**

* Sender domain `kinglord.ac.uk` had **no DMARC policy**
* **No SPF or DKIM enforcement**
* Allowed spoofed or look-alike domains to bypass email filters

**Impact:**
**Email passed authentication checks despite being malicious.**

---

## **4. Social Engineering Technique**

**Technique Used:** **Typosquatting**

**Evidence:**

* Legitimate domain: `kingford.ac.uk`
* Malicious sender: `kinglord.ac.uk`
* Visual similarity exploited user trust

**MITRE ATT&CK Mapping:**

* **T1583.001 – Acquire Infrastructure: Domains**

---

## **5. Malicious Attachment Analysis**

**Attachment Name:**
**library-invoice.pdf.html**

**Observations:**

* HTML file disguised as a PDF invoice
* Executes JavaScript locally when opened
* Redirects victim to a fake university login portal

**File Hash (MD5):**
**442f2965cb6e9147da7908bb4eb73a72**

---

## **6. Obfuscation & Redirect Logic**

**MITRE Technique:**

* **T1027 – Obfuscated / Encrypted Files or Information**

**JavaScript Behavior:**

```javascript
var src = reversed.split("").reverse().join("");
window.location.replace(src);
```

**Obfuscation Techniques Observed:**

* URL stored as Unicode-encoded characters
* String reversal used to evade static detection
* Runtime decoding and redirect execution

**Hidden Message Discovered:**
**I love to phish books from libraries ^^**

**Attacker Intent:**
Credential harvesting combined with psychological signaling common in real-world phishing campaigns.

---

## **7. Redirect Chain & IDN Homograph Attack**

**First Redirect URL:**
**[http://xn--librarytlu-13cwe32432-kwr.com:8082](http://xn--librarytlu-13cwe32432-kwr.com:8082)**

**Explanation:**

* Uses Cyrillic character **“і”** instead of ASCII **“i”**
* Browser converts deceptive domain to **Punycode (xn--)**
* Prevents users from visually detecting the fake domain

**Attack Type:**
**IDN Homograph Attack**

---

## **8. Landing Page**

**Final Landing Page:**
**[http://lib-service.com:8083](http://lib-service.com:8083)**

**Characteristics:**

* Fake university login portal
* Styled to resemble legitimate academic services
* Designed to harvest credentials

---

## **9. Threat Intelligence Attribution**

**Threat Actor:**
**Cobalt Dickens | Silent Librarian**

**Known For:**

* Targeting universities and research institutions
* Credential harvesting
* Proxy and redirect infrastructure
* Academic espionage campaigns

**MITRE Target Sector:**
**Research and Proprietary Data**

---

## **10. Infrastructure & Reputation**

* **lib-service.com flagged as malicious** in threat intelligence platforms
* Infrastructure reused across multiple campaigns
* Passive DNS records show rotating IP addresses indicating evasion tactics

---

## **11. MITRE ATT&CK Summary**

| **Tactic**           | **Technique**                      |
| -------------------- | ---------------------------------- |
| Resource Development | **T1583.001 – Domain Acquisition** |
| Defense Evasion      | **T1027 – Obfuscation**            |
| Initial Access       | **Phishing (HTML Attachment)**     |
| Credential Access    | **Fake Login Portal**              |
| Command & Control    | **Redirect-Based Infrastructure**  |

---

## **12. Final Assessment**

This incident was part of a **coordinated phishing campaign targeting academic institutions** rather than an isolated attack. The attacker demonstrated advanced understanding of:

* Email authentication weaknesses
* Domain impersonation techniques
* JavaScript obfuscation
* IDN homograph abuse
* Reuse of known malicious infrastructure

Immediate remediation and long-term email security improvements were recommended.

---

## **13. Detection Gaps Identified**

* Lack of **DMARC enforcement**
* **HTML attachments not restricted**
* No alerting on **abnormal MFA approval patterns**

---

## **14. Recommended Mitigations**

* Enforce **DMARC policy (p=reject)**
* Block or sandbox **HTML attachments**
* Monitor **MFA fatigue attacks**
* User awareness training for **phishing indicators**

---

## **15. Case Status**

* **Status:** Confirmed Phishing Campaign
* **Severity:** High
* **Confidence:** High

---

