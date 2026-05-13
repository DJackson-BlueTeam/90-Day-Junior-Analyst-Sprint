![alt text](https://github.com/DJackson-BlueTeam/90-Day-Junior-Analyst-Sprint/blob/ed374f0c351733e2dc3441614a35c0bba4949763/Daily-Logs/Shift%20Review/Shift%20Recap.png)

# Shift 5 Report: Module 6/Advance Static Analysis/Volatility Forensics
### Date: May 11th, 2026
### Shift: 9:30 - 23:00

### Executive Summary

Day 2 operations focused on the stais and HBI analysis of `Malware.Stage0.exe`. I was able to capture the malware's post-execution memory state, revealing sophisticated stealth tactics and confirming its identity as an **Emotet Trojan Downloader**. There were intelligence regarding its in-memory execution, masquerading, and C2 control obtained through advacne memory forensics. The enviorment was promptly re-isolated post-capture and secured for reporting.

### 1. Engineering & Analysis Wins (Week 2, Day 2)

**Splunk Correlation Mastery**
- Progressed through **Udemy Module 6 (Working with Lookups)**, completing the theory for enriching log data with external threat intelligence. Applied correlation logic to existing telemetry.
    
**Memory Dump Capture**
- Successfully captured the `Malware.Stage0_Memory.elf` file from the FLARE-VM's post-execution state (after `Malware.Stage0` detonation), overcoming Windows Threat Protection hurdles.
    
**Volatility 3 Operational**
- Resolved symbol file download issues via temporary internet access and activated the `vol-env` environment for memory forensics.
    
**Process Identification (`windows.pslist`)**
- Confirmed `powershell.exe` (PID 2324) executed but was found in a "Disabled" state in memory, indicating rapid termination or hiding.

**Network Artifacts (`windows.netscan`)** 
- Identified `WerFault.exe` (PID 6680) attempting a C2 connection to `127.0.0.1:8443` (local loopback) which was `CLOSED`. This confirmed the Port 8443 C2 channel identified in the decoded payload.
     
### In-Memory String Extraction (`strings`)

**Malicious Component** 
  - Found `werflt.exe` executing from `C:\Users\Public\` and actively masquerading as `WerFault.exe`.

**Payload Type**
- Discovered strings definitively classifying `Malware.Stage0` as a `TrojanDownloader:097M/Emotet.ALZ!MTB`.

**C2 URL & Persistence**
- Identified `ownhttp://zz.8282.space/nw/ss/C:Windows\SysWOW64` as the download URL and `schtasks /create /tn XN-DTZY /TR C:\Windows\SysWOW64\VIP.exe` for scheduled task persistence.

**Defense Evasion**
- Confirmed `SysWOW64` execution context and attempts to disable `WindowsUpdate`.

### 2. Obstacles & Resolutions

**Memory Dump Path Error (`VERR_PATH_NOT_FOUND`)** 
- Initially encountered errors due to incorrect host user profile paths in the output filename.

**Resolution**
- Corrected the output path to `C:\MemoryDumps\` on the host machine and manually created the folder.

**Memory Dump Transfer (Windows Threat Protection)**
- `scp` transfer was initially blocked by Windows Threat Protection.

**Resolution** 
- Temporarily disabled Windows Threat Protection on the host machine to allow the `stage0.elf` file transfer.

**Volatility `vol-env` Activation Error** 
- Encountered `No such file or directory` due to incorrect pathing for `vol-env`.

**Resolution** 
- Identified nested `volatility3` directory structure and activated the environment from the correct `~/volatility3` path.

**Volatility `SyntaxError: source code cannot contain null bytes`**
- Attempted to execute the `.elf` file directly as a Python script.

**Resolution**
- Corrected command syntax to `python3 vol.py -f [memory_dump] [plugin]`, ensuring proper Volatility 3 execution.

**Volatility Symbol File Download Failure**
- Volatility 3 failed to download symbol files due to Ubuntu's network isolation.

**Resolution**
- Temporarily enabled NAT on Ubuntu and configured Netplan for DHCP, allowing symbol files to download, then re-isolated the VM.

