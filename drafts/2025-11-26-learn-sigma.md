---
title: Learn Sigma
image:
    path: /assets/posts/2025-11-26-learn-sigma/1.jpg
date: 2025-11-26 23:32:45 +/-TTTT
categories: [LetsDefend]
tags: [LetsDefend, Sigma Rules, Security Analyst] 
---

A hands-on challenge to understand Sigma rule structure and detection logic.

---

## SCRIPT

Your organization has detected a ransomware infection on one of its critical systems, and it is imperative that you address this issue immediately. This type of malware searches for valuable files, such as sensitive documents and configuration files, and encrypts them using a strong encryption algorithm.

The investigation has revealed that the ransomware may have used the Windows utility bitsadmin.exe to download additional malicious payloads or communicate with its command-and-control (C2) server.

Your task is to carefully review the Sigma rule, answer the related questions, and understand how different rule sections (selection, condition, fields, tags, logsource) work together to detect malicious activity.

```
title: File Download Via Bitsadmin
id: d059842b-6b9d-4ed1-b5c3-5b89143c6ede
status: test
description: Detects usage of bitsadmin downloading a file
references:
    - https://blog.netspi.com/15-ways-to-download-a-file/#bitsadmin
    - https://isc.sans.edu/diary/22264
    - https://lolbas-project.github.io/lolbas/Binaries/Bitsadmin/
author: Michael Haag, FPT.EagleEye
date: 2017-03-09
modified: 2023-02-15
tags:
    - attack.defense-evasion
    - attack.persistence
    - attack.t1197
    - attack.s0190
    - attack.t1036.003
logsource:
    category: process_creation
    product: windows
detection:
    selection_img:
        - Image|endswith: '\bitsadmin.exe'
        - OriginalFileName: 'bitsadmin.exe'
    selection_cmd:
        CommandLine|contains: ' /transfer '
    selection_cli_1:
        CommandLine|contains:
            - ' /create '
            - ' /addfile '
    selection_cli_2:
        CommandLine|contains: 'http'
    condition: selection_img and (selection_cmd or all of selection_cli_*)
fields:
    - CommandLine
    - ParentCommandLine
falsepositives:
    - Some legitimate apps use this, but limited.
level: medium
```

#### Which executable file was specifically targeted by this Sigma rule?
- bitsadmin.exe
```
selection_img:
        - Image|endswith: '\bitsadmin.exe'
        - OriginalFileName: 'bitsadmin.exe'
```

#### What command-line option is used to indicate a file transfer in this rule?
- /transfer
```
/transfer
```

#### What logical expression in the condition field combined the criteria to trigger this rule?
- condition: selection_img and (selection_cmd or all of selection_cli_*)
```
 selection_cli_2:
        CommandLine|contains: 'http'
    condition: selection_img and (selection_cmd or all of selection_cli_*)
```

#### Which specific field did this rule capture that shows the command being executed?
- CommandLine
```

```

#### Which single ATT&CK tactic tag is listed first in this rule?
- attack.defense-evasion
```
tags:
    - attack.defense-evasion
```

#### What is the primary category of events that this Sigma rule was written to monitor?
- process_creation
```
logsource:
    category: process_creation
```

#### What specific command-line argument did this rule look for to identify HTTP-based downloads?
- http
```
selection_cli_2:
        CommandLine|contains: 'http'
```

#### Which command-line option must be present to create a new transfer using bitsadmin?
- /create
```
selection_cli_1:
        CommandLine|contains:
            - ' /create '
```