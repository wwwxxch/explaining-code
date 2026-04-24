---
name: explore-code
description: Explore the codebase to understand how something works — trace data flows, follow call chains, diagnose the origin of a bug, build a mental model of a subsystem. Use when asked "how does X work?", "walk me through X", "how is X implemented?", "trace this bug", or "where does this data come from?".
---

# Explore Code

Explore the codebase to answer "how does X work?" questions or diagnostic questions like "where does this data come from?" and "why is this happening?". Produce clear explanations at the level of a senior engineer onboarding onto a subsystem — enough to build a working mental model, not so much that it reads like annotated source code.

## Step 1 — Understand the Question and Assess Complexity

Parse what the user is asking about. They might say:

- "How does message virtualization work?" — a subsystem
- "How do we handle billing for on-demand usage?" — a feature flow
- "How is the auth service structured?" — an architectural overview
- "Walk me through what happens when a user sends a message" — a runtime trace
- "Why is this field null when it reaches the UI?" — a diagnostic trace

Identify the scope. If it's ambiguous, make your best guess and state your interpretation before exploring. Don't ask — explore and let the user redirect if you're off.

**Assess complexity to decide the approach:**

- **Simple** (a single module, a small utility, a narrow question like "how does function X work"): Skip parallel exploration entirely. A single agent explores and explains in one pass. Go to Step 2b.
- **Complex** (a subsystem spanning multiple files/services, a cross-cutting feature, a full architectural overview, a data flow that crosses several layers): Spawn parallel explorer agents first, then hand off to the explainer. Go to Step 2a.

When in doubt, lean toward the simple path — you can always spawn explorers if the single agent hits a wall.

## Step 2a — Explore (complex questions only)

Decompose the question into 2-4 parallel exploration angles. Each angle should cover a distinct slice so the explorers aren't duplicating work. For example, for "how does message virtualization work?" you might split into:

- Explorer 1: the data model and state management
- Explorer 2: the rendering pipeline and DOM interaction
- Explorer 3: the scroll/measurement infrastructure

The right decomposition depends on the question — use your judgment. For narrow questions, 2 explorers is fine. For broad subsystems, use up to 4.

Spawn all explorers in a single message using the `Agent` tool with:
- `subagent_type`: `general-purpose`
- `description`: short label for this exploration angle
- `prompt`: the template from `references/explorer-prompt.md`, filled in with the question and this explorer's specific angle

Each explorer should:
- Start broad: Glob for relevant directories, Grep for key types/interfaces/class names
- Follow the thread: once an entry point is found, trace the call chain — callers, callees, data flow, type definitions
- Read the actual code, don't guess from file names
- Stop when it can describe the full path from input to output without hand-waving
- Note anything surprising, non-obvious, or easy to misunderstand

Each explorer returns structured findings: components found, flow traced, files read, non-obvious points. Overlap is fine — the synthesizer will reconcile.

Then proceed to Step 3.

## Step 2b — Direct Explore + Explain (simple questions)

Spawn a single `Agent` subagent that explores and explains in one pass:
- `subagent_type`: `general-purpose`
- `prompt`: the template from `references/explainer-prompt.md`, adapted to note that no pre-gathered findings are provided — the agent must do its own exploration (Glob, Grep, Read) before writing the explanation

Proceed to Step 4.

## Step 3 — Synthesize (complex questions only)

Once all explorers have returned, spawn a single `Agent` subagent to synthesize their findings into one coherent explanation:
- `subagent_type`: `general-purpose`
- `prompt`: the template from `references/explainer-prompt.md`, filled in with the original question and all explorer findings

The explainer reconciles overlapping findings, resolves contradictions by checking the code directly, and weaves the separate slices into a unified picture.

## Step 4 — Present

Take the explainer's output and present it to the user. You may lightly edit for clarity or add context from the conversation, but don't substantially rewrite — the explainer agent's writing is the product.

## Output Format

The explanation follows this structure, but adapt it to the question. Not every section applies every time.

**Overview** — 1-2 paragraphs. What is this thing, what does it do, why does it exist. A reader should be able to decide from this alone whether to keep reading.

**Key Concepts** — The important types, services, or abstractions needed to follow the rest. Brief definitions, not exhaustive.

**How It Works** — The core of the explanation. Walk through the flow step by step: what triggers it, what happens, where data goes, what the decision points are. Use prose, not pseudocode. Reference specific files and functions. Include a mermaid or ASCII diagram when it clarifies flow across multiple components.

**Where Things Live** — A brief map of the relevant files/directories. Just what a reader needs to start working in this area.

**Gotchas** — Non-obvious behavior, historical context, sharp edges. Skip if nothing to call out.

## Platform Notes

This SKILL.md uses Claude Code tool names (`Agent`, `Glob`, `Grep`, `Read`). See `references/platform-tools.md` for equivalents on other platforms.
