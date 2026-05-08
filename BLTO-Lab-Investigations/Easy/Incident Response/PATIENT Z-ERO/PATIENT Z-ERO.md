![alt text](https://github.com/DJackson-BlueTeam/90-Day-Junior-Analyst-Sprint/blob/e78587686a77985fe6a314eb7a28eaf848c8f3ce/BLTO-Lab-Investigations/Easy/Incident%20Response/PATIENT%20Z-ERO/PATIENT%20Z-ERO%20IMG/z-ero.png)

#### You’re a CRISIS responder deployed to the Zeta-9 Bio-Tech facility.
#### Mission: 
#### Use Graylog to analyze the Project-Z edge device and confirm whether a breach has occurred. Uncover how it happened — and why. Good luck, responder. The fate of Zeta-9 and possibly the world may depend on what you find.

## Scenario

#### Deep beneath the city, Zeta-9 Corporation’s secret bioweapon project went catastrophically wrong after an explosion released a mind-altering virus into the wild. The company covered it up as a “minor fire,” but leaked internal files from a mysterious hacktivist group told another story — one of containment failure and stolen research. Now, all contact with the facility is lost. As part of Zeta-9’s elite CRISIS unit, you’re deployed on-site to investigate the breach. The complex is silent — lights flicker, systems are offline, and something feels off. Your first task: access your C.R.I.S.I.S workstation. A briefing document on your desktop may provide more information... For this challenge, a GrayLog server is available at: URL: http://localhost:9000 Credentials: admin / Password A config file was obtained and is available at: /home/ubuntu/Documents/backup/backup.conf WARNING: graylog can take a few minutes to load!

#### Investigation Questions

**1. What is the vendor of the Project Z firewall?**

- Looking through the logs in Ubuntu, I seen a Firewall name "FortiFirewall" and look up the potential "Company". 

![alt text](https://github.com/DJackson-BlueTeam/90-Day-Junior-Analyst-Sprint/blob/2cf4defd991f8d1f2005a836270dc9967f4f4718/BLTO-Lab-Investigations/Easy/Incident%20Response/PATIENT%20Z-ERO/PATIENT%20Z-ERO%20IMG/1..png)

**2. What is the firmware version is running on the firewall?**

- I grep for version and got results 

![alt text](https://github.com/DJackson-BlueTeam/90-Day-Junior-Analyst-Sprint/blob/e78587686a77985fe6a314eb7a28eaf848c8f3ce/BLTO-Lab-Investigations/Easy/Incident%20Response/PATIENT%20Z-ERO/PATIENT%20Z-ERO%20IMG/2..png)

Answer: 7.0.14

**3. The firewall has an administrative user what is their username?**
- I used `-i` since I was not sure if `user` was capitalized or not.

![alt text](https://github.com/DJackson-BlueTeam/90-Day-Junior-Analyst-Sprint/blob/e78587686a77985fe6a314eb7a28eaf848c8f3ce/BLTO-Lab-Investigations/Easy/Incident%20Response/PATIENT%20Z-ERO/PATIENT%20Z-ERO%20IMG/3..png)

Answer: admin

**4. Recently, the Project Z network has been unstable. The IT team suspects a possible breach. Can you identify if there is any CVE with a public exploit that could compromise the edge device?**

- This question needed a little OSINT routine.

![alt text](https://github.com/DJackson-BlueTeam/90-Day-Junior-Analyst-Sprint/blob/e78587686a77985fe6a314eb7a28eaf848c8f3ce/BLTO-Lab-Investigations/Easy/Incident%20Response/PATIENT%20Z-ERO/PATIENT%20Z-ERO%20IMG/4..png)

Answer: CVE-20025-55591

**5. A threat actor attempted to access the management console. What is the attacker’s IP address?**

- I grep for ssh and notice a ping request was made.
- Usually when a ping request is executed, is comes from the ip address that attempting to connect to another ip.
- In this case it was the adversary making the ping request.

![alt text](https://github.com/DJackson-BlueTeam/90-Day-Junior-Analyst-Sprint/blob/e78587686a77985fe6a314eb7a28eaf848c8f3ce/BLTO-Lab-Investigations/Easy/Incident%20Response/PATIENT%20Z-ERO/PATIENT%20Z-ERO%20IMG/5..png)

Answer: 38.68.134.215

**6. There have been VPN connection attempts. Which user successfully established a connection?**

- The ip had to be similar to the ip of the ip that attempted the access management console. 
- So I pulled all ip's for the config.file "backup.txt" (I coverted it to readable text by decoding the base64).

![alt text](https://github.com/DJackson-BlueTeam/90-Day-Junior-Analyst-Sprint/blob/e78587686a77985fe6a314eb7a28eaf848c8f3ce/BLTO-Lab-Investigations/Easy/Incident%20Response/PATIENT%20Z-ERO/PATIENT%20Z-ERO%20IMG/6..png)

Answer: 38.68.134.103 (I put what was in the screenshot, maybe the were a minor malfunction within the investigation environment since it is a RETIRED system.)

**7. A specific user added the successful VPN user, what is the users name?**

- I spent alot of time trying to find the user, but kept running into dead ends, so I went to the interface of graylog and pulled the results.
- Evidently, all of the information that was on the GUI interface was not logged in the backup.conf file.

![alt text](https://github.com/DJackson-BlueTeam/90-Day-Junior-Analyst-Sprint/blob/e78587686a77985fe6a314eb7a28eaf848c8f3ce/BLTO-Lab-Investigations/Easy/Incident%20Response/PATIENT%20Z-ERO/PATIENT%20Z-ERO%20IMG/8..png)
Answer: zeta_admin

**8. What is the source IP address used during the VPN connection?**

![alt text](https://github.com/DJackson-BlueTeam/90-Day-Junior-Analyst-Sprint/blob/e78587686a77985fe6a314eb7a28eaf848c8f3ce/BLTO-Lab-Investigations/Easy/Incident%20Response/PATIENT%20Z-ERO/PATIENT%20Z-ERO%20IMG/8.%201.png)

Answer: 10.1.1.10

**9. Which internal Zeta-9 IP address was the attacker scanning?**

- grep for ping again and got the results on the zeta9 ip.

![alt text](https://github.com/DJackson-BlueTeam/90-Day-Junior-Analyst-Sprint/blob/e78587686a77985fe6a314eb7a28eaf848c8f3ce/BLTO-Lab-Investigations/Easy/Incident%20Response/PATIENT%20Z-ERO/PATIENT%20Z-ERO%20IMG/9..png)
Answer: 192.168.86.1

**10. Which ports was the attacker mainly interested in discovering on the internal network?**

- I had a similar issue with searching through the .conf file that was coverted to .txt for readable purposes and couldn't get the ports I end up discovering through the GUI interface of Graylogs.
- I created a Aggregation Chart with the number of time the dstport was accessed from an ip.

![alt text](https://github.com/DJackson-BlueTeam/90-Day-Junior-Analyst-Sprint/blob/e78587686a77985fe6a314eb7a28eaf848c8f3ce/BLTO-Lab-Investigations/Easy/Incident%20Response/PATIENT%20Z-ERO/PATIENT%20Z-ERO%20IMG/10..png)
