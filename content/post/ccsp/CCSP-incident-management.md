---
title: "CCSP Incident Management"
date: 2024-11-18T23:34:05Z
description: "Article description."
draft: true
featured: false
toc: false

usePageBundles: false
featureImage: "/images/path/file.jpg"
featureImageAlt: 'Description of image'
featureImageCap: 'This is the featured image.'
thumbnail: "/images/path/thumbnail.png"
shareImage: "/images/path/share.png"
codeMaxLines: 10
codeLineNumbers: false
figurePositionShow: true
categories:
  - Technology
tags:
  - Tag_name1
  - Tag_name2

---


The incident manager is in the second phase, detection and analysis.

The Cloud Security Alliance (CSA) guidance 4.0 document (watch for v5 as of May 2023) breaks down the phases in the
following manner:

Preparation: Establishing an incident response capability so that the organization is ready to respond to incidents.

Process to handle the incidents
Handler communications and facilities
Incident analysis hardware and software
Internal documentation (port lists, asset lists, network diagrams, current baselines of network traffic)
Identifying training
Evaluating infrastructure by proactive scanning and network monitoring, vulnerability assessments, and performing risk
assessments
Subscribing to third-party threat intelligence services
Detection and Analysis: This is really when managing a real risk begins. Preparation is getting ready. Detection is when
it has happened, and we discover it in someway. The analysis is often stated as triage. It is necessary to determine
what is happening and what will be handled by the team first.

Alerts (endpoint protection, network security monitoring, host monitoring, account creation, privilege escalation, other
indicators of compromise, SIEM, security analytics for baseline and anomaly detection, and user behavior analytics)
Validate alerts (reducing false positives) and escalation
Estimate the scope of the incident
Assign an incident manager who will coordinate further actions
Designate a person who will communicate the incident containment and recovery status to senior management
Build a timeline of the attack
Determine the extent of the potential data loss
Notification and coordination activities
Containment, Eradication, and Recovery: We often want to start the containment as soon as we detect it. We need to
analyze it to be able to determine the containment step. This could occur minutes, hours, or days after the incident
happens. Once the attack is contained, it is necessary to clean and remove any remnants of the attack (e.g., Is there a
virus on a system that needs to be removed?). Otherwise, it is necessary to return our environments to a normal
condition. It could be that we also need to change a control, alter a configuration, add a new control somewhere, etc.
to ensure this does not happen again.

Containment: Taking systems offline. Considerations for data loss versus service availability. Ensuring systems don’t
destroy themselves upon detection
Eradication and Recovery: Clean up compromised devices and restore systems to normal operation. Confirm systems are
functioning properly. Deploy controls to prevent similar incidents
Documenting the incident and gathering evidence ( chain of custody)
Post-mortem: Now that things are back to normal, what could have been handled differently that would have made things
better? This is a meeting to work together to get better. It is not a finger-pointing exercise. Own what we did right
and what we did wrong.

What could have been done better? Could the attack have been detected sooner? What additional data would have been
helpful to isolate the attack faster?
Does the IR process need to change? If so, how?
Governance is the oversight provided by the Board of Directors and the C-suite, encompassing corporate governance,
security governance, and data governance.

Privacy is a critical topic. There are so many regulations being created or updated to force companies to take the act
of protecting their customers' and employees' personal information.

Neither governance nor privacy is directly part of the incident response. They are not phases. Depending on how you look
at things, they are connected but not directly.

## Reference:

* (ISC)² CCSP Certified Cloud Security Professional Official Study Guide, 3rd Edition. Pg 209, 229.
* The Official (ISC)² CCSP CBK Reference, 4th Edition. Pg 249-251.