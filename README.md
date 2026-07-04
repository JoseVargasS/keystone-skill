# Keystone

Full-spectrum codebase hardening skill for any TypeScript, JavaScript, or Google Apps Script project.

Audits architecture, extracts modules from monoliths, enforces community coding standards, generates missing docs and CI, annotates code with purpose comments, removes dead code, and raises test coverage.

## Install

```bash
npx skills add josev/keystone-skill --skill keystone -g
```

Requires Node.js 18+. Install globally (`-g`) to make it available across all projects.

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
