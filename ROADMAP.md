# Learning Roadmap: AI Agents / Agentic AI

## Why

This repo currently covers chatbot/LLM **evaluation** (Promptfoo against
Gemini) — single-turn, multi-turn, and red-team testing. It does not yet
cover **agents**: LLMs that plan, call tools, and take multi-step or
multi-agent action. A number of target job postings list "Experience with
AI Agents / Agentic AI, including LangChain, LangGraph, AutoGen, or CrewAI"
as a requirement, which this project can't honestly back yet.

This roadmap closes that gap one framework at a time. Each milestone follows
the same shape as the rest of this repo: build something small, then write a
Promptfoo eval config against it — so every framework produces both a working
skill and a comparable, resume-able test artifact, not just a tutorial
notebook.

## Sequencing

Order chosen for conceptual buildup, not alphabetical:

1. **LangChain** — foundational primitives (tools, chains, memory, agent
   executors). Everything below builds on similar concepts.
2. **LangGraph** — same ecosystem as LangChain, but graph/state-machine
   control flow for more complex, controllable agent loops (retries,
   branching, human-in-the-loop).
3. **CrewAI** — role-based multi-agent orchestration. Higher-level and more
   opinionated than LangGraph; an easier on-ramp into multi-agent systems
   than AutoGen.
4. **AutoGen** — multi-agent *conversational* orchestration (Microsoft).
   The most different mental model (agents literally converse with each
   other), best tackled last once single-agent and role-based multi-agent
   concepts are already familiar.

## Milestones

### 1. LangChain

**Learn:**
- Tools and tool-calling (binding functions the LLM can invoke)
- Agent executors and the reasoning loop (plan → act → observe)
- Memory (conversation buffer/summary) for stateful agents

**Project:** A support-bot agent that can call a mock `lookup_order_status(order_id)`
tool instead of guessing — extends the existing `support-bot` theme from
single-turn Q&A into actual tool use.

**Eval it:** `promptfooconfig.langchain.yaml` — assert the agent calls the
tool (not fabricates an answer) when asked about an order, and handles a
missing/invalid order ID gracefully.

### 2. LangGraph

**Learn:**
- Graph/state-machine modeling of agent control flow (nodes, edges, state)
- Conditional branching and loops (e.g., retry-on-failure, escalation paths)
- Persistence/checkpointing for resumable agent runs

**Project:** A LangGraph version of the support agent with an explicit
escalation loop: try to resolve the user's issue up to N times, then route
to a "human handoff" terminal node if unresolved.

**Eval it:** `promptfooconfig.langgraph.yaml` — assert the escalation path
triggers correctly after repeated failed resolution attempts, and that the
agent doesn't loop indefinitely.

### 3. CrewAI

**Learn:**
- Role/goal/backstory-based agent definitions
- Task delegation and crew orchestration (sequential vs. hierarchical process)
- Inter-agent handoffs and shared context

**Project:** A two-agent crew — a "support agent" that triages the request
and a "billing agent" that handles pricing/invoice questions — splitting
responsibilities the single-turn support-bot currently handles alone.

**Eval it:** `promptfooconfig.crewai.yaml` — assert billing-flavored
questions get routed to (or answered with input from) the billing agent, and
general questions are handled by the support agent directly.

### 4. AutoGen

**Learn:**
- Conversable agents and multi-agent conversation patterns
- Group chat / manager-orchestrated multi-agent flows
- Human-in-the-loop and termination conditions

**Project:** A two-agent conversation where a "customer" agent (simulating a
frustrated or ambiguous user) converses with a "support" agent, testing how
the support agent handles multi-turn back-and-forth driven by another LLM
rather than a scripted test case.

**Eval it:** `promptfooconfig.autogen.yaml` — assert the conversation
terminates within a bounded number of turns and the support agent stays
on-policy (no fabricated pricing, no reveal of system instructions) across
the full exchange, reusing the same policy assertions as
`promptfooconfig.redteam.yaml`.

## Suggested layout (for when scaffolding begins)

```
agents/
  langchain/   # + its own README
  langgraph/
  crewai/
  autogen/
promptfooconfig.langchain.yaml
promptfooconfig.langgraph.yaml
promptfooconfig.crewai.yaml
promptfooconfig.autogen.yaml
```

Follows the existing `promptfooconfig.multiturn.yaml` /
`promptfooconfig.redteam.yaml` naming convention already in this repo.

## Status

- [ ] LangChain
- [ ] LangGraph
- [ ] CrewAI
- [ ] AutoGen

Update this checklist as milestones are completed.
