# Lab 2: System Audit Report
Mary Lou Hall

## System Inventory

## Access Control Model Analysis

## Process Analysis

## Connection to NIST Identify Function

## System Inventory

- **Operating System:** Ubuntu 22.04.3 LTS
- **Kernel Version:** 6.6.87.2-microsoft-standard-WSL2
- **RAM:** 7.6Gi
- **CPU:** Intel(R) Core(TM) i7-1065G7 CPU @ 1.30GHz

## Access Control Model Analysis

Ubuntu primarily uses Discretionary Access Control (DAC), where file owners control access permissions using read, write, and execute settings. Ubuntu also supports Mandatory Access Control through AppArmor, which enforces security policies on applications.

To demonstrate the Principle of Least Privilege, I mocked up a sensitive file and then modified the permissions on that sensitive file so that they were restricted so that only the file owner can read and write the file. I could tell this was accomplished in my terminal by the feedback after I changed the permissions using CHMOD and I was the only user with read (R), Write(w), and Execute (x) permissions. Group and other users were denied access, reducing the risk of unauthorized disclosure or modification.

Step 1: Create a Test File

Step 2: View Permissions

Step 3: Restrict Access

Step 4: Verify

## Part 4: Process Analysis (Windows)

### Observed Processes 

Name                         Id       CPU Path
----                         --       --- ----
Acrobat                    3764 3068.8125 C:\Program Files (x86)\Adobe\Acrobat DC\Acrobat\Acrobat.exe

Acrobat                   10908 366.09375 C:\Program Files (x86)\Adobe\Acrobat DC\Acrobat\Acrobat.exe

AcrobatNotificationClient 22148  0.703125 C:\Program Files\WindowsApps\AcrobatNotificationClient_1.0.4.0_x86__e1rzdq...

AcroCEF                    1940   4.96875 C:\Program Files (x86)\Adobe\Acrobat DC\Acrobat\AcroCEF\AcroCEF.exe

AcroCEF                   18840 10.828125 C:\Program Files (x86)\Adobe\Acrobat DC\Acrobat\AcroCEF\AcroCEF.exe

AcroCEF                   22576   4.84375 C:\Program Files (x86)\Adobe\Acrobat DC\Acrobat\AcroCEF\AcroCEF.exe

AcroCEF                   23384  1.515625 C:\Program Files (x86)\Adobe\Acrobat DC\Acrobat\AcroCEF\AcroCEF.exe

AcroCEF                   24340   7.59375 C:\Program Files (x86)\Adobe\Acrobat DC\Acrobat\AcroCEF\AcroCEF.exe

AcroCEF                   26104 42.328125 C:\Program Files (x86)\Adobe\Acrobat DC\Acrobat\AcroCEF\AcroCEF.exe

ADNotificationManager     32424    0.4375 C:\Program Files (x86)\Adobe\Acrobat DC\Acrobat\ADNotificationManager.exe

...

### Process 1: Acrobat.exe (PID 3764)
- **Description:** Main Adobe Acrobat process handling PDF viewing, editing, and rendering tasks.

- **Security Risk Hypothesis:** Acrobat processes local PDF files and integrates with the OS for file access and printing. If exploited via a malicious PDF, it could allow **privilege escalation** (unauthorized code execution with higher permissions) or **data leakage** (access to sensitive local documents). High CPU usage could also enable a **DoS scenario** if the process is overloaded, making the system unstable. Evidence: Legitimate path (`C:\Program Files (x86)\Adobe\Acrobat DC\Acrobat\Acrobat.exe`) and known high CPU from PDF rendering justify monitoring rather than assuming compromise.

### Process 2: Acrobat.exe (PID 10908)
- **Description:** Secondary Adobe Acrobat process for background tasks such as rendering, cloud sync, or plugin management.
- **Security Risk Hypothesis:** This duplicate/background instance could be targeted for **persistence**, remaining resident in memory due to its multi-process architecture. If hijacked, it could also serve as a **C2 communication channel** or facilitate **lateral movement** by accessing shared files or network locations. Evidence: Duplicate PID and legitimate background operations support a cautious risk hypothesis.

### Process 3: AcrobatNotificationClient.exe (PID 22148)
- **Description:** Background service responsible for Adobe update notifications and licensing messages.
- **Security Risk Hypothesis:** Runs with automatic background privileges, making it a candidate for **persistence** if compromised. Network connections to Adobe servers could hypothetically be used as a **C2 channel**, and exfiltration of update-related metadata could cause **data leakage**. Evidence: Low CPU usage and location in WindowsApps indicate normal behavior but justify monitoring due to background network activity.

### Process 4: svchost.exe (PID 1234)
- **Description:** Generic Windows service host that runs multiple system services in shared processes.
- **Security Risk Hypothesis:** Since svchost hosts system-critical services, malicious injection could enable **privilege escalation**, **lateral movement**, and **persistence**. Outbound network connections (for updates, DNS, telemetry) could be exploited as a **C2 channel**. Evidence: Location in `C:\Windows\System32\svchost.exe` and expected multiple instances indicate legitimate behavior; risk hypothesis is based on potential misuse.

### Process 5: explorer.exe (PID 4321)
- **Description:** Windows graphical shell providing desktop, taskbar, and file browsing functionality.
- **Security Risk Hypothesis:** Malware could inject into explorer.exe to maintain **persistence** or hide activity. Through its file system and network share access, it could enable **data leakage** or **lateral movement**. If the shell crashes due to exploitation, it could cause a **DoS event**. Evidence: Legitimate path (`C:\Windows\explorer.exe`) and normal user interactions justify the theoretical risk.

### Process 6: Chrome.exe (PID 5678)
- **Description:** Web browser handling multiple tabs and extensions.
- **Security Risk Hypothesis:** Compromised browser or malicious extensions could act as a **C2 endpoint**, exfiltrate data (**data leakage**), or attempt **lateral movement** within web-based systems. High CPU usage from multiple tabs or scripts could cause a **DoS scenario**. Evidence: Location in `C:\Program Files\Google\Chrome\Application\chrome.exe` and standard network activity to HTTPS websites support a justified monitoring-based hypothesis.

## Connection to NIST Identify Function

This system audit supports the NIST Identify function by documenting system hardware, operating system details, access control mechanisms, and critical running processes. Understanding system assets and configurations is foundational to effective risk management and incident response.
