---
title: "The API Mandate Was Never About APIs"
description: "Conway's Law and the shape of systems"
date: 2026-07-31
featured: false
draft: false
toc: true
tags:
  - system-architecture
  - systems-thinking
  - ai
---

## A memo nobody can produce

The most influential engineering memo of the last quarter-century may not exist as a document. The "API Mandate," attributed to Jeff Bezos around 2002, has no confirmed primary source. There is no email, no archived directive, no signed page. What survives is a description, written years later by someone who was not in the room.

That someone was Steve Yegge. In October 2011, while at Google, he wrote an internal rant about platforms and accidentally posted it in public.[\[1\]](/post/system-architecture/the-api-mandate-conways-law/#references) Buried in it was the now-famous account of Bezos's directive, offered with the hedge that it was "back around 2002 I think, plus or minus a year." Paraphrased, the mandate ran:

- All teams will expose their data and functionality through service interfaces.
- Teams must communicate with each other only through those interfaces.
- There is no other permitted form of interprocess communication. No direct linking, no direct reads of another team's data store, no shared memory, no back doors.
- The technology does not matter. HTTP, an RPC framework, a custom protocol. Any of them.
- Every interface must be designed to be externalizable from the start, ready to be exposed to outside developers.
- Anyone who does not do this will be fired.

Amazon has never confirmed the memo. It has never denied it either. The last line, the firing threat, is best read as Yegge's punchline rather than a quoted sentence. But the shape of the thing is real, and the shape is what matters.

## The clauses are about communication, not code

Read the six points again and notice what they never mention: better software, faster code, cleaner abstractions. Every clause is about *who is allowed to talk to whom, and how*. The mandate does not describe a technology. It describes a communication structure between teams.

That is the insight worth extracting. The API Mandate is an organizational intervention wearing a technical costume.

## Conway's Law, and why the mandate is a lever on it

In 1968, Melvin Conway published an observation that has aged into a law:[\[2\]](/post/system-architecture/the-api-mandate-conways-law/#references)

> Any organization that designs a system will produce a design whose structure is a copy of the organization's communication structure.

The usual reading is fatalistic. Ship the org chart. Four teams building a compiler will build a four-pass compiler, whether or not four passes is the right design. The structure of the software mirrors the structure of the conversations that produced it.

Conway's Law is normally cited as a warning. The API Mandate reads it the other way around, as a control surface. If system structure inevitably mirrors communication structure, then you do not fix the system by editing the system. You fix it by editing how teams are permitted to communicate. Constrain every team to talk only through a defined, externalizable interface. The architecture that falls out is then unavoidable: a set of loosely coupled services with clean contracts. The good architecture is not mandated directly. It is made inevitable by the communication rule.

This is sometimes called the Inverse Conway Maneuver: shape the teams and their allowed channels so the desired architecture becomes the path of least resistance. The mandate is that maneuver, issued a decade before it had a name.

## The systems-thinking reading

The frame goes deeper than org design.

A system is not a pile of components. It is components plus the relationships between them. The behavior you care about, the health or the dysfunction, usually lives in the connections rather than the parts. Change a relationship and the whole thing shifts.

The API Mandate is a statement about connections, not parts. It leaves each team free to build its part however it likes. Any language, any datastore, any internal design. What it fixes, ruthlessly, is the *coupling* between parts. No shared memory. No reaching into a neighbor's database. One kind of connection only: a call across a defined interface.

That distinction is the leverage point. Weak interventions operate at the level of events and numbers: a code-review policy, a style guide, a quota. Strong interventions operate at the level of structure and the rules of interaction. The mandate ignores the parts entirely and legislates the structure of the connections. It is a textbook high-leverage move, aimed at the layer where a small rule reorganizes everything downstream.

It also explains a failure mode of monoliths in systems terms. A monolith is a system whose connections are invisible and unconstrained. Any module can reach into any other. The coupling is total and untracked, so a change anywhere can surface as a break anywhere else. Teams step on each other because nothing marks where one team's responsibility ends and another's begins. The mandate makes every connection explicit, typed, and crossable only through the front door. It converts hidden coupling into declared contracts. Feedback loops that used to travel through a shared database now travel through an interface you can see, version, and reason about.

## Why it paid off

The benefits followed from the structure, not from any individual API.

Making teams interoperate only through documented interfaces removed whole classes of problem at once. Duplicated work became visible, because a capability was a callable service rather than a private implementation copied five times. Data hoarding eased, because access meant calling an interface rather than owning the table. Silos, both technical and human, thinned out, because the interface was the contract and the contract was the entire relationship.

The most consequential side effect was accidental optionality. The rule to design every interface as *externalizable* was framed as internal discipline. It meant that when Amazon later chose to sell storage and compute to the outside world, the interfaces were already built for strangers. Simple Storage Service reportedly grew from a handful of services at launch into a fleet of more than two hundred distributed microservices, all connected through APIs.[\[3\]](/post/system-architecture/the-api-mandate-conways-law/#references) The externalizable interface was the seam that made that growth survivable, and the seam that made a cloud business possible at all.

## The mandate in the age of agents

The frame becomes sharper, not dated, in the era of large language models, agents, and the Model Context Protocol.

An LLM agent built in isolation has exactly the problem the monolith had. It works inside its own chat window and nowhere else. A second program that wants the same capability has to reimplement it or scrape its output. There is no interface. It is a back door, or it is nothing.[\[4\]](/post/system-architecture/notes-on-mcp/#references)

MCP is the API Mandate applied to the boundary between a model and a capability. Its central rule is the mandate's central rule, rephrased for a new decade: expose functionality through a defined interface, and let other software reach it only through that interface. A capability behind an MCP tool becomes reusable, deterministic to invoke, and composable, for the same reason a service behind an Amazon API did. The connection is standardized, so the thing behind it stops being a private trick and becomes a shared building block.

Conway's Law has not gone anywhere either. A team that bolts a model into one application ships that org boundary. It gets an agent that lives in one product and dies there. A team that defines the capability as an externalizable interface first, and treats the model as one consumer among many, gets something other systems can build on. The choice between those two outcomes is not primarily a modeling choice. It is the same communication-structure choice Amazon forced in 2002.

The externalizable-first instinct is the durable lesson. It cost discipline up front and bought a cloud. Applied now, at the seam between models and the tools they call, it is the difference between an agent that is a demo and a capability that is infrastructure.

## What the myth is really about

It hardly matters now whether the mandate was one email from Bezos, a set of directives from engineers and the CTO of the day, Al Vermeulen, or a story that grew a sharper edge with each retelling. The lore is durable because it encodes something true. Software structure follows communication structure. The reliable way to change the software is to change the rules of communication. And the interface, designed for a stranger from the very start, is the most valuable thing a system can own.

The parts will always ask to be optimized one at a time. The systems that lasted legislated the connections instead.

---

## References

1. Yegge, S. (2011). *Stevey's Google Platforms Rant.* [online] Available at: https://gist.github.com/chitchcock/1281611 [Accessed 31 Jul. 2026].
2. Conway, M.E. (1968). *How Do Committees Invent?* Datamation, 14(4), pp.28-31. [online] Available at: https://www.melconway.com/Home/Committees_Paper.html [Accessed 31 Jul. 2026].
3. Vogels, W. (2021). *Keynote.* AWS re:Invent 2021, Las Vegas, 2 December. Reported in: SiliconANGLE, *Amazon CTO Werner Vogels recalls the past to position AWS and cloud for the future.* [online] Available at: https://siliconangle.com/2021/12/02/amazon-cto-werner-vogels-reaches-recalls-past-position-aws-cloud-future/ [Accessed 31 Jul. 2026].
4. Pessoa, D. (2026). *A Socket Between a Model and a Capability: Notes on MCP.* [online] Available at: /post/system-architecture/notes-on-mcp/ [Accessed 31 Jul. 2026].
