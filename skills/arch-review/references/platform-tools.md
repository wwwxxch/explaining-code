# Platform Tools Reference

This skill is written using Claude Code tool names. When running on another platform, map the tool names using the table below.

## Spawning subagents

| Action | Claude Code | Gemini CLI |
|--------|-------------|------------|
| Spawn a single subagent | `Agent` tool with `subagent_type: general-purpose` | TODO |
| Spawn multiple parallel subagents | Multiple `Agent` calls in one message | TODO |
| Pass a prompt to the subagent | `prompt` parameter on the `Agent` tool | TODO |

## Invoking another skill

| Action | Claude Code | Gemini CLI |
|--------|-------------|------------|
| Invoke a sibling skill (e.g. `explore-code`) | `Skill` tool with the skill name | TODO |

## How to use this file

When SKILL.md says "spawn a subagent using `Agent`" or "invoke the `explore-code` skill", consult this table and substitute the correct tool for the platform you are running on.
