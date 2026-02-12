
#  Analyst Notes

###  Attack Pattern Observed

The attack followed a clear and structured intrusion chain:

1. Brute force login attempts
2. Successful authentication
3. Malicious PHP file upload
4. Web shell execution via encoded parameters
5. Reverse shell establishment
6. Sensitive file access
7. Data exfiltration

This indicates a deliberate and automated attack, likely conducted using scripting tools rather than manual exploitation.

---

###  Security Weaknesses Identified

* No brute-force rate limiting
* No upload file validation
* PHP execution allowed in uploads directory
* No monitoring for suspicious User-Agent patterns
* No outbound traffic monitoring controls

---

###  Why This Was High Severity

* Remote code execution achieved
* Reverse shell established
* Sensitive configuration accessed
* Potential database compromise
* Confirmed data exfiltration

This represents full application compromise.

---

###  Recommended Detection Improvements

* Alert on high-volume POST requests to `login.php`
* Detect repeated HTTP 401 responses from same IP
* Monitor file creation in `/uploads/` directory
* Alert on Base64-encoded parameters in URL queries
* Detect web server processes spawning shell commands
* Monitor outbound TCP connections to unknown external IPs

---

###  Key Learning Outcomes

* Web shells often use encoded query parameters
* `php-fpm` executing system commands is high-risk behavior
* Upload directories should never allow executable scripts
* User-Agent analysis can reveal automation tools
* Base64 decoding is critical in web attack investigations

