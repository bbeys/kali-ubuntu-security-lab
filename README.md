
Readme · MD
# Home Cybersecurity Lab: Kali Linux + Ubuntu Server
 
An isolated virtual lab I built to practice network reconnaissance, SSH exploitation, brute-force attack simulation, and log analysis, as part of my prep for TryHackMe's SOC Level 1 path.
 
## Setup
 
Two VMs in VirtualBox:
 
- **Kali Linux** (attacker) - installed from the official ISO, custom credentials
- **Ubuntu Server 26.04 LTS** (target) - installed from the official ISO, custom credentials
Both VMs run two network adapters:
 
- **Host-Only** (192.168.6.0/24) - isolates the two VMs from the internet so all attack/target traffic stays contained
- **NAT** - gives each VM internet access separately, for updates and package installs
Steps taken: configured the Host-Only network with DHCP, installed both VMs, set up dual-adapter networking, confirmed connectivity with ping and nmap, enabled OpenSSH on the Ubuntu target, and took snapshots at each working stage.
 
## What I did
 
**Reconnaissance.** Scanned the target from Kali:
```
nmap -sV 192.168.6.4
```
Found the open SSH port and service version.
 
**Remote access.** Connected to the target over SSH:
```
ssh beisa@192.168.6.4
```
 
**Brute-force simulation.** Ran Hydra against the SSH service with a password list:
```
hydra -l beisa -P passlist.txt ssh://192.168.6.4
```
 
**Log analysis.** Checked the SSH authentication logs on Ubuntu:
```
sudo journalctl -u ssh -n 50
```
The failed login attempts showed up as multiple `Failed password` entries from the same source IP within a few seconds, and the target automatically rate-limited that IP (`srclimit_penalise`). This is the kind of pattern worth recognizing when reviewing authentication logs.
 
## Skills used
 
VirtualBox networking (host-only, NAT), Linux administration (Kali, Ubuntu Server), Nmap, SSH, Hydra, journalctl, isolated lab design.
 
## Note
 
All testing was against my own VMs, on a network with no route to the internet. Nothing external was touched.
 
