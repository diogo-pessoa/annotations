---
title: "STRIDE, LINDDUN and Risk Rating: Finding and Ranking Threats"
date: 2026-10-15T16:45:00+02:00
draft: true
toc: true
tags:
  - ccsp
  - security
  - threat-modeling
---

**The [Four Question Frame](/post/ccsp/ccsp-threatmodeling/) tells you what to ask. It does not tell you how to answer "what can go wrong?" or "what do we fix first?". That is the job of a framework. STRIDE is the most common one,[\[7\]](#references) but it only does half the work.**

*These notes were written while studying for the CCSP, alongside OWASP material and the AWS Skill Builder course Threat Modeling for Builders (2025). They are kept provider-neutral.*

## Two jobs, two kinds of framework

The word "framework" covers two different kinds of tool. Mixing them up is the most common source of confusion.

| Job | Four Question Frame | Frameworks in this post |
|---|---|---|
| Find threats | Q2: What can go wrong? | STRIDE (security), LINDDUN (privacy) |
| Rank threats | Q3: What are we going to do about it, and in what order? | DREAD, OWASP Risk Rating, CVSS |

The usual pattern is to pair one of each. STRIDE finds the threats, LINDDUN adds the privacy ones when personal data is involved, and a risk rating puts the list in order. End-to-end methods such as PASTA and OCTAVE wrap a whole process around these steps. They are left for a later post.

## STRIDE

Loren Kohnfelder and Praerit Garg created STRIDE at Microsoft in 1999.[\[1\]](#references) It is a mnemonic for six threat categories.[\[3\]](#references) Each one is the negation of a security property. That pairing is the most useful thing to remember: once you know the threat, you know which kind of control answers it.

| Threat | Property violated | Question to ask |
|---|---|---|
| **S**poofing | Authentication | Can someone pretend to be someone or something else? |
| **T**ampering | Integrity | Can someone change data or code they should not? |
| **R**epudiation | Non-repudiation | Can someone deny they did something, with no way to prove otherwise? |
| **I**nformation disclosure | Confidentiality | Can someone see data they should not? |
| **D**enial of service | Availability | Can someone stop the system from serving legitimate users? |
| **E**levation of privilege | Authorization | Can someone do more than they are allowed to? |

### Spoofing

Spoofing is pretending to be someone or something you are not. The attacker claims a false identity, whether a user, a process, a server or a device, to gain trust they should not have.

- *Examples:* logging in with stolen credentials, replaying a session token, forging an email sender, a fake server answering for a real API through DNS or ARP spoofing.
- *Mitigations:* strong authentication (multi-factor, signed tokens, mutual TLS), credential protection and rotation, short-lived sessions with replay protection, and server certificate validation.

### Tampering

Tampering is unauthorized modification of data or code. It can happen in transit, at rest, or in memory.

- *Examples:* altering a request between client and server, editing a price field in a form, changing a configuration file, swapping a dependency in the build pipeline.
- *Mitigations:* integrity checks (hashes, digital signatures, message authentication codes), TLS for data in transit, strict write permissions, signed artifacts, and server-side validation of every input the client can touch.

### Repudiation

Repudiation is the ability to deny an action because the system cannot prove who did it. The threat is not the action itself. It is the missing evidence.

- *Examples:* a user disputes a transaction and there is no log tying it to them, an administrator deletes records and also deletes the audit trail, shared accounts make it impossible to tell people apart.
- *Mitigations:* audit logs that capture who, what and when, logs stored where the actor cannot change them (append-only or write-once storage), individual accounts instead of shared ones, synchronized clocks, and digital signatures on high-value actions.

### Information disclosure

Information disclosure is exposure of data to someone who is not authorized to see it.

- *Examples:* a publicly readable storage bucket, stack traces or secrets in error messages, unencrypted traffic, an API that returns more fields than the client needs, side channels such as timing differences.
- *Mitigations:* encryption in transit and at rest, least-privilege access, data minimization, generic error messages, secrets management, and response filtering.

### Denial of service

Denial of service makes a system unavailable or degraded for legitimate users. It does not have to be a flood. Any way to exhaust a resource counts.

- *Examples:* a traffic flood, an expensive query sent in a loop, a regular expression that backtracks forever, filling a disk with logs, locking out every account by failing logins on purpose.
- *Mitigations:* rate limiting and throttling, quotas, timeouts, input size limits, autoscaling and redundancy, upstream traffic filtering, and lockout policies that slow attackers without locking out users.

### Elevation of privilege

Elevation of privilege is gaining capabilities beyond what you were granted. A normal user acts as an administrator, or an outside caller runs code inside the system.

- *Examples:* changing a user ID in a URL to reach another account, injection that leads to code execution, a misconfigured role that lets a user grant themselves more permissions, escaping a container.
- *Mitigations:* authorization checks on every request (on the server, never only in the client), least privilege, input validation and parameterized queries, sandboxing, and separation of duties for permission changes.

### Applying STRIDE to a diagram

STRIDE works on a data flow diagram (DFD). Not every threat applies to every element. The STRIDE-per-element chart from Shostack's book narrows the search:[\[2\]](#references)[\[9\]](#references)

| DFD element | S | T | R | I | D | E |
|---|---|---|---|---|---|---|
| External entity | ✓ | | ✓ | | | |
| Process | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ |
| Data store | | ✓ | ✓* | ✓ | ✓ | |
| Data flow | | ✓ | | ✓ | ✓ | |

\* Repudiation applies to a data store when it holds logs.

The logic is simple once spelled out. Only things that claim an identity can be spoofed. Only things that act can deny acting. A data flow has no identity and takes no action, so it can only be read, changed or blocked. Processes do everything, so they get all six.

A variant, STRIDE-per-interaction, looks at each flow together with its source and destination instead of each element alone. It yields fewer duplicate findings but takes more effort to learn.

### Where STRIDE falls short

STRIDE is a checklist of categories, not a method. It is easy to teach and hard to misuse, which explains its reach. It also has two gaps that matter here:

- **It does not cover privacy.** A system can be perfectly secure and still track, profile or re-identify its users. STRIDE has no category for that. LINDDUN fills this gap.
- **It does not rank anything.** Every finding comes out with equal weight, and walking six categories over every element produces a long list. A risk rating has to follow.

## LINDDUN

LINDDUN does for privacy what STRIDE does for security. Researchers at KU Leuven developed it, and it also works on a DFD.[\[4\]](#references) The categories are:

- **Linking:** connecting data items or actions to the same person, even without knowing who they are.
- **Identifying:** learning the identity of the person behind the data.
- **Non-repudiation:** a person cannot deny a claim, such as having used a service.
- **Detecting:** inferring involvement just by observing that data or a person exists in the system.
- **Data disclosure:** collecting, storing, processing or sharing more personal data than needed.
- **Unawareness and unintervenability:** people are not told, or cannot control, how their data is used.
- **Non-compliance:** the system breaks privacy law, regulation or its own policy.

Two contrasts with STRIDE are worth holding on to.

First, non-repudiation flips sides. In STRIDE it is a security goal: the system must prove who did what. In LINDDUN it can be the harm: a whistleblower or a patient may need to deny having used a service. A strong audit trail that fixes a repudiation threat can create a privacy threat. The two frameworks run on the same diagram and can pull a design in opposite directions. That tension is the reason to run both.

Second, the attacker changes. In STRIDE, the threat usually comes from outside the trust boundary. In LINDDUN, the system owner itself can be the threat: an operator who collects too much, keeps it too long, or links it in ways the user never agreed to.

Use LINDDUN next to STRIDE whenever the system handles personal data. Under GDPR-style regulation, that is most systems.

## Ranking threats

A threat list is not a plan. Question 3 needs an order of work, and that order comes from risk:

> Risk = Likelihood × Impact

The three schemes below are three ways to put numbers on that formula. They differ in what they measure and how much judgment they leave to the rater.

### DREAD

DREAD was STRIDE's companion at Microsoft. It scores each threat on five factors, usually from 1 to 10, and averages them:

- **D**amage: how bad is it if it happens?
- **R**eproducibility: how reliably can the attack be repeated?
- **E**xploitability: how much effort or skill does it take?
- **A**ffected users: how many people are hit?
- **D**iscoverability: how easy is the weakness to find?

Damage and affected users describe impact. The other three describe likelihood. The average blends them into one number.

Microsoft later dropped DREAD from its own process.[\[2\]](#references) The scores were too subjective: two engineers rated the same threat very differently, because each factor had no fixed definition of what a 3 or a 7 means. Discoverability drew the most criticism, because it rewards obscurity. A hard-to-find flaw is still a flaw, and attackers are often better at finding it than the rater assumes. Many teams that still use DREAD set discoverability to the maximum by default. DREAD survives as a teaching tool and a quick triage aid.

### OWASP Risk Rating

The OWASP Risk Rating Methodology answers DREAD's subjectivity by naming every factor.[\[5\]](#references) It is covered in detail in [the main threat modeling post](/post/ccsp/ccsp-threatmodeling/#rating-risk). In short:

- **Likelihood** comes from eight factors: four about the threat agent (skill, motive, opportunity, size) and four about the vulnerability (ease of discovery, ease of exploit, awareness, intrusion detection).
- **Impact** comes from technical factors (loss of confidentiality, integrity, availability, accountability) and business factors (financial, reputation, non-compliance, privacy).
- Each factor scores 0 to 9. The averages map to low, medium or high, and a matrix combines likelihood and impact into a severity from Note to Critical.

Its key improvement is that business impact overrides technical impact when it is known. A technically severe flaw in a system that holds nothing of value is a low risk. DREAD cannot express that.

### CVSS

CVSS (Common Vulnerability Scoring System) is maintained by FIRST. Version 4.0 was released in 2023.[\[6\]](#references) It scores a known vulnerability from 0.0 to 10.0 using fixed metric values instead of free judgment:

- **Base** metrics describe the vulnerability itself: how it is reached (attack vector, complexity, requirements, privileges needed, user interaction) and what it breaks (confidentiality, integrity and availability, on the vulnerable system and on systems beyond it).
- **Threat** metrics adjust for whether exploits exist in the wild.
- **Environmental** metrics adjust for the deployment: how much the affected asset matters, and which controls already exist.
- **Supplemental** metrics add context, such as safety impact or whether the fix can be automated, without changing the score.

The score maps to a severity: None (0.0), Low (0.1 to 3.9), Medium (4.0 to 6.9), High (7.0 to 8.9) and Critical (9.0 to 10.0).

CVSS is the most consistent of the three, because every metric has a fixed set of values. It also has a clear limit. FIRST itself states that the Base score measures severity, not risk. A critical CVSS score in a test system with no data is still a lower priority than a medium score on the payment path. The Environmental metrics exist to close that gap, but many teams publish and act on the Base score alone.

CVSS also fits a different moment. It was built for vulnerabilities that already exist in shipped software. At design time, many threats have no concrete vulnerability yet, so most metrics are guesses. It fits best once a threat from the model has turned into a real bug.

### Comparing the three

| | DREAD | OWASP Risk Rating | CVSS |
|---|---|---|---|
| Rates | A threat | A risk (threat in context) | A known vulnerability |
| Scale | 1 to 10 per factor, averaged | 0 to 9 per factor, then a severity matrix | 0.0 to 10.0, fixed metric values |
| Business context | Implicit, through damage | Explicit, and it overrides technical impact | Only through Environmental metrics |
| Consistency between raters | Low | Medium | High |
| Best moment | Quick triage at design time | Design-time prioritization | After a vulnerability is found |

## Choosing

No framework is best in general. The SEI's review of threat modeling methods reached the same conclusion: pick based on the goal, and combine where it helps.[\[8\]](#references) For the frameworks here:

| If you need to... | Use |
|---|---|
| Find security threats in a design | STRIDE on a DFD |
| Find privacy threats when personal data is involved | LINDDUN, alongside STRIDE |
| Triage a long threat list quickly | DREAD, with discoverability set to the maximum |
| Prioritize threats with business context | OWASP Risk Rating |
| Prioritize concrete vulnerabilities consistently | CVSS, including Environmental metrics |

A reasonable default for most engineering teams is STRIDE to find threats, LINDDUN when personal data is involved, and OWASP Risk Rating to decide what to fix. CVSS takes over once a threat becomes a tracked vulnerability.

The deeper point is that a framework is a prompt, not the model. STRIDE's six letters do not find threats, and a score does not make a decision. People reasoning about a specific system do both. A framework is useful exactly as far as it keeps that reasoning structured and complete enough. It becomes a burden the moment filling in its boxes replaces thinking about the system.

## References

1. [Kohnfelder, L. and Garg, P. (1999), *The threats to our products* (via Adam Shostack)](https://shostack.org/files/microsoft/The-Threats-To-Our-Products.docx)
2. Shostack, A. (2014), *Threat Modeling: Designing for Security*, Wiley.
3. [Microsoft: Threat Modeling Tool threats (STRIDE)](https://learn.microsoft.com/en-us/azure/security/develop/threat-modeling-tool-threats)
4. [LINDDUN privacy threat modeling](https://linddun.org/)
5. [OWASP Risk Rating Methodology](https://owasp.org/www-community/OWASP_Risk_Rating_Methodology)
6. [FIRST: Common Vulnerability Scoring System v4.0](https://www.first.org/cvss/v4.0/specification-document)
7. [OWASP Threat Modeling Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Threat_Modeling_Cheat_Sheet.html)
8. [SEI: Threat Modeling: 12 Available Methods](https://insights.sei.cmu.edu/blog/threat-modeling-12-available-methods/)
9. [Osterman, L., *Threat Modeling Again, STRIDE per Element* (Microsoft archive)](https://learn.microsoft.com/en-us/archive/blogs/larryosterman/threat-modeling-again-stride-per-element)
