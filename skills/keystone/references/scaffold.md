# `keystone scaffold`

Create the project's missing documentation and infrastructure. Detects what's missing and only creates what doesn't exist.

## Flow

1. **Explore**
   - Check which files exist: `AGENTS.md`, `CONTEXT.md`, `CONTRIBUTING.md`, `README.md`, `TESTING.md`, `.github/ci.yml`
   - Read `package.json` to understand stack, scripts, dependencies
   - Read `tsconfig.json` / `jsconfig.json` if they exist
   - Use `context7_query-docs` for up-to-date CI templates matching the project stack (e.g. "GitHub Actions CI for Expo project")

2. **Plan**
   - List missing files
   - Do NOT overwrite existing files — ask if they should be updated

3. **Execute**
   - Each created file must use real project info (stack, scripts, repo URL from `package.json`, etc.)
   - **README.md**: name, short description, stack, main scripts, how to start, links to docs
   - **CONTEXT.md**: domain vocabulary extracted from code (read key modules with codegraph), architectural decisions, references to key modules
   - **AGENTS.md**: rules for AI assistants: conventions the project follows, verify commands, coding principles
   - **CONTRIBUTING.md**: style standards, branch strategy, PR checklist, how to run tests locally
   - **TESTING.md**: which framework is used, where tests live, which seams are tested, expected coverage, how to run
   - **`.github/ci.yml`**: workflow that runs `lint` + `typecheck` + `test` on every push/PR. Use `context7_query-docs` for an up-to-date template matching the project stack.

4. **Verify**
   - Show summary: "Created: README.md, TESTING.md, .github/ci.yml | Already existed: AGENTS.md, CONTRIBUTING.md"
   - If CI was created, ask whether to push to test it
