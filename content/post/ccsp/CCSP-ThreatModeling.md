---
title: "Threat Modeling: Four Questions and a Model That Is Never the Thing"
date: 2024-09-07T17:05:20+01:00
draft: true
toc: true
tags:
  - ccsp
  - security
  - threat-modeling
---

*These notes were written while studying for the CCSP, alongside OWASP material and the AWS Skill Builder course Threat Modeling for Builders (2025). They are kept provider-neutral. The deeper look at STRIDE and risk-rating frameworks is in [STRIDE, LINDDUN and Risk Rating](/post/ccsp/ccsp-threatmodeling-stride-and-frameworks/).*

## What threat modeling is

> Threat modeling works to identify, communicate, and understand threats and mitigations within the context of protecting something of value.
> — OWASP[\[1\]](#references)

Three verbs carry the definition. *Identify* is the analysis. *Communicate* is the part people skip: a threat model is a shared artifact, not one engineer's private worry list. *Understand* means the team knows why each mitigation exists. And all of it is anchored to "something of value". With no asset worth protecting, there is nothing to model.

OWASP lists what a threat model typically holds:

- A description of the subject being modeled.
- Assumptions that can be checked or challenged later as the threat landscape changes.
- Potential threats to the system.
- Actions that can be taken to mitigate each threat.
- A way to validate the model and to verify that the actions worked.

## A model is not the system

A system model is a representation of a system. Teams use models in design and maintenance so everyone understands how the parts work together to produce the intended value.

If a model were 100% accurate, it would no longer be a model. It would be the thing itself.

That sentence is a permission slip. The goal is not a perfect diagram. The goal is a model just detailed enough to reason about what can go wrong. A data flow diagram with components, flows and trust boundaries is usually enough to start.

## Shostack's Four Question Frame

Adam Shostack's frame[\[2\]](#references) reduces the whole practice to four questions:

1. **What are we working on?**
2. **What can go wrong?**
3. **What are we going to do about it?**
4. **Did we do a good job?**

The wording has evolved on purpose. "We" replaced "you" to make it collaborative. "Working on" replaced "building", because "building" pulls people toward a waterfall mindset where the model is drawn once. "Are" replaced "should" in question 3 to push toward action.

The Threat Modeling Manifesto prefers "did we do a good *enough* job?". The point is not to be good at threat modeling. The point is to improve the system.

### 1. What are we working on?

Scope the work and model it. It can be as small as one feature in a sprint or as large as a whole system. Smaller is usually better: one asset, one entry point and two components is already a useful threat model.

### 2. What can go wrong?

List threats. This can be a free brainstorm or a structured method such as STRIDE, kill chains or attack trees. Most of the interesting threats sit on data flows that cross a trust boundary. The frameworks are compared in [STRIDE, LINDDUN and Risk Rating](/post/ccsp/ccsp-threatmodeling-stride-and-frameworks/).

### 3. What are we going to do about it?

Every threat gets a decision. The four classic risk responses:

| Response | Meaning | Example |
|---|---|---|
| Avoid | Remove the design element that creates the risk. | Drop a feature that stores data you do not need. |
| Mitigate | Add a control that lowers likelihood or impact. | Encryption, authentication, input validation, rate limits. |
| Transfer | Shift the risk to another party. | A managed provider, insurance, a contract clause. |
| Accept | Document the risk and live with it. | A low-impact threat where the fix costs more than the loss. |

Acceptance is a valid answer only when it is explicit and has an owner. An unrecorded threat is not accepted. It is ignored.

### 4. Did we do a good job?

Review the model. Are the threats complete enough? Does each mitigation actually exist, and is it tested? Would someone new to the system reach the same conclusions from the model alone?

## Rating risk

Question 3 needs an order of work, and that order comes from risk:

> Risk = Likelihood × Impact

- **Likelihood** is how likely the threat is to occur over the expected lifetime of the system.
- **Impact** is the potential cost of the damage if it does.

The OWASP Risk Rating Methodology[\[9\]](#references) turns both into scores. Each factor gets a value from 0 to 9, and the average gives the overall level.

| Estimate | Factors |
|---|---|
| Likelihood: the threat agent | Skill level, motive, opportunity, size of the group |
| Likelihood: the vulnerability | Ease of discovery, ease of exploit, awareness, intrusion detection |
| Impact: technical | Loss of confidentiality, integrity, availability, accountability |
| Impact: business | Financial damage, reputation damage, non-compliance, privacy violation |

Step 4 of the method[\[10\]](#references) maps each score to a level (0 to under 3 is low, 3 to under 6 is medium, 6 to 9 is high), then combines the two levels into a severity:

| Impact \ Likelihood | Low | Medium | High |
|---|---|---|---|
| **High** | Medium | High | Critical |
| **Medium** | Low | Medium | High |
| **Low** | Note | Low | Medium |

Three things stand out:

- **Business impact wins.** When it is known, use it instead of technical impact. OWASP's own example has a high technical impact but a low business impact, so the overall severity is low. Without the business context, security and the business stop trusting each other's ratings.
- **Fix the most severe first, but not everything.** Fixing a cheap, minor risk does little for the overall risk profile. Some loss is acceptable: spending 100,000 to stop 2,000 of fraud a year takes 50 years to pay back, unless reputation damage changes the sum.
- **The numbers are estimates.** OWASP says so itself, warns that the method is debated, and points to more established ones such as NIST SP 800-30. The value of a rating is a shared scale the team can argue over, and one scale used across every threat model. The method is meant to be customized: add factors, rename the options to fit your organization, or weight them.

## When to threat model

Threat model often. But the reason to threat model is to drive improvements, so balance how often you do it against the value you get from it.

Good times to revisit a threat model:

- When you design a new system or feature.
- When you introduce changes beyond the scope of existing threat models.
- When earlier assumptions or models are invalidated.
- When you identify a new threat to an existing system.

OWASP adds two more triggers: after a security incident, and after architectural or infrastructure changes.

## Threat modeling on a public cloud

When the system runs on a public cloud provider, the shared responsibility model sets the boundary of the threat model:

- The **cloud provider** is responsible for security **of** the cloud.
- The **customer** is responsible for security **in** the cloud.

| Layer | Owner | Examples |
|---|---|---|
| Customer data | Customer | Classification, encryption choices, retention. |
| Applications | Customer | Application code, dependencies, input validation, secrets handling. |
| Identity and access | Customer | Users, roles, RBAC (role-based access control) policies, least privilege. |
| Configuration | Customer | Network rules, storage access settings, logging and monitoring. |
| Foundation services | Provider | Compute, storage, database and networking software. |
| Hardware | Provider | Servers, storage devices, network equipment. |
| Global infrastructure | Provider | Regions, availability zones, data centers, physical security. |

### Implications for the model

1. **Scope.** The threat model covers the customer layers. Do not threat model the provider's hypervisor or data center. The provider already does that, and you cannot change it anyway.
2. **The provider is an assumption, not a blind spot.** Record what you rely on as explicit assumptions (for example "the provider isolates tenants" or "the provider encrypts disks at rest"). Back them with the provider's compliance reports (SOC 2, ISO 27001), so they can be challenged later.
3. **The line moves with the service model.** With IaaS (virtual machines) the customer owns the OS, patching and network setup. With PaaS (managed databases, containers) the provider takes the OS and runtime. With SaaS or serverless the customer keeps little more than data, identity and configuration. Pick the service model, then redraw the boundary.
4. **Most cloud threats are customer threats.** Misconfiguration and over-broad permissions cause most cloud breaches: public storage buckets, leaked access keys, wildcard IAM policies. Identity is the new perimeter, so RBAC and least privilege deserve their own trust boundary in the data flow diagram.
5. **The provider's control plane is an entry point.** The provider's management API and console can create, change or delete everything. Treat them as entry points with their own threats (stolen credentials, privilege escalation) and mitigations (MFA, short-lived credentials, audit logs).
6. **Transfer is not abdication.** Choosing a managed service transfers some risk to the provider (question 3). The data and the customer relationship stay yours. A provider outage or region failure is still your availability threat to plan for (multi-zone or multi-region design, backups).

## Tooling

Question 1 needs a diagram. Two free options:

- [draw.io](https://app.diagrams.net/)[\[11\]](#references) with a community library of threat modeling shapes[\[12\]](#references) (processes, data stores, external entities, trust boundaries).
- [PlantUML](https://plantuml.com/)[\[13\]](#references), which renders diagrams from plain text. The diagram then lives in version control next to the code and changes in the same review.

Question 2 benefits from a consistent way to write threats. [Threat Composer](https://github.com/awslabs/threat-composer#readme)[\[4\]](#references) is an open-source tool for writing threat models. It guides you to write threats in a consistent "threat grammar" (source, prerequisites, action, impact, affected assets), links them to mitigations, and keeps assumptions next to the threats they support.

For the end-to-end process, the OWASP Threat Modeling Process[\[14\]](#references) and the OWASP Threat Modeling Cheat Sheet[\[15\]](#references) are good references. The Threat Modeling Manifesto[\[5\]](#references) sums up the values behind the practice, and Microsoft's SDL pages[\[7\]](#references) show how one large organization fits threat modeling into its development lifecycle.

## Open questions for later study

- ~~How do STRIDE, PASTA and LINDDUN (privacy) compare in practice?~~ Covered in [STRIDE, LINDDUN and Risk Rating](/post/ccsp/ccsp-threatmodeling-stride-and-frameworks/) (STRIDE and LINDDUN; PASTA still to come).
- How do you keep a threat model alive after launch instead of letting it rot?
- How do you build a reusable threat catalog across teams?

## References

1. [OWASP: Threat Modeling (community page)](https://owasp.org/www-community/Threat_Modeling)
2. [Shostack's Four Question Frame for Threat Modeling](https://github.com/adamshostack/4QuestionFrame)
3. [Threat Modeling: 12 Available Methods (SEI)](https://insights.sei.cmu.edu/blog/threat-modeling-12-available-methods/)
4. [Threat Composer (GitHub)](https://github.com/awslabs/threat-composer#readme)
5. [Threat Modeling Manifesto](https://www.threatmodelingmanifesto.org/)
6. [STRIDE-per-element (Microsoft archive)](https://learn.microsoft.com/en-us/archive/blogs/larryosterman/threat-modeling-again-stride-per-element)
7. [Microsoft Threat Modeling and SDL](https://www.microsoft.com/en-us/securityengineering/sdl/threatmodeling)
8. [Pluralsight Threat Modeling path](https://app.pluralsight.com/paths/skills/threat-modeling)
9. [OWASP Risk Rating Methodology](https://community.owasp.org/OWASP_Risk_Rating_Methodology)
10. [OWASP Risk Rating Methodology, Step 4: Determining the severity of the risk](https://community.owasp.org/OWASP_Risk_Rating_Methodology#step-4-determining-the-severity-of-the-risk)
11. [draw.io](https://app.diagrams.net/)
12. [Threat modeling shape library for draw.io (GitHub)](https://github.com/michenriksen/drawio-threatmodeling)
13. [PlantUML](https://plantuml.com/)
14. [OWASP Threat Modeling Process](https://owasp.org/www-community/Threat_Modeling_Process)
15. [OWASP Threat Modeling Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Threat_Modeling_Cheat_Sheet.html)

Further study: the Pluralsight path[\[8\]](#references), and the SEI overview of other methods such as PASTA and attack trees[\[3\]](#references). The STRIDE-per-element approach[\[6\]](#references) is covered in the follow-up post.
