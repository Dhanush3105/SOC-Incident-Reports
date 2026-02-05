# MITRE ATT&CK Mapping – Incident 02
## Phishing Books – University MFA Abuse

| Tactic               | Technique ID | Technique Name                              | Evidence |
|----------------------|--------------|---------------------------------------------|--------- |
| Initial Access       | T1566        | Phishing                                    | User received a phishing email with a malicious HTML attachment impersonating a university library invoice |
| Resource Development | T1583.001    | Acquire Infrastructure: Domains             | Look-alike domains such as kinglord.ac.uk and lib-service.com were used |
| Defense Evasion      | T1027        | Obfuscated / Encrypted Files or Information | JavaScript used Unicode encoding and reversed strings to hide redirect URLs |
| Defense Evasion      | T1036        | Masquerading                                | HTML file disguised as a PDF invoice (library-invoice.pdf.html) |
| Credential Access    | T1556        | Modify Authentication Process               | Fake login page designed to harvest university credentials |
| Command and Control  | T1071.001    | Application Layer Protocol: Web Protocols   | Redirect-based phishing infrastructure using HTTP |
