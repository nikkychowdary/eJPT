# eJPT notes
Resources and methodology required to pass eJPT certification. (eLearnSecurity Junior Penetration Tester)




Routing:
- to check ip route : ip route
- To add routing, ip route add <ip>/CIDR via gatewayIP

 <hr />
  

IP and MAC Address:

- ifconfig
- ip addr show [interface]
  
 <hr />

Ping Sweep:

- fping -a -g <ip>/24 2> /dev/null
- nmap -sn -n <ip>/24
 <hr />
  
Nmap Scans needed:

- OS detection: nmap -O -Pn network/CIDR

- Quick Scanning: nmap -sC -sV -T4 network/CIDR

- Full Scanning: nmap -sC -sV -T4 -p- network/CIDR

- UDP Scanning: nmap -sU -sV -T4 network/CIDR

<hr />
Banner grabbing (HTTP):

- nc -v <machine IP> PORT
- HEAD / HTTP/1.0

<hr />
Banner grabbing (HTTPS):

- openssl s_client -connect <machine IP>:PORT
- HEAD / HTTP/1.0
- To debug the host, openssl s_client -connect hack.me:443 -debug

- To know the state of the connection, openssl s_client -connect hack.me:443 -state

- no information related to certificate, state or other information but communication with server, openssl s_client -connect hack.me:443 -quiet

<hr />
  
SQLMAP:

- To check if website is vulnerable to SQLi, sqlmap -u http://<machine IP>:PORT -p parameter

- Check if POST parameter is vulnerable to SQLi, sqlmap -u http://<machine IP>:PORT --data POSTstring -p parameter

- To check if we can get os-shell using SQLmap, sqlmap -u http://<machine IP>:PORT --os-shell

- To dump the tables, sqlmap -u http://<machine IP>:PORT --dump

- Sqlmap cheatsheet i used : https://medium.com/@NikhilPinnamaneni/sqlmap-cheat-sheet-80006296ca2a

 <hr />
  
John:

- unshadow passwd shadow > hashpass
- john --wordlist=/path/to/wordlist.txt -users=user.txt hashpass
  
<hr />

HYDRA:

- FTP: hydra -l user -P passlist.txt ftp://10.10.181.249


- SSH: hydra -l <username> -P <full path to pass> 10.10.181.249 -t 4 ssh


- Post Webforms: hydra -l <username> -P <wordlist> 10.10.181.249 http-post-form "/:username=^USER^&password=^PASS^:F=incorrect" -V

<hr />
Subdomain Enum:

- To enumerate subdomains, sublist3r -d site.com

<hr />

Windows Shares using Null Sessions:

- -via nmblookup, nmblookup -A <machine IP>

- List shares using smbclient, smbclient -L //<machine IP> -N

- Mount Shares using smbclient, smbclient //<machine IP>/share -N

- Exploiting Null Sessions using Enum4linux: To check if the remote host is vulnerable to null session, enum4linux -n 192.168.99.162

- To gather info, enum4linux -a <machine IP>

<hr />
  
ARP Spoofing:

- To enable Port Forwarding, echo 1> /proc/sys/net/ipv4/ip_forward

- Run arpspoof, arpspoof -i [interface] -t <target machine IP> -r <host machine IP>

<hr />
  
Metasploit:

- Basic commands of metasploit-framework:

- To start metasploit-framework using CLI, msfconsole

- Without banner, msfconsole -q

after starting of MSF, these are the basic commands,
- search x
- use x
- info
- options, show advanced options
- SET X (e.g. set RHOST 10.10.10.10, set payload x)

<hr />
  
Meterpreter Commands:

- After getting meterpreter shell, we'll can issue some commands which will help us in exploiting the machine,

- background
- sessions -l
- sessions -i N (N is number)
- sysinfo, ifconfig, route, getuid
- getsystem (privesc)
- bypassuac
- download x /root/
- upload x C:\\Windows
- shell
- hashdump
For auto routing, use post/multi/manage/autoroute then set the options {SESSIONS, SUBNET} (will work if meterpreter shell shows that user is root)

Check active route table, route

port forwarding, portfwd add -L <attacker machine IP> -l listening_port -p 80 -r <host machine IP>

<hr />
  
Spawning a TTY shell:

- python -c 'import pty;pty.spawn("/bin/bash");'

<hr />
Start python based file server on our own instance:

- python3 -m http.server 80

<hr />

Gobuster:

- gobuster dir -u http://<ip>/ -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -t 15

<hr />
Find:

- find / -iname * (keyword) * 2>/dev/null
<hr />

PIVOTING: (from meterpreter)


- run autoroute –h

- run autoroute -s 10.1.1.0 -n 255.255.255.0  # Add a route to 10.10.10.1/255.255.255.0

- background

- route print

- route add 10.1.1.0 255.255.255.0 1 # where 1 is the session id

- use auxiliary/scanner/portscan/tcp

- In the meterpreter session there is an utility "portfwd" which allows forwarding remote machine port to the local machine port. 

- session -i 1

- portfwd -h


- portfwd add -l 1234 -p 21 -r 192.69.228.3 #where l is local port and p is victims port and r is victim's ip

- portfwd list

- nmap -sV -A -p1234 127.0.0.1

<hr />

Windows Command Line:



To search for a file starting from current directory;

dir /b/s "*.conf*"
dir /b/s "*.txt*"
dir /b/s "*filename*"


Check routing table:
route print
netstat –r

Check Users:
net users

List drives on the machine:
wmic logicaldisk get Caption,Description,providername

<hr />
  
**All the Best for your Exam :)**

