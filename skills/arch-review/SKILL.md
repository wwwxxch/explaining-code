---
name: arch-review
description: Review the architecture of a subsystem — identify structural problems, evaluate whether the current design fits the use case, find issues that will block future work. Use when asked "what's wrong with this architecture?", "is this design appropriate?", "how should we refactor this?", "find architectural issues", or "critique this subsystem".
---

# Architecture Review

Review a codebase subsystem for architectural problems — not line-level bugs or style. Structural concerns: abstraction boundaries, data modeling, coupling, how the design will evolve.

## Before You Begin — Platform Tools

This SKILL.md references Claude Code tool names (`Agent`, `Skill`). Before spawning any subagent or invoking the sibling `explore-code` skill, open `references/platform-tools.md` and substitute the equivalents for the platform you are running on.

## Step 1 — Ensure Architectural Context Exists

Before critiquing, an architectural explanation of the subsystem is required. Critics without context produce shallow or wrong findings.

Check the current conversation:

- **Context exists** — the user already provided an architectural explanation, or a previous turn produced one (e.g., `explore-code` was just run). Reuse it and skip to Step 2.
- **Context missing** — invoke the `explore-code` skill first to produce the explanation. Use its output as input to Step 2.

Err on the side of running `explore-code`. A reused explanation from conversation history is fine only if it covers the specific subsystem being reviewed.

## Step 2 — Spawn Critics

Spawn multiple critic agents in parallel. Each reads the architectural explanation, then forms its own judgment from the actual code.

Launch 2-3 critics in a single message using the `Agent` tool. For each:
- `subagent_type`: `general-purpose`
- `description`: e.g., "Architectural critic A"
- `prompt`: the template from `references/critic-prompt.md`, filled in with:
  1. The architectural explanation from Step 1
  2. The relevant file paths
  3. The critique rubric from `references/critique-rubric.md`

Running them in parallel surfaces different angles. Each critic returns structured findings with severity (`structural` | `concern` | `observation`), evidence, and impact.

## Step 3 — Lead Judgment

You are a pragmatic lead, not an aggregator. Read the critic findings and categorize:

- **Act on** — Architectural problems worth fixing now
- **Consider** — Real concerns, but the cost/benefit is unclear
- **Noted** — Valid observations, low priority
- **Dismissed** — Wrong, missing context, or style preference

Don't parrot every critic finding. Filter for what actually matters in this codebase's context.

## Step 4 — Present

Present in this order:

1. The architectural explanation (from Step 1) — stands on its own so a reader who just wants to understand the system doesn't have to read the critique
2. The critique verdict, organized by the four categories above
