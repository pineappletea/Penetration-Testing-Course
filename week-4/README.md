# Week 4: Active recon and nmap


## z) Read articles and watch videos, take notes

### [Santos et al: The Art of Hacking (Video Collection): [..] 4.3 Surveying Essential Tools for Active Reconnaissance](https://learning.oreilly.com/videos/the-art-of/9780135767849/9780135767849-SPTT_04_00)

### [Lyon 2009: Nmap Network Scanningg: Chapter 1. Getting Started with Nmap ](https://nmap.org/book/nmap-overview-and-demos.html)

### [Lyon 2009: Nmap Network Scanning: Chapter 15. Nmap Reference Guide](https://nmap.org/book/man.html)
#### Port scanning basics
#### Port scanning techniques

### man nmap

## a) How does nmap work? Run the following tests on your own machine against a practice target, analyze traffic with a network sniffer(wireshark).

### TCP connect scan -sT
### TCP SYN "used to be stealth" scan, -sS
### ping sweep -sn
### don't ping -Pn
### version detection -sV

## b) Nmap operations. Try the examples, explain results and purpose.

### -p1-100, --top-ports 5

### ip-address choice with netmask (10.10.10.0/24) and range (10.10.10.100-130)

### output files -oA foo

### version scanning -sV

### OS fingerprinting, version detection, scripts, traceroute -A 

### runtime interactions

### compare running nmap with and without sudo

### compare -sV to -A in terms of duration

## c) Run nmap -sV on a local web server.

## d) UDP scanning

### What are the most common and interesting services you can find with UDP-scanning?
### Why is UDP scanning difficult and unreliable?
### Why should you use the --reason flag with UDP scanning?

## e) Open connection to the HackTheBox network and show that the connection is working.

## f) Perform active recon on one of the HackTheBox machines. Analyze the results.


## Sources

https://terokarvinen.com/2021/hakkerointi-kurssi-tunkeutumistestaus-ict4tn027-3005/

https://learning.oreilly.com/videos/the-art-of/9780135767849/9780135767849-SPTT_04_00
