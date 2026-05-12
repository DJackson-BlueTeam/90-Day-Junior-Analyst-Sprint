![alt text](https://github.com/DJackson-BlueTeam/90-Day-Junior-Analyst-Sprint/blob/ed374f0c351733e2dc3441614a35c0bba4949763/Daily-Logs/Shift%20Review/Shift%20Recap.png)

# Shift 4 Report: Reporting & Investigation Mastery
### Date: May 11th, 2026
### Shift: 9:30am - 19:30

### Executive Summary
This I have focused on Splunk Correlation capabilities with utilizing `transaction` `startswith` and `endswith` to join realted events from the udemy Power User Certification Path. In addition, there was an "Controlled Leak" from SillyPutty to capture the second-stage payload. Despite initlas delays due to machine recovery, all primary objectives were met by documenting critical intelligence on the malware's in-memory execution and C2 infrastructure. The environment was successfully re-isolated post-capture. However, while screenshoting the valuable information, the flare machine froze. In inditation, the RAM needs to be upgraded.

1. Splunk Correlation Mastery
  - I Progressed through Udemy Modules 5 (Correlating Events) and partially of Moduke 6 (Working with Lookups). I have applied the `transaction` command to connect with data with the Splunk systems that was tied together with similar `JSESSIONID` and userIPAddress relations.

 2. Persistent FieldExtraction
  - I was able to confirm all critical Sysmon fields that are permanently extracted as Global Knowledge Objects, optimizing search efficiency.  
