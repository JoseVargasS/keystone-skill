# `keystone annotate <target>`

Add purpose comments to functions, modules, and types. Target is a file, directory, or specific function.

## Flow

1. **Explore**
   - `codegraph_explore "<target>"` to get the source + callers + dependencies of the target
   - Read the full target file
   - Identify public functions (exported) and complex internal functions (non-trivial logic)

2. **Plan**
   - For each function/module, determine:
     - **Purpose**: what it does in domain language (not implementation)
     - **Where it's used**: extracted from codegraph callers
     - **Edge cases**: which boundary values it handles, what happens on failure
   - Prioritize public functions and domain logic over trivial helpers

3. **Execute**
   - Comment format:
     - Module header: `// --- <name> ---`, `// Callers: <list>`, purpose description
     - Public function: `// <purpose>. Used in: <callers>. <edge cases>`
     - Complex internal function: `// <purpose>. <edge cases if any>`
     - Type/interface: `// <purpose>`
   - Do NOT comment: trivial getters, obvious delegations, `return` statements
   - Do NOT comment every line — the code reads itself

4. **Verify**
   - Read the resulting file and verify comments add information that the code doesn't convey by itself
   - `eslint .` (confirm no comment-related rules are broken)
