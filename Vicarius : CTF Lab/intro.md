# Guide to Vicarius Purple Team CTF – Exploitation & Mitigation Lab

## Overview
This guide provides a structured walkthrough for the Vicarius Purple Team CTF, covering both **offensive exploitation** and **defensive mitigation** across Windows and Linux systems.  
Participants will simulate real-world attack scenarios, retrieve security artifacts (flags), and apply remediation using **Vicarius vRx**.

The training emphasizes the full **Purple Team lifecycle**:  
**Discover → Exploit → Detect → Remediate → Validate**

---

## Key Learnings
- Perform network and service enumeration using Nmap
- Exploit real-world vulnerabilities on Windows and Linux
- Achieve privilege escalation using misconfigurations
- Retrieve user-level and admin/root-level flags
- Detect vulnerabilities using Vicarius vRx detection scripts
- Remediate issues using patchless and configuration-based fixes
- Understand attacker techniques and defensive controls together

---

## Hands-on Lab Overview

### Lab 1: Windows Server – Exploitation & Mitigation

#### Objective
Exploit a vulnerable Windows Server, escalate privileges, retrieve flags, and remediate vulnerabilities using Vicarius vRx.

#### Key Activities
- Network enumeration using Nmap
- Exploiting Rejetto HFS (CVE-2024-23692)
- Gaining initial shell access via Metasploit
- Privilege escalation using a misconfigured scheduled task
- Retrieving:
  - User Flag
  - Administrator Flag
- Detecting vulnerabilities using vRx detection scripts
- Remediating:
  - Vulnerable HFS service
  - Misconfigured scheduled task

#### Artifacts Collected
- User Flag
- Administrator Flag
- Detection Output (HFS + Scheduled Task)
- Mitigation Output (HFS + Scheduled Task)

---

### Lab 2: Ubuntu 24 – Exploitation & Mitigation

#### Objective
Exploit a vulnerable Linux system, gain root access, retrieve flags, and remediate vulnerabilities using Vicarius vRx.

#### Key Activities
- Network enumeration using Nmap
- Identifying vulnerable Erlang SSH service
- Exploiting CVE-2025-32433 to gain shell access
- Privilege escalation via misconfigured sudo permissions
- Retrieving:
  - User Flag
  - Root Flag
- Detecting vulnerabilities using vRx Linux scripts
- Remediating:
  - Erlang vulnerability
  - Risky sudo NOPASSWD configuration

#### Artifacts Collected
- User Flag
- Root Flag
- Detection Output (Erlang + Sudo)
- Mitigation Output (Erlang + Sudo)

---

### Lab 3: Extra Challenge – KeePass Exploitation & Patchless Protection

#### Objective
Exploit a memory-based KeePass vulnerability and protect the application using Vicarius Patchless Protection.

#### Key Activities
- Exploiting KeePass (CVE-2023-32784) to retrieve the master password
- Memory dump analysis using provided tooling
- Enabling Patchless Protection via Vicarius dashboard
- Verifying protection through logs and event viewer

#### Outcome
- Successful credential extraction
- Validation of runtime protection without patching
- Demonstration of real-time exploit prevention

---

## Conclusion
This Purple Team CTF equips participants with practical, end-to-end security skills by combining **attack techniques** with **modern remediation strategies**.

By completing these labs, you gain hands-on experience in:
- Real-world exploitation paths
- Common privilege escalation mistakes
- Detection engineering
- Patchless and configuration-based mitigation

This approach mirrors real enterprise security operations, strengthening both **offensive awareness** and **defensive readiness**.
