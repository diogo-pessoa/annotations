---
title: "CCSP-Glossary -Notes"
date: 2024-09-02T16:32:31+01:00
description: "Acronyms, terms with brief descriptions"
featured: true
draft: false
toc: true
categories:
  - cybersecurity
tags:
  - ccsp
  - exam-guide
  - cloud
  - glossary
series:
  - ccsp-exam-prep
---

### Drafts

`Revise`

* **Homomorphic encryption** is a form of encryption that allows computations to be performed
  directly on
  encrypted data without needing to decrypt it first. The results of these computations, when
  decrypted, match the results of operations as if they had been performed on the original,
  unencrypted data. This property makes homomorphic encryption particularly useful in scenarios
  where
  data privacy is critical, such as secure cloud computing, confidential data analysis, and
  privacy-preserving machine learning.

`Revise`

* **Asymmetric encryption**, also known as public-key encryption, is a type of encryption that uses
  a pair of keys — a public key and a private key — to secure data. The public key is openly shared
  and can be used by anyone to encrypt data, while the private key is kept secret and is used to
  decrypt the data. This approach ensures that only the intended recipient, who possesses the
  corresponding private key, can access the original information.

`Revise`

* **SSMS Secret Sharing** allows users to securely store and manage sensitive information like
  passwords, connection strings, or keys directly in SQL Server Management Studio (SSMS). This can
  be achieved through the SQL Server's Credential Management feature or by using the Azure Key Vault
  integration for enhanced security.

`Revise`

* **An IaaS service** model allows an organization to retain the most control of their IT assets in
  the
  cloud; the cloud customer is responsible for the operating system, the applications, and the data
  in the cloud. The private cloud model allows the organization to retain the greatest degree of
  governance control in the cloud; all the other deployment models would necessitate giving up
  governance control in an environment with pooled resources.

`Revise`

* **“unvalidated redirects and forwards.”** - Train users to recognize invalidated links.
* Oddly enough, this may be a good topic to explain during user training; when an attacker is trying
  to conduct an attack by exploiting unvalidated redirects and forwards, it is often in conjunction
  with a social engineering/phishing aspect. Users trained to recognize social engineering/phishing
  indicators might be able to avoid susceptibility to these attacks.

`Revise`

**cloud carrier** is “an intermediary that provides connectivity and transport of cloud services
from Cloud Providers to Cloud Consumers.”

The Diffie-Hellman key exchange process is designed to allow two parties to create a shared secret (
symmetric key) over an untrusted medium. RADIUS is an outmoded access control service for remote
users. RSA is an encryption scheme. TACACS is a network access protocol set used through a
centralized server.

* **Shadow IT** - Pending
* **Virtual sprawl** - refers to the uncontrolled expansion of digital infrastructure, resources,
  and platforms within an organization, often resulting in inefficiencies, management challenges,
  and increased cybersecurity risks. This concept is analogous to urban sprawl, where unchecked
  growth leads to disorganized development, resource depletion, and difficulty in management.

* **IRM/DRM** - IRM (Information Rights Management) and DRM (Digital Rights Management) are
  technologies and methodologies used to control and protect access to digital information and
  content.

* **PLA** Public License Agreement
* **ROI**  Return On Investment
* **SDN** software-defined networking
* **VMI** Vendor-Managed Inventory
* **ECC** Enterprise Control Center

* **BIA** Business Impact Analysis
* **TCO** Total cost of ownership
* **SANS Institute**

* **COBIT** (Control Objectives for Information and Related Technologies) is a framework for
  governing and managing enterprise IT. It was developed by ISACA (Information Systems Audit and
  Control Association) to help organizations achieve their goals for information technology
  governance and management, ensuring that IT investments align with business objectives and deliver
  value while managing risks and resources effectively.

* **Spoliation** is the term used to describe the destruction of potential evidence (intentionally
  or otherwise); in various jurisdictions, it can be a crime, or the grounds for another lawsuit.
* **ISMS** Information Security Management System
* **OWASP**: Open Worldwide Application Security Project


* **ONF** - Organization normative framework
* **UPS** Uninterruptable Power supply

* **SAN** Storage Area Network
* **CASB** Cloud access Security Broker


* **DAM** - Digital Asset Management

* **CCSK** Certificate of Cloud Security Knowledge
* **CSA**  Cloud Security Alliance
* **CCM** Cloud Controls Matrix
* **HVAC** Heating, Ventilation, and Air Conditioning
* **PKI**  Public Key Infrastructure
* **HSM** - Hardware security Module

---

### Personal Data Management

#### Acronyms
* **PII** Personal Identifiable Information
    * **PCI** Payment Card Information
    * **PHI** Protected Health Information
#### Terms

---

* **SAML** Security Assertion Markup Language

* **STRIDE** Spoofing, Tampering, Repudiation, Information Disclosure, Denial of Service, and
  Elevation of Privilege



* **ATASM** Architecture, Threats, Attack Surfaces, and Mitigations

* **PASTA** Process for Attack Simulation and Threat Analysis
* **NIST** National Institute of Standards and Technology
* **STAR** Security, Trust, Assurance, and Risk
* **CIA** triad: confidentiality, integrity, and availability.
* **ANF** Application Normative Framework
* **ONF** Organization Normative Framework

---

### SDLC & vulnerability Management

#### Acronyms

* **DAST** Dynamic Application Security Testing
* **SAST** Static application security testing

#### Terms

---

### Disaster Recovery & Business Continuity

#### Acronyms

* **RPO** Recovery Point Objective
* **BC** Business Continuity
* **DR** Disaster Recovery
* **MTD/MAD** Maximum Tolerable (or Accepted) Downtime
* **BCP** Business Continuity Plan
* **SLE** - Single Loss Expectancy

#### Terms
* **MTTR** (Mean Time to Recovery or Mean Time to Repair) is a key metric used in IT service
  management, DevOps, and incident management to measure the average time it takes to recover from a
  failure or restore a system to its normal operating state after an incident. MTTR is a critical
  indicator of an organization's ability to respond to and recover from incidents, such as hardware
  failures, software bugs, network outages, or security breaches.
* **ARO** Annual Rate of Occurrence is a metric used in risk management and cybersecurity to
  estimate the expected frequency with which a specific risk or threat is likely to occur within a
  year. It is typically used in conjunction with other metrics.
* **Asset Value (AV)** The financial value of the asset.
* **Exposure Factor (EF)**  The percentage of the asset expected to be damaged by the risk.

* **ALE** Annual Loss Expectancy, to quantify and evaluate the potential impact of risks on an
  organization.

### Incident Management

#### Acronyms

**SIEM** (Security Information and Event Management). Microsoft What
is [SIEM](https://www.microsoft.com/en-us/security/business/security-101/what-is-siem)
* **IoC**  indicator of compromise
* **DREAD** Disaster, Reproducibility, Exploitability, Affected Users, and Discoverability.

#### Terms
* **threat actor** A threat actor is an individual or group that poses a potential risk to an
  organization's security by attempting to exploit vulnerabilities, often with malicious intent.

### Intrusion Detection

#### Acronyms
* **HIDS** (Host-based Intrusion Detection System)
* **NIDS** (Network-based Intrusion Detection System) is a security solution designed to monitor and
  analyze network traffic to detect and alert on suspicious activities, potential threats, or
  unauthorized access attempts within a network.
* **Honeypot** is a security mechanism set up to detect, deflect, or study attempts at unauthorized
  access to information systems.

#### Terms

---

### Data Management & Control

#### Acronyms
* **DLP** Data Loss Prevention

#### Terms

* **Cloud Data Lifecycle** **C**reate **S**tore **U**se **S**hare **A**rchive **D**estroy
* **Cryptographic erasure** is a method of data destruction where encryption keys are deleted, rendering
  the encrypted data unreadable and effectively destroyed.

---

### Audits

#### Acronyms

* **SOC 1** report focuses on outsourced services that could impact a company's financial reporting.
  By providing a SOC 1 report from the third-party, companies can effectively communicate
  information about their risk management and controls framework to multiple stakeholders.

* **SOC 2** They're intended to help organizations and service providers design and implement their
  control environment

#### Terms

---
