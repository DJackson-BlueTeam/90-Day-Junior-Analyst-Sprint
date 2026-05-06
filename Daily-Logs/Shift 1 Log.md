![alt text](https://github.com/DJackson-BlueTeam/90-Day-Junior-Analyst-Sprint/blob/b73f6075e4928a6bd4ace0829a9a9458b6836287/Daily-Logs/Shift%20Review/Shift%20Recap.png)

# Shift 1 Report: SIEM Zeroing & Telemetry Extraction
### Date: May 4th. 2026
### Shift: 7:30 - 19:00

## Engineering Hurdles Udemy Splunk Core Power User Certificaiton Preparation/ Malware Analysis and Networking Forensics 
### 1. Splunkbase Credential Gate**

   **The Conflict**
   - I have encountered a conflict between Splunkbase and Local Instance credentials during Eventgen installation.
   - I notice I have installed the most-recent updated version of Eventgen which was not compatiable with the free license Splunk Interface.
   - I was not able to upload any training data that can be utilized for the training course.
     
   **Solution**
   - I have to download the 7.2.1 version that is compatiable with the free splunk interface for training purposes.

![alt text](https://github.com/DJackson-BlueTeam/90-Day-Junior-Analyst-Sprint/blob/b73f6075e4928a6bd4ace0829a9a9458b6836287/Daily-Logs/Shift%20Review/1.%20Eventgen.png)

### 2. Sysmon XML Parsing Failures**

  **The Conflict**
  - I have identified that the `xmlkv` command failed to extract Sysmon attributes (`Name=Image`) due to it only reconizing the standard `<Tag>Value</Tag>` pairs.

  **What is xmlkv?**
  - XML Key-Values is a splunk search command used to automatically extract fields from events that contains XML-formatted data. 

![alt text](https://github.com/DJackson-BlueTeam/90-Day-Junior-Analyst-Sprint/blob/b73f6075e4928a6bd4ace0829a9a9458b6836287/Daily-Logs/Shift%20Review/2.%20xmlkv.png)

  **Solution**

  - **Custom Regex Implementation**
    - I has to develope and deploy custom `rex` (Regualr Expression) extractions on my FLARE-VM to bypass the XML parser to properly conduct Malware Analysis.

![alt text](https://github.com/DJackson-BlueTeam/90-Day-Junior-Analyst-Sprint/blob/b73f6075e4928a6bd4ace0829a9a9458b6836287/Daily-Logs/Shift%20Review/3.Fields.png)

![alt text](https://github.com/DJackson-BlueTeam/90-Day-Junior-Analyst-Sprint/blob/b73f6075e4928a6bd4ace0829a9a9458b6836287/Daily-Logs/Shift%20Review/4.%20Example%20of%20Extraction.png)
  
  - **Persistence**
    - I save the extraction filters as permanent `Global Knowledge Objects` in the `index=main sourcetype="XmlWinEventLog:Microsoft-Windows-Sysmon/Operational"` to ensure that the telemetry survives system reboots.
    - Also created a snapshot of the FLARE-VM before proceeding with Malware detonations.

  **Verbos Mode Validation**
  - Confirmed that the custom extractions require "Verbos Mode" to render high-fidelity process and network data in the Search HEad.

  **Telemetry Chart**
  
|   |   |   |  
|---|---|---|
|**Field Name**|**Regex Pattern**|**Purpose**|
|**_Category 1: Process Linage (The "Execution") Pillar_**|||
|Image|`Name='Image>(?<Image>[^<]+)`|The path of the malicious binary.|
|CommandLine|`Name='CommandLine'>(?<CommandLine>[^<]+)`|Arugments used (encoded Powershell)|
|ParentImage|`Name='ParentImage'>(?<ParentImage>[^<]+)`|The "Father" process (`word.exe` spawning` cmd.exe`)|
|ParentCommandLine|`Name='ParentCommandLine'>(?<ParentCommandLine>[^<]+)`|How the parent process was executed.|
|User|`Name='User'>(?<User>[^<]+)`|The account context `AUTHORITY\SYSTEM`|
|CurrentDirectory|`Name='CurrentDirectory'>(?<CurrentDirectory>[^<]+)`|Where the malware is sitting on the disk|
|Hashes|`Name='Hashes'>(?<Hashes>[^<]+)`|MD5/SHA256 for VirusTotal lookups.|
|**_Category 2: Network Telemetry (The C2 Pillar)_**|
|Source_IP|`Name='SourceIp'>(?<Source_IP>[^<]+)`|The infected host IP|
|Dest_IP|`Name='DestinationIp'>(?<Dest_IP>[^<]+)`|The Attacker's C2 server IP|
|Source_Port|`Name='SourcePort'>(?<Source_Port>[^<]+)`|Local port used|
|Dest_Port|`Name='DestinationPort'>(?<Dest_Port>[^<]+)`|Target Port|
|Protocol|`Name='Protocol'>(?<Protocol>[^<]+)`|TCP vs UDP|
|QueryName|`Name='QueryName'>(?<DNS_Query>[^<]+)`|(EventID 22) The domain the malware is looking up.|
|**_Category 3: Persistence & Stealth_**|||
|TargetFilename|`Name='TargetFilename'>(?<File_Created>[^<]+)`|(EventID 11) New files dropped by mawalre|
|TargetObject|`Name='TargetObject'>(?<Registry_Path>[^<]+)`|(Event 12/13) Registry Keys Modified for boot-start.|
|Details|`Name='Details'>(?<Registry_Value>[^<]+)`|The actual data written to a registry key.|
