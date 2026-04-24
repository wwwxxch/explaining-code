# Platform Tools Reference

This skill is written using Claude Code tool names. When running on another platform, map the tool names using the table below.

## Spawning subagents / Multi-step Logic

| Action | Claude Code | Gemini CLI | Cursor (Composer/Chat) |
|--------|-------------|------------|------------------------|
| Spawn a single subagent | `Agent` tool with `subagent_type: general-purpose` | `@generalist` or `@codebase_investigator` | Not supported. Perform the exploration directly in the current chat/composer context. |
| Spawn multiple parallel subagents | Multiple `Agent` calls in one message | Multiple `@<subagent>` calls in one message | Not supported. Iterate through subsystems sequentially or use `@Codebase` to gather broad context. |
| Pass a prompt to the subagent | `prompt` parameter on the `Agent` tool | Text after the `@<subagent>` invocation | N/A |

**Gemini CLI note — prefer `codebase_investigator` for explorers.**
Gemini CLI ships with a built-in `codebase_investigator` subagent purpose-built for exploring codebases, mapping architecture, and tracing bug root causes. When this skill calls for explorer subagents (Step 2a), prefer `@codebase_investigator` over `@generalist` — it is scoped and tuned for exactly this work. Use `@generalist` for the synthesizer in Step 3 and for the single-pass path in Step 2b.

**Cursor note.**
Since Cursor doesn't support spawning independent agents, use **Composer (Cmd+I)** with "Agent" mode. Instead of parallel explorers, give the Composer the list of subsystems to explore and let it iterate through them. Use `@Codebase` for initial indexing.

## File and code exploration

| Action | Claude Code | Gemini CLI | Cursor |
|--------|-------------|------------|--------|
| Read file | `Read` | `read_file` | Automatic or `@filename` |
| Search file contents (regex) | `Grep` | `search_file_content` | `@Codebase` or global search |
| Find files by name/pattern | `Glob` | `glob` | `@Codebase` or File search |

## How to use this file

When SKILL.md says "spawn a subagent using `Agent`", or mentions `Glob` / `Grep` / `Read`, consult this table and substitute the correct tool or workflow for your platform.
