# Platform Tools Reference

This skill is written using Claude Code tool names. When running on another platform, map the tool names using the table below.

## Spawning subagents / Multi-step Logic

| Action | Claude Code | Gemini CLI | Cursor (Composer/Chat) |
|--------|-------------|------------|------------------------|
| Spawn a single subagent | `Agent` tool with `subagent_type: general-purpose` | `@generalist` | Not supported. Perform the critique directly in the current chat/composer context. |
| Spawn multiple parallel subagents | Multiple `Agent` calls in one message | Multiple `@generalist` calls in one message | Not supported. Run the critique once in a single pass instead of parallel critics. |
| Pass a prompt to the subagent | `prompt` parameter on the `Agent` tool | Text after the `@generalist` invocation | N/A |

## Invoking another skill

| Action | Claude Code | Gemini CLI | Cursor |
|--------|-------------|------------|--------|
| Invoke a sibling skill (e.g. `explore-code`) | `Skill` tool with the skill name | `activate_skill` tool with the skill name | Not supported. Refer to the sibling skill's documentation manually and perform its steps inline. |

## How to use this file

When SKILL.md says "spawn a subagent using `Agent`" or "invoke the `explore-code` skill", consult this table and substitute the correct tool or workflow for your platform.
