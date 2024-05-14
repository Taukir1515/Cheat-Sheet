# Windows Discover & Mount
- SMB= Server Message Block
- SMB is a Windows implementation of a file-sharing
- SMB uses ports 139 or 445

# Nmap Scripts
```
smb-protocols
```
- Shows SMB Protocol dialects
- Issue: SMB v1
  
```
script smb-protocols
```
- Shows SMB Protocol dialects
- Issue: SMB v1

```
smb-security-mode 
```
- Issue: Messege_signing: disabled

```
smb-enum-sessions 
```

*Giving arguments to smb-enum-sessions will provide additional information*

```
smb-enum-sessions --script-args smbusername=administrator,smbpassword=password IP_address
```

```
smb-enum-shares 
```
- Shows all the SMB shares including Guest, IPC, and Print etc.

```
smb-enum-shares --script-args smbusername=administrator,smbpassword=password 
```
- Shows permissions to the shared accounts

```
smb-enum-users --script-args smbusername=administrator,smbpassword=password 
```
- Issue: See the flags 

```
smb-enum-stats --script-args smbusername=administrator,smbpassword= password 
```
- Server statistics:
	- How many files are sent?
	- Failed Logins
	- Permissions etc.

```
smb-enum-domains --script-args smbusername=administrator,smbpassword=password 
```
- Names of connected Groups

  
```
smb-enum-groups --script-args smbusername=administrator,smbpassword=password 
```
- Provides list of users in Groups


```
smb-enum-services --script-args smbusername=administrator,smbpassword=password 
```
- Stat about running services
  
```
smb-enum-shares,smb-ls --script-args smbusername=administrator,smbpassword=password IP_address
```
- “smb-ls” tells us what's inside each of the shares like finding directory

# SMBMap

### Nmap script: smb-protocols IP
- Shows SMB Protocol dialects
- Issue: SMB v1

### TOOL: smbmap
```
smbmap -u username -p "" -d . -H IP_addr
```
- Shows Permissions of an account with a null password
- -d = directory to see
- -H = Host IP address

```
smbmap -u administrator -p password -H IP_addr -x "ipconfig"
```
- -x = command to execute
  
### After getting SMB connection:
```
smbmap -u administrator -p password -H IP_addr -L
```
- -L = List out the contents of different drives

```
smbmap -u administrator -p password -H IP_addr -r 'C$'
```
- -r = listing a drive content

### Uploading file to target device:
- Create a file (named 'backdoor') in the attacker's device
```
touch backdoor
```
- Uploading 'backdoor' file to the target device where the flag is located
```
smbmap -u administrator -p password -H IP_addr --upload "/root/backdoor" "C$\backdoor"
```
- Check again if the 'backdoor' file is uploaded:
```
smbmap -u administrator -p password -H IP_addr -r "C$"
```
### Downloading file from target device:
```
smbmap -u administrator -p password -H IP_addr --download "C$\flag.txt"
```
- File will be downloaded to the attacker device's “/root” folder.


# SMB - Samba 1
* Always check both TCP and UDP ports using Nmap *

### Nmap Scripts: SMB-os-discovery
### Metasploit Script: auxiliary/scanner/smb/smb_version
### nmblookup: 
• nmblookup –h
• nmblookup -A IP_addr
### smbclient: 
```
smbclient -h
smbclient -L IP_addr -N
```
- -L = List of Hosts
- -N = Null Session = No password

### rpcclient:
• rpcclient -h
• rpcclient -U "" -N IP_addr
- -U= Username
- -N= Null Session= No Password


SMB - Samba 2
 rpcclient:
•	rpcclient -U "" -N IP_addr
After connecting to rpcclient:
•	? 		  	  <<< help
•	srvinfo
•	enumdomusers  	  <<< getting user list
•	lookupnames username  <<< Get full SID of admin
•	exit

enum4linux:
•	enum4linux -h
•	enum4linux -o IP_addr
		-o = Get OS information
•	enum4linux -U IP_addr
		- U = Get user list

 smbclient:
•	smbclient -L IP_addr -N


Nmap Script:
•	smb-protocols
•	smb-enum-users

 Metasploit:
•	auxiliary/scanner/smb/smb2


SMB - Samba 3
 
Nmap script:
•	smb-enum-shares

Metasploit:
•	auxiliary/smb/smb_enumshares

enum4linux:
•	enum4linux -S IP_addr
 		-S =Share information
•	enum4linux -G IP_addr
		-G = Group Information
•	enum4linux -i IP_addr
	-i = network interface


 smbclient:
•	smbclient -L IP_addr -N
		-L = IP list
		-N= Null session (No password)

Getting connection:
•	smbclient //IP_addr/Public -N
	help
	ls
	cd
	get  >>> to download a file
	exit

rpcclient:
•	rpcclient -U "" -N IP_addr

After connecting...
	enumdomgroups

SMB Dictionary Attack


Metasploit:
•	auxiliary/scanner/smb/smb_login
	wordlist >> /usr/share/wordlists/metasploit/unix_passwords.txt

Hydra:
•	hydra -l admin -P /usr/share/wordlists/rockyou.txt IP_addr smb 


smbmap >> To Login Access
•	smbmap -H IP-addr -u admin -P password

 smbclient  >> To see any shares are browse-able
•	smbclient -L IP_addr -U username
•	smbclient //IP_addr/username -U username
o	help
o	ls
o	get
o	exit

PIPE
The way the services talk to each other is through pipes.
Named pipes are pipes that are known.
If we get into SMB, there is a chance that we can get into other services that are piped through it. 

Metasploit:
•	auxiliary/scanner/smb/pipe_auditor

enum4linux:
•	enum4linux -r -u "admin" -p "password" IP_addr
		-r = User's SID








