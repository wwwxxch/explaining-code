# Code Exploration & Architecture Review Skills

Two skills for reading and evaluating codebases. Originally forked from a Cursor plugin (`how`) and split into focused, composable skills for Claude Code, Cursor, and (planned) Gemini CLI.

## Skills

### `explore-code` — understand how something works

Trace data flows, follow call chains, diagnose the origin of a bug, or build a mental model of a subsystem. Produces a structured explanation with Overview, Key Concepts, How It Works, Where Things Live, and Gotchas.

**When to use**:

- "How does message virtualization work?"
- "Walk me through what happens when a user sends a message"
- "Why is this value null by the time it reaches the UI?"
- "How is the auth middleware implemented?"

**How it works**: For simple questions, a single agent explores and explains in one pass. For complex questions, 2-4 explorer agents investigate different angles in parallel, then a synthesizer weaves their findings into one coherent explanation.

### `arch-review` — evaluate the architecture

Review a subsystem for structural problems — abstraction boundaries, data modeling, coupling, evolution readiness. Not line-level bugs or style.

**When to use**:

- "What's wrong with this architecture?"
- "Is this design appropriate for what we need?"
- "How should we refactor this?"
- "Find architectural issues in the auth layer"

**How it works**: First ensures an architectural explanation exists (invoking `explore-code` if needed). Then spawns 2-3 critic agents in parallel, each forming an independent judgment from the code. Results are categorized into Act on / Consider / Noted / Dismissed.

## Installation

### Claude Code

**Option A — install as a plugin (recommended for sharing)**

```bash
/plugin marketplace add wwwxxch/explore-and-review
/plugin install explore-and-review
```

**Option B — symlink into user skills (recommended for local development)**

```bash
git clone <repo-url> ~/src/explore-and-review
ln -s ~/src/explore-and-review/skills/explore-code ~/.claude/skills/explore-code
ln -s ~/src/explore-and-review/skills/arch-review ~/.claude/skills/arch-review
```

Edits to the repo take effect immediately — no reinstall needed.

### Cursor

Cursor auto-loads skills from repos containing a `.cursor-plugin/plugin.json`. Follow the Cursor plugin install flow (UI or config) pointing at this repo. Skills register automatically based on each `SKILL.md` frontmatter.

### Gemini CLI

Planned — see `references/platform-tools.md` in each skill for the tool mapping that will be needed once Gemini CLI support is added.

## Structure

```text
.
├── .claude-plugin/
│   └── plugin.json              # Claude Code manifest
├── .cursor-plugin/
│   └── plugin.json              # Cursor manifest
└── skills/                      # Shared across all platforms
    ├── explore-code/
    │   ├── SKILL.md
    │   └── references/
    │       ├── explorer-prompt.md
    │       ├── explainer-prompt.md
    │       └── platform-tools.md
    └── arch-review/
        ├── SKILL.md
        └── references/
            ├── critic-prompt.md
            ├── critique-rubric.md
            └── platform-tools.md
```

Each `SKILL.md` holds the routing logic. The `references/` folder holds prompt templates for subagents, plus a `platform-tools.md` map for cross-platform tool-name equivalents.

## Platform Support

| Platform    | Status       | Notes                                                     |
| ----------- | ------------ | --------------------------------------------------------- |
| Claude Code | ✅ supported | Tool names used directly in SKILL.md                      |
| Cursor      | ✅ supported | Original format — verified with the upstream `how` plugin |
| Gemini CLI  | ⏳ planned   | `platform-tools.md` has TODO placeholders                 |

## Credits

Forked from [poteto/how](https://github.com/poteto/how) by Lauren Tan (MIT License). Modified to split the original single skill into two focused skills (`explore-code` and `arch-review`), remove hardcoded model names, and add cross-platform plugin manifests (Claude Code, Cursor, with Gemini CLI planned).

## License

MIT — see [LICENSE](LICENSE). Copyright held jointly by the original author and the fork maintainer.
