---
title: Explosion
image:
    path: /assets/posts/2025-11-30-explosion/1.png
date: 2025-11-30 17:48:45 +/-TTTT
categories: [HackTheBox]
tags: [HackTheBox, Redis, Port 3389, RDP]
---

INTRO

---

## Enumeration

```
➜ nmap 10.129.1.13
Starting Nmap 7.95 ( https://nmap.org ) at 2025-12-03 02:36 EST
Nmap scan report for 10.129.1.13 (10.129.1.13)
Host is up (0.22s latency).
Not shown: 995 closed tcp ports (reset)
PORT     STATE SERVICE
135/tcp  open  msrpc
139/tcp  open  netbios-ssn
445/tcp  open  microsoft-ds
3389/tcp open  ms-wbt-server
5985/tcp open  wsman
```

```
➜ nmap 10.129.1.13 -sCV -p- --min-rate 500 -T5 -Pn
Starting Nmap 7.95 ( https://nmap.org ) at 2025-12-03 02:37 EST
Warning: 10.129.1.13 giving up on port because retransmission cap hit (2).
Nmap scan report for 10.129.1.13 (10.129.1.13)
Host is up (0.21s latency).
Not shown: 65486 closed tcp ports (reset), 35 filtered tcp ports (no-response)
PORT      STATE SERVICE       VERSION
135/tcp   open  msrpc         Microsoft Windows RPC
139/tcp   open  netbios-ssn   Microsoft Windows netbios-ssn
445/tcp   open  microsoft-ds?
3389/tcp  open  ms-wbt-server Microsoft Terminal Services
|_ssl-date: 2025-12-03T00:41:10+00:00; -6h59m59s from scanner time.
| rdp-ntlm-info:
|   Target_Name: EXPLOSION
|   NetBIOS_Domain_Name: EXPLOSION
|   NetBIOS_Computer_Name: EXPLOSION
|   DNS_Domain_Name: Explosion
|   DNS_Computer_Name: Explosion
|   Product_Version: 10.0.17763
|_  System_Time: 2025-12-03T00:41:01+00:00
| ssl-cert: Subject: commonName=Explosion
| Not valid before: 2025-12-02T00:32:51
|_Not valid after:  2026-06-03T00:32:51
5985/tcp  open  http          Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-server-header: Microsoft-HTTPAPI/2.0
|_http-title: Not Found
47001/tcp open  http          Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-server-header: Microsoft-HTTPAPI/2.0
|_http-title: Not Found
49664/tcp open  msrpc         Microsoft Windows RPC
49665/tcp open  msrpc         Microsoft Windows RPC
49666/tcp open  msrpc         Microsoft Windows RPC
49667/tcp open  msrpc         Microsoft Windows RPC
49668/tcp open  msrpc         Microsoft Windows RPC
49669/tcp open  msrpc         Microsoft Windows RPC
49670/tcp open  msrpc         Microsoft Windows RPC
49671/tcp open  msrpc         Microsoft Windows RPC
Service Info: OS: Windows; CPE: cpe:/o:microsoft:windows
```

## Port 3389 - RDP

```
xfreerdp /u:administrator /cert:ignore /dynamic-resolution
```

![](/assets/posts/2025-11-30-explosion/2.png)