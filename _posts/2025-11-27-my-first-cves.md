---
title: My first CVE's
image:
    path: /assets/posts/2025-11-27-my-first-cves/1.png
date: 2025-11-27 10:19:00 +/-TTTT
categories: [Security Research]
tags: [CVE, MonicaHQ 4.1.2]
---


During an investigation on the application [Monica](https://hub.docker.com/_/monica), I identified 5 new CVEs. All of them are Client-Side Injection vulnerabilities leading to Stored XSS.

## CVE-2024-54994
### CSTI Leading to Stored Cross-Site Scripting (XSS)

**Description**: MonicaHQ v4.1.2 was found to contain multiple client-side injection vulnerabilities through the *first_name* and *last_name* parameters in the *Add a new relationship* function.  
**Affected Versions**: v4.1.2  
**Researcher**: [Nicolás Gula](https://www.linkedin.com/in/josebonzini/)  
**Disclosure Link**: [GitHub](https://github.com/p314dO/CVEs/tree/main/CVE-2024-54994)  
**NIST CVE Link**: https://nvd.nist.gov/vuln/detail/CVE-2024-54994  

### Details

- MonicaHQ 4.1.2 is vulnerable to client-side template injection. An authenticated attacker can inject malicious code into the *first_name* and *last_name* parameters in the *Add a new relationship* form.
- To exploit this vulnerability, the following payload can be injected into the *first_name* and *last_name* parameters:

```
{% raw %}ty {{toString().constructor.constructor('alert(1)')()}}.{% endraw %}
```


**First Case**: Enter a name in the *first_name* parameter and the payload in the *last_name* parameter.  
**Second Case**: Enter the payload in the *first_name* parameter and leave the *last_name* parameter empty.

### PoC

![cve-2024-5994](/assets/posts/2025-11-27-my-first-cves/first_case.gif)   
![cve-2024-5994](/assets/posts/2025-11-27-my-first-cves/second_case.gif)

---

## CVE-2024-54996
### CSTI Leading to Stored Cross-Site Scripting (XSS)

**Description**: MonicaHQ v4.1.2 was found to contain multiple authenticated client-side injection vulnerabilities via the *title* and *description* parameters in */people/ID/reminders/create*.  
**Affected Versions**: v4.1.2  
**Researcher**: [Nicolás Gula](https://www.linkedin.com/in/josebonzini/)  
**Disclosure Link**: [GitHub](https://github.com/p314dO/CVEs/tree/main/CVE-2024-54996)  
**NIST CVE Link**: https://nvd.nist.gov/vuln/detail/CVE-2024-54996  

### Details

- MonicaHQ 4.1.2 is vulnerable to client-side template injection. An authenticated attacker can inject malicious code into the *title* and *description* fields of the "*What would you like to be reminded of about Test?*" form within the */people/ID/reminders/create* section.
- To exploit this vulnerability, the following payload can be injected into the *title* and *description* fields:

```
{% raw %}ty {{toString().constructor.constructor('alert(1)')()}}.{% endraw %}
```


### PoC

![cve-2024-5994](/assets/posts/2025-11-27-my-first-cves/PoC.gif)

---

## CVE-2024-54997
### CSTI Leading to Stored Cross-Site Scripting (XSS)

**Description**: MonicaHQ v4.1.1 was found to contain an authenticated client-side injection vulnerability via the input text field in */journal/entries/ID/edit*.  
**Affected Versions**: v4.1.1  
**Researcher**: [Nicolás Gula](https://www.linkedin.com/in/josebonzini/)  
**Disclosure Link**: [GitHub](https://github.com/p314dO/CVEs/tree/main/CVE-2024-54997)  
**NIST CVE Link**: https://nvd.nist.gov/vuln/detail/CVE-2024-54997  

### Details

- MonicaHQ 4.1.1 is vulnerable to client-side template injection. An authenticated attacker can inject malicious code into the *entry* field in the "*Edit a journal entry*" form within the */journal/entries/ID/edit* section.
- To exploit this vulnerability, the following payload can be injected into the *entry* field:

```
{% raw %}ty {{toString().constructor.constructor('alert(1)')()}}.{% endraw %}
```


### PoC

![cve-2024-5997](/assets/posts/2025-11-27-my-first-cves/PoC2.gif)

---

## CVE-2024-54998
### CSTI Leading to Stored Cross-Site Scripting (XSS)

**Description**: MonicaHQ v4.1.2 was found to contain an authenticated client-side injection vulnerability via the *reason* parameter in */people/h:[id]/debts/create*.  
**Affected Versions**: v4.1.2  
**Researcher**: [Nicolás Gula](https://www.linkedin.com/in/josebonzini/)  
**Disclosure Link**: [GitHub](https://github.com/p314dO/CVEs/tree/main/CVE-2024-54998)  
**NIST CVE Link**: https://nvd.nist.gov/vuln/detail/CVE-2024-54998  

### Details

- MonicaHQ 4.1.2 is vulnerable to client-side template injection. An authenticated attacker can inject malicious code into the *reason* field in the "*Add Debt*" form within the */people/h:[id]/debts/create* section.
- To exploit this vulnerability, the following payload can be injected into the *reason* field:

```
{% raw %}ty {{toString().constructor.constructor('alert(1)')()}}.{% endraw %}
```

### PoC

![cve-2024-5998](/assets/posts/2025-11-27-my-first-cves/Poc3.gif)

---
## CVE-2024-54999
### CSTI Leading to Stored Cross-Site Scripting (XSS)

**Description**: MonicaHQ v4.1.2 was found to contain a client-side injection vulnerability via the *last_name* parameter in the *General Information* module.  
**Affected Versions**: v4.1.2  
**Researcher**: [Nicolás Gula](https://www.linkedin.com/in/josebonzini/)  
**Disclosure Link**: [GitHub](https://github.com/p314dO/CVEs/tree/main/CVE-2024-54999)  
**NIST CVE Link**: https://nvd.nist.gov/vuln/detail/CVE-2024-54999  

### Details

- MonicaHQ 4.1.2 is vulnerable to client-side template injection. An authenticated attacker can inject malicious code into the *last_name* field in the "General Information" form within */settings*.
- To exploit this vulnerability, the following payload can be injected into the *last_name* field:

```
{% raw %}ty {{toString().constructor.constructor('alert(1)')()}}.{% endraw %}
```
----


![lol](/assets/posts/2025-11-27-my-first-cves/meme.webp)

## References

- [CVE-2024-54994](https://nvd.nist.gov/vuln/detail/CVE-2024-54994)
- [CVE-2024-54996](https://nvd.nist.gov/vuln/detail/CVE-2024-54996)
- [CVE-2024-54997](https://nvd.nist.gov/vuln/detail/CVE-2024-54997)
- [CVE-2024-54998](https://nvd.nist.gov/vuln/detail/CVE-2024-54998)
- [CVE-2024-54999](https://nvd.nist.gov/vuln/detail/CVE-2024-54999)
- [Github with Pocs](https://github.com/p314dO/CVEs/tree/main)
- [Evading defences using vuejs script gadgets](https://portswigger.net/research/evading-defences-using-vuejs-script-gadgets)