# taa — These Are Agents

A personal collection of Claude Code agents.

`taa` sits between [`tao`](https://github.com/kyuhyunhan/tao) (orchestrators) and [`tas`](https://github.com/kyuhyunhan/tas) (skills):

```
tao   — orchestrators (deferred — not built yet)
 └── taa   — agents (this repo)
      └── tas   — skills
```

Each taa agent is a portable role definition: persona, tools, and model. Agents may invoke tas skills at runtime. Project-specific context (architecture rules, conventions) is injected by the consuming project's `CLAUDE.md` — the agent itself stays portable.

This is not a framework. It is one developer's set of agents — built from real workflow, shaped by actual needs.

## Installation

```bash
git clone git@github.com:kyuhyunhan/taa.git
cd taa
./setup           # install or refresh all agents
./setup --sync    # pull latest + refresh + prune dangling
./setup --list    # status per agent
./setup --help    # all flags
```

Agents install as symlinks from the repo into `~/.claude/agents/`. Editing an agent in the repo takes effect immediately.

## Agents

Each agent is a single `.md` file (Claude Code native subagent format). Catalog grows as agents are added — see `agents/` for the current set.

## Authoring a new agent

See [CLAUDE.md](CLAUDE.md) for agent format specs, naming conventions, and commit conventions.

## Inspiration

The architecture follows the "Thin Harness, Fat Skills" principle described in [Garry Tan's essay](https://github.com/garrytan/gbrain/blob/master/docs/ethos/THIN_HARNESS_FAT_SKILLS.md), extended one layer up: agents are the thin persona-and-tools wrapper around skills.

## Related

- [`tas`](https://github.com/kyuhyunhan/tas) — skills (the layer below)
- [`tao`](https://github.com/kyuhyunhan/tao) — orchestrators (the layer above; currently deferred)
