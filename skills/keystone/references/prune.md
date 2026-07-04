# `keystone prune <target>`

Find and remove dead code, unused exports, unreferenced parameters. Target is a file, directory, or "all".

## Flow

1. **Explore**
   - `codebase-memory_search_graph` with `min_degree=0` to find nodes with no incoming connections
   - `codegraph_explore "<target>"` with `maxFiles: 20` to understand dependencies
   - `codebase-memory_trace_path` with direction `both` for each candidate to verify it has no callers
   - Review each module's public exports and check whether they're imported elsewhere
   - Cross-reference exported symbols against import usage

2. **Plan**
   - Mark candidates in categories:
     - 🔴 **Safe**: not exported + not called internally. Delete directly.
     - 🟡 **Low risk**: exported but no imports in the repo. Could be public API. Ask.
     - 🟢 **Parameter**: function parameter not used. Remove and clean up callers.
   - If target is "all", prioritize modules with the most dead code

3. **Execute**
   - Delete or comment out (if uncertain and needs confirmation)
   - Clean up imports in files that referenced removed code
   - Do NOT touch `node_modules`, `dist`, `build`, `.expo`

4. **Verify**
   - `eslint .` (0 new errors — confirm no unused imports that lint didn't catch before)
   - `npx tsc --noEmit` if TS
   - `npm test`
   - `codebase-memory_search_graph` with `min_degree=0` again to confirm reduction
