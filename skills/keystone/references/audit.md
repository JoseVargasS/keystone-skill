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

## Sensitivity (over-engineering)

Apply project conventions before flagging. These are NOT over-engineering:
- Separate `.styles.ts` files when the project uses that convention
- Facades over multi-file APIs (improves DX)
- Interfaces documenting a public contract (even with one implementation)
- Well-named wrappers that add error handling, logging, or type safety

These ARE over-engineering:
- Wrapper function that calls one other function with no added logic
- Config flag/variable that never changes with no plan to
- Abstract class with exactly one concrete subclass
- Hand-rolled stdlib (e.g., date formatting when `Intl.DateTimeFormat` exists)
- Dependency that the platform already provides

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
| Over-engineering | `codegraph_explore` for single-implementation abstractions, dead flags, hand-rolled stdlib. `search` for wrappers that only delegate. Apply sensitivity rules before reporting. |
| Sensitivity | Read `AGENTS.md` for project conventions. Read `DESIGN.md` for architecture intent. Skip findings that match established patterns. |
