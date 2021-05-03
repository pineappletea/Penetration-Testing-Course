# Week 5: Walkthrough


## z) Read three walkthrough articles and watch three walkthrough videos, take short notes.

Wanted to include some Windows machines, haven't seen much on them yet.

### Articles

#### [HTB: Tabby by 0xdf](https://0xdf.gitlab.io/2020/11/07/htb-tabby.html)

- nmap shows apache http(80) and tomcat(8080)
- website is using php
- uses gobuster with -x php
- installs tomcat to figure out the location of the tomcat-users.xml -file
- looks up tomcat manager credentials from the file
- msfvenom to generate a payload
- zip2john, john
- LXC exploit

#### [HTB: Sauna(Windows) by 0xdf](https://0xdf.gitlab.io/2020/07/18/htb-sauna.html)

- so many open ports on windows server
- gobuster for directory brute force
- smbmap
- tried zone transfer with dig
- kerbrute for brute forcing user names
- GetNPUsers.py with the found usernames to look for vulnerable ones
- hashcat to crack password from hash
- registry has a winlogon page with default username and password
- Evil-WinRM to create remote shell
- secretsdump.py for a DCSync attack

#### [HTB: Blocky by 0xdf](https://0xdf.gitlab.io/2020/06/30/htb-blocky.html)

- open ports FTP (21), SSH (22) and HTTP (80) from nmap
- gobuster to brute force directories
- wpscan to scan for vulnerabilities in wordpress
- opens .jar files with jd-gui
- found the password in a Java class as sql password
- logged in as user notch found with WPScan and the sql password, user was in sudo group

### Videos

#### [HTB: Optimum(Windows) by HackerSploit](https://www.youtube.com/watch?v=gDMGU3rdwfw&ab_channel=HackerSploit)

- single open service for HttpFileServer
- opens up the webpage in browser, picks up on file server program rejetto HFS version 2.3
- runs searchsploit on HFS
- googles "hfs 2.3 exploit"
- uses metasploit package, gets meterpreter running
- sysinfo
- shell
- tried getsystem, getprivs for privilege escalation
- migrates meterpreter to a 64 bit process for better stability
- uses local_exploit_suggester 
- searches "windows server 2012 r2 privilege escalation"
- uses the metasploit package he found, which works after fixing LHOST

#### [HTB: Doctor by IstantechGroup](https://www.youtube.com/watch?v=fkutK6kcDKY&ab_channel=IstantechGroup)

- ubuntu with ssh, apache httpd 2.4.41
- looks up /etc/hosts to find hostname
- creates a new user
- uses a form on the site to execute a script
- netcat to listen, python script to open bash
- reads the backup log of apache server
- finds a post request logged with a password in plaintext
- splunkwhisperer for privilege escalation

#### [HTB: Shocker by IppSec] (https://www.youtube.com/watch?v=IBlTdguhgfY&ab_channel=IppSec)

- could use the specific version flag of the ubuntu SSH to exploit, but choosing to use http
- apache version information can be used to identify the ubuntu version
- gobuster to find directories
- using burp suite to view the http requests
- shellshock
- reverse shell cheat sheet
- linux privesc

## a) Find and try 5 new tools from the walkthroughs.

### gobuster

Trying this against the Armageddon machine on HTB.

``` sudo apt install gobuster ```

``` gobuster dir --help ```

``` gobuster dir -u http://10.10.10.233 -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -o gobuster ```

Turns out my search was a bit more thorough than I had hoped and I interrupt it at 11712/220561 searches.

Still, it very quickly found a few results.

![gobuster](/week-5/gobuster.png)

### netcat

Just going to try some real basics here.

``` nc -l -p 1234 ```

and in another terminal ``` nc localhost 1234 ```

and throw in some messages to this chat session.

![netcat localhost](/week-5/netcat.png)

### wpscan

### kerbrute


## b) Recon and analyze 5 HackTheBox machines.

### Delivery 

### ScriptKiddie

### Spectra

### Love

### Ready

## c) Name 1-3 walkthroughs in which services like the ones in section b) are exploited. 

## Sources

https://terokarvinen.com/2021/hakkerointi-kurssi-tunkeutumistestaus-ict4tn027-3005/

https://0xdf.gitlab.io/2020/11/07/htb-tabby.html

https://0xdf.gitlab.io/2020/07/18/htb-sauna.html

https://0xdf.gitlab.io/2020/06/30/htb-blocky.html

https://www.youtube.com/watch?v=gDMGU3rdwfw&ab_channel=HackerSploit

https://www.youtube.com/watch?v=fkutK6kcDKY&ab_channel=IstantechGroup

https://www.youtube.com/watch?v=IBlTdguhgfY&ab_channel=IppSec

https://www.youtube.com/watch?v=aKShnpOXqn0&ab_channel=TomScott

https://en.wikipedia.org/wiki/Kerberos_(protocol)