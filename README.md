# keystone

Full-spectrum codebase hardening skill for any TypeScript, JavaScript, or Google Apps Script project.

Audits architecture, extracts modules from monoliths, enforces community coding standards, generates missing docs and CI, annotates code with purpose comments, removes dead code, and raises test coverage.

## Install

```bash
npx skills add JoseVargasS/keystone-skill --skill keystone -g
```

### Source Formats

```bash
# GitHub shorthand
npx skills add JoseVargasS/keystone-skill --skill keystone -g

# Full GitHub URL
npx skills add https://github.com/JoseVargasS/keystone-skill --skill keystone -g

# Direct path to the skill
npx skills add https://github.com/JoseVargasS/keystone-skill/tree/main/skills/keystone -g
```

### Options

| Option                    | Description                                              |
| ------------------------- | -------------------------------------------------------- |
| `-g, --global`            | Install to user directory instead of project             |
| `-a, --agent <agents...>` | Target specific agents (e.g., `claude-code`, `opencode`) |
| `-s, --skill <skills...>` | Install specific skills by name                          |
| `-l, --list`              | List available skills without installing                 |
| `--copy`                  | Copy files instead of symlinking to agent directories    |
| `-y, --yes`               | Skip all confirmation prompts                            |

### Examples

```bash
# Install globally to all agents
npx skills add JoseVargasS/keystone-skill --skill keystone -g -y

# Install only to OpenCode and Claude Code
npx skills add JoseVargasS/keystone-skill --skill keystone -a opencode -a claude-code -g

# Install at project level (shared with team)
npx skills add JoseVargasS/keystone-skill --skill keystone
```

### Installation Scope

| Scope       | Flag      | Location                          | Use Case                                      |
| ----------- | --------- | --------------------------------- | --------------------------------------------- |
| **Project** | (default) | `./&lt;agent&gt;/skills/`         | Committed with your project, shared with team |
| **Global**  | `-g`      | `~/&lt;agent&gt;/skills/`         | Available across all projects                 |

## Prerequisites (recommended)

Keystone works best with these MCP tools. If missing, it will suggest installing them:

- **[codegraph](https://github.com/colbymchenry/codegraph)** — pre-indexed code graph. One call replaces dozens of grep/read round-trips.
- **[codebase-memory](https://github.com/DeusData/codebase-memory-mcp)** — knowledge graph with search, impact analysis, and architecture overview.
- **[context7](https://github.com/upstash/context7)** — up-to-date docs for any library or framework.

## Commands

| Command | Purpose |
|---------|---------|
| `audit` | Full project scan: dead code, circular deps, low coverage, missing docs, broken conventions |
| `scaffold` | Create AGENTS.md, CONTEXT.md, CONTRIBUTING.md, README.md, TESTING.md, .github/ci.yml |
| `extract <target>` | Extract functions from a monolith into a hook/service/helper module |
| `decouple <target>` | Break circular dependencies (ref bridge, mediator, DI) |
| `annotate <target>` | Add purpose comments to functions, modules, and types |
| `conform <target>` | Fix colors, naming, imports, types, inline styles against community standards |
| `prune <target>` | Find and remove dead code, unused exports, unreferenced parameters |
| `harden <target>` | Add tests for files with low coverage |

Every command follows: **Explore** (MCP) → **Plan** → **Execute** (never hardcode, replicate patterns) → **Verify** (tsc + eslint + test).

## Supported agents

- OpenCode
- Claude Code
- Codex
- Cursor
- Any agent that supports the [skills spec](https://vercel-labs-skills.mintlify.app)

## License

MIT
