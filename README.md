# Cloud-Home-Lab
<h2>Project walk-through:</h2>

<p align="center">
My first step was to run a port scan using nmap against the IP address of the vulnerable machine : <br/>

![image alt](https://github.com/Samuel-James971/Pen-Testing/blob/main/1.png?raw=true)
<br />
<br />
In the command I used the -sV option for version enumeration and -p- to conduct a full port scan, the results of the scan showed that port 80 is being used for the HTTP service and port 22 is being used for SSH. :  <br/>
![image alt](https://github.com/Samuel-James971/Pen-Testing/blob/main/2.png?raw=true)
<br />
<br />
Next, I wanted to run an enumeration scan on the targets IP address to identify any hidden directories, I used the tool Drib for this which is preinstalled on Kali Linux. I was able to identify a few files and directories from the scan and decided to enter a few of them into a search engine. One of the directories lead me to an interesting discovery. The robots.txt file contained the name of another directory.: <br/>
![image alt](https://github.com/Samuel-James971/Pen-Testing/blob/main/3.png?raw=true)
<br />
<br />
However, this identified directory could not be found. There could be other directories starting with the same ~ character, however. In an attempt to find more directories I used a method called “fuzzing” which is the process of guessing directory names. I used an automated tool called FFUF for this process:   <br/>
![image alt](https://github.com/Samuel-James971/Pen-Testing/blob/main/4.png?raw=true)
<br />
<br />
After running the FFUF tool on the ~ sign I was able to find a directory named ~secret, after opening this file I found a hint that says the SSH private key is hidden somewhere in this directory. I ran another FFUF scan this time on ~secret. This scan brute forced the ~secret directory in order to search for hidden files. One file returned many responses, so I decided to open it on the browser. This is what the file contained:  <br/>
![image alt](https://github.com/Samuel-James971/Pen-Testing/blob/main/5.png?raw=true)
<br />
<br />
I put the contents of the file into a cipher identifier and discovered that it was a base 58 cipher. The same website had a cipher decoder, so I put the encrypted message into that. After decoding the message, I found that the encrypted message contained the SSH key. I saved the SSH key as a file named “key” on my machine using the cat >> key command.:  <br/>
![image alt](https://github.com/Samuel-James971/Pen-Testing/blob/main/7.png?raw=true)
<br />
<br />

To begin with exploitation my next step was to attempt to crack the passphrase of the key file. One of the best tools for cracking passwords is John the ripper, which is also configured in Kali Linux by default. For the SSH key I had to first of extract the hash from the key before I could crack it. This can be done by using ssh2john which is a utility within the john the ripper tool. I extracted the hash by using the command ssh2john key > hash. 
Now I was able to use john the ripper to crack the hash file and find the password of the SSH key. After running john, the ripper using the default wordlist “fasttrack.txt” the results showed that the password was “P@55w0rd!”:  <br/>
![image alt](https://github.com/Samuel-James971/Pen-Testing/blob/main/8.png?raw=true)
<br />
<br />
Now that I has the password, I was able to log in as the user “icex64@LupinOne”. My immediant next step was to check the sudo permissions of the current user “icex64”, I was able to find a python file. I attempted to view the contents of the python file “heist.py” that seemed to be owned by another user named “arsene”, it however only displayed a text to be displayed upon execution. I continued to enumerate the target machine in an attempt to find a vulnerability. 
I used the find command to identify files with full permissions on the target machine. I found another file named “webbrowser.py” which after running the ls -l command I found that the root user owns it. the file permissions are set to 777 which means the current user has read, write, and executed permissions for the python file. This means that the file could be edited and have code entered into it. it also means that it is susceptible to python library hijacking.:  <br/>
![image alt](https://github.com/Samuel-James971/Pen-Testing/blob/main/9.png?raw=true)
<br />
<br />
 To begin python library hijacking I utilised the nano command to edit the python script to call /bin/bash code into it. here is the code I entered into the python file os.system (“/bin/bash”)
After this I ran the sudo command along with the coordinates specified in the permissions check on “ice64x” to switch the user to “arsene”. 
:  <br/>
![image alt](https://github.com/Samuel-James971/Pen-Testing/blob/main/10.png?raw=true)
<br />
<br />
I then began to identify the privileges of the user “arsene”. After analysing the outputs I spotted a new vulnerability that could allow me to escalate user privilege. The vulnerability I discovered was PIP Privilege escalation. Python package manager (PIP) is a widely used tool for managing and installing Python packages, however it has been found to have privilege escalation vulnerabilities that can be exploited by attackers to gain access to a user’s system . 
:  <br/>
![image alt](https://github.com/Samuel-James971/Pen-Testing/blob/main/11.png?raw=true)
<br />
<br />
I used these three commands to escalate user privilege, all had to do was then simply CD into root and open the root.txt file and the vulnerable VM had been completed.
![image alt](https://github.com/Samuel-James971/Pen-Testing/blob/main/12.png?raw=true)
<br />
As you can see after opening the root.txt I had completed the vulnerable VM.
![image alt](https://github.com/Samuel-James971/Pen-Testing/blob/main/13.png?raw=true)





<!--
 ```diff
- text in red
+ text in green
! text in orange
# text in gray
@@ text in purple (and bold)@@
```
--!>
