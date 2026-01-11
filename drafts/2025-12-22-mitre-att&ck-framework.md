---
title: MITRE ATT&CK Framework - Introduction
image:
    path: /assets/posts/2025-12-22-mitre/1.webp
date: 2025-12-22 05:09:45 +/-TTTT
categories: [MITRE ATT&CK Framework]
tags: [MITRE ATT&CK Framework]
---

![image](./assets/posts/2025-12-22-mitre/ATT&CK_red.png)

## Introduction

Cyberattacks have evolved from simple methods into highly sophisticated techniques as digital systems became more complex.
Because modern systems are so intricate, identifying and understanding advanced attack vectors has become significantly harder.
To effectively counter these threats, it is now essential to model attack steps systematically.
MITRE ATT&CK has emerged as a vital tool for organizing, detailing, and understanding the lifecycle of a cyberattack.

## MITRE

Founded in 1958 in the USA, MITRE is an independent advisor dedicated to national security and public interest through [innovative problem-solving](https://www.mitre.org/). Its expertise spans critical sectors, including Cybersecurity, AI/Machine Learning, Defense, Health, Aerospace, and Homeland Security. Acts as a bridge between government and industry to advance technical solutions for complex global challenges.

## MITRE ATT&CK Framework

It stands for Adversarial Tactics, Techniques, and Common Knowledge.Introduced by MITRE in 2013, this knowledge database is continuously updated to keep pace with evolving technology. It enables the systematic analysis of cyberattacks by breaking them down into specific stages and providing a deep dive into the methods used in each. It is an essential resource for everyone in the cybersecurity industry, serving as the standard for modeling and understanding adversary behavior.

## Matrix

The Matrix is a visualization tool used to classify and display the specific methods (attack vectors) used by cyber adversaries.
he matrix is highly customizable, allowing for the creation of visuals tailored to almost any subject or technical environment.
Its primary goal is to turn complex attacker behaviors into useful, structured visuals that make technical details easier to understand and analyze.

To give you a complete picture based on the latest standards, there are three primary versions:
- [Enterprise (IT/Cloud/Containers)](https://attack.mitre.org/matrices/enterprise/)
- [Mobile](https://attack.mitre.org/matrices/mobile/)
- [ICS (Industrial Control Systems)](https://attack.mitre.org/matrices/ics/)

The matrix is organized by Tactics (the attacker's goal, e.g., "Initial Access") and Techniques (how they achieve it, e.g., "Spearphishing").

![image](./assets/posts/2025-12-22-mitre/2.png)

![image](./assets/posts/2025-12-22-mitre/3.png)

## Tactics

A Tactic represents the "Why"—it is the technical goal or objective an adversary wants to achieve (e.g., gaining access, stealing data, or evading detection). Tactics are the high-level categories used to group attacker behaviors and visualize the different stages of a cyberattack.

![image](./assets/posts/2025-12-22-mitre/4.png)

## Techniques and Sub-Techniques

While Tactics define the attacker's objective (the "Why"), Techniques and Sub-Techniques describe the specific actions and methods (the "How") used to reach that goal. These represent the primary ways an adversary performs an action. For example, under the Initial Access tactic, a common technique is Phishing.These provide a more granular level of detail. Using the Phishing example, sub-techniques specify the exact delivery method, such as Spearphishing Attachment or Spearphishing Link.

![image](./assets/posts/2025-12-22-mitre/5.png)

## Procedure

Procedures represent the real-world implementation of techniques. While a technique is the "how," a procedure is the specific instance or "proof" of that technique in action.It identifies the exact tools, malware, or software used by a specific threat actor (e.g., AppleJeus) to carry out a technique. Procedures provide the most granular level of detail, helping security teams understand the specific patterns and "signatures" of different adversary groups.

![image](./assets/posts/2025-12-22-mitre/6.png)

---

In an era where digital threats are becoming increasingly complex, the MITRE ATT&CK Framework provides the common language needed for modern defense. By breaking down the "Why" (Tactics), the "How" (Techniques), and the "Evidence" (Procedures), it transforms raw threat intelligence into actionable security.

Whether you are a security analyst hunting for threats or an engineer building defenses, mastering this framework is no longer optional—it is the foundation for staying one step ahead of the adversary.