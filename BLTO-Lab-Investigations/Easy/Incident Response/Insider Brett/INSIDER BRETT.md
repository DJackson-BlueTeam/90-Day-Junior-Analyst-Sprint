![alt text](https://github.com/DJackson-BlueTeam/90-Day-Junior-Analyst-Sprint/blob/df44d46e9b466c30a2c24630096475e77e58431f/BLTO-Lab-Investigations/Easy/Incident%20Response/Insider%20Brett/Insider%20Brett%20Img/Img.png)

# Scenario

### Thanks for offering to help, Defender, we appreciate it!  
  
### As we discussed earlier, Initech’s CISO, Milton Waddams, CISSP, CISM, was the victim of a blackmail attempt requesting payment of 50 BTC. We have identified that one of our IT contractors (Brett Hart) was the likely culprit, but we just don’t know how he managed it as he was only a standard user.  
  
### We managed to collect some artifacts from a folder called “Hacking” on his Kali Virtual Machine and we’ve put these into the Investigation folder for you to take a look at.  
  
### We’ve been assured by Milton that the environment is employing all the best practices and is “unhackable”. Please can you investigate and let us know your feedback?

(This Investigation was a practical analyzing the documents that was pulled from the victims machine. Only screenshots and Answer will be posted since no thorough investigation for this challenge wasn't necessary.)

#### Investigation

**1. What time was the nmap scan initiated?**

![alt text](https://github.com/DJackson-BlueTeam/90-Day-Junior-Analyst-Sprint/blob/df44d46e9b466c30a2c24630096475e77e58431f/BLTO-Lab-Investigations/Easy/Incident%20Response/Insider%20Brett/Insider%20Brett%20Img/1..png)

Answer: Thu Apr 11 07:10:24 2024

**2. What is the full nmap command that ran?**

![alt text](https://github.com/DJackson-BlueTeam/90-Day-Junior-Analyst-Sprint/blob/df44d46e9b466c30a2c24630096475e77e58431f/BLTO-Lab-Investigations/Easy/Incident%20Response/Insider%20Brett/Insider%20Brett%20Img/2..png)

Answer: nmap -sS --script *smb*,ldap* -sV --version-all -T5 -oN scan.txt 192.168.25.0/24

**3. What is the MAC address of the first responding IP?**

![alt text](https://github.com/DJackson-BlueTeam/90-Day-Junior-Analyst-Sprint/blob/df44d46e9b466c30a2c24630096475e77e58431f/BLTO-Lab-Investigations/Easy/Incident%20Response/Insider%20Brett/Insider%20Brett%20Img/3..png)

Answer: 00:50:56:F4:3C:76

**4. What is the domain as determined by the LDAP scripts?**

![alt text](https://github.com/DJackson-BlueTeam/90-Day-Junior-Analyst-Sprint/blob/f549b63ee0b5bd74256e9c6c001756691d7cc08f/BLTO-Lab-Investigations/Easy/Incident%20Response/Insider%20Brett/Insider%20Brett%20Img/4..png)

Answer: initech.local

**5. What is the dnsHostName?**

![alt text](https://github.com/DJackson-BlueTeam/90-Day-Junior-Analyst-Sprint/blob/f549b63ee0b5bd74256e9c6c001756691d7cc08f/BLTO-Lab-Investigations/Easy/Incident%20Response/Insider%20Brett/Insider%20Brett%20Img/5..png)

Answer: voenmeh-d0f286a.initech.local

**6. Which enabled accounts had passwords guessed by the SMB brute force script?**

![alt text](https://github.com/DJackson-BlueTeam/90-Day-Junior-Analyst-Sprint/blob/f549b63ee0b5bd74256e9c6c001756691d7cc08f/BLTO-Lab-Investigations/Easy/Incident%20Response/Insider%20Brett/Insider%20Brett%20Img/6..png)

Answer: IT, milton-waddams

**7. What is the IP of the connecting machine in the active SMB session, and when did they log in?**

![alt text](https://github.com/DJackson-BlueTeam/90-Day-Junior-Analyst-Sprint/blob/f549b63ee0b5bd74256e9c6c001756691d7cc08f/BLTO-Lab-Investigations/Easy/Incident%20Response/Insider%20Brett/Insider%20Brett%20Img/7..png)

Answer: 192.168.25.130, 2024-04-11T10:47:17

**8. What exploit did the attacker use in msfconsole?**

![alt text](https://github.com/DJackson-BlueTeam/90-Day-Junior-Analyst-Sprint/blob/f549b63ee0b5bd74256e9c6c001756691d7cc08f/BLTO-Lab-Investigations/Easy/Incident%20Response/Insider%20Brett/Insider%20Brett%20Img/8..png)

Answer: exploit/windows/smb/psexec

**9. What was the name of the uploaded payload?**

![alt text](https://github.com/DJackson-BlueTeam/90-Day-Junior-Analyst-Sprint/blob/f549b63ee0b5bd74256e9c6c001756691d7cc08f/BLTO-Lab-Investigations/Easy/Incident%20Response/Insider%20Brett/Insider%20Brett%20Img/9..png)

Answer: VdMXyqeN.exe

**10. What is the title of the open Window in the first grabbed screenshot?**

![alt text](https://github.com/DJackson-BlueTeam/90-Day-Junior-Analyst-Sprint/blob/f549b63ee0b5bd74256e9c6c001756691d7cc08f/BLTO-Lab-Investigations/Easy/Incident%20Response/Insider%20Brett/Insider%20Brett%20Img/10..png)

Answer: Active Directory Users and Computers

**11. What file did the attacker upload and where?**

![alt text](https://github.com/DJackson-BlueTeam/90-Day-Junior-Analyst-Sprint/blob/f549b63ee0b5bd74256e9c6c001756691d7cc08f/BLTO-Lab-Investigations/Easy/Incident%20Response/Insider%20Brett/Insider%20Brett%20Img/11.png)

Answer: WARNING.txt, c:\documents and settings\milton-waddams\desktop\

**12. What is the BTC address in the second grabbed screenshot?**

![alt text](https://github.com/DJackson-BlueTeam/90-Day-Junior-Analyst-Sprint/blob/f549b63ee0b5bd74256e9c6c001756691d7cc08f/BLTO-Lab-Investigations/Easy/Incident%20Response/Insider%20Brett/Insider%20Brett%20Img/12..png)

Answer: mpMKeox8YRcvwEVMuigWmGnJpMJVFhL683

**13. What tool is CRACKED.txt the output of?**
Answer: John The Ripper

**14. What is the password for Brett?**

![alt text](https://github.com/DJackson-BlueTeam/90-Day-Junior-Analyst-Sprint/blob/f549b63ee0b5bd74256e9c6c001756691d7cc08f/BLTO-Lab-Investigations/Easy/Incident%20Response/Insider%20Brett/Insider%20Brett%20Img/14..png)

Answer: VERYSECURE!
