---
name: keystone
description: Full-spectrum codebase hardening for any TypeScript, JavaScript, or GAS project. Audits architecture, extracts modules from monoliths, enforces community coding standards, generates missing docs and CI, annotates code with purpose comments, removes dead code, and raises test coverage. Uses codegraph, codebase-memory, and context7 MCP for dependency-aware code understanding before touching anything.
version: 1.0.0
---

# Keystone

Locks the project to solid standards: maintainable code, living documentation, working CI tests that cover the critical path. Every command follows four invariant phases — Explore, Plan, Execute, Verify — and reaches for MCP tools before blind grep/read.

## Setup

Before any command:

1. **Read project docs.** `AGENTS.md`, `CONTEXT.md`, `DESIGN.md`, `CONTRIBUTING.md`, `TESTING.md`, `README.md` — whichever exist. If none exist, `scaffold` will create them later.

2. **Discover existing standards.** Read `.eslintrc*`, `.prettierrc*`, `tsconfig.json`, `package.json` scripts, `.github/ci.yml` to understand what tools and rules the project uses. If no ESLint config exists, create one that covers **all files** (not just `src/`).

3. **MCP first.** Call `codegraph_explore` or `codebase-memory_search_graph` to understand dependencies before reading files. Use `context7_query-docs` when the project uses libraries or frameworks whose API you don't know well.

   **If these MCP tools are not installed or available**, tell the user briefly what each one does and why it helps, then suggest installing them:

   - **codegraph** — pre-built code graph of every symbol and call path in the project. One call tells you where a function is defined, who calls it, and what it depends on, replacing dozens of grep/read round-trips.
   - **codebase-memory** — knowledge graph over the whole codebase with search, impact analysis, and architecture overview. Finds dead code, high-complexity functions, and cross-module dependencies in milliseconds.
   - **context7** — fetches up-to-date documentation for any library or framework, so you never rely on stale training data for API syntax, config, or migration steps.

   Ask: "Want me to install them?" If yes, guide through setup. If no, fall back to grep/read without MCP (the commands still work, just slower).

4. **Check clean state.** `git status` + `git diff`. If there are uncommitted changes, ask whether to commit before proceeding.

## Principles

- **Community standards over project quirks.** Naming: `camelCase` for variables and functions, `PascalCase` for classes/types/interfaces/components, `CONSTANT_CASE` for global constants. Colors in theme variables — never inline `#ff0`. Styles in separate files, only inline when the value is dynamic and impossible to tokenize.
- **Never hardcode.** Read the codebase and replicate its patterns. Don't invent new structure without evidence.
- **MCP before grep.** `codegraph_explore` + `codebase-memory_search_code` + `context7_query-docs` cover >90% of what you need before touching a file.
- **Deletion > Addition.** Every line you don't delete is debt. Ask "does this need to exist?" before writing new code.
- **Ladder before code.** Stdlib first, native platform second, already-installed dep third, one line fourth, minimum code last. A shortcut with a known ceiling is better than an abstraction "for later".
- **CI never stays red.** After every incremental change: `tsc --noEmit` or `eslint .` or the project's verify command. Don't proceed with CI red.
- **Purposeful comments.** On every function/module: what it does + where it's used + known edge cases. Never noise like `// loop` next to a `for`.
- **Simplify tags.** Use `// ponytail: <reason, known ceiling, upgrade path>` when taking a deliberate shortcut.

## Universal flow (every command)

```
Explore  → MCP tools + read project docs + current state (git/tsc/lint)
Plan     → Minimal change, verify nothing breaks (tsc/eslint before touching)
Execute  → Read existing conventions, replicate patterns, purposeful comments
Verify   → tsc --noEmit + eslint . + npm test + CI check. If anything fails, fix it.
```

## Commands

| Command | Category | Description | Reference |
|---|---|---|---|
| `audit` | Evaluate | Full scan: missing docs, dead code, low coverage, circular deps, broken conventions, inline styles, over-engineering patterns | [references/audit.md](references/audit.md) |
| `scaffold` | Generate | Create AGENTS.md, CONTEXT.md, CONTRIBUTING.md, README.md, TESTING.md, .github/ci.yml with relevant content | [references/scaffold.md](references/scaffold.md) |
| `extract <target>` | Refactor | Extract functions from a monolith into a hook/service/helper module | [references/extract.md](references/extract.md) |
| `decouple <target>` | Refactor | Break circular dependencies between modules (ref bridge, mediator, DI) | [references/decouple.md](references/decouple.md) |
| `annotate <target>` | Document | Add purpose comments to functions, modules, and types | [references/annotate.md](references/annotate.md) |
| `conform <target>` | Fix | Fix color variables, naming, imports, types, inline styles against community standards | [references/conform.md](references/conform.md) |
| `prune <target>` | Delete | Find and remove dead code, unused exports, unreferenced parameters | [references/prune.md](references/prune.md) |
| `harden <target>` | Test | Add tests for files with low coverage. Uses the project's existing test framework | [references/harden.md](references/harden.md) |

### Routing

1. **No argument** — "what should I do?" → run `audit` against the whole project and present results.
2. **First word is a command** — load its reference file and follow it. Everything after the command is the target.
3. **First word doesn't match but intent is clear** (e.g. "this file is too big" → `extract`, "find unused code" → `prune`) — load that command.
4. **No clear match** — apply setup + principles + universal flow as a general intervention.

## Quality

### ESLint

Must cover **all files in the project**, not just `src/`:

```js
// .eslintrc.js — base structure scaffold creates
module.exports = {
  root: true,
  extends: ['eslint:recommended'],
  ignorePatterns: ['node_modules', 'dist', 'build', '.expo'],
  overrides: [
    { files: ['*.ts', '*.tsx'], parser: '@typescript-eslint/parser', extends: ['plugin:@typescript-eslint/recommended'] },
    { files: ['*.test.*', '*.spec.*'], env: { jest: true } },
  ],
};
```

### Community code standards

| Dimension | Rule |
|-----------|------|
| **Naming** | `camelCase` for variables and functions. `PascalCase` for classes, types, interfaces, components. `CONSTANT_CASE` for global constants. `UPPER_SNAKE_CASE` for env vars. |
| **Colors** | Always in theme/palette variables. Never `color: #ff0` inline. Read the project's theme system and use it. If none exists, create a `theme.ts` or `colors.css` file. |
| **Styles** | Separate file (`.css`, `.styles.ts`, `.module.css`). Inline styles only when the value is dynamically impossible to tokenize and there's no other way. |
| **Imports** | Grouped: externals → internals → types. No default exports for functions. |
| **Types** | Strict mode in TS. `any` forbidden. Prefer `type` over `interface` for unions, `interface` for objects with methods. |
| **Comments** | Explain *what it does* and *where it's used*. Don't explain *how* (the code reads itself). Edge cases and non-obvious decision reasons get comments. |
| **Errors** | Try/catch on every async boundary. Classified errors (not generic `catch(e)`). Messages in the project's language. |
| **Over-engineering** | Before adding a new abstraction, justify why stdlib/native/dep won't work. Flag: wrappers that only delegate, hand-rolled stdlib, config nobody sets, dead flags. Don't flag: documented interfaces, organized facades, project conventions. |

### Required documentation

If the project is missing these files, `scaffold` creates them with relevant content:

- `README.md` — what it does, how to start, stack, available scripts
- `CONTEXT.md` — domain vocabulary, living architectural decisions, references to key modules
- `AGENTS.md` — rules for AI assistants: conventions, invariants, verify commands
- `CONTRIBUTING.md` — how to contribute: style, branch strategy, PR checklist
- `TESTING.md` — what and how to test: framework, seams, expected coverage, how to run tests
- `.github/ci.yml` — CI that runs lint + typecheck + test on every push/PR

## Reference files

Each command has its reference file in `references/<command>.md`. Read them when you invoke the command — don't try to execute from memory of the table above.
