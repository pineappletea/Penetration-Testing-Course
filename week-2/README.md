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





## a) List tools and techniques phase of the Lockheed Martin Cyber Kill Chain, atleast one for each phase. Try 1-3 of the tools.

### Reconnaissance
passive: company webpage, linkedIn, financial records, search engines
active: port scanning, banner grabbing, service detection scans

### Weaponization

### Delivery

### Exploitation

### Installation

### Command and control (C2)

### Actions on objectives

## b) Install Metaspoitable 2. Break in in multiple ways.

## c) Install a machine from VulnHub. Break in.
