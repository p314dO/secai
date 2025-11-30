---
title: Malicious AutoIT
image:
    path: /assets/posts/2025-11-28-malicious-autoit/1.png
date: 2025-11-28 20:16:45 +/-TTTT
categories: [LetsDefend]
tags: [LetsDefend, AutoIT, Security Analyst, Malware] 
draft: true
---

Analyze the AutoIT malware as a malware analyst.

---

## SCRIPT

Our organization's Security Operations Center (SOC) has detected suspicious activity related to an AutoIt script. Can you analyze this exe and help us answer the following questions?


#### What is the MD5 hash of the sample file?
Die is not really necessary in this step.
```
PS C:\Users\LetsDefend\DEsktop\ChallengeFile> certutil -hashfile .\sample MD5
MD5 hash of .\sample:
XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
CertUtil: -hashfile command completed successfully.
PS C:\Users\LetsDefend\DEsktop\ChallengeFile>
```

#### According to the Detect It Easy (DIE) tool, what is the entropy of the sample file?

![](/assets/posts/2025-11-28-malicious-autoit/3.png)

#### According to the Detect It Easy(DIE) tool, what is the virtual address of the “.text” section?
Answer Format: 0x0000

![](/assets/posts/2025-11-28-malicious-autoit/4.png)
![](/assets/posts/2025-11-28-malicious-autoit/5.png)

**0x1000**




#### References

- [AutoIT](https://www.autoitscript.com/site/autoit/)
- [T1059.010](https://attack.mitre.org/techniques/T1059/010/)