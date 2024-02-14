<h1>TryHackMe Simple Capture the Flag</h1>

<h2>Description</h2>
This is a relatively quick capture the flag room that provides opportunities for several core pen-test techniques including port scanning, enumeration, privilege escalation and more. There are no real instructions for the room so there are many ways to get to the same destination, but there are 10 questions to answer which essentially guide how the path. <a href="https://tryhackme.com/room/easyctf">room link</a>
<br />

<h2>Utilities Used</h2>

- <b>Tryhackme Attack Box</b> 
- <b>nmap</b>
- <b>gobuster</b>

<h2>Environments Used </h2>

<h2>Program walk-through:</h2>


<b>Port scanning</b> <br/>
<img src="https://i.imgur.com/UG8VLQa.jpeg" height="80%" width="80%" alt="install curl"/></br>
The first two questions are port related so we will start by running an nmap scan. The command I ran was <b> nmap -sC -Sv -Pn -T4 -p- "ip address" </b>.</br>
<b>-sC</b> scans for scripts </br>
<b>-sV</b> scans for different versions</br>
<b>-p-</b> scans all ports </br>
<b>-T4</b> speeds up the scan</br>
<b>-Pn</b> avoids host discovery and only scans for ports (to avoid detection)</br></br>
This scan answers our first two questions.</br>
<b>How many services are running under port 1000</b></br>
2 (21 and 80)</br>
<b>What is running on the higher port?</b></br>
ssh
<br /><br>
<b>Web enumeration</b> <br/><br/>
<b>What's the CVE you're using against the application?</b></br>
To solve this question, we're going to have to enumerate the web application. We can do this by running a gobuster command that will search through the common directories. There are a lot of wordlists to choose from but I used <a href="https://github.com/digination/dirbuster-ng/blob/master/wordlists/common.txt">this one</a>. </br>
<img src="https://i.imgur.com/IZ9FCG4.jpeg" height="80%" width="80%" alt="gobuster"/> <br/>
<br />
After looking through the directories, the page /simple popped up as an interesting one. At the bottom of the page there is a version listed.</br>
<img src="https://i.imgur.com/nW5ogQS.jpeg" height="30%" width="30%" alt="version number"/> <br/>
There are multiple ways to go about searching for the exploit, so I'll try a couple. First I went to exploit.db and searched for "cms made simple" which brought up a lot of options.</br>
<img src="https://i.imgur.com/PO1U9v9.jpeg" height="80%" width="80%" alt="vulnerabilities"/> <br/>
Searching with the version number brought up no results. There are some very interesting options there, but I'm not entirely sure which one to choose. Let's try searchsploit.</br>
<img src="https://i.imgur.com/t9a2064.jpeg" height="80%" width="80%" alt="searchsploit"/> <br/>
I was able to include the version number in this search which helped narrow the search to one, making my decision pretty easy.</br>
<img src="https://i.imgur.com/2F8U60Z.jpeg" height="80%" width="80%" alt="exploit 2019"/> <br/>
Revisiting the question:</br>
<b>What's the CVE you're using against the application?</b><br/>
CVE-2019-9053</br>
<b>To what kind of vulnerability is the application vulnerable?</b></br>
SQLI </br></br>
<b>What's the password?</b></br>
Ok now that we know what exploit to use, let's download it and put it to work. <br/>
<img src="https://i.imgur.com/gXgDSqC.jpeg" height="80%" width="80%" alt="python"/> <br/><br/>
To run this exploit, we will execute the command <b>python2 46635.py -u "ip address" -c -w /usr/share/wordlists/rockyou.txt</b>. <br/>
We use python to run the exploit, using -c for "crack" and -w to specify the wordlist. Like earlier, there are a lot of wordlists to choose from, but rockyou is usually a good place to start. <br/>
<img src="https://i.imgur.com/tfJ28sx.jpeg" height="80%" width="80%" alt="rockyou"/> <br/><br/>
Voila! After letting it do it's thing for a while, we came up with the password. (Note: I got excited and took the screenshot before the password came out and didn't realize until now)<br/><br>
<b>What's the password?</b><br/>
secret <br/><br/>
<b>Where can you login with the details obtained?</b><br/>
Well, according to our nmap scan that we ran at the beginning, we have ssh open at port 2222. That would be a good place to start.<br/>

Using the credentials that we gained from the password crack, we successfully ssh'd into Mitch's server. From here with a quick search using "ls" we can find the flag and view it's contents.<br/><br/>
<b>Where can you login with the details obtained?</b><br/>
SSH<br/>
<b>What's the user flag?</b><br/>
G00d j0b, keep up!<br/>
<b>Is there any other user in the home directory? What's it's name?</b><br/>
This one is quick and easy. Let's navigate to the home directory and then use ls.<br/>
<img src="https://i.imgur.com/CzJEIf4.jpeg" height="30%" width="30%" alt="users"/> <br/><br/>
We see two two users. "mitch" and our answer <b>"sunbath"</b>.<br/><br/>
<b>Privilege Escalation</b> <br/><br/>
<b>What can you leverage to spawn a privileged shell?</b><br/>
Using the command <b>sudo -l</b> we can list out the allowed commands for the user. Doing so revels one result: usr/bin/vim<br/>
<b>VIM</b> is our answer.<br/><br/>
<b>What's the root flag?</b><br/>
Last question! Ok,we know we can use vim. Let's create a shell to find the flag. To do so, I used <a href="https://gtfobins.github.io/">GTFOBins</a>. From here I typed vim into the search bar and used the sudo option, allowing us to run commands as a superuser. <br/>
<img src="https://i.imgur.com/pD4D47l.jpeg" height="80%" width="80%" alt="vim bash"/> <br/><br/>
Using this command gives us a root shell. After confirming I was root with "whoami", I used ls to discover the files. There's only one file here, our flag!<br/>
<img src="https://i.imgur.com/iBJGhHs.jpeg" height="50%" width="50%" alt="flag"/> <br/><br/>
<b>What's the root flag?</b><br/>
W3ll d0n3. You made it!<br/><br/>
That was fun! We got to use several different techniques here. Looking forward to another CTF.





