# `keystone decouple <target>`

Break circular dependencies between modules. Target can be a file, a pair of modules, or "all" for the whole project.

## Flow

1. **Explore**
   - `codegraph_explore "<target>"` with `maxFiles: 20` to get the full dependency graph
   - Identify cycles: A → B → A or longer chains
   - `codebase-memory_trace_path` with direction `both` for each module in the cycle

2. **Plan**
   - Evaluate strategies in order of preference:
     - **1. Ref bridge**: module A exposes a ref that B writes and A reads later. Useful when A and B are hooks in the same component.
     - **2. Interface mediator**: create a third module C with the shared functions. A and B import from C.
     - **3. Merge**: if A and B are cohesive, merge them into a single module.
     - **4. DI / Provider**: move the dependency to a context/provider that both consume.
   - Pick the simplest strategy that removes the cycle without creating new debt

3. **Execute**
   - Implement the chosen strategy
   - Type the bridge/interface correctly (TS strict)
   - Comment at each cycle point explaining why it existed and how it was resolved
   - Update imports in all affected files

4. **Verify**
   - `eslint .` (0 errors)
   - `npx tsc --noEmit` if TS
   - `npm test`
   - `codegraph_explore "<target>"` to confirm the cycle no longer exists
