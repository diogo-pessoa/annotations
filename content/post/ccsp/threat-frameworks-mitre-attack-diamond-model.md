---
title: "Threat Frameworks: MITRE ATT&CK and the Diamond Model"
date: 2026-08-01T11:58:00+02:00
draft: false
toc: true
tags:
  - ccsp
---

**Two frameworks that get confused because they sit next to each other on the intrusion-analysis shelf. They answer different questions and compose cleanly once the split is clear.**

## The short version

* **MITRE ATT&CK** is a *knowledge base* of how adversaries behave — a catalog of tactics and techniques observed in the wild.
* **The Diamond Model** is an *analytic method* for a single intrusion — a way to structure what you know about one event and pivot to what you don't.

One is the dictionary. The other is the sentence you write with it.

## MITRE ATT&CK

ATT&CK stands for Adversarial Tactics, Techniques, and Common Knowledge. It is a living matrix, curated by MITRE from real-world reporting.

* **Tactics** are the adversary's goals — the *why* of a step (Initial Access, Persistence, Privilege Escalation, Exfiltration, and so on). They are the columns of the matrix.
* **Techniques** are the *how* — the concrete methods used to achieve a tactic (e.g. *Phishing* for Initial Access, *Valid Accounts* for Persistence). Sub-techniques refine these further.
* Techniques carry IDs (like `T1566` for Phishing), documented procedures, detections, and mitigations.

Matrices exist for Enterprise, Mobile, and ICS. The value is a shared vocabulary: a detection engineer, a red team, and a threat-intel analyst can all point at the same technique ID and mean the same thing.

## The Diamond Model

Introduced by Caltagirone, Pendergast, and Betz (2013), the Diamond Model frames a single malicious event around four connected vertices:

* **Adversary** — who is behind it.
* **Capability** — the tools and techniques they used (malware, exploits, TTPs).
* **Infrastructure** — the physical/logical resources used to deliver capability (C2 servers, domains, email accounts).
* **Victim** — the target (organization, host, person, data).

The four are linked by edges, and the core analytic move is the **pivot**: knowing one vertex lets you discover another. A malware sample (capability) resolves to a C2 domain (infrastructure), which points to other victims, which narrows the adversary. The model also carries meta-features (timestamp, kill-chain phase, result) and two axes — a *social-political* axis (adversary intent, victim relationship) and a *technology* axis (how capability and infrastructure connect).

## How they differ

| | MITRE ATT&CK | Diamond Model |
|---|---|---|
| Type | Knowledge base / taxonomy | Analytic model |
| Scope | All observed adversary behavior | One intrusion event |
| Question | *What do attackers do, and how?* | *What do we know about this attack, and where do we pivot next?* |
| Output | Technique IDs, detections, mitigations | A structured event you can correlate and hunt from |

## How they compose

They are complementary, not competing. In practice they layer with the **Cyber Kill Chain** (the phased view of an attack's lifecycle) to form the common triad of intrusion analysis:

* The **Kill Chain** tells you *what phase* the attack is in.
* **ATT&CK** tells you *which technique* was used in that phase.
* The **Diamond Model** organizes *the specific event* — mapping the observed technique to the capability vertex, the C2 to infrastructure — and drives the pivot to the next lead.

A Diamond event's capability is often just an ATT&CK technique ID. That is the seam where the two click together: the analytic model borrows its capability vocabulary from the knowledge base.

## References

* [MITRE ATT&CK](https://attack.mitre.org/)
* [The Diamond Model of Intrusion Analysis (EC-Council)](https://www.eccouncil.org/cybersecurity-exchange/ethical-hacking/diamond-model-intrusion-analysis/)
* [Caltagirone, Pendergast & Betz (2013), *The Diamond Model of Intrusion Analysis* (original paper)](https://www.activeresponse.org/wp-content/uploads/2013/07/diamond.pdf)
