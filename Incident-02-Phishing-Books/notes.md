# Analyst Notes – Incident 02
## Phishing Books – University MFA Abuse

### 1. How the Incident Was Discovered
This incident was not detected by automated SIEM alerts. It was reported directly by the end user after noticing multiple unsolicited MFA approval requests. This highlights the importance of user awareness and reporting in phishing detection.

### 2. Why Email Security Failed
The sender domain had DMARC set to "none", with no SPF or DKIM enforcement. Because of this misconfiguration, the phishing email was able to bypass email security filters despite impersonating a trusted academic domain.

### 3. Social Engineering Insight
The attacker used typosquatting by replacing characters in the legitimate university domain (kingford.ac.uk → kinglord.ac.uk). This small visual difference is often overlooked by users, making the attack highly effective.

### 4. Malicious Attachment Behavior
The attachment was an HTML file disguised as a PDF invoice. Unlike traditional malware, HTML attachments can execute JavaScript directly in the browser without macros or downloads, making them effective for phishing.

### 5. Obfuscation Techniques Observed
The embedded JavaScript stored URLs as Unicode-encoded characters and reversed strings. This technique hides malicious indicators from static scanners and casual inspection. The script dynamically reconstructed the URL at runtime and redirected the victim automatically.

### 6. IDN Homograph Attack
The phishing URL used a Cyrillic character instead of a standard ASCII character, causing the browser to convert it into Punycode format (xn--). This prevents users from visually identifying the fake domain and is an advanced phishing evasion technique.

### 7. Attacker Intent
The final landing page mimicked a legitimate university login portal to harvest credentials. The presence of a hidden mocking message inside the script suggests confidence and reuse of infrastructure, which is common in long-running phishing campaigns.

### 8. Threat Actor Context
Threat intelligence links the infrastructure to Cobalt Dickens (Silent Librarian), a known threat actor group that targets universities and research institutions to steal credentials and access proprietary academic data.

### 9. Key SOC Takeaways
- User reports can be the first and only alert for phishing incidents
- DMARC, SPF, and DKIM misconfigurations significantly increase risk
- HTML attachments should be treated as high-risk
- IDN homograph attacks are increasingly common in academic phishing
- MITRE ATT&CK mapping helps standardize incident documentation

### 10. What I Learned From This Incident
This investigation improved my understanding of phishing infrastructure, obfuscation techniques, IDN attacks, and the importance of manual analysis when automated tools fail. It reinforced how real-world phishing campaigns combine multiple evasion techniques rather than relying on a single weakness.
