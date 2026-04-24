# Platform Tools Reference

This skill is written using Claude Code tool names. When running on another platform, map the tool names using the table below.

## Spawning subagents

| Action | Claude Code | Gemini CLI |
|--------|-------------|------------|
| Spawn a single subagent | `Agent` tool with `subagent_type: general-purpose` | TODO |
| Spawn multiple parallel subagents | Multiple `Agent` calls in one message | TODO |
| Pass a prompt to the subagent | `prompt` parameter on the `Agent` tool | TODO |

## File and code exploration

| Action | Claude Code | Gemini CLI |
|--------|-------------|------------|
| Read file | `Read` | TODO |
| Search file contents (regex) | `Grep` | TODO |
| Find files by name/pattern | `Glob` | TODO |

## How to use this file

When SKILL.md says "spawn a subagent using `Agent`", or mentions `Glob` / `Grep` / `Read`, consult this table and substitute the correct tool for the platform you are running on.
