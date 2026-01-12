# Lab 3: Extra Challenge – KeePass Exploitation & Patchless Protection

---

## 🎁 Extra Flag

### Exploiting Keepass vuln

1) Run Keepass version 2.53 and type the master password:

```text
Passw0rd!
```

---

## Prerequisites
(Jump to point 2 if you are working with our VMs)

If you're working in your environment, please follow the instructions.

Download and install this file:
https://dotnet.microsoft.com/es-es/download/dotnet/thank-you/runtime-7.0.14-windows-x64-installer

Download and install the following file:
https://download.visualstudio.microsoft.com/download/pr/93961dfb-d1e0-49c8-9230-abcba1ebab5a/811ed1eb63d7652325727720edda26a8/dotnet-sdk-8.0.100-win-x64.exe

Download the exploit from the following link:
https://github.com/vdohney/keepass-password-dumper

Save the exploit in the following path:

```text
C:\Vulnerabilities\keepass-dumper-CVE-2023-32784\
```

Note: In the VM, you have these files installed. You don’t need to do it.

---

## Step 2: Dump the Memory

Then dump the memory file to get the master password using the script called `memory-dump.ps1` that is in the path:

```text
C:\Vulnerabilities\keepass-dumper-CVE-2023-32784\
```

Run:

```powershell
powershell.exe -ExecutionPolicy Bypass -file memory-dump.ps1
```

Note: You need to enter the process ID that references to KeePass.

---

## Step 3: Extract the Password

Now is the time to get the results from the file `keepass.dmp`:

```bash
dotnet.exe run .\keepass.dmp
```

As you can see, we obtained the master password:

```text
Passw0rd!
```

---

## 🛡️ Using Patchless Protection in Keepass CVE-2023-32784

Now we will protect the getting credential attack against Keepass using vRx Patchless Protection technology.

Check if the CVE is present in the asset.

```text
Vulns Discovery -> Active CVE
```

Click the app Keepass and Enable Patchless Protection from the dashboard.

---

## Validation

Check in the asset that Patchless Protection is enabled from the Powershell terminal.

Run the following command:

```powershell
powershell.exe -command "Get-Content -tail 100 'C:\Program Files\Vicarius\Topia\Trace\TopiaTrace.log'"
```

---

## Event Logs

Now, check the logs in Event Logs.

As you can see, our application was protected by our Patchless Protection technology.

---

## Conclusion

This concludes the lab exercises for the **Vicarius Purple Team CTF – Exploitation & Mitigation Lab**.

Throughout these labs, participants successfully simulated real-world attack scenarios across Windows and Linux environments, retrieved multiple security artifacts, and applied effective remediation using Vicarius vRx.

By completing these exercises, participants gained hands-on experience in:

- Network and service enumeration  
- Exploiting real-world vulnerabilities  
- Privilege escalation techniques  
- Artifact and flag retrieval  
- Vulnerability detection using vRx  
- Patchless and configuration-based remediation  

These labs demonstrate the full **Purple Team lifecycle**—from exploitation to detection and mitigation—providing practical insight into how offensive techniques are countered with modern defensive controls.

Participants are encouraged to review the mitigation outputs, detection logs, and protection mechanisms enabled during the exercises to reinforce learning outcomes and best practices.

---

**End of Lab Exercises**
