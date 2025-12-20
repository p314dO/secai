---
title: Preignition
image:
    path: /assets/posts/2025-11-30-preignition/1.png
date: 2025-12-03 06:10:05 +/-TTTT
categories: [HackTheBox]
tags: [HackTheBox, ]
---

"Preignition" is a "Very Easy" skill assessment machine within the Starting Point Tier 0 of the Hack The Box (HTB) platform. It is designed to introduce beginners to fundamental penetration testing techniques, specifically web enumeration and directory brute-forcing. 

---

## Enumeration

```
➜  Preignition nmap -sCV -Pn --min-rate 500 -T5 10.129.2.26 -p-
Starting Nmap 7.95 ( https://nmap.org ) at 2025-12-03 13:37 EST
Warning: 10.129.2.26 giving up on port because retransmission cap hit (2).
Nmap scan report for 10.129.2.26 (10.129.2.26)
Host is up (0.22s latency).
Not shown: 65371 closed tcp ports (reset), 163 filtered tcp ports (no-response)
PORT   STATE SERVICE VERSION
80/tcp open  http    nginx 1.14.2
|_http-title: Welcome to nginx!
|_http-server-header: nginx/1.14.2

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 149.39 seconds
```

## Port 80 - nginx 1.14.2

![](/assets/posts/2025-11-30-preignition/2.png)

```
➜  ~ ffuf -w /usr/share/seclists/Discovery/Web-Content/directory-list-2.3-small.txt:FUZZ -u http://10.129.2.26/FUZZ -e .php

        /'___\  /'___\           /'___\
       /\ \__/ /\ \__/  __  __  /\ \__/
       \ \ ,__\\ \ ,__\/\ \/\ \ \ \ ,__\
        \ \ \_/ \ \ \_/\ \ \_\ \ \ \ \_/
         \ \_\   \ \_\  \ \____/  \ \_\
          \/_/    \/_/   \/___/    \/_/

       v2.1.0-dev
________________________________________________

 :: Method           : GET
 :: URL              : http://10.129.2.26/FUZZ
 :: Wordlist         : FUZZ: /usr/share/seclists/Discovery/Web-Content/directory-list-2.3-small.txt
 :: Extensions       : .php
 :: Follow redirects : false
 :: Calibration      : false
 :: Timeout          : 10
 :: Threads          : 40
 :: Matcher          : Response status: 200-299,301,302,307,401,403,405,500
________________________________________________

admin.php               [Status: 200, Size: 999, Words: 132, Lines: 32, Duration: 230ms]
```

![](/assets/posts/2025-11-30-preignition/3.png)

```
admin : admin
```

![](/assets/posts/2025-11-30-preignition/4.png)
