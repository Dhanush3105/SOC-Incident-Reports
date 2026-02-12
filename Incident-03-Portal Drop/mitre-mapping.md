
#  MITRE ATT&CK Mapping

##  Initial Access

###  Brute Force

* **Technique ID:** T1110
* **Description:** Attacker performed repeated login attempts against `login.php` to gain valid credentials.

---

##  Execution

### 🔹 Server Software Component – Web Shell

* **Technique ID:** T1505.003
* **Description:** Attacker uploaded `invoice.php` to the uploads directory and executed commands via HTTP requests.

---

##  Discovery

### 🔹 System Information Discovery

* **Technique ID:** T1082
* **Evidence:** First decoded command was `whoami`.

---

##  Command & Control

### 🔹 Application Layer Protocol (HTTP)

* **Technique ID:** T1071.001
* **Description:** Attacker communicated with the web shell using HTTP POST requests with Base64-encoded parameters.

---

##  Exfiltration

### 🔹 Exfiltration Over C2 Channel

* **Technique ID:** T1041
* **Description:** CRM database data was exfiltrated to `portaldrop2025.xyz`.

---
