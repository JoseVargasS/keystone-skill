# `keystone extract <target>`

Extract a group of functions from a monolithic component into a hook/service/helper module. The target is a specific file or function name.

## Flow

1. **Explore**
   - `codegraph_explore "<target>"` to understand the module: deps, callers, dependencies
   - Read the full target file
   - Identify cohesive function groups (operations over the same domain, same state, same side effects)
   - Verify the target has no circular dependencies with the extraction destination

2. **Plan**
   - Define the new module's interface: what props/params it takes, what it returns
   - Check for similar patterns in the codebase (e.g. other hooks) to replicate structure
   - Do NOT create abstractions that don't already exist: if the project doesn't use hooks, extract as plain functions

3. **Execute**
   - Create the file in the project's canonical location (e.g. `src/hooks/` for React, `src/utils/` for GAS)
   - Move functions, rename to community naming (camelCase)
   - Add types/interfaces if TS
   - Purpose comment at the top of the module and on each exported function
   - In the original file, replace with import + delegation
   - Do NOT leave duplicate code — if the moved function was used in N places, all must import from the new module

4. **Verify**
   - `eslint .` (0 new errors)
   - `npx tsc --noEmit` if TS
   - `npm test` (tests must pass without changes)
   - If something fails, fix imports or types
