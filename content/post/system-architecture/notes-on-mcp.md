---
title: "A Socket Between a Model and a Capability: Notes on MCP"
date: 2026-07-31
featured: false
draft: false
toc: true
tags:
  - system-architecture
  - mcp
  - agents
  - protocol
  - llm
---

## Where this started

A language-model "agent" is easy to build and easy to trap. Mine worked fine on its own. I typed at it, it reasoned, it reached for a few tools, it answered. But it only existed inside my chat window. If a second program wanted the same capability, its options were poor. It could reimplement the whole thing. Or it could shell out to the agent and scrape the output. Neither is a real interface.

The Model Context Protocol fixes that.[\[1\]](/post/system-architecture/notes-on-mcp/#references) Not because it is clever. It is deliberately not clever. It fixes it because it draws one boundary in the right place. These notes are the attempt to understand it, rather than just use it.

## The one-sentence model

MCP is JSON-RPC 2.0[\[2\]](/post/system-architecture/notes-on-mcp/#references) with a small, fixed vocabulary. A **host** runs the model. Through its **client**, the host opens a connection to a **server**, and calls **tools** on it. That is nearly the whole idea.

Think of it as a standard connection between a model runtime and a capability. The value lives in one word: *standard*. The shape is the same everywhere. So the capability behind it gains three properties at once. It is reusable. It is deterministic to invoke. It composes with other capabilities.

Everything else is detail hanging off that.

## The three primitives

A server can expose three kinds of thing. The difference matters, because it is the part people blur.

| Primitive | What it is | Everyday analogy |
|---|---|---|
| **Tools** | Callable, typed functions. They *do* something and return a result. | a function call |
| **Resources** | Readable data addressed by a URI. | a GET request, or a file |
| **Prompts** | Parameterized prompt templates. | a mail-merge template |

The trap is the third one. A Prompt sounds like it might be "a saved agent." It is not. It is inert. It hands the model some pre-filled text and stops. It does not run a loop. It does not decide anything. It does not act. There is no "agent" primitive in MCP at all. So to expose something that *acts autonomously*, do not reach for a Prompt. Wrap it as a **Tool**. The Tool is the only primitive with agency behind it.

## Client and server, and the direction that trips everyone up

Here is the confusion worth unlearning early. An agent that *consumes* tools is an MCP **client**. It reaches out and calls things. A chat agent calling a search tool and a calendar tool is a client the whole time. It feels like the star of the show, so it is easy to assume it is the server. It is not.

To take that same capability and let *other* programs call it, you build a **server**. One rule follows, and it saves hours of confusion:

> A server cannot call its own tools.

Something else opens the connection and drives the conversation. First `initialize`, then `tools/list`, then `tools/call`. That "something else" is a client. On the wire, the last step is unremarkable. That is the point. A call and its answer look like this:

```json
// client -> server
{ "jsonrpc": "2.0", "id": 3, "method": "tools/call",
  "params": { "name": "get_weather", "arguments": { "city": "Lisbon" } } }

// server -> client
{ "jsonrpc": "2.0", "id": 3,
  "result": { "content": [ { "type": "text", "text": "22C, clear" } ] } }
```

That is the whole protocol theater. A named method. Some arguments. A matching `id` on the way back. Everything the model can do reduces to messages of that shape.

This is why a server cannot be demoed by running it alone. There is nothing to watch. It needs a caller on the other end of the wire, a real client speaking exactly that JSON.

The lesson arrived the embarrassing way. To "prove" the server worked, I imported its tool functions into a Python script and called them directly. Everything passed. It proved nothing. The test exercised the *function bodies* and bypassed the three things that were actually new: the server process, the transport, and the protocol. A real test drives the server as a black box, over the wire, the way a stranger's program would.

## The layered path of a single call

A tool call travels through more layers than the chat transcript suggests:

```
model emits a tool-call (structured JSON)
  -> host / client runtime
    -> MCP server (often a child process over a pipe)   <- auth lives HERE
      -> the real backend the tool talks to
```

The load-bearing detail is where authentication belongs. It belongs at the server layer, not in the model's head. The model sits on the client side. It asks; it never answers calls. It should never hold a credential. It asks a server, and the server is the component trusted to reach the backend. Keep the secret as far from the probabilistic part as possible.

## Turning an agent into a server: wrap or extract

There are two honest ways to expose an existing agent's capability as a tool. They trade off in opposite directions.

| | **Wrap** | **Extract** |
|---|---|---|
| How | The tool internally spins up the whole agent (LLM and all) | You reimplement the capability as a plain, deterministic function |
| Upside | Fast; reuses the existing agent verbatim | No model cost per call, testable, honest |
| Downside | Carries the subprocess along; slow; non-deterministic | More work up front |

Wrapping is the tempting shortcut. It is fine for a prototype. But it inherits everything: the latency, the cost of a model call on every invocation, and non-determinism. The same input can give different output. Extracting is more work. What comes back is a function you can unit-test and reason about. The exercise also tends to reveal something useful. The "intelligence" that felt necessary was, for this one capability, a deterministic procedure all along.

## Transport: how the two ends actually talk

Two transports cover almost everything. Choosing wrong is a common early mistake.

- **stdio** — the server is a child process the host spawns. They talk over standard input and output. It is simple and great for local development. But it binds the caller to a live session. No server sits there waiting for arbitrary clients. It is a private pipe, not a public door.
- **streamable HTTP** — the server is a real endpoint. Any client can reach it without being tied to one session. This is the right choice the moment a second consumer, or a consumer you do not control, needs the capability.

The rule of thumb: stdio for a tool a single host launches for itself, HTTP for a capability meant to be *shared*.

## A few principles worth keeping

**One tool, not a toolbox.** The strongest instinct here is also the wrong one: decompose the capability into a dozen small, "clean" tools. It looks tidy. It is a mistake. When the point is to expose *one capability*, expose one tool that wraps the whole interaction. A sprawl of granular tools enlarges the surface everyone must learn. Worse, it pushes the orchestration back out to the caller, which is the exact job the tool was meant to absorb.

**Two separate auth boundaries for a shared server.** One boundary faces downstream. The server proves it may reach the backend, using its own credential. The other faces upstream. The caller proves it may use the server, using a signed-request scheme of the kind cloud APIs use. These are different problems. Solve them separately, and do not let one leak into the other.

**The server is a seam, not a feature.** The reason to do any of this is not the protocol for its own sake. A shared server becomes the single place a capability is *defined*. Every consumer then inherits the same behavior, instead of each growing its own slightly-wrong copy. That is the payoff. It is an old idea wearing new protocol clothes: a stable interface over a single implementation.

## The caveat worth keeping honest

MCP is young. The ecosystem around it moves fast enough that some of these edges will have shifted by the time you read this. These notes also come from someone who built exactly one server, not someone who has run one in anger for a year. So take the shape of the thing and hold the details loosely. The shape is the durable part: the standard connection, the three primitives, the client-drives-server rule, the tool-not-toolbox instinct. The details are the part I expect to keep relearning.

---

## References

1. Anthropic (2024). *Introduction — Model Context Protocol.* [online] Available at: https://modelcontextprotocol.io/introduction [Accessed 31 Jul. 2026].
2. JSON-RPC Working Group (2013). *JSON-RPC 2.0 Specification.* [online] Available at: https://www.jsonrpc.org/specification [Accessed 31 Jul. 2026].
