![alt text](https://github.com/DJackson-BlueTeam/90-Day-Junior-Analyst-Sprint/blob/4cfd6c2542257be510b7f55ce11a68b445bbbc20/BLTO-Lab-Investigations/Easy/Security-Operations/FunGames/FunGamesImg/fungames.png)

### FunGames is an e-commerce platform for video games owned by FunTech Inc. and has become the target of a criminal who managed to obtain access credentials and exfiltrate some sensitive data. FunTech analysts provide the fugames.pcap file to analyze and retrieve all the pieces of information about the attack.

## Scenario

FunGames is an e-commerce platform for video games owned by FunTech Inc. and has become the target of a criminal who managed to obtain access credentials and exfiltrate some sensitive data. FunTech analysts provide the fugames.pcap file to analyze and retrieve all the pieces of information about the attack.

### Investigation Question

**1. What is the IP address of the attacker who is performing the attack?**

- To get the ip address of the victim in wireshark, I filtered for `SYN` and `ACK` flag in Wireshark.
- `tcp.flags.syn == 1 && tcp.flags.ack == 0`
- Below shows continue communication between the source ip and the destination ip during the 3 way hand-shake.

![alt text](https://github.com/DJackson-BlueTeam/90-Day-Junior-Analyst-Sprint/blob/4cfd6c2542257be510b7f55ce11a68b445bbbc20/BLTO-Lab-Investigations/Easy/Security-Operations/FunGames/FunGamesImg/1..png)

Answer: 192.168.8.130

**2. What is the IP address of the victim?**

- In the same filter, we can see the victims ip address that is responding to the adversary.
 
![alt text](https://github.com/DJackson-BlueTeam/90-Day-Junior-Analyst-Sprint/blob/4cfd6c2542257be510b7f55ce11a68b445bbbc20/BLTO-Lab-Investigations/Easy/Security-Operations/FunGames/FunGamesImg/2..png)


Answer: 192.168.8.142

**3. Which attack was performed by the attacker?**

- I filter for `http.request.method == "GET"` to observe what request the adversary was making through the network.
- I stopped at packet 1319 and notice there was a sql user-agent.
- When an adversary performing sql queries for malicious reasons, its an indicator that the adversary is conducting SQL injections. 

![alt text](https://github.com/DJackson-BlueTeam/90-Day-Junior-Analyst-Sprint/blob/4cfd6c2542257be510b7f55ce11a68b445bbbc20/BLTO-Lab-Investigations/Easy/Security-Operations/FunGames/FunGamesImg/3..png)
 
 Answer: SQLi
 
Q4) It seems the attacker used a famous tool to perform the attack.

![alt text](https://github.com/DJackson-BlueTeam/90-Day-Junior-Analyst-Sprint/blob/4cfd6c2542257be510b7f55ce11a68b445bbbc20/BLTO-Lab-Investigations/Easy/Security-Operations/FunGames/FunGamesImg/4..png)

- SQLmap is a tool used for penetration purposes that automates process of detecting and exploiting SQL injections
Answer: SQLMAP

**5. In one of the packets, it is possible to view the victim's username and password.**

- I was combing through the OK responses since they were human readable text outputs in html format.
- I notice there was a sql query formate followed by some outputs in the next text/html output.

![alt text](https://github.com/DJackson-BlueTeam/90-Day-Junior-Analyst-Sprint/blob/4cfd6c2542257be510b7f55ce11a68b445bbbc20/BLTO-Lab-Investigations/Easy/Security-Operations/FunGames/FunGamesImg/5.1.png)

![alt text](https://github.com/DJackson-BlueTeam/90-Day-Junior-Analyst-Sprint/blob/4cfd6c2542257be510b7f55ce11a68b445bbbc20/BLTO-Lab-Investigations/Easy/Security-Operations/FunGames/FunGamesImg/5.2.png)

- I opened my note and copied the format in the first screenshot and the second screenshot and "carved-out" the results shown below.
 
![alt text](https://github.com/DJackson-BlueTeam/90-Day-Junior-Analyst-Sprint/blob/4cfd6c2542257be510b7f55ce11a68b445bbbc20/BLTO-Lab-Investigations/Easy/Security-Operations/FunGames/FunGamesImg/5.3.png)

- By doing so, I was able to pull the username and password of the victim. 

Answer: mjarovic, Ma77.J@r0v1c-2024

**6. Once the attacker obtained the victim's credentials he accessed the system via SSH. To gain root privileges, they transferred a file to the victim's machine. What is the name of the file?**

- I was still in the filter of `http && 192.168.8.142` to maintain the pattern of the adversary malicious activities. 
- I notice a text/html exploit file in the "GET" protocol

![alt text](https://github.com/DJackson-BlueTeam/90-Day-Junior-Analyst-Sprint/blob/4cfd6c2542257be510b7f55ce11a68b445bbbc20/BLTO-Lab-Investigations/Easy/Security-Operations/FunGames/FunGamesImg/5.4.png)

Answer: Exploit

Q7) What is the sha256 hash of the file above?
- Here I exported the file from Wireshark and save it in the ubuntu directory.

![alt text](https://github.com/DJackson-BlueTeam/90-Day-Junior-Analyst-Sprint/blob/4cfd6c2542257be510b7f55ce11a68b445bbbc20/BLTO-Lab-Investigations/Easy/Security-Operations/FunGames/FunGamesImg/6.1.png)

![alt text](https://github.com/DJackson-BlueTeam/90-Day-Junior-Analyst-Sprint/blob/4cfd6c2542257be510b7f55ce11a68b445bbbc20/BLTO-Lab-Investigations/Easy/Security-Operations/FunGames/FunGamesImg/6.2.png)

- Then, I was able to extract the malicious file SHA256 hash value.

![alt text](https://github.com/DJackson-BlueTeam/90-Day-Junior-Analyst-Sprint/blob/4cfd6c2542257be510b7f55ce11a68b445bbbc20/BLTO-Lab-Investigations/Easy/Security-Operations/FunGames/FunGamesImg/6.3.png)

Q8) With which CVE is this type of vulnerability identified?
- I had uploaded the hashvalue to VirusTotal and observing the data. 
- There was a CVE documented on the hashvalue through the community.

![alt text](https://github.com/DJackson-BlueTeam/90-Day-Junior-Analyst-Sprint/blob/4cfd6c2542257be510b7f55ce11a68b445bbbc20/BLTO-Lab-Investigations/Easy/Security-Operations/FunGames/FunGamesImg/6.4.png)

Answer: CVE-2024-1086

9. After obtaining root privileges, it seems that the attacker exfiltrated sensitive data without transferring any files. Provide the string related to this data.

- This took me a minute to get to.
- after conducting some research on data exfiltration without a file transfer, I was able to discover that data can be trasnferred through dns "tunneling" and ssh protocol. 
- I filtered for dns and notice a "Malformed Packet"
- This indicates that the packet was too large for wireshark to hold show the display of the packet showed as an error.

![alt text](https://github.com/DJackson-BlueTeam/90-Day-Junior-Analyst-Sprint/blob/4cfd6c2542257be510b7f55ce11a68b445bbbc20/BLTO-Lab-Investigations/Easy/Security-Operations/FunGames/FunGamesImg/6.5.png)

- I had copy the hex string, since thee question was only asking for the string of the data. 

![alt text](https://github.com/DJackson-BlueTeam/90-Day-Junior-Analyst-Sprint/blob/4cfd6c2542257be510b7f55ce11a68b445bbbc20/BLTO-Lab-Investigations/Easy/Security-Operations/FunGames/FunGamesImg/6.6.png)

Answer: j4672616e6b204d696c6c7320313233343536373839313233343536372065787020646174652030382f32382063767620313233200a

**10. It seems that the string has been encoded. What data did the attacker manage to obtain through exfiltration?**

- I used CyberChef to decode the string. 
- In return, the adversary was able to retrieve a card information for the victim.

![alt text](https://github.com/DJackson-BlueTeam/90-Day-Junior-Analyst-Sprint/blob/4cfd6c2542257be510b7f55ce11a68b445bbbc20/BLTO-Lab-Investigations/Easy/Security-Operations/FunGames/FunGamesImg/7.1.png)

Answer: Frank Mills 1234567891234567 exp date 08/28 cvv 123

11. Provide the Mitre ID of this technique—in regard to the previous question.
- Based on the previous question, I did perform some research in regards to the stealth activity of the adversary and discover a MITRE ATT&CK technique. 

![alt text](https://github.com/DJackson-BlueTeam/90-Day-Junior-Analyst-Sprint/blob/4cfd6c2542257be510b7f55ce11a68b445bbbc20/BLTO-Lab-Investigations/Easy/Security-Operations/FunGames/FunGamesImg/7.2.png)

Answer: T1071.004

![alt text](https://github.com/DJackson-BlueTeam/90-Day-Junior-Analyst-Sprint/blob/4cfd6c2542257be510b7f55ce11a68b445bbbc20/BLTO-Lab-Investigations/Easy/Security-Operations/FunGames/FunGamesImg/7.3.png)
