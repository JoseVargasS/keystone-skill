# `keystone harden <target>`

Add tests for files with low coverage. Target is a file or directory.

## Flow

1. **Explore**
   - Read `TESTING.md` if it exists for framework, seams, expected coverage
   - Read `package.json` for test scripts
   - Run `npx jest --coverage --collectCoverageFrom="<target>"` (or equivalent)
   - For each file in target, identify:
     - Exported functions without coverage
     - Uncovered branches (ifs, switches, early returns)
     - Edge cases (boundary values, errors, empty state)
   - `codegraph_explore "<target>"` to understand what each uncovered function does and where it's used

2. **Plan**
   - Write tests only for **exported functions** (module seams). Don't test internals.
   - Prioritize:
     1. Functions with business logic (transformations, validations, calculations)
     2. Functions with multiple branches
     3. Functions that handle errors
   - Use the project's existing test framework (jest, vitest, mocha, node:test)

3. **Execute**
   - Create test file alongside the target file: `<file>.test.ts` (or `.test.mjs` for ESM, `.test.js` for CJS)
   - Tests must be **independent** (no shared mutable state between tests)
   - Test names in domain language: `describe("calculateTotal")` → `it("sums base price + tax when no discount")`
   - Expected values from independent source of truth (don't recompute from the code)
   - Don't mock unless necessary (IO, network, timers)

4. **Verify**
   - `npm test` (new + existing tests pass)
   - `npx jest --coverage --collectCoverageFrom="<target>"` — coverage of target must increase
   - `eslint .` (0 errors)
