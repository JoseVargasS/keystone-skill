# `keystone audit [target]`

Full project scan. If no target, scans everything.

## Flow

1. **Explore**
   - Read AGENTS.md, CONTEXT.md, DESIGN.md if they exist
   - `codegraph_explore` to understand module structure and dependencies
   - `codebase-memory_search_graph` to find unused symbols and high coupling
   - Check whether `.github/ci.yml`, `TESTING.md`, `CONTRIBUTING.md` exist
   - Run `eslint .` (must cover all files — verify it doesn't have `ignorePatterns` that exclude project dirs)
   - Run `npx tsc --noEmit` if TS
   - Run `npm test` (or `pnpm test`) to see current coverage

2. **Plan**
   - Categorize findings:
     - 🔴 **Blocking**: no CI, no tests, circular deps, eslint doesn't cover everything
     - 🟡 **Warning**: missing docs, low coverage, inline colors/styles, `any` types
     - 🟢 **Info**: inconsistent naming, messy imports, missing comments
   - Prioritize by impact: blockers first, then warnings, then info

3. **Execute**
   - Write report to `$TEMP/keystone-audit-<timestamp>.html`
   - Template: Tailwind via CDN, cards per category with severity badge
   - Each finding includes: location, problem, impact, proposed solution

4. **Verify**
   - Show the report path to the user and ask: "Which one should we tackle first?"
   - When the user picks, load the corresponding command (`scaffold`, `extract`, `conform`, etc.)

## Auto checks

| Check | How |
|-------|-----|
| ESLint coverage | `eslint .` against the whole repo (no filters) |
| CI exists | `Test-Path .github/ci.yml` (Win) or `test -f .github/ci.yml` (nix) |
| Dead code | `codebase-memory_search_graph` with min_degree=0 |
| Circular deps | `codegraph_explore "circular dependency"` |
| Inline colors | Search for hex/rgb/hsl outside theme variables |
| Inline styles | Search for inline style objects in JSX/TSX |
| Coverage | `npx jest --coverage` or the project's test command |
