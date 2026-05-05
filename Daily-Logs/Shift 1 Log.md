# Shift 1 Report: SIEM Zeroing & Telemtry Extraction
### Date: May 4th. 2026
### Shift: 7:30 - 19:00

## Engineering Hurdles Udemy Splunk Core Power User Certificaiton Preparation/ Malware Analysis and Networking Forensics 
### 1. Splunkbase Credential Gate**

   **The Conflict**
   - I have encountered a conflict between Splunkbase (Cloud) and Local Instance credentials during Eventgen installation.
   - I notice I have installed the most-recent updated version of Eventgen which was not compatiable with the free license Splunk Interface.
   - Was not able to upload any training data that can be utilized for the training course.
     
   **Resolution**
   - I have to download the 7.2.1 version that is compatiable with the free splunk interface for training purposes.
     
### 2. Sysmon XML Parsing Failures**

  **The Conflict**
  - I have identified that the xmlkv command failed to extract Sysmon attributes (`Name=Image`) due to it only reconizing the standard `<Tag>Value</Tag>` pairs.

  **Tactical Solution**
  - Custom Regex Implementation: I has to develope and deploy 14 custom `rex` (Regualr Expression) extractions on my FLARE-VM to bypass the XML parser to properly conduct Malware Analysis.
  - Persistence: I save the extraction filters as permanent `Global Knowledge Objects` in the `index=main sourcetype="XmlWinEventLog:Microsoft-Windows-Sysmon/Operational"` to ensure that the telemetry survives system reboots.
  - Also created a snapshot of the FLARE-VM before proceeding with Malware detonations.

  **Verbos Mode Validation**
  - Confirmed that the custom extractions require "Verbos Mode" to render high-fidelity process and network data in the Search HEad.

  **Telemetry Status: Green**
  - **Execution Pillar**
    - Image, CommandLine, ParentImage etc. are not structured and searchable.
  - **C2 Pillars**:
    - Source_Ip, Dest_IP etc. are verified via test browser traffic. 

