# `keystone conform <target>`

Fix code against community standards. Target is a file, directory, or "all".

## Flow

1. **Explore**
   - Read the project's theme/palette system. If none exists, check `DESIGN.md` for context
   - `codegraph_explore "<target>"` to understand the module
   - Run community standard checks on the target

2. **Plan**
   - Categorize fixes:
     - **Colors**: inline hex/rgb/hsl values → theme variable. Find the existing color system (e.g. `palette.ts`, `colors.css`, `theme.ts`). Create one if it doesn't exist.
     - **Naming**: variables/functions not using camelCase, classes without PascalCase, constants without CONSTANT_CASE
     - **Inline styles**: move to separate file. Only keep inline when the value is dynamic and can't be tokenized.
     - **Imports**: reorder: externals → internals → types. No default exports.
     - **Types**: replace `any` with specific types. Interfaces for objects with methods, type for unions.
   - Do NOT change business logic or alter behavior

3. **Execute**
   - One category at a time (colors → naming → styles → imports → types)
   - If the project has no color system, create one using existing theme names from the most used components
   - After each category, run `eslint .` and `tsc --noEmit` (if TS)

4. **Verify**
   - `eslint .` (0 errors)
   - `npx tsc --noEmit` if TS
   - `npm test`
   - Confirm no inline color values remain outside theme variables
   - Confirm no inline styles remain (except dynamic values that can't be tokenized)
