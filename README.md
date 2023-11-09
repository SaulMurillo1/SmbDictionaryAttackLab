<h1>SMB Dictionary Attack</h1>


<h2>Description</h2>
This project consists of using known wordlists to conduct a dictionary attack on an SMB server. 
<br />
<br />
- A wordlist file is a list on common or bad passwords which can be used for brute force or dictionary attacks. A famous wordlist is the rockyou.txt wordlist, which contains over 14 million passwords that were leaked in past data breaches. I will provide examples of using different Kali Linux tools to conduct dictionary attacks. 
<br />
<br />

Disclaimer: The Kali Linux user machine and target machine used in this project was provided to me by INE for educational purposes. The misuse of the information in this project lab can result in criminal charges brought against the individual/individuals in question.
<br />


<h2>Languages and Utilities Used</h2>

- <b>nmap</b>
- <b>msfconsole</b>
- <b>hydra</b>
- <b>smbmap</b>
- <b>smbclient</b>


<h2>Environments Used </h2>

- <b>Kali Linux</b>

<h2>Project walk-through:</h2>

<p align="left">
Ping target machine to verify that it is active: <br/>
<br/>
- INE has provided me with Kali Linux GUI & target machine (target machine is one ip address above from mine).  I will run and ip a command to verify my own ip address. 
<br/>
<br/>
Commands: ip a
<br/>
ping 192.86.121.3
<br/>
<br/>
<img src="https://i.imgur.com/lj1us1d.png" height="80%" width="80%" alt="SMB Nmap Scripting" class="center"/>
<br />
<br />
<br />
<br />
<br />
<br />
<br />
Run nmap scan to look for open ports: <br/>
<br/>
- We can see that port 445, also known as SMB, is open. 
<br/>
<br/>
Command: nmap 192.86.121.3
<br/>
<br/>
<img src="https://i.imgur.com/E5yXhP6.png" height="80%" width="80%" alt="SMB Nmap Scripting" class="center"/>
<br />
<br />
<br />
<br />
<br />
<br />
<br />
Use the msfconsole tool (metasploit) to run a login brute force attack on the SMB server user Jane: <br/>
<br/>
- In this scenario, we already know that there is a user Jane so we will try to use the metasploit smb_login module with a password wordlist to try and find Jane's password. 
<br/>
- We can see that by using a password wordlist, we were able to find a match for user Jane's password, which is abc123. This is one reason why it is bad practice to use small straightforward passwords.
<br/>
<br/>
Commands: msfconsole
<br/>
use auxiliary/scanner/smb/smb_login
<br/>
options
<br/>
set rhosts 192.86.121.3
<br/>
set pass_file /usr/share/wordlists/metasploit/unix_passwords.txt
<br/>
set smbuser jane
<br/>
exploit
<br/>
<br/>
<img src="https://i.imgur.com/dOAXa38.png" height="80%" width="80%" alt="SMB Nmap Scripting" class="center"/>
<br />
<img src="https://i.imgur.com/MSLfZtm.png" height="80%" width="80%" alt="SMB Nmap Scripting" class="center"/>
<br />
<img src="https://i.imgur.com/Oj2hjIe.png" height="80%" width="80%" alt="SMB Nmap Scripting" class="center"/>
<br />
<br />
<br />
<br />
<br />
<br />
<br />
Use the hydra tool to run a brute force attack and try to enumerate the user admin's password: <br/>
<br/>
- Hydra is an open source, password brute-forcing tool designed around flexibility and high performance in online brute-force attacks. 
<br/>
- As we can see in the command below, I used the hydra tool to brute force the password for the user admin. First, I used gzip to unzip the wordlist I planned on using for the attack.  Then, I used the popular rockyou.txt wordlist to conduct the brute force attack.  I was able to get a valid password for user admin, which was password1. 
<br/>
<br/>
Commands: gzip -d /usr/share/wordlists/rockyou.txt.gz
<br/>
hydra -l admin -P /usr/share/wordlists/rockyou.txt 192.86.121.3 smb
<br/>
<br/>
<img src="https://i.imgur.com/fVUOy7c.png" height="80%" width="80%" alt="SMB Nmap Scripting" class="center"/>
<br />
<br />
<br />
<br />
<br />
<br />
<br />
Now that user admin's password is known, use smbmap tool to enumerate user shares & permissions: <br/>
<br/>
- We can see that the nancy share is read only while the admin and shawn shares are read, write. 
<br/>
<br/>
Command: smbmap -H 192.86.121.3 -u admin -p password1
<br/>
<br/>
<img src="https://i.imgur.com/ijAWuWq.png" height="80%" width="80%" alt="SMB Nmap Scripting" class="center"/>
<br />
<br />
<br />
<br />
<br />
<br />
<br />




</p>
