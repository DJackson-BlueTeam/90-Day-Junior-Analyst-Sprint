![alt text](https://github.com/DJackson-BlueTeam/90-Day-Junior-Analyst-Sprint/blob/ed374f0c351733e2dc3441614a35c0bba4949763/Daily-Logs/Shift%20Review/Shift%20Recap.png)

# Shift 4 Report: Reporting & Investigation Mastery
### Date: May 11th, 2026
### Shift: 9:30am - 19:30

### Executive Summary
This I have focused on Splunk Correlation capabilities with utilizing `transaction` `startswith` and `endswith` to join realted events from the udemy Power User Certification Path. In addition, there was an "Controlled Leak" from SillyPutty to capture the second-stage payload. Despite initlas delays due to machine recovery, all primary objectives were met by documenting critical intelligence on the malware's in-memory execution and C2 infrastructure. The environment was successfully re-isolated post-capture. However, while screenshoting the valuable information, the flare machine froze. In inditation, the RAM needs to be upgraded.

**1. Splunk Correlation Mastery**
  - I Progressed through Udemy Modules 5 (Correlating Events) and partially of Moduke 6 (Working with Lookups). I have applied the `transaction` command to connect with data with the Splunk systems that was tied together with similar `JSESSIONID` and userIPAddress relations.

**2. Persistent FieldExtraction**
  - I was able to confirm all critical Sysmon fields that are permanently extracted as Global Knowledge Objects, optimizing search efficiency.

**3. Controlled Internet Leak**
  - By successully configuring, and executed a temporary, highly restricted interent access via pfSense for `SillyPuTTy`. This allowed the malware to attempt `phoning` the remote C2 for access.

![alt text](https://github.com/DJackson-BlueTeam/90-Day-Junior-Analyst-Sprint/blob/663bf3071255738f06bc2bd345d079fb05e83f05/Daily-Logs/Shift%20Review/Live%20Detonation/2.%20SillyPuTTY%20FW%20Rule.png)
  - Procmon was setup tp monitor specific events from the malicious binary `putty[.]exe`

![alt text](https://github.com/DJackson-BlueTeam/90-Day-Junior-Analyst-Sprint/blob/663bf3071255738f06bc2bd345d079fb05e83f05/Daily-Logs/Shift%20Review/Live%20Detonation/3.%20Procmon%20Monitoring.png)

**4. Second-Stage Payload Capture**
  - Since the binary was compressed, it hid most of it activity from the network to prevent monitoring systems.
  - The compressed payload wa executed through a PowerShell Execution (lost the live data from machine reset).
  - This confirmed this was a in-memory execution.

**5. C2 Infrastructure Mapping**
  - Wireshark analysis confirmed the malware attempted to resolve the `bonus2.corporatebonucapplication.local`. which was blocked by pfSense, preventing a Full C2 connection through port 8443

### Obstacles and Resolution 

**1. Shift Delay**
  - Recovered from a very extended work shift causing a delay in Week 2 Day 1 Kick off.
- I adjusted the schedule to extend core shift hour to ensure all objective for today was met.

**2. pfSense Rule Activation**
  - The "TEMP_MALWARE_DOWNLOAD_SillyPuTTy" firewall rule was inactive despite being configure.
  - I had identified the uncheck activition box in the GUI. I enabled and reapplied the rule.

**3. No File Creation (EventID 11)**
  - No file creation was detected forthe second stage payload.
  - I was able to identify the execution was in memory from a `powershell.exe` commandline.

**4. DNS Resolution Block**
  - the malware DNS query for `bonus2.corporateapplication.local` resulted in a "No Such Name" response.
  - This was due to pfSense successfully blocked the DNS resolution to prevent the malware to establish a C2 connection.
 
Analyst Note
---
- The live Detonation data was lost from the Flare-VM machine reset. I was able to retrive the splunk instance logs, however, the wireshark network monitoring was lost and could not show proof of pfSense denying the C2 Connection the malicious binary attempted to perform.
- This is the first Live Detonation, so I expected some errors to occure. 
- I do expect the incident reports of Live Detonations to improve over this 90 Day SOC Analyst/ Junior Analyst Roadmap
