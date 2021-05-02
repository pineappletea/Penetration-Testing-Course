# Week 5: Walkthrough


## z) Read three walkthrough articles and watch three walkthrough videos, take short notes.

Wanted to include some Windows machines, haven't seen any exploits done on them yet.

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


#### [HTB: Sauna by 0xdf](https://0xdf.gitlab.io/2020/07/18/htb-sauna.html)

#### [HTB: Devel by 0xdf](https://0xdf.gitlab.io/2019/03/05/htb-devel.html)

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

## b) Recon and analyze 5 HackTheBox machines.

## c) Name 1-3 walkthroughs in which services like the ones in section b) are exploited. 

## Sources

https://terokarvinen.com/2021/hakkerointi-kurssi-tunkeutumistestaus-ict4tn027-3005/

https://0xdf.gitlab.io/2020/11/07/htb-tabby.html

https://0xdf.gitlab.io/2020/07/18/htb-sauna.html

https://0xdf.gitlab.io/2019/03/05/htb-devel.html

https://www.youtube.com/watch?v=gDMGU3rdwfw&ab_channel=HackerSploit

https://www.youtube.com/watch?v=fkutK6kcDKY&ab_channel=IstantechGroup

https://www.youtube.com/watch?v=IBlTdguhgfY&ab_channel=IppSec

https://www.youtube.com/watch?v=aKShnpOXqn0&ab_channel=TomScott
