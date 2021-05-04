# Week 4: Active recon and nmap


## z) Read articles and watch videos, take short notes. 

### [Santos et al: The Art of Hacking (Video Collection): [..] 4.3 Surveying Essential Tools for Active Reconnaissance](https://learning.oreilly.com/videos/the-art-of/9780135767849/9780135767849-SPTT_04_00)

- in active reconnaissance you send packets to target, may be detected
- port scanning, web service review, vulnerability scanning 
- progressively "louder" - more detectable tools

Tools
- nmap port scanner
- masscan the fastest port scanner for large numbers of ports
- udpprotoscanner for udp
- EyeWitness to gather information on all the web pages to figure out which to focus on

- in nmap -A flag does OS and version detection
- for internet -Pn is recommended, to skip host discovery and thus see ports blocked by firewall

Vulnerability scanners
- OpenVAS for network vulnerabilities
- Nikto
- burp
- nmap has a variety of vulnerability scanning scripts

### [Lyon 2009: Nmap Network Scanning: Chapter 1. Getting Started with Nmap ](https://nmap.org/book/nmap-overview-and-demos.html)
- -sL flag lists IP addresses in target range and does reverse-DNS, for stealth
- filtered means blocked by firewall, deny-by-default is good practice
- gathered information on ports, services and versions leads to finding the right exploit
- nmap was used to scan a companies 100k hosts daily

### [Lyon 2009: Nmap Network Scanning: Chapter 15. Nmap Reference Guide](https://nmap.org/book/man.html)

#### Port scanning basics

- open: accepting connections
- closed: port open but no application is listenijng
- filtered: port cant be reached, usually blocked by firewall

#### Port scanning techniques
- performing most scans requires root access because of sending and receiving raw packets
- default scan is TCP SYN, which sends packets as if you were going to open a connection
- TCP connect scan is done when theres no root privileges, uses operating systems ``` connect ```
- UDP scan sends mostly empty UDP packets to common ports

### man nmap
- uses raw IP packets to determine hosts, services, OS, versions, firewall etc
- -t4 for faster execution
- you can input list of hosts from a file
- tools for firewall evasion and spoofing
- can use hostname instead of IP address and flag --resolve-all to scan all the results of the DNS lookup
- you could target the whole internet with target /0
- use host discovery to quickly find machines from a large IP range
- this is probably the most detailed and well written man page ive seen

## a) How does nmap work? Run the following tests on your own machine against a practice target, analyze traffic with a network sniffer(wireshark).

### TCP connect scan -sT

![nmap-st](/week-4/nmap-st.png)

Looking at wireshark theres 2 packets sent and 2 received. The first package is SYN to establish connection. Second is SYN ACK response saying there is a service. Then nmap sends ACK to complete the connection, and straight after RST, ACK to close the connection.

### TCP SYN "used to be stealth" scan, -sS

Compared to connect scan, there is no ACK package sent to the server. Instead, connection is closed with RST package after receiving SYN, ACK response.

### ping sweep -sn

I ran this into a virtual box host-only network with kali and metasploitable virtual machines.
![nmap-st](/week-4/nmap-sn.png)

In the results 192.168.56.100 is the DHCP server, .101 is metasploitable, .102 is Kali and .1 is the adapter.

The packages sent are just "Who has?" requests sent to all the IP-addresses in range. Most of the time used for the scan seems to be just waiting for responses.

### don't ping -Pn

![nmap-pn](/week-4/nmap-pn.png)

On wireshark these packages look identical to TCP SYN scan.

![wireshark](/week-4/shark-pn.png)

### version detection -sV

![nmap version detection](/week-4/nmap-sv.png)

This took quite a while, roughly 2300 packets were exchanged. 80 seconds to run them is still very long, Im wondering if doing it on localhost in a machine without any network adapters is a problem for this. 

The packages sent are trying to provoke a certain type of response, which would reveal the service and its version.

Retrying this against the metasploitable 2 machine gave more sensible results. 

![nmap version detection](/week-4/nmap-sv2.png)


## b) Nmap operations. Try the examples, explain results and purpose.

### -p1-100 

Only scans the ports in the defined range.

### --top-ports 5 

Scans the 5 most common ports, which seem to be 21, 22, 23, 80 and 443. 

![nmap top ports](/week-4/nmap-top-ports.png)

### ip-address choice with netmask (10.10.10.0/24) and range (10.10.10.100-130)

Netmasked 10.10.10.0/24 means range 10.10.10.0-255, 10.10.10.100-130 is 30 ip-addresses in the range.

### output files -oA foo

Writes results to file.

![nmap write to file](/week-4/nmap-oa.png)


### OS fingerprinting, version detection, scripts, traceroute -A

-A does a ton of things. Version detection Ive mentioned above. OS fingerprinting is nmap gathering up packages it receives from the target and comparing them to a database of OS fingerprints to figure out which one it is. The scripts gather additional information on the services that are running, for example tries to log in to the ftp server as an anonymous user.

![nmap -A](/week-4/nmap-oa.png)

### runtime interactions

Pressing any key brings up a status report with things like progress % and expected time to complete the scan. Lowercase v increases verbosity, upper case V decreases it.

### compare running nmap with and without sudo

![nmap -A](/week-4/nmap-sudo.png)

With sudo a SYN scan is used, without its connect scan. 

### compare -sV to -A in terms of duration

![nmap -sV vs -A](/week-4/nmap-sudo.png)

A is a little slower which makes sense, since it includes version scanning and also does additional stuff.

## c) Run nmap -sV on a local web server.

![nmap -sV vs -A](/week-4/nmap-webserv.png)

![nmap -sV vs -A](/week-4/ws-webserv.png)

There is a fair amount of http-requests that get replied with 400 or 404, i imagine detecting this type of scanning would not be too difficult.

## d) UDP scanning

### What are the most common and interesting services you can find with UDP-scanning

Most common ones are DNS, SNMP, and DHCP. 

### Why is UDP scanning difficult and unreliable?

There is no guarantee that you'll get a response from an open port, its rare to get a response. The result state of open|filtered means there was no response even after waiting, and nmap can't tell if the package was filtered by the firewall or simply wasn't responded to by the system. States "closed" and "filtered" are the results of error code responses. So in a way the logic is reverse to how TCP works: the responses you get are error codes and mean the ports are closed, not receiving an error means "there may be something here".

### Why should you use the --reason flag with UDP scanning?

The reason flag adds a column to explain why nmap classified the port the way it did. This gives you additional information to work out whats happening.

## e) Open connection to the HackTheBox network and show that the connection is working.

To open the connection Ive downloaded the .ovpn-file to connect to the VPN. I navigate to my downloads folder with ``` cd ~/Downloads``` I then open the connection with ```sudo openvpn lab_pineappletea.ovpn```. On the hackthebox website, i start the machine Armageddon and take down its IP address 10.10.10.233. To make sure the connection is right I run ``` ipcalc 10.10.10.233 ``` and get the private internet results I was looking for. 

![ipcalc](/week-4/ipcalc.png)

I open up my browser to http://10.10.10.233/ and see the Welcome to Armageddon page.

## f) Perform active recon on one of the HackTheBox machines. Analyze the results.

Since the machine is still an active one on HackTheBox, this section is password protected.

[Link](https://www.protectedtext.com/ollinhtb)


## Sources

https://terokarvinen.com/2021/hakkerointi-kurssi-tunkeutumistestaus-ict4tn027-3005/

https://learning.oreilly.com/videos/the-art-of/9780135767849/9780135767849-SPTT_04_00

https://nmap.org/book/nmap-overview-and-demos.html

https://nmap.org/book/man.html

https://nmap.org/book/scan-methods-connect-scan.html

https://nmap.org/book/synscan.html

https://condor.depaul.edu/glancast/443class/docs/vbox_host-only_setup.html

https://nmap.org/book/man-version-detection.html

https://nmap.org/book/man-runtime-interaction.html

https://nmap.org/book/scan-methods-udp-scan.html



