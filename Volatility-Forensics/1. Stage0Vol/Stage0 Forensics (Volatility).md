![alt text](https://github.com/DJackson-BlueTeam/90-Day-Junior-Analyst-Sprint/blob/d3924d6521ebad06aef91ef331c7d6031cd3b15f/Volatility-Forensics/1.%20Stage0Vol/VolImg/werfault.png)

This malware perform Process Injection Tactics to remain hidden within the system. By doing so, it was able to evade detection to continue its stealth. This malware had the capabilities to write a process within the process by execution API with low level coding. You can review the statics analysis I have conducted separately from this write-up to get a fully understanding of the Malware stealth methodologies ([90-Day-Junior-Analyst-Sprint/Malware-Analysis-Reports/2. Stage0/1. Static Analysis (Advance)/Malware.Stage0.md at main · DJackson-BlueTeam/90-Day-Junior-Analyst-Sprint](https://github.com/DJackson-BlueTeam/90-Day-Junior-Analyst-Sprint/blob/main/Malware-Analysis-Reports/2.%20Stage0/1.%20Static%20Analysis%20\(Advance\)/Malware.Stage0.md)) .  

This write up consist of conducting forensic search from a snapshot of my Flare-VM machine to identify the processes and unveil the true purpose of the malware. By conducting this search, it sure took some time from running a roughly 5 hours static analysis to an addition 3 to 4 hours a forensics since the malware was able to delete itself from the machine if it could not reach the domain to the 2nd payload. 

Lets Begin. 

For starter, I had to re-execute the binary to run it live in my VM in order to take a snapshot of the machine. Below shows the code I used to pull the virtual machine snapshot.

![[1. Forensic Snapshot.png]](https://github.com/DJackson-BlueTeam/90-Day-Junior-Analyst-Sprint/blob/d3924d6521ebad06aef91ef331c7d6031cd3b15f/Volatility-Forensics/1.%20Stage0Vol/VolImg/1.%20Forensic%20Snapshot.png)

I had trouble getting the snapshot to remain on my Host Operating System due to Windows Threat Protecting constantly removing the snapshot from my host. It keep considering as a threat so I had to turn off the Threat Protection System to:

1. Keep the snapshot on my Host Machine

2. Be able to transfer the snapshot to my ubuntu desktop "ubuntu-splunk".

The screenshot below shows the transferring of the `.elf` file to my ubuntu. 
Using command `scp` "Secure Copy Protocol", allowed me to take the entire snapshot over to my ubuntu. It did take roughly 11 minute for the process to be completed since it was a large file.

![[Stage0Vol/VolImg/2..png]](https://github.com/DJackson-BlueTeam/90-Day-Junior-Analyst-Sprint/blob/d3924d6521ebad06aef91ef331c7d6031cd3b15f/Volatility-Forensics/1.%20Stage0Vol/VolImg/2..png)

I had to input my password of my ubuntu to start the transfer.

![[Stage0Vol/VolImg/3..png]](https://github.com/DJackson-BlueTeam/90-Day-Junior-Analyst-Sprint/blob/d3924d6521ebad06aef91ef331c7d6031cd3b15f/Volatility-Forensics/1.%20Stage0Vol/VolImg/3..png)

![[3.3.png]](https://github.com/DJackson-BlueTeam/90-Day-Junior-Analyst-Sprint/blob/d3924d6521ebad06aef91ef331c7d6031cd3b15f/Volatility-Forensics/1.%20Stage0Vol/VolImg/3.3.png)

The screenshot below shows the transfer was successful.

![[Stage0Vol/VolImg/4..png]](https://github.com/DJackson-BlueTeam/90-Day-Junior-Analyst-Sprint/blob/d3924d6521ebad06aef91ef331c7d6031cd3b15f/Volatility-Forensics/1.%20Stage0Vol/VolImg/4..png)

I ended up changing the name to only stage0.elf for quick research when typing out the search cmdline. 

For the first forensics, I could not find any processes of the `werflt.exe` that started the process injection. This is do to the malware terminating itself since it could not reach the 2nd stage payload. 

After scanning through the processes, I pivoted to a powershell.exe that remained in the snapshot shown below. 

![[Stage0Vol/VolImg/5..png]](https://github.com/DJackson-BlueTeam/90-Day-Junior-Analyst-Sprint/blob/d3924d6521ebad06aef91ef331c7d6031cd3b15f/Volatility-Forensics/1.%20Stage0Vol/VolImg/5..png)

I then attempted to follow the pid to see if any pieces of evidence remained from the `werflt.exe` 
I didn't come across anything related to `werflt.exe`.
After going back and reviewing my static analysis, I observed that the `werflt.exe` had created and written a file name `Werfault.exe` in the machine.

This gave me a clue to look for the written file. By doing so I discovered the netscan of the `werfault.exe` with a closed connection.

![[Stage0Vol/VolImg/6..png]](https://github.com/DJackson-BlueTeam/90-Day-Junior-Analyst-Sprint/blob/d3924d6521ebad06aef91ef331c7d6031cd3b15f/Volatility-Forensics/1.%20Stage0Vol/VolImg/6..png)

It shows that Werfault.exe tried to communicate to an external ip through `port 8443`. However, the executable was unable to reach the remote ip and closed the connection. 

At this point, I though my forensic digging was over. Then I though about using strings on the snapshot to see if I can pull up werflt.exe since it did placed itself in the `\Users\Public\` directory.

![[Stage0Vol/VolImg/7..png]](https://github.com/DJackson-BlueTeam/90-Day-Junior-Analyst-Sprint/blob/d3924d6521ebad06aef91ef331c7d6031cd3b15f/Volatility-Forensics/1.%20Stage0Vol/VolImg/7..png)

This gave me insight that maybe there more evidence I can retreive from the werflt.exe. 

Scanning through the output, there was a ProcessStart initiated by the `werflt.exe`

![[Stage0Vol/VolImg/8..png]](https://github.com/DJackson-BlueTeam/90-Day-Junior-Analyst-Sprint/blob/d3924d6521ebad06aef91ef331c7d6031cd3b15f/Volatility-Forensics/1.%20Stage0Vol/VolImg/8..png)

The `werflt.exe` was able to masquerade itself from the human eye but was still running in the background executing it Process injections. 

![[Stage0Vol/VolImg/9..png]](https://github.com/DJackson-BlueTeam/90-Day-Junior-Analyst-Sprint/blob/d3924d6521ebad06aef91ef331c7d6031cd3b15f/Volatility-Forensics/1.%20Stage0Vol/VolImg/9..png)

So next I looked up `SysWOW64` that was created by the `werflt.exe` with in the machine and numerous powershell.exe was retrieved. I knew I was heading on the right path to discover more. 

![[Stage0Vol/VolImg/10..png]](https://github.com/DJackson-BlueTeam/90-Day-Junior-Analyst-Sprint/blob/d3924d6521ebad06aef91ef331c7d6031cd3b15f/Volatility-Forensics/1.%20Stage0Vol/VolImg/10..png)

As I was scrolling through the output I filtered for, I came across a domain that was had a path to a potential 2nd stage payload.

- Possibly, the WerFault.exe attempted to reach the domain `ownhttp://zz[.]8282[.]space/nw/ss/` with the following path to the second payload. `wmiprvse.exe`


![[Stage0Vol/VolImg/11..png]](https://github.com/DJackson-BlueTeam/90-Day-Junior-Analyst-Sprint/blob/d3924d6521ebad06aef91ef331c7d6031cd3b15f/Volatility-Forensics/1.%20Stage0Vol/VolImg/11..png)

Right after that, there was a schtasks executable that created and written another file called `VIP.exe`. 
`VIP.exe` must of allowed adversary to maintain persistence within the system. This was very valuable evidence. 

This information encourage me to dive deeper into the snapshot. Maybe I can find what type of malware was used? 

After carefully scanning through the information that all started with `werflt.exe` there was a socketTrojanDownloader  and the name of the payload is `097M/Emotet.ALZ!MTB`

![[Stage0Vol/VolImg/13..png]](https://github.com/DJackson-BlueTeam/90-Day-Junior-Analyst-Sprint/blob/d3924d6521ebad06aef91ef331c7d6031cd3b15f/Volatility-Forensics/1.%20Stage0Vol/VolImg/13..png)

**Emotet** 
is a banking trojan that have worm-like capabilities and have to ability to dropping other malware into the system. 

It was a fun journey deepdiving with Volatility for the first time and will be more to come! Thank you for reading!
