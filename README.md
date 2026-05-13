# Code Exploration & Architecture Review Skills

Two skills for reading and evaluating codebases. Originally forked from a Cursor plugin (`how`) and split into focused, composable skills for Claude Code, Cursor, and Gemini CLI.

## Skills

Two skills - `explore-code` & `arch-review`

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
/plugin marketplace add wwwxxch/explaining-code
/plugin install explaining-code
```

**Option B — symlink into user skills (recommended for local development)**

```bash
git clone <repo-url> ~/src/explaining-code
ln -s ~/src/explaining-code/skills/explore-code ~/.claude/skills/explore-code
ln -s ~/src/explaining-code/skills/arch-review ~/.claude/skills/arch-review
```

Edits to the repo take effect immediately — no reinstall needed.

### Cursor

Cursor auto-loads skills from repos containing a `.cursor-plugin/plugin.json`. Follow the Cursor plugin install flow (UI or config) pointing at this repo. Skills register automatically based on each `SKILL.md` frontmatter.

### Gemini CLI

Install as an extension from the repo:

```bash
gemini extensions install wwwxxch/explaining-code
```

Skills auto-load from the extension's `skills/` directory on next session start. Gemini CLI reads the same `SKILL.md` format as Claude Code; tool-name differences (e.g. `@generalist` instead of `Agent`, `read_file` instead of `Read`) are handled via each skill's `references/platform-tools.md`. The `explore-code` skill automatically prefers Gemini's built-in `codebase_investigator` subagent for the explorer role.

## Structure

```text
.
├── .claude-plugin/
│   └── plugin.json              # Claude Code manifest
├── .cursor-plugin/
│   └── plugin.json              # Cursor manifest
├── gemini-extension.json        # Gemini CLI manifest
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

| Platform    | Status       | Notes                                                                                                                          |
| ----------- | ------------ | ------------------------------------------------------------------------------------------------------------------------------ |
| Claude Code | ✅ supported | Tool names used directly in SKILL.md. Supports parallel subagents.                                                             |
| Cursor      | ✅ supported | Use **Composer (Agent mode)**. Does not support parallel subagents; follow steps sequentially and use `@Codebase` for context. |
| Gemini CLI  | ✅ supported | Uses built-in `generalist` / `codebase_investigator` subagents; handles tool mapping via `platform-tools.md`.                  |

## Credits

Forked from [poteto/how](https://github.com/poteto/how) by Lauren Tan (MIT License). Modified to split the original single skill into two focused skills (`explore-code` and `arch-review`), remove hardcoded model names, and add cross-platform plugin manifests for Claude Code, Cursor, and Gemini CLI.

## License

MIT — see [LICENSE](LICENSE). Copyright held jointly by the original author and the fork maintainer.
