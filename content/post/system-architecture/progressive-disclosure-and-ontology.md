---
title: "Two Papers That Changed How I Organize My Notes"
date: 2026-07-25
featured: false
draft: false
toc: true
tags:
  - system-architecture
  - information-architecture
  - knowledge-management
  - reflections
---

## Where this started

I keep a personal knowledge base (Evernote, Obsidian). It began as a folder of markdown files and, like these things always do, it grew: a master index, then per-topic indexes, then cross-links between notes, then indexes *of* the indexes. At some point I realized I had built an elaborate table of contents that I never actually read. When I wanted something, I didn't browse the index — I already half-knew where it lived, and I went straight there.

That nagging feeling — that most of my organizing effort was for a reader who never showed up — is what made two papers land for me recently. One is about generating ontologies with language models; the other is about how an LLM agent should read a wiki. Read together, they quietly rearranged my thinking about what a knowledge base is *for*.

## What I read

The first paper asks whether a language model can build a formal ontology — the typed, structured backbone that says "this is a concept, this is a property, these things relate this way."[\[1\]](/post/system-architecture/progressive-disclosure-and-ontology/#references) The honest answer the authors land on is: sort of. Guided carefully — one question at a time, with reusable design patterns injected — a good model can match or beat a *novice* engineer. But it reliably fumbles the hard parts: the precise axioms, the domains and ranges, the constraints. It's an assistant that drafts, not a machine that decides. That framing stuck with me more than any benchmark number.

The second paper is the one I keep returning to.[\[2\]](/post/system-architecture/progressive-disclosure-and-ontology/#references) It's a careful, preregistered experiment: take a real 709-page wiki, keep the page *content* byte-identical, and change only *how a reader reaches it*. Version one hands over the whole fat index. The final version hands over a small, keyword-ranked set of one-line summaries and lets the reader pull the full page only when it decides the page is worth it. That progression — from "here's everything" to "here's a ranked hint, ask for more" — is what people call **progressive disclosure**.

The measured result was a **30–58% drop in cost** with quality that held up. But the finding that actually reframed things for me wasn't the percentage. It was the mechanism the author names directly, which he calls the efficiency paradox: *a capable agent never loads the big index anyway.* It infers where the page probably is and reads it. So the elaborate index was solving a problem the competent reader didn't have. The real win came from making each individual fetch more targeted — citing fewer pages, taking fewer detours.

I sat with that for a while, because it described my own behavior in my own notes exactly. I *am* the agent that skips the index.

## What I decided to change

None of this told me to go build something bigger. If anything it told me to build *less*, and to be more disciplined about the small things. A few decisions came out of it.

**The summary is the interface, not the index.** The thing a reader — me, six months later, or a model I've pointed at my notes — actually routes on is a single honest line describing what a note contains. So that's where the care goes now. Every note earns one summary line. The sprawling index files can stay, but I've stopped treating them as the machine's front door. They're for browsing and for the graph view, which is a human pleasure, not a retrieval mechanism.

**A convention beats a cathedral.** The ontology paper gave me permission to stop over-thinking structure. I don't need a formal reasoner or rigorous axioms for a pile of personal notes. I need a *convention*: a small, consistent vocabulary — namespaced keys, a clear line between "this points to a full document" and "this is a standalone idea," a stable tag set. That thin backbone is what keeps the summaries coherent enough to rank. It's cheap, and it's the part a language model can genuinely help me draft and normalize.

**Tags for breadth; full-text for recall; embeddings only if it hurts.** The one thing progressive disclosure is bad at is the *exploratory* question — "show me everything I've ever written that touches this idea." Jumping straight to the known page doesn't help there. I was tempted to reach for a search cluster, and then I remembered the size of what I actually have: a few dozen notes. Standing up a whole search service for that is the opposite of the lesson. So the plan is boring and correct: fix my tags first so a broad question becomes a tag filter, add a small local full-text index that returns ranked summaries, and only consider fancy semantic search if I ever feel real pain from synonyms. The ranking rule I like — trust the summary most, then the tags, then the raw body — is really just progressive disclosure expressed as a weighting.

## The caveat I want to keep honest

It would be easy to quote "58% cheaper" as if it were a law. It isn't. Both papers rest on thin evidence — one of them on a single model, a single corpus, and a single rater who was also the author, with its own reliability check coming up short. The mechanisms are convincing; the exact numbers are not gospel. So I'm adopting the *shape* of the idea — summaries as the interface, a thin ontology as the backbone, ranked disclosure over bulk dumps — and holding the figures loosely.

What I'm left with is smaller and calmer than the system I had been drifting toward. Less index, more summary. Less structure, but more consistent structure. A knowledge base that assumes its reader is capable and impatient — because, when I'm the reader, I am.

---

## References

1. Lippolis, A.S., Saeedizade, M.J., Keskisärkkä, R., Zuppiroli, S., Ceriani, M., Gangemi, A., Blomqvist, E. and Nuzzolese, A.G. (2025). *Ontology Generation using Large Language Models.* arXiv:2503.05388 [cs.AI]. Available at: https://arxiv.org/abs/2503.05388 [Accessed 25 Jul. 2026].
2. Cochran, T.O. (2026). *Progressive Disclosure for LLM-Maintained Wiki Knowledge Bases: a Preregistered Ablation.* arXiv:2607.04576 [cs.CL]. Available at: https://arxiv.org/abs/2607.04576 [Accessed 25 Jul. 2026].
