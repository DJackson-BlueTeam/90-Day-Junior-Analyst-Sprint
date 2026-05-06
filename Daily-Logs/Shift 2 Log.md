![alt text](https://github.com/DJackson-BlueTeam/90-Day-Junior-Analyst-Sprint/blob/488491fb21c4bb5e04c5165ed3279ae31e2bb5e4/Daily-Logs/Shift%20Review/Shift%20Recap.png)

# Shift 2 Report: Dynamic Detonation & Behavioral Analysis
### Date: May 5th. 2026
### Shift: 4:30 - 23:00

### **Summary**
Shift 2 fouced on the dynamic detonatino of the `SillyPutty` sample within the isolated FLARE-VM sandbox.
The objective was to confirm the behavors formulated during Day 1 static triage. Telemetry was successfully capture in aplunk Enterprise and FakeNet-NG, identifying a multi-stage execution chaing and a remote C2 callback.

**Host-Based Indicators**

**Packing Confirmation**
  - I verified a gap between the Virutal Size (169,468) and Raw Data Size (29,600), confirming that the binary was packed to evade static setection.

![alt text](https://github.com/DJackson-BlueTeam/90-Day-Junior-Analyst-Sprint/blob/ffdbf1492e63816bd0121f370b21c419d073930a/Malware-Analysis-Reports/1.%20Silly%20Putty/SillyPutty%20Img/8.1.png)

![alt text](https://github.com/DJackson-BlueTeam/90-Day-Junior-Analyst-Sprint/blob/ffdbf1492e63816bd0121f370b21c419d073930a/Malware-Analysis-Reports/1.%20Silly%20Putty/SillyPutty%20Img/8.2.png)

**Execution Lineage (Event 1)**
  - The malware utilized a Decoy Tactic by launching a legitimate "PuTTY Configuration" window.

  ![alt text](https://github.com/DJackson-BlueTeam/90-Day-Junior-Analyst-Sprint/blob/ffdbf1492e63816bd0121f370b21c419d073930a/Malware-Analysis-Reports/1.%20Silly%20Putty/SillyPutty%20Img/9..png)
  
  - In the background, a blue screen flashed initiating a powershell execution.

**Persistence**
- Telemtry suggestion preparation for registy-based persistence, consistent with the "Modify Registry" capability.

**Network-Based Indicators**

**DNS Interception**
- FakeNet-NG capture a DNS query to github.com. This indicates the malware likely used a trusted domain to host second-stage payloads or configuration files.

**C2 Callback**
- _Adversary IP_: 7.74.181.33
- _Callback Port_: Port 22 (TCP)
  - The malware attempted to tunnel traffic over port 22 (SSH) to bypass outbound port-filtering rules (evasion technique).
 
**Technical Obstacles & Solutions**

**DataTransfer Failures (SSH/SCP)** 
- I encountered a `ConnectionTimed Out` error when attempting to push Base64 strings from the FLARE-VM to the Ubuntu forensic workstation.

**Solution**
- I had to install OpenSSH on the ubunutu to allow data transfer within the control environment.
- In addition, had to update the ufw to allow SSH 22 traffic within the controlled enviorment.

**Telemtry & Environment Status**
Splunk Status
- The Unufied Hunt Query successfully correlated host process starts with network connections attempts.

![alt text](https://github.com/DJackson-BlueTeam/90-Day-Junior-Analyst-Sprint/blob/ffdbf1492e63816bd0121f370b21c419d073930a/Malware-Analysis-Reports/1.%20Silly%20Putty/SillyPutty%20Img/10..png)

**Snapshot Intergrity**
- Reverted to baseline after capturing SillyPutty detonation state for forensic study. 
