# TAA Development

## Language

All files and documents must be written in English.

## Project structure

```
taa/
├── CLAUDE.md             # Project governance
├── README.md             # Project identity, agent catalog
├── setup                 # Install script (symlinks agents into ~/.claude/agents/)
└── agents/
    ├── agent.md.tmpl     # Authoring starter — copy to a new agent file
    └── {name}.md         # Single-file agent definition (Claude Code native format)
    └── {name}/           # Directory form when an agent ships companion files
        ├── {name}.md     # Agent definition
        └── {companion}   # Optional: scripts, prompt fragments, fixtures used by this agent alone
```

Most agents are single `.md` files. Use the directory form only when an agent legitimately needs companion files; do not mint shared locations speculatively.

## Relationship to TAS and TAO

```
tao   — orchestrators (deferred; not built yet)
 └── taa   — agents (this repo)
      └── tas   — skills
```

- **tas** provides pure-function skills, available globally via `~/.claude/skills/`.
- **taa** provides agent definitions, available globally via `~/.claude/agents/`. Agents may invoke tas skills.
- **tao** is reserved for orchestration (multi-agent workflows). Currently deferred — no orchestrators built yet.

A taa agent must not depend on the existence of any specific tas skill — agents describe roles and tools, not skill chains. Skill invocation happens at runtime through the model's normal skill-routing.

## Agent vs. skill (don't confuse the two)

- **Skill** (`tas/skills/{name}/SKILL.md`) — a pure function. Single transformation. No persona, no tools binding.
- **Agent** (`taa/agents/{name}.md`) — a persona with tools and a model. May invoke skills as part of its work. Is delegated to via Claude Code's Task tool.

If a unit of behavior is a transformation, it belongs in tas. If it is a role with tools and a working memory, it belongs in taa.

## Agent format

Each agent lives in `agents/{name}.md` with YAML frontmatter, following the Claude Code subagent specification.
Start from [`agents/agent.md.tmpl`](agents/agent.md.tmpl) — copy it to a new agent file and fill in.

Required fields:
- `name` must match the filename (without `.md`) exactly.
- `description` specifies when to invoke the agent and when NOT to. This is the routing input — its quality determines delegation quality.

Optional fields:
- `tools`: list of Claude Code tools the agent is permitted to use. Omit to grant all tools.
- `model`: model alias (e.g., `haiku`, `sonnet`, `opus`). Omit to inherit from the parent session.

Body: prose that defines the agent's role, scope, and operating rules. Keep agents focused — one well-scoped role per file.

## Agent naming

Agents are named by **role**, not by codebase. A taa agent should be portable across projects: if the name only makes sense for one project, the role is project-bound and probably should not live in taa.

- Good: `code-reviewer`, `swift-developer`, `aws-serverless-developer`, `test-runner`, `explorer`.
- Bad: `lexio-swift-developer`, `acme-pipeline-runner` — project prefixes do not belong in taa.

When a project needs project-specific behavior on top of a portable agent, the project's `CLAUDE.md` injects the context (architecture rules, conventions, etc.) — the agent definition stays portable.

## Installation

Agents are installed via symlinks from the repo into `~/.claude/agents/`:

```bash
./setup           # install or refresh all agents
./setup --sync    # git pull + refresh + prune dangling
./setup --list    # status per agent
./setup --help    # all flags
```

Editing an agent file in the repo takes effect immediately — the symlink points to the live repo.

## Commit conventions

```
agent(name): description       # Agent add, modify, or remove
harness: description           # Meta documents, structure, format rules
```

Examples:
```
agent(code-reviewer): add architecture-conformance check
agent(swift-developer): tighten scope to exclude server-side Swift
agent(aws-serverless-developer): rename from server-developer for portability
harness: define agent format spec
harness: initialize project structure
```
