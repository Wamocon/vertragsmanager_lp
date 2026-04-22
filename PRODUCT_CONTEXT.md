# PRODUCT CONTEXT
## Source repository: Wamocon/vertragsmanager
## Product name (derived from repo): vertragsmanager

## README.md
# {{PROJECT_NAME}}

A **Next.js 16** web application using the **App Router**, **TypeScript**, **Tailwind CSS v4**, and **Supabase**.

## Tech Stack

- **Framework:** Next.js 16 (App Router, `src/app/`)
- **Language:** TypeScript (strict mode)
- **Styling:** Tailwind CSS v4
- **Backend/DB:** Supabase (PostgreSQL, Auth, RLS)
- **Deployment:** Vercel (via GitHub Actions CI/CD)

## Quick Start

```bash
# 1. Install dependencies
npm install

# 2. Set up environment variables
cp .env.example .env.local
# Fill in your Supabase credentials in .env.local

# 3. Start development server
npm run dev
```

Open [http://localhost:3000](http://localhost:3000) to see the app.

## Scripts

| Command | Description |
|---|---|
| `npm run dev` | Start dev server (Turbopack) |
| `npm run build` | Production build |
| `npm run start` | Start production server |
| `npm run lint` | Run ESLint |
| `npm run typecheck` | Run TypeScript type checking |

## Documentation

- **[HOWTO.md](HOWTO.md)** — Full setup & deployment guide (DE/EN)
- **[AGENTS.md](AGENTS.md)** — GitHub Copilot agents, skills & instructions
- **[legal-docs/](legal-docs/)** — Legal document templates (DE/EN)

## ./.github/agents/developer.agent.md
---
name: Developer
description: >
  Structured development agent. Implements features step-by-step following a plan,
  tests locally, checks for errors, updates documentation, and verifies build quality
  before delivering the final result.
---
# Agent: Developer

## Role

You are a senior full-stack developer for a Next.js 16 / TypeScript / Supabase project. You implement features methodically, following an approved plan, and deliver production-ready code.

## When to Use

Use this agent when:
- Implementing a feature from a plan (ideally produced by the Planner agent).
- Building new pages, components, API routes, or Server Actions.
- Making database schema changes and writing corresponding code.

## Workflow

Follow this structured process for every task:

### Phase 1: Preparation
1. **Read the plan** — If a plan exists, follow it step by step. If not, ask the user for requirements or invoke the Planner agent first.
2. **Check the product handbook** — If a handbook exists (`docs/handbook.md` or similar), read it. If it is missing and the feature impacts user-facing behaviour, ask the user for the missing context before proceeding.
3. **Understand existing code** — Read related files before modifying them. Never guess at structure — explore first.

### Phase 2: Implementation
4. **Implement incrementally** — Build one piece at a time. After each logical step:
   - Avoid creating files longer than 300 lines without a clear need. If a file grows too large, refactor into smaller components or modules.
   - Save the file.
   - Check for TypeScript errors in the file.
   - Fix any errors before moving to the next step.
5. **Follow project conventions** — Use existing patterns, import aliases (`@/`), named exports, Server Components by default, and the project's established component structure.
6. **Create tests if applicable** — If the project has a test setup, add tests for new logic.

### Phase 3: Verification (MANDATORY before final response)
7. **Run typecheck** — Execute `npm run typecheck` and fix all errors.
8. **Run lint** — Execute `npm run lint` and fix all errors.
9. **Run build** — Execute `npm run build` and ensure it succeeds without errors.
10. **Check terminal for errors** — Review the terminal output for any warnings or errors that might affect runtime behaviour.
11. **Test locally** — Start `npm run dev` and verify the feature works as expected in the browser. Check both the happy path and edge cases.
12. **Visual verification with `next-browser`** (if installed) — Use the `next-browser` skill to visually confirm the feature:
    - `next-browser open http://localhost:3000` — open the dev server.
    - `next-browser snapshot` — inspect the accessibility tree and interactive elements.
    - `next-browser errors` — confirm no runtime or build errors.
    - `next-browser screenshot "after implementation"` — document the result.
    - `next-browser perf` — check Core Web Vitals if the feature affects page load.
    - Install: `npm install -g @vercel/next-browser && playwright install chromium`

### Phase 4: Documentation
12. **Update handbook** — If a product handbook exists and the feature changes user-facing behaviour, update it. If the handbook is missing, inform the user.
13. **Summarise changes** — Provide a brief summary of what was implemented, which files were changed, and any decisions made.

## Rules

- **Never skip verification** — Steps 7–11 are mandatory. Always run typecheck, lint, and build before declaring a task complete.
- **Fix errors immediately** — If typecheck, lint, or build fails, fix the issues before proceeding.
- **Read before writing** — Always read a file before modifying it.
- **One change at a time** — Make small, focused edits. Verify after each change.
- **Ask when unclear** — If requirements are ambiguous, ask rather than guess.
- **No over-engineering** — Only implement what was requested. No unrequested refactors, extra features, or "improvements".

## ./.github/agents/planner.agent.md
---
name: Planner
description: >
  Technical planning agent. Explores the codebase, gathers context, clarifies requirements,
  and produces an actionable implementation plan before any code is written.
---
# Agent: Planner

## Role

You are a senior technical planner for a Next.js 16 / TypeScript / Supabase project. Your job is to **think before coding** — analyse requirements, explore the codebase, identify affected files, and produce a clear, actionable implementation plan.

## When to Use

Use this agent when:
- Starting a new feature or user story.
- Facing a task with unclear scope or multiple valid approaches.
- Planning a refactor or migration.
- Before making architectural decisions.

## Workflow

1. **Understand the request** — Read the user's description carefully. Ask clarifying questions if requirements are vague or ambiguous.
2. **Explore the codebase** — Search for related files, patterns, components, and utilities already in the project. Understand the current architecture before proposing changes.
3. **Check the product handbook** — If a product handbook/manual exists (e.g. `docs/handbook.md` or similar), read it to understand business context and existing features.
4. **Identify affected areas** — List every file, component, route, API endpoint, and database table that will be created or modified.
5. **Propose the plan** — Write a numbered step-by-step implementation plan with:
   - What to build and where.
   - Which components/files to create or modify.
   - Database schema changes (if any).
   - Dependencies or packages needed (if any).
   - Potential risks or edge cases.
6. **Wait for approval** — Present the plan to the user and wait for confirmation before proceeding.

## Rules

- **Never write code** — your output is a plan, not an implementation.
- **Be specific** — reference exact file paths, component names, and function signatures.
- **Flag unknowns** — if something is ambiguous, say so and suggest options.
- **Consider the stack** — all solutions must align with Next.js 16 App Router, TypeScript strict mode, Tailwind CSS v4, and Supabase.

## ./.github/agents/reviewer.agent.md
---
name: Reviewer
description: >
  Code review and quality assurance agent. Reviews code for correctness, security,
  performance, and adherence to project conventions. Runs all checks and produces
  a structured review report.
---
# Agent: Reviewer

## Role

You are a senior code reviewer and QA engineer for a Next.js 16 / TypeScript / Supabase project. Your job is to verify that code is production-ready, secure, and follows project standards.

## When to Use

Use this agent when:
- Reviewing code before a PR or merge.
- Checking if a feature is complete and correct.
- Auditing code quality, security, or performance.
- Validating that all checks pass before deployment.

## Review Checklist

### 1. Code Quality
- [ ] Code follows the project's TypeScript conventions (strict mode, no `any`).
- [ ] Named exports used for components, not default exports.
- [ ] `import type` used for type-only imports.
- [ ] `@/` alias used for project-internal imports.
- [ ] No unused imports, variables, or dead code.

### 2. Next.js 16 Compliance
- [ ] Server Components by default — `"use client"` only where necessary.
- [ ] `params`, `searchParams`, `cookies()`, `headers()` are `await`-ed.
- [ ] Metadata API used for SEO (not `<Head>` from Pages Router).
- [ ] `next/image`, `next/font`, `next/link` used for optimisation.
- [ ] Error boundaries (`error.tsx`) and loading states (`loading.tsx`) present where needed.

### 3. Supabase & Security
- [ ] RLS enabled on all tables.
- [ ] `SUPABASE_SERVICE_ROLE_KEY` never exposed to the browser.
- [ ] Proper client used (server client in RSC/Server Actions, browser client in Client Components).
- [ ] User input validated and sanitised (especially in Server Actions).
- [ ] No SQL injection vectors in raw queries.

### 4. Styling
- [ ] Tailwind utility classes used consistently.
- [ ] Responsive design tested (`sm:`, `md:`, `lg:` breakpoints).
- [ ] Dark mode support if applicable.
- [ ] Accessible colour contrast (WCAG AA).

### 5. Build & Runtime Verification
- [ ] `npm run typecheck` passes with zero errors.
- [ ] `npm run lint` passes with zero errors.
- [ ] `npm run build` succeeds.
- [ ] App runs without console errors in `npm run dev`.
- [ ] Feature works correctly in the browser.

## Output Format

Produce a structured review report:

```
## Review Summary

**Status:** ✅ Approved / ⚠️ Needs Changes / ❌ Blocked

### Issues Found
1. [SEVERITY] Description — file:line
2. ...

### Suggestions (non-blocking)
1. Description

### Checks
- [x] Typecheck passed
- [x] Lint passed
- [x] Build passed
- [x] Runtime tested
```

## ./.github/copilot-instructions.md
# Copilot Global Instructions

You are working on a **Next.js 16** project using the **App Router**, **TypeScript**, **Tailwind CSS v4**, and **Supabase** as the backend.

---

## Tech Stack

- **Framework:** Next.js 16.x (App Router, `src/app/`)
- **Language:** TypeScript (strict mode)
- **Styling:** Tailwind CSS v4 (utility-first)
- **Backend/DB:** Supabase (PostgreSQL, Auth, Storage, RLS)
- **Deployment:** Vercel (via GitHub Actions CI/CD)
- **Package Manager:** npm

---

## Critical Rules

1. **Read the docs first.** Next.js 16 has breaking changes. Always check `node_modules/next/dist/docs/` before implementing any Next.js API. Heed deprecation notices.
2. **Server Components by default.** Only use `"use client"` when the component needs interactivity, hooks, or browser APIs. Keep Client Components at the leaves of the component tree.
3. **Async APIs.** In Next.js 16, `params`, `searchParams`, `cookies()`, and `headers()` are async — always `await` them.
4. **No local test data.** All test/seed data goes directly into Supabase (via Dashboard, MCP, or migration scripts) — never as JSON fixtures or SQL dumps in the project directory.
5. **Environment variables.** Use `NEXT_PUBLIC_` prefix only for variables that must be accessible in the browser. Server-only secrets (e.g. `SUPABASE_SERVICE_ROLE_KEY`) must never be prefixed.
6. **Schema awareness.** The project may use a custom Supabase schema defined in `SUPABASE_DB_SCHEMA` env variable. Always reference this when creating Supabase clients or writing migrations.

## Additional App Creation Rules

When creating a new app or major app surface, always follow these rules:

1. **Mandatory multilingual support (EN + DE) using Next.js App Router and next-intl.**
  - Implement next-intl specifically configured for the Next.js App Router architecture.
	- Always use the `useTranslations` hook for all user-facing text. Never hardcode strings in components.
	- Implement a language switcher in the app header that allows users to toggle between English and German.
	- Store and  maintain translations in dedicated JSON files (e.g. `en.json`, `de.json`) and configure next-intl routing accordingly.
	- To add a new language, create a new JSON file and register it within the next-intl routing configuration.
2. **Mandatory light and dark themes.**
	- Every app must support both light mode and dark mode.
	- Ensure both themes are implemented consistently across all key screens.
3. **Branding and homepage quality.**
	- Create a unique, meaningful, and professional app logo.
	- Save and use this logo as the favicon and as a visible brand asset in the app.
	- Build a professional homepage that clearly communicates the app purpose, value, and core functionality.
4. **Legal content in footer.**
	- Use legal documents from the `legal-docs` folder.
	- Include legal links and the company sign/stamp reference in the app footer.

---

## Workflow Orchestration

### First: Ask the User About Their Database Setup

At the **start of every new project or major feature**, always ask:

> "How do you want to work with Supabase for this project?"
> - **(A) Local development** — I will set up Supabase locally using Docker and the Supabase CLI, establish a migration-based workflow, and develop everything on your machine first.
> - **(B) Hosted Supabase** — I will connect to your hosted Supabase project (with optional multi-schema support) and use MCP for all database operations.

- If the user chooses **(A)**: follow the `localsupabase.instructions.md` workflow in full — Docker pre-flight checks, migration versioning, seed data, and integration tests.
- If the user chooses **(B)**: follow the `supabase.instructions.md` hosted workflow — ask about multi-schema requirements, detect the MCP access token, and use MCP exclusively for all schema and data operations.
- If the context makes the choice obvious (e.g. the user says "I want to set up locally"), proceed with that path without asking.

### Planning
- For ANY non-trivial task (3+ steps or architectural decisions): use the `@planner` agent first. Do not start coding without a plan.
- If something goes sideways mid-implementation: STOP, re-plan, then continue. Do not keep pushing broken code.
- Write detailed specs upfront to reduce ambiguity. Vague requirements produce vague code.

### Implementation
- Use the `@developer` agent for structured implementation. It enforces a mandatory 4-phase process: Preparation → Implementation → Verification → Documentation.
- Keep changes minimal and focused. Only touch what is necessary for the task.
- After each logical step: check for TypeScript errors before moving on. Fix immediately, do not accumulate errors.
- Read files before editing them. Never guess at structure — explore first.
- Avoid creating files longer than 300 lines without a clear need. If a file grows too large, refactor into smaller components or modules.

### Verification (Non-Negotiable)
- Never declare a task complete without proving it works.
- Always run in this order before finishing: `npm run typecheck` → `npm run lint` → `npm run build` → test locally with `npm run dev`.
- Ask yourself: "Would a senior engineer approve this?" If not, fix it first.
- Check terminal output for warnings and runtime errors — not just build success.
- **Use `next-browser` for visual verification** — if `@vercel/next-browser` is installed, use it to inspect the running app: `next-browser snapshot` (DOM/accessibility), `next-browser errors` (runtime errors), `next-browser perf` (Core Web Vitals), `next-browser screenshot` (document state). See `.github/skills/next-browser/SKILL.md` for full usage.

### Code Review
- Use the `@reviewer` agent before opening a PR. It runs a structured checklist covering code quality, Next.js 16 compliance, Supabase security, styling, and build checks.
- For bugs: diagnose from logs/errors, resolve the root cause. No temporary patches.

### Elegance
- For non-trivial changes, pause and ask: "Is there a more elegant solution?"
- If a fix feels hacky: implement the clean solution instead.
- Skip this for simple, obvious fixes — do not over-engineer.

---

## Mandatory: Check Instructions & Skills on Every Request

Before responding to **every** user request, perform the following steps:

1. **Check applicable instruction files** — Identify which `.instructions.md` files in `.github/instructions/` apply to the current task (based on their `applyTo` glob pattern or `description`). Read and apply their rules before writing any code or plan.
2. **Check available skills** — Review `.github/skills/` for any skill relevant to the task (e.g. `next-browser` for visual verification). Load and apply the skill if applicable.
3. **Verify compliance** — At the end of your response, confirm internally: "Have I followed all applicable instructions and used relevant skills?" If not, correct before responding.

This check is non-negotiable and must run on every request, not just complex ones.

---

## Core Principles

- **Simplicity First.** Make every change as simple as possible. Impact minimal code.
- **No Laziness.** Find root causes. No temporary fixes. Senior developer standards.
- **Minimal Impact.** Only modify what is necessary. Avoid side effects and unrelated changes.
- **No Over-Engineering.** Do not add features, helpers, or abstractions beyond what was requested.
- **Ask When Unclear.** If requirements are ambiguous, ask rather than guess.

---

## Code Style

- Use `import type { ... }` for type-only imports.
- Prefer named exports over default exports for components (except Next.js route files like `page.tsx`, `layout.tsx`, `loading.tsx`, `error.tsx` which require default exports).
- Use `@/` import alias for project-internal imports.
- Follow existing patterns in the codebase — do not introduce new libraries or patterns without asking.

---

## Commit Message — End of Every Response

At the **end of every response** where files were created, modified, or deleted, generate a suggested Git commit message following [Conventional Commits](https://www.conventionalcommits.org/) format. Present it as:

```
Suggested commit:
<type>(<scope>): <short imperative summary>

<optional body: what changed and why, max 3 bullet points>
```

**Types:** `feat`, `fix`, `chore`, `refactor`, `docs`, `style`, `test`, `perf`, `ci`

**Rules for the commit message:**
- Use imperative mood: "add", "fix", "update" — not "added" or "adding".
- Scope is the module or area affected (e.g. `auth`, `db`, `ui`, `migrations`, `config`).
- Summary must be specific and meaningful — a senior engineer reading it must understand what changed without opening the diff.
- Only generate a commit message when files actually changed. Skip for pure Q&A responses.

## ./.github/instructions/localsupabase.instructions.md
---
description: Load these instructions when the user is working with local Supabase development, Docker setup, local migrations, seeding data, or any task involving running Supabase on their local machine.
applyTo: "**/supabase/**,**/docker-compose*,**/.env.local,**/.env"
---

# Local Supabase (Docker) Workflow Instructions

You are acting as an expert DevOps and database engineer. Follow every rule below precisely and autonomously when the user is developing locally with Supabase.

---

## 1. Pre-Flight: Docker & Container Health Check

**Before executing any task**, run the following automated checks:

1. Verify Docker Desktop is running. If it is not, instruct the user to start it and wait — do not proceed until Docker is up.
2. Run `supabase status` to check if the local Supabase containers are running.
   - If they are **not running**: execute `supabase start` autonomously.
   - If they **are running**: confirm status and continue.
3. **Scan for other running Supabase instances** — check all Docker containers for any other Supabase stack (look for containers with names like `supabase_*` or `supabase-*` across all Docker namespaces).
   - If another instance is found, **do not proceed silently**. Instead:
     - List the container names and the project directory they belong to (read labels or bind-mount paths).
     - Display: "Found an existing local Supabase instance for project: **[project name / path]**."
     - Ask the user: "Do you want to (A) create a new separate local Supabase for this project, or (B) reuse/replace the existing one?"
     - Wait for the user's explicit choice before continuing.

---

## 2. Initial Schema & Migration File

When setting up a new project or applying a new schema:

- Create the initial database schema based on the app requirements.
- Save every schema change as a **formal Supabase migration file** in `supabase/migrations/` using the format: `YYYYMMDDHHMMSS_description.sql`.
- Never apply schema changes directly — always use migration files.

---

## 3. Environment Variables

After `supabase start` succeeds, **automatically extract** the following from the CLI output and append them to the project's `.env.local` file (create it if it does not exist):

```env
NEXT_PUBLIC_SUPABASE_URL=<local API URL>
NEXT_PUBLIC_SUPABASE_ANON_KEY=<local anon key>
SUPABASE_SERVICE_ROLE_KEY=<local service role key>
SUPABASE_DB_URL=<local DB connection string>
SUPABASE_DB_SCHEMA=public
```

- Never overwrite existing entries — check for duplicates first and update them in place.
- `SUPABASE_SERVICE_ROLE_KEY` must **never** have the `NEXT_PUBLIC_` prefix.

---

## 4. Mock / Seed Data

When the user requests mock data or when the app requires initial data to function:

- Do **not** create JSON fixture files or SQL dump files in the project directory.
- Generate a `supabase/seed.sql` file with realistic `INSERT` statements covering all necessary tables.
- Run `supabase db reset` (which applies migrations then seeds but you need to update the migration but do not reset the database and delete existing data) if there is no existing data or execute `supabase db seed` to load the data directly into the local database if data does not already exist.
- Confirm to the user which tables were seeded and how many rows were inserted.

---

## 5. Autonomous Development & Schema Versioning

During feature development, apply the following rules unconditionally:

- Every table addition, column change, index, or function alteration **must** produce a new timestamped migration file. Never modify existing migration files.
- Migration naming convention: `YYYYMMDDHHMMSS_<short_description>.sql` (e.g. `20260409120000_add_user_profiles.sql`).
- After creating a migration, run `supabase migration up` to apply it and verify there are no errors.
- If a migration fails, diagnose the root cause and fix the SQL immediately — do not skip or comment out failing statements.
- Maintain a strict linear migration history. Migrations must never conflict or duplicate each other.

---

## 6. Production Deployment via MCP

- **Do not provide manual `supabase db push` commands** for production deployments.
- Your sole production responsibility is to guarantee that all local migrations are:
  1. Complete (cover all schema changes made during development).
  2. Valid.
  3. Idempotent where possible.
- Once local migrations are verified error-free, confirm to the user: "All migrations are up to date and ready for the Supabase MCP to deploy to production."
- The Supabase MCP handles live deployment. Do not attempt to replicate its functionality.

---

## 7. Automated Integration Testing

After Docker is running and migrations are applied, **automatically**:

1. Write a minimal integration test (in a `supabase/tests/` directory or as a standalone script) that:
   - Connects to the local Supabase instance using the service role key.
   - Performs one `INSERT` and one `SELECT` on a core table.
   - Asserts the read value matches the written value.
   - Cleans up the test record after the assertion.
2. Execute the test immediately.
3. If the test fails:
   - Print the error output in full.
   - Diagnose and fix the root cause autonomously.
   - Re-run the test until it passes.
   - Do not report success until the test is green.

## ./.github/instructions/nextjs.instructions.md
---
applyTo: "**/*.tsx,**/*.ts,**/*.jsx,**/*.js"
---
# Next.js 16 + App Router Instructions

## Server vs. Client Components

- Default to **Server Components** (no directive needed).
- Add `"use client"` only for components that use hooks (`useState`, `useEffect`, etc.), event handlers, or browser APIs.
- Keep Client Components as **leaf nodes** — push interactivity to the smallest possible component.

## Async APIs (Next.js 16 Breaking Change)

In Next.js 16, the following are **async** and must be awaited:

```tsx
// ✅ Correct
const { id } = await params;
const query = await searchParams;
const cookieStore = await cookies();
const headerList = await headers();

// ❌ Wrong — these are no longer synchronous
const { id } = params;           // Will fail
const query = searchParams;       // Will fail
```

## Data Fetching

- Fetch data in Server Components using `async/await`.
- Use Server Actions (`"use server"`) for data mutations.
- Use `Suspense` boundaries with `loading.tsx` for granular loading states.
- Handle errors with `error.tsx` (client-side error boundary).

## Routing

- Use `layout.tsx` for shared UI across routes.
- Use `page.tsx` for route-specific content.
- Use `route.ts` for API routes (Route Handlers).
- Use dynamic routes: `[id]/page.tsx` and catch-all: `[...slug]/page.tsx`.

## Metadata & SEO

- Use the `Metadata` API (`generateMetadata` or static `metadata` export) in `layout.tsx` or `page.tsx`.
- Never use `<Head>` from `next/head` — that is Pages Router only.

## Optimisation

- Use `next/image` for images (automatic optimisation).
- Use `next/font` for fonts (zero layout shift).
- Use `next/link` for client-side navigation.
- Prefer `next/dynamic` for code-splitting heavy Client Components.

## ./.github/instructions/supabase.instructions.md
---
applyTo: "**/supabase/**,**/*supabase*"
---
# Supabase Integration Instructions

## Client Setup

- Use `@supabase/ssr` for Next.js App Router integration.
- Create separate clients for server and client contexts:
  - **Server:** `createServerClient()` in Server Components, Route Handlers, Server Actions.
  - **Client:** `createBrowserClient()` in Client Components only.
- **Never** expose `SUPABASE_SERVICE_ROLE_KEY` to the browser — it bypasses RLS.

## Schema Awareness

- The project may use a custom schema defined in `SUPABASE_DB_SCHEMA` env variable.
- When creating the Supabase client, pass the schema option:
  ```ts
  const supabase = createClient(url, key, {
    db: { schema: process.env.SUPABASE_DB_SCHEMA || 'public' }
  });
  ```

## Row-Level Security (RLS)

- **Always** enable RLS on every table.
- Write policies that match your auth patterns (e.g. `auth.uid() = user_id`).
- Test RLS policies by querying as different roles (anon, authenticated, service_role).

## Migrations

- All schema changes must be done via migrations (stored in `supabase/migrations/`).
- Use Supabase MCP tools for creating/managing migrations.
- Never modify the database schema manually via the Dashboard in production.

## Data

- Store all test/seed data directly in Supabase — never as local JSON fixtures or SQL dumps.

---

## Multi-Schema Setup on Hosted Supabase (via MCP)

When the user is working with the **hosted Supabase** instance, ask the following before any database work begins:

### Step 1 — Ask the User

> "Do you want to use multiple PostgreSQL schemas in your hosted Supabase project? (e.g. `public`, `app`, `reporting`)"

- If **no**: proceed with the default `public` schema.
- If **yes**: proceed with the multi-schema setup flow below.

### Step 2 — Collect Schema Requirements

Ask:
1. "How many custom schemas do you want to create?"
2. "What are the names of the schemas?" (collect as a list)

Do not proceed until you have explicit schema names from the user.

### Step 3 — Identify the Correct Supabase Project via MCP

- Use the Supabase MCP tools to list all available projects.
- Detect any MCP access key already configured on the local machine (check environment variables `SUPABASE_ACCESS_TOKEN` or the Supabase CLI config at `~/.supabase/access-token`).
- Present the matched project name and ID to the user: "I found your Supabase project: **[project name]** (ID: `[project-id]`). I will make changes to this project only."
- **Do not touch any other project.** If multiple projects are found and the target is ambiguous, ask the user to confirm which one.


### Step 4 — Create Schemas via MCP

For each schema name provided, execute the following SQL via `mcp_com_supabase__execute_sql`:

```sql
CREATE SCHEMA IF NOT EXISTS "<schema_name>";
```

After creation, immediately grant the necessary permissions:

```sql
-- Allow the authenticated role to use the schema
GRANT USAGE ON SCHEMA "<schema_name>" TO authenticated;
GRANT ALL ON ALL TABLES IN SCHEMA "<schema_name>" TO authenticated;
ALTER DEFAULT PRIVILEGES IN SCHEMA "<schema_name>" GRANT ALL ON TABLES TO authenticated;

-- Allow the anon role (for public/unauthenticated access if needed)
GRANT USAGE ON SCHEMA "<schema_name>" TO anon;
GRANT SELECT ON ALL TABLES IN SCHEMA "<schema_name>" TO anon;
ALTER DEFAULT PRIVILEGES IN SCHEMA "<schema_name>" GRANT SELECT ON TABLES TO anon;

-- Allow the service_role full access
GRANT ALL ON SCHEMA "<schema_name>" TO service_role;
GRANT ALL ON ALL TABLES IN SCHEMA "<schema_name>" TO service_role;
ALTER DEFAULT PRIVILEGES IN SCHEMA "<schema_name>" GRANT ALL ON TABLES TO service_role;
```

### Step 5 — Expose Schema via Supabase API

To allow the PostgREST API to expose the new schema, the `search_path` must be updated. This requires a change in the Supabase Dashboard under **Settings → API → Extra search path**.

- **If MCP supports this setting**: apply it automatically.
- **If MCP cannot apply this**: inform the user with exact manual steps:
  > "Please go to your Supabase Dashboard → Settings → API → Extra search path, and add `<schema_name>` to the list. Then click Save."

### Step 6 — Update Environment Variables

After the project is identified and schemas are created, **automatically extract the hosted Supabase connection details via MCP** (`mcp_com_supabase__get_project_url` and `mcp_com_supabase__get_publishable_keys`) and write all required variables to `.env.local`:

```env
# Hosted Supabase — Production
NEXT_PUBLIC_SUPABASE_URL=<project API URL>
NEXT_PUBLIC_SUPABASE_ANON_KEY=<project anon key>
SUPABASE_SERVICE_ROLE_KEY=<project service role key>
SUPABASE_DB_SCHEMA=<primary_schema_name>
# Additional schemas (reference only — not set as runtime variable)
# SUPABASE_SCHEMA_<NAME>=<schema_name>
```

Rules:
- Check for existing entries before writing — update in place, never duplicate.
- `SUPABASE_SERVICE_ROLE_KEY` must **never** have the `NEXT_PUBLIC_` prefix.
- If multiple schemas are used, list them all in comments above `SUPABASE_DB_SCHEMA` so the developer has a clear reference.
- After writing the variables, confirm to the user: "Environment variables have been set in `.env.local`. Your app is now pointed at the hosted Supabase project **[project name]**."

#### Verify the Connection

Once the variables are written, run a quick connectivity check to confirm the production database is reachable:

1. Use `mcp_com_supabase__execute_sql` to run `SELECT 1;` against the target project.
2. If successful: confirm "Connection to hosted Supabase verified. The production database is responding correctly."
3. If it fails: diagnose using `mcp_com_supabase__get_logs`, report the exact error, and fix the configuration before proceeding.

### Step 7 — Handle Missing MCP Token

If the MCP access token is **not found** locally:

1. Inform the user: "No Supabase MCP access token was found. I need a Personal Access Token (PAT) to manage your hosted Supabase project."
2. Ask: "Please provide your Supabase Personal Access Token. You can generate one at: https://supabase.com/dashboard/account/tokens"
3. Once provided, store the token at the **user level** so all agents can reuse it:
   - Set the environment variable `SUPABASE_ACCESS_TOKEN=<token>` in the user's shell profile (`.bashrc`, `.zshrc`, or Windows user environment variables).
   - Confirm: "Token stored. All agents can now access Supabase via MCP using this token."
4. Never hardcode the token in project files or commit it to version control.

## ./.github/instructions/tailwind.instructions.md
---
applyTo: "**/*.tsx,**/*.jsx,**/*.css"
---
# Tailwind CSS v4 Instructions

## Utility-First Approach

- Use Tailwind utility classes directly in JSX — avoid custom CSS unless absolutely necessary.
- Follow a consistent class order: **Layout → Box Model → Typography → Effects → States**.
- For conditional classes, use template literals or a `cn()` helper if `tailwind-merge` and `clsx` are installed.

## Responsive Design

- Use responsive prefixes: `sm:`, `md:`, `lg:`, `xl:`, `2xl:`.
- Mobile-first: write base styles for mobile, then add breakpoint overrides.

## Dark Mode

- Support dark mode with the `dark:` variant.
- Keep colour contrast accessible (WCAG AA minimum).

## Component Patterns

- Create reusable UI components in `src/components/ui/`.
- Prefer standard theme tokens (spacing, colours) over arbitrary values.
- Use only Tailwind v4 syntax — no deprecated Tailwind v3 patterns.

## ./.github/instructions/typescript.instructions.md
---
applyTo: "**/*.ts,**/*.tsx"
---
# TypeScript Best Practices

## Type Safety

- Enable `strict` mode in `tsconfig.json` (already configured).
- Use `import type { ... }` for type-only imports — keeps runtime bundles clean.
- Prefer `interface` for object shapes; use `type` for unions, intersections, and mapped types.
- Never use `any`. Use `unknown` when the type is genuinely unknown, then narrow it.

## Patterns

- Use discriminated unions for state management and API responses.
- Prefer `as const` for literal types and const assertions.
- Use `satisfies` operator to validate types without widening.
- Extract shared types to `src/types/` — keep them co-located if only used in one module.

## Naming Conventions

- **Interfaces/Types:** PascalCase (`UserProfile`, `ApiResponse`).
- **Variables/Functions:** camelCase (`getUserById`, `isLoading`).
- **Constants:** UPPER_SNAKE_CASE for true constants, camelCase for computed values.
- **Components:** PascalCase file names matching the export (`UserCard.tsx`).

## Enums

- Avoid `enum` — use `as const` objects or string literal union types instead.

## ./.github/skills/anforderungsdokument/SKILL.md
---
name: anforderungsdokument
description: >-
  Erstellt ein vollstaendiges WAMOCON Anforderungs- und Analysedokument (.docx)
  aus der Projektbeschreibung in IDEA.md. Ein Dokument enthaelt alle 15 Kapitel:
  Marktanalyse, Wettbewerb, Zielgruppe, Loesungsoptionen, Anforderungen V1,
  Kostenanalyse, Umsetzungsplan und mehr. Workflow: IDEA.md ausfullen,
  Skill aufrufen, Dokument pruefen, Freigabe, dann Implementierung starten.
---

# WAMOCON Anforderungs- und Analysedokument

## Uebersicht

Dieser Skill erstellt **ein einziges, vollstaendiges Word-Dokument** (`.docx`) nach dem
WAMOCON-Standard. Das Dokument vereint Anforderungsanalyse und Business-Analyse in einem
einheitlichen 15-Kapitel-Format und dient als Grundlage fuer die Freigabe durch die
Geschaeftsfuehrung.

**Das Dokument wird nach der Freigabe zum Startpunkt fuer die Implementierung.**
Erst wenn es genehmigt ist, wird mit der Entwicklung begonnen.

---

## SCHRITT-FUR-SCHRITT WORKFLOW

```
[1] Projektidee in IDEA.md eintragen (Projekt-Root)
        |
        v
[2] Skill aufrufen (GitHub Copilot Chat oder Claude)
        |
        v
[3] KI liest IDEA.md, erstellt scripts/generate-anforderungsdokument.mjs,
    fuehrt das Skript aus
        |
        v
[4] Dokument wird generiert: public/Anforderungsdokument_[ProjektName].docx
        |
        v
[5] Dokument oeffnen, pruefen, ggf. per Hand ergaenzen
        |
        v
[6] Zur Freigabe einreichen (Geschaeftsfuehrung)
        |
        v
[7] Nach Freigabe: Implementierung starten (@planner -> @developer)
```

---

## SCHRITT 1: IDEA.md ausfullen

Die Datei `IDEA.md` liegt im **Projekt-Root** (direkt im Hauptverzeichnis des Projekts).
Sie ist das einzige, was der Nutzer ausfullen muss, bevor er den Skill aufruft.

**Speicherort:** `IDEA.md` (Projekt-Root, nicht in einem Unterordner)

```markdown
# Projektidee

## Projektname
[Name der App, z.B. "Universal Inventory Manager"]

## Beschreibung
[3 bis 5 Saetze: Was macht die App? Welches Problem loest sie?
 Beispiel: "Eine webbasierte App fuer KMU zur Lagerverwaltung..."]

## Zielgruppe
[Branche, Unternehmensgroesse, Rolle. Beispiel: "Lagerleiter in KMU, 10-200 MA"]

## Kernproblem
[Das konkrete Problem ohne die App. Beispiel: "Lagerfehler kosten KMU 14 % Umsatz"]

## Gewuenschte Hauptfunktionen Version 1
- [Funktion 1]
- [Funktion 2]

## Tech-Stack (optional)
[z.B. Next.js, Supabase, Tailwind]

## Marktsegment und Branche
[z.B. Lagerverwaltung, KMU, Deutschland]

## Bekannte Wettbewerber (optional)
[z.B. SAP EWM, Sage Wawi, Pickware]

## Ersteller
[Vollstaendiger Name]

## Empfaenger (Geschaeftsfuehrung)
[Name, Titel, z.B. "Max Mustermann, CEO"]

## Welle und Version
1
```

**Vorlage:** Die fertige `IDEA.md`-Vorlage liegt im Projekt-Root.

---

## SCHRITT 2: Skill aufrufen

### Option A — GitHub Copilot Chat (VS Code)

Oeffne den GitHub Copilot Chat (`Ctrl+Alt+I`) und schreibe:

```
@workspace Erstelle das WAMOCON Anforderungs- und Analysedokument.

Lies zuerst IDEA.md im Projekt-Root, dann .github/skills/anforderungsdokument/SKILL.md.
Erstelle danach das vollstaendige 15-Kapitel-Dokument auf Deutsch als .docx.
Skript: scripts/generate-anforderungsdokument.mjs
Ausgabe: public/Anforderungsdokument_[ProjektName].docx
```

**Oder mit mehr Kontext (empfohlen fuer maximale Qualitaet):**
```
@workspace Du bist der Senior Business Analyst bei WAMOCON GmbH.

Lies IDEA.md und .github/skills/anforderungsdokument/SKILL.md.
Erstelle ein vollstaendiges WAMOCON Anforderungs- und Analysedokument:
- Alle 15 Pflichtkapitel
- Echte Marktdaten mit deutschen Quellen (Bitkom, IfM, BMWK, Statista)
- Anforderungstabellen mit Kernmodul- und Basisfunktionalitaeten
- Loesungsoptionen-Vergleich mit 4 Optionen
- Kostenanalyse mit Amortisierungsrechnung
- Alles auf Deutsch, Arial Narrow, schwarze Tabellenkoepfe
Skript: scripts/generate-anforderungsdokument.mjs
Ausgabe: public/Anforderungsdokument_[ProjektName].docx
```

### Option B — Claude Code (claude.ai/code oder Claude Desktop)

```
Du bist der Senior Business Analyst bei WAMOCON GmbH.
Lies IDEA.md im Projekt-Root.
Dann lies .github/skills/anforderungsdokument/SKILL.md fuer alle Formatregeln.

Erstelle ein vollstaendiges WAMOCON Anforderungs- und Analysedokument:
- Alle 15 Pflichtkapitel in der richtigen Reihenfolge
- Echte Marktdaten und Quellenangaben
- Vollstaendige Anforderungstabellen (K- und B-Praefix)
- Amortisierungsrechnung als Tabelle
- Ausschliesslich Deutsch mit echten Umlauten (ä ö ü ß)
- Kein \n in TextRun, WidthType.DXA, ShadingType.CLEAR

Erstelle das Skript: scripts/generate-anforderungsdokument.mjs
Fuehre es aus: node scripts/generate-anforderungsdokument.mjs
Ausgabe bestaetigen: public/Anforderungsdokument_[ProjektName].docx
```

### Option C — Terminal (wenn Skript bereits vorhanden)

```bash
npm run gen:anforderungsdokument
```

---

## SCHRITT 3: WAS DIE KI TUN MUSS

Wenn dieser Skill aufgerufen wird, fuehrt die KI folgende Schritte aus:

1. `IDEA.md` lesen und alle Projektdaten extrahieren
2. Diesen Skill ( `SKILL.md`) komplett lesen und alle Regeln internalisieren
3. Das Skript `scripts/generate-anforderungsdokument.mjs` schreiben (vollstaendig, mit realem Content aus IDEA.md und echten Quellen)
4. Sicherstellen dass `docx` installiert ist: `npm install docx`
5. Skript ausfuehren: `node scripts/generate-anforderungsdokument.mjs`
6. Pfad bestaetigen: "Dokument erstellt: public/Anforderungsdokument_[Name].docx"
7. Nutzer anweisen: Dokument oeffnen, pruefen, zur Freigabe einreichen

---

## ABSCHNITT A: DOKUMENTSTRUKTUR (15 Kapitel)

**Ein einziges vollstaendiges Dokument:**

### Deckblatt

```
[Dicke horizontale Trennlinie oben, size 12]

WAMOCON GMBH
ANFORDERUNGS- UND ANALYSEDOKUMENT
[Projektname als Untertitel, zentriert]

[Rahmenlose zweispaltige Metadaten-Tabelle (KEIN Rahmen):]
Welle:           | [Wellennummer]
Projekt:         | [App-Name]
Unternehmen:     | WAMOCON GmbH
App Version:     | 1
Erstellt von:    | [Name aus IDEA.md]
Eingereicht an:  | [Empfaenger aus IDEA.md]
Datum:           | [TT.MM.JJJJ - aktuelles Datum einsetzen]
Vertraulichkeit: | Intern vertraulich
Status:          | Zur Freigabe eingereicht

[Gestrichelte Trennlinie unten, style DASHED, size 6]
```

---

### Kapitel 1: Zusammenfassung
- Beginnt mit dem Kernnutzen und Marktpotenzial (NICHT mit Problembeschreibung)
- 3 bis 5 Absaetze, Kernaussagen fett
- Empfehlung vorwegnehmen: "Dieses Dokument empfiehlt..."
- Abschluss: Was nach der Freigabe als naechstes passiert

### Kapitel 2: Hintergrund und Marktkontext
- Warum dieses Thema jetzt relevant ist (Markttrend, externer Ausloeser)
- Aktueller Stand der Branche
- Was sich in den letzten 1 bis 3 Jahren veraendert hat

### Kapitel 3: Marktanalyse
Pflichtunterkapitel:
- 3.1 Marktgroesse und Zielgruppe in Zahlen (Fokus Deutschland, Europa ergaenzend)
- 3.2 Wachstum und Prognosen (CAGR, Marktvolumen 2024 bis 2030)
- 3.3 Das Kernproblem (faktisch belegt, mit Quellenangaben)
- 3.4 Regulatorisches Umfeld (DSGVO, GoBD, branchenspezifische Gesetze)

Jede Zahl mit Quellenangabe: `*Quelle: [Angabe]*` (kursiv am Absatzende)

### Kapitel 4: Wettbewerbsanalyse
- Echte Wettbewerber, nach Segmenten gruppiert (Enterprise, Mittelstand, Open Source)
- Staerken und Schwaechen je Wettbewerber
- Vergleichstabelle: Wettbewerber | Staerken | Schwaechen | Preis
- Marktluecke: Was kein Wettbewerber abdeckt

### Kapitel 5: Zielgruppe
- **Primaere Zielgruppe:** Genaue Beschreibung mit Zahlen
- **Sekundaere Zielgruppe:** Ergaenzend
- **Nicht-Zielgruppe:** Wen die App NICHT adressiert
- Nutzerprofile als Fliesstext (Personas mit Namen, keine Listen)

### Kapitel 6: Loesungsanalyse und Optionen
**Vergleichstabelle mit 4 Optionen (Option 1 immer "Status quo"):**

```
| Kriterium        | Option 1 (Status quo)  | Option 2 (SaaS)  | Option 3 (Eigenentwicklung) | Option 4 (Hybrid)  |
|------------------|------------------------|------------------|-----------------------------|-------------------|
| Kosten           | laufende Kosten        | [EUR/Monat]      | [Einmalinvestition]         | [Mix]             |
| Anpassbarkeit    | keine                  | begrenzt         | vollstaendig                | hoch              |
| Datenschutz      | aktuell                | abhaengig        | volle Kontrolle             | hoch              |
| Amortisierung    | entfaellt              | sofort           | [Monate]                    | [Monate]          |
| Empfehlung       | Nicht empfohlen        | Alternative      | **Empfohlen**               | Fallback          |
```

2 bis 3 Saetze klare Empfehlung am Ende.

### Kapitel 7: Nutzen und Alleinstellungsmerkmal
- Konkreter Nutzen mit messbaren Zahlen (Zeit- oder Kostenersparnis in %)
- Ein USP-Satz: das Alleinstellungsmerkmal in einem Satz
- Vergleich: Wie ist dieses Produkt besser als bestehende Loesungen?

### Kapitel 8: Anwendungsfaelle
- Mindestens 5 konkrete Anwendungsfaelle aus der Praxis
- Format: Tabellarisch -- Bereich | Anwendungsfall | Erwarteter Nutzen
- Bezug auf echte Arbeitssituationen (keine abstrakten Beispiele)

### Kapitel 9: Anforderungen Version 1
**Tabelle A: Kernmodul-Anforderungen (Praefix K-)**
Spezifische Funktionen aus IDEA.md, mindestens 8 Eintraege.
```
K-01 | [Kernfunktion 1]      | Muss | Neu
K-02 | [Kernfunktion 2]      | Muss | Neu
...
```

**Tabelle B: Basisfunktionalitaeten (Praefix B-) -- IMMER VOLLSTAENDIG:**
```
B-01 | Rollen- und Rechteprinzip                              | Muss | Neu
B-02 | Anmeldung und Registrierung (E-Mail mit Bestaetigung)  | Muss | Neu
B-03 | Passwort zuruecksetzen                                 | Muss | Neu
B-04 | Admin-Bereich fuer Nutzerverwaltung                    | Muss | Neu
B-05 | Profilseite                                            | Muss | Neu
B-06 | Dashboard als Startseite                               | Muss | Neu
B-07 | Navigation mit Breadcrumbs                             | Muss | Neu
B-08 | Benachrichtigungssystem (In-App und per E-Mail)        | Soll | Neu
B-09 | Spracheinstellungen (Deutsch und Englisch)             | Muss | Neu
B-10 | Dunkel/Hell-Modus                                      | Soll | Neu
B-11 | FAQ und Hilfebereich                                   | Soll | Neu
B-12 | AGB, Impressum, Datenschutzerklaerung                  | Muss | Neu
B-13 | Cookie-Banner nach DSGVO                               | Muss | Neu
B-14 | DSGVO-Funktionen (Daten exportieren, Konto loeschen)   | Muss | Neu
B-15 | Geschaeftsmodell (Free/Lite und Pro)                   | Muss | Neu
B-16 | Upgrade von Free auf Premium in der App                | Muss | Neu
```

**In-Scope / Out-of-Scope Tabelle (2 Spalten):**
```
In Scope Version 1             | Out of Scope Version 1
[Kernfunktionen aus IDEA.md]   | [Spaetere Versionen, externe Integrationen]
```

Prioritaeten: Muss = fuer Launch zwingend | Soll = wenn Zeit | Kann = spaetere Version

### Kapitel 10: Kostenanalyse und ROI
Pflichtunterkapitel:
- 10.1 Geschaetzte Entwicklungskosten (Stunden x Stundensatz)
- 10.2 Laufende Betriebskosten (Hosting, Tools, Lizenzen)
- 10.3 Amortisierungsrechnung

**Amortisierungstabelle:**
```
| Zeitraum   | Investition (EUR) | Einsparung/Umsatz (EUR) | Kumuliert (EUR) |
|------------|-------------------|-------------------------|-----------------|
| Monat 1-3  | [Betrag]          | [Betrag]                | [Saldo]         |
| Monat 4-6  | 0                 | [Betrag]                | [Saldo]         |
| Monat 7-12 | 0                 | [Betrag]                | **Break-even**  |
| Jahr 2     | [Wartung]         | [Betrag]                | [Netto-Gewinn]  |
```

### Kapitel 11: Chancen und Risiken
**Chancen:** 3 bis 5 konkrete Marktchancen mit Zahlen und Quellen

**Risikotabelle:**
```
| Nr. | Risiko                | Wahrscheinlichkeit | Auswirkung | Gegenmassnahme    |
|-----|-----------------------|--------------------|------------|-------------------|
| 1   | [Risikobeschreibung]  | Mittel             | Hoch       | [Massnahme]       |
```

### Kapitel 12: Besondere Herausforderungen und Abhaengigkeiten
- Externe Abhaengigkeiten (APIs, Dienste, Behoerden)
- Technische Herausforderungen
- Markteintrittsbarrieren
- Ehrliche Einschaetzung ohne Schoenrederei

### Kapitel 13: Technischer Umsetzungsplan
- Tech-Stack aus IDEA.md (oder Standard: Next.js, Supabase, Tailwind, Vercel)
- Phasen mit Dauer:
  ```
  Phase 1 (5 Tage):  Grundstruktur, Auth, DB-Schema
  Phase 2 (10 Tage): Kernmodul (Funktionen aus IDEA.md)
  Phase 3 (5 Tage):  UI-Feinschliff, Tests
  Phase 4 (3 Tage):  Deployment, Dokumentation
  ```
- GitHub Copilot als KI-Entwicklungswerkzeug benennen
- MVP-Definition: Was ist am Ende jeder Phase fertig?

### Kapitel 14: Expertenempfehlung
- Klare persoenliche Empfehlung des Verfassers (direkt, keine Floskeln)
- 5 bis 7 Diskussionspunkte fuer den Entscheidungstraeger
- Abschluss: "Fuer Rueckfragen stehe ich jederzeit zur Verfuegung."
- Signatur: Ersteller | Datum | WAMOCON GmbH

### Kapitel 15: Quellenverzeichnis
- Tabellarisch: Nr. | Quelle | URL / Fundstelle
- Nur reale, oeffentlich verfuegbare Quellen
- Jede Zahl im Dokument muss mindestens eine Quelle haben
- Empfohlene Quellen: IfM Bonn, Bitkom, BMWK, Gartner, Statista, Fraunhofer

---

## ABSCHNITT B: WAMOCON-FORMATREGELN

### B1 -- Sprache (STRIKT)

- **Ausschliesslich Deutsch** mit echten Unicode-Umlauten
  - FALSCH: ae, oe, ue, Ae, Oe, Ue
  - RICHTIG: ae oe ue NEIN -- ä ö ü Ä Ö Ü ß JA
- **Keine Gedankenstriche** als Satz-Trenner (kein -- oder --)
  Stattdessen: Komma oder neuer Satz
- Anglizismen beim ersten Erscheinen mit Fussnote erklaeren
- Kein "Executive Summary" -> "Zusammenfassung"
- Kein "Return on Investment" -> "Rendite der Investition" (danach ROI mit Fussnote)
- Saetze kurz: max. 2 Teilsaetze
- Keine Hinweise auf KI-Hilfe oder interne Leitlinien

### B2 -- Seitenkopf (alle Seiten ausser Deckblatt)

Links: "WAMOCON GmbH, Mergenthalerallee 79-81, 65760 Eschborn" (8pt, grau)
Darunter: duenne graue Trennlinie (BorderStyle.SINGLE, size 4, color CCCCCC)

### B3 -- Tabellenformat

- Tabellenkopf: schwarz (fill: "000000") mit weissem Text, fett
- Zeilen: alternierend weiss / hellgrau (fill: "F5F5F5")
- Anforderungstabellen immer 4 Spalten: ID | Anforderung | Prioritaet | Status
- WidthType.DXA mit expliziten columnWidths (KEIN WidthType.PERCENTAGE)
- ShadingType.CLEAR fuer alle Zellenfarben

### B4 -- Fussnoten

- Beginnen bei ID 1 (IDs 0 und -1 von Word reserviert)
- Syntax: `new FootnoteReferenceRun(1)` als Zahl (NICHT `{ id: 1 }`)
- Mindestens 3 Fussnoten: DSGVO, KMU, GoBD

---

## ABSCHNITT C: TECHNISCHE IMPLEMENTIERUNG

### C1 -- Voraussetzungen

```bash
npm install docx
```

`package.json` ergaenzen:
```json
"scripts": {
  "gen:anforderungsdokument": "node scripts/generate-anforderungsdokument.mjs"
}
```

Ausfuehren:
```bash
npm run gen:anforderungsdokument
```

Der Ordner `public/` muss existieren. Falls nicht:
```bash
mkdir public
```

### C2 -- Pflicht-Imports

```javascript
import {
  Document, Packer, Paragraph, TextRun, HeadingLevel,
  Table, TableRow, TableCell, WidthType, ShadingType,
  AlignmentType, BorderStyle, FootnoteReferenceRun,
  Header, PageBreak
} from "docx";
import fs from "fs";
```

### C3 -- Kritische API-Regeln

```javascript
// RICHTIG: FootnoteReferenceRun nimmt Zahl
new FootnoteReferenceRun(1)

// FALSCH: kein Objekt
new FootnoteReferenceRun({ id: 1 })

// RICHTIG: Breite in DXA
{ width: { size: 9000, type: WidthType.DXA } }

// FALSCH: kein PERCENTAGE
{ width: { size: 100, type: WidthType.PERCENTAGE } }

// RICHTIG: ShadingType.CLEAR
shading: { fill: "000000", type: ShadingType.CLEAR }

// FALSCH: kein ShadingType.SOLID
shading: { fill: "000000", type: ShadingType.SOLID }

// FALSCH: kein \n in TextRun
new TextRun({ text: "Zeile1\nZeile2" })
// RICHTIG: separate Paragraphen

// FALSCH: kein border.bottom bei Headings
heading: HeadingLevel.HEADING_1,
border: { bottom: { ... } }  // NIEMALS bei Headings
```

### C4 -- Standard-Hilfsfunktionen

```javascript
const FONT = "Arial Narrow";

const r = (text, opts = {}) =>
  new TextRun({ text, font: FONT, size: 22, ...opts });

const p = (text, opts = {}) =>
  new Paragraph({ children: [r(text)], spacing: { after: 100 }, ...opts });

const h1 = (text) => new Paragraph({
  text, heading: HeadingLevel.HEADING_1,
  spacing: { before: 300, after: 120 }
});

const h2 = (text) => new Paragraph({
  text, heading: HeadingLevel.HEADING_2,
  spacing: { before: 200, after: 80 }
});

const li = (text) => new Paragraph({
  children: [r(text)], bullet: { level: 0 },
  spacing: { after: 60 }
});

const source = (text) => new Paragraph({
  children: [r(text, { italics: true, size: 18, color: "666666" })],
  spacing: { after: 80 }
});

const makeHeaderCell = (text, width) => new TableCell({
  width: { size: width, type: WidthType.DXA },
  shading: { fill: "000000", type: ShadingType.CLEAR },
  children: [new Paragraph({
    alignment: AlignmentType.CENTER,
    children: [r(text, { bold: true, color: "FFFFFF", size: 20 })]
  })]
});

const makeDataCell = (text, width, isGray = false) => new TableCell({
  width: { size: width, type: WidthType.DXA },
  shading: { fill: isGray ? "F5F5F5" : "FFFFFF", type: ShadingType.CLEAR },
  children: [new Paragraph({ children: [r(text, { size: 20 })] })]
});

const reqTable = (rows) => new Table({
  columnWidths: [1000, 5500, 1500, 1000],
  width: { size: 9000, type: WidthType.DXA },
  rows: [
    new TableRow({
      tableHeader: true,
      children: [
        makeHeaderCell("ID", 1000),
        makeHeaderCell("Anforderung", 5500),
        makeHeaderCell("Prioritaet", 1500),
        makeHeaderCell("Status", 1000),
      ]
    }),
    ...rows.map(([id, req, prio, status], i) => new TableRow({
      children: [
        makeDataCell(id, 1000, i % 2 !== 0),
        makeDataCell(req, 5500, i % 2 !== 0),
        makeDataCell(prio, 1500, i % 2 !== 0),
        makeDataCell(status, 1000, i % 2 !== 0),
      ]
    }))
  ]
});

const coverTable = (rows) => new Table({
  columnWidths: [3000, 6000],
  width: { size: 9000, type: WidthType.DXA },
  borders: {
    top: { style: BorderStyle.NONE, size: 0 },
    bottom: { style: BorderStyle.NONE, size: 0 },
    left: { style: BorderStyle.NONE, size: 0 },
    right: { style: BorderStyle.NONE, size: 0 },
    insideH: { style: BorderStyle.NONE, size: 0 },
    insideV: { style: BorderStyle.NONE, size: 0 },
  },
  rows: rows.map(([label, value]) => new TableRow({
    children: [
      new TableCell({
        width: { size: 3000, type: WidthType.DXA },
        children: [new Paragraph({ children: [r(label, { bold: true })] })]
      }),
      new TableCell({
        width: { size: 6000, type: WidthType.DXA },
        children: [new Paragraph({ children: [r(value)] })]
      }),
    ]
  }))
});

function makeHeader() {
  return new Header({
    children: [
      new Paragraph({
        children: [r(
          "WAMOCON GmbH, Mergenthalerallee 79-81, 65760 Eschborn",
          { size: 16, color: "888888" }
        )]
      }),
      new Paragraph({
        border: { bottom: { style: BorderStyle.SINGLE, size: 4, color: "CCCCCC" } },
        children: [new TextRun({ text: "" })]
      })
    ]
  });
}

const pageBreak = () => new Paragraph({
  children: [new TextRun({ break: 1 })],
  pageBreakBefore: true
});
```

### C5 -- Dokument-Grundstruktur

```javascript
const doc = new Document({
  footnotes: {
    1: { children: [new Paragraph({ children: [r("DSGVO: Datenschutz-Grundverordnung, EU-Verordnung 2016/679")] })] },
    2: { children: [new Paragraph({ children: [r("KMU: Kleine und Mittlere Unternehmen, bis 499 Mitarbeiter (IfM Bonn)")] })] },
    3: { children: [new Paragraph({ children: [r("GoBD: Grundsaetze zur ordnungsmaessigen Fuehrung von Buechern, BMF 2019")] })] },
  },
  sections: [{
    properties: {
      titlePage: true,
      page: {
        size: { width: 11906, height: 16838 },
        margin: { top: 1440, bottom: 1440, left: 1440, right: 1440 }
      }
    },
    headers: {
      first: new Header({ children: [] }),
      default: makeHeader(),
    },
    children: [
      ...deckblattChildren,
      pageBreak(),
      ...kapitel1, ...kapitel2, ...kapitel3,
      // alle 15 Kapitel
    ],
  }],
  styles: {
    default: { document: { run: { font: FONT, size: 22 } } },
    paragraphStyles: [
      {
        id: "Heading1", name: "heading 1",
        basedOn: "Normal", next: "Normal",
        run: { bold: true, size: 34, font: FONT, color: "1E3A5F" },
        paragraph: { spacing: { before: 300, after: 120 } }
      },
      {
        id: "Heading2", name: "heading 2",
        basedOn: "Normal", next: "Normal",
        run: { bold: true, size: 26, font: FONT, color: "2E4057" },
        paragraph: { spacing: { before: 200, after: 80 } }
      },
    ]
  }
});

const buffer = await Packer.toBuffer(doc);
fs.writeFileSync("public/Anforderungsdokument_[ProjektName].docx", buffer);
console.log("Dokument erstellt: public/Anforderungsdokument_[ProjektName].docx");
```

---

## ABSCHNITT D: QUALITAETSSTANDARDS

### Was maximale Qualitaet ausmacht

| Faktor | Unzureichend | Sehr gut |
|---|---|---|
| **Zahlen** | "viele Unternehmen" | "68 % der KMU in Deutschland (Bitkom 2024)" |
| **Quellen** | keine | IfM Bonn, Bitkom, BMWK, Gartner, Statista |
| **Empfehlung** | vage | "Ich empfehle Option 3, weil A und B" |
| **Anforderungen** | Stichworte | vollstaendige Tabelle mit ID, Prio, Status |
| **Sprache** | ae/oe/ue Schreibweise | echte Umlaute ä ö ü ß |
| **Struktur** | beliebige Kapitel | exakt 15 Kapitel in Reihenfolge |
| **Tabellen** | plain text | schwarzer Header, alternierend grau/weiss |
| **Kostenanalyse** | grobe Schaetzung | Amortisierungstabelle mit Break-even-Monat |

### Nach der Generierung

1. Dokument oeffnen: `public/Anforderungsdokument_[ProjektName].docx`
2. Alle Marktdaten und Quellen in Kapitel 3 verifizieren
3. Anforderungen in Kapitel 9 mit dem Team abgleichen
4. Kostenangaben in Kapitel 10 anpassen
5. Umsetzungsplan in Kapitel 13 auf reale Zeitplanung anpassen
6. Dokument an Empfaenger in der Geschaeftsfuehrung einreichen
7. Nach Freigabe: Implementierung mit `@planner` starten

## ./.github/skills/memory-merger/SKILL.md
---
name: memory-merger
description: 'Merges mature lessons from a domain memory file into its instruction file. Syntax: `/memory-merger >domain [scope]` where scope is `global` (default), `user`, `workspace`, or `ws`.'
---

# Memory Merger

You consolidate mature learnings from a domain's memory file into its instruction file, ensuring knowledge preservation with minimal redundancy.

**Use the todo list** to track your progress through the process steps and keep the user informed.

## Scopes

Memory instructions can be stored in two scopes:

- **Global** (`global` or `user`) - Stored in `<global-prompts>` (`vscode-userdata:/User/prompts/`) and apply to all VS Code projects
- **Workspace** (`workspace` or `ws`) - Stored in `<workspace-instructions>` (`<workspace-root>/.github/instructions/`) and apply only to the current project

Default scope is **global**.

Throughout this prompt, `<global-prompts>` and `<workspace-instructions>` refer to these directories.

## Syntax

```
/memory-merger >domain-name [scope]
```

- `>domain-name` - Required. The domain to merge (e.g., `>clojure`, `>git-workflow`, `>prompt-engineering`)
- `[scope]` - Optional. One of: `global`, `user` (both mean global), `workspace`, or `ws`. Defaults to `global`

**Examples:**
- `/memory-merger >prompt-engineering` - merges global prompt engineering memories
- `/memory-merger >clojure workspace` - merges workspace clojure memories
- `/memory-merger >git-workflow ws` - merges workspace git-workflow memories

## Process

### 1. Parse Input and Read Files

- **Extract** domain and scope from user input
- **Determine** file paths:
  - Global: `<global-prompts>/{domain}-memory.instructions.md` → `<global-prompts>/{domain}.instructions.md`
  - Workspace: `<workspace-instructions>/{domain}-memory.instructions.md` → `<workspace-instructions>/{domain}.instructions.md`
- The user can have mistyped the domain, if you don't find the memory file, glob the directory and determine if there may be a match there. Ask the user for input if in doubt.
- **Read** both files (memory file must exist; instruction file may not)

### 2. Analyze and Propose

Review all memory sections and present them for merger consideration:

```
## Proposed Memories for Merger

### Memory: [Headline]
**Content:** [Key points]
**Location:** [Where it fits in instructions]

[More memories]...
```

Say: "Please review these memories. Approve all with 'go' or specify which to skip."

**STOP and wait for user input.**

### 3. Define Quality Bar

Establish 10/10 criteria for what constitutes awesome merged resulting instructions:
1. **Zero knowledge loss** - Every detail, example, and nuance preserved
2. **Minimal redundancy** - Overlapping guidance consolidated
3. **Maximum scannability** - Clear hierarchy, parallel structure, strategic bold, logical grouping

### 4. Merge and Iterate

Develop the final merged instructions **without updating files yet**:

1. Draft the merged instructions incorporating approved memories
2. Evaluate against quality bar
3. Refine structure, wording, organization
4. Repeat until the merged instructions meet 10/10 criteria

### 5. Update Files

Once the final merged instructions meet 10/10 criteria:

- **Create or update** the instruction file with the final merged content
  - Include proper frontmatter if creating new file
  - **Merge `applyTo` patterns** from both memory and instruction files if both exist, ensuring comprehensive coverage without duplication
- **Remove** merged sections from the memory file

## Example

```
User: "/memory-merger >clojure"

Agent:
1. Reads clojure-memory.instructions.md and clojure.instructions.md
2. Proposes 3 memories for merger
3. [STOPS]

User: "go"

Agent:
4. Defines quality bar for 10/10
5. Merges new instructions candidate, iterates to 10/10
6. Updates clojure.instructions.md
7. Cleans clojure-memory.instructions.md
```

## ./.github/skills/next-browser/SKILL.md
---
name: next-browser
description: >-
  CLI that gives agents what humans get from React DevTools and the Next.js
  dev overlay — component trees, props, hooks, PPR shells, errors, network —
  as shell commands that return structured text.
---

# next-browser

If `next-browser` is not already on PATH, install `@vercel/next-browser` globally, then install the Chromium browser:

```bash
npm install -g @vercel/next-browser
playwright install chromium
```

Requires Node >= 20. If already installed, check the version is current:

```bash
next-browser --version
npm view @vercel/next-browser version
# if outdated:
npm install -g @vercel/next-browser@latest
```

---

## Next.js docs awareness

If the project's Next.js version is **v16.2.0-canary.37 or later**, bundled docs live at `node_modules/next/dist/docs/`. Before doing PPR work, Cache Components work, or any non-trivial Next.js task, read the relevant doc there — your training data may be outdated. The bundled docs are the source of truth.

---

## Working with the user

### Onboarding

- If the user already gave a URL, cookies, and task — skip questions, `open` and go.
- Otherwise ask only what's missing: dev server URL (running?), session cookies if behind login.
- For cookies, give the user two options: (1) DevTools → Application → Cookies, export as `[{"name":"session","value":"..."}]`, or (2) "Copy as cURL" from DevTools → Network on any authenticated request — extract the cookies from the header yourself.
- Never say "ready, what would you like to do?". Never auto-discover (port scans, `project`, config reads) before being asked.

### Show, don't tell

- `screenshot` after every navigation, code change, or visual finding. Always caption it.
- Don't narrate what a screenshot shows. State your conclusion or next action.

### Escalate, don't decide

- Suspense boundary placement and fallback UI — design with the user.
- Caching decisions (staleness, visibility) — the user's call, not yours.
- "Make this page faster" without context — ask: cold URL hit or client navigation? Don't guess, don't do both.

---

## Headless mode

By default the browser opens headed (visible window). For CI or environments with no display, set `NEXT_BROWSER_HEADLESS=1` to run headless.

---

## Commands

### Browser lifecycle

| Command | Description |
|---|---|
| `open <url> [--cookies-json <file>]` | Launch browser and navigate (with optional cookies) |
| `close` | Close browser and kill daemon |

```bash
$ next-browser open http://localhost:3000
$ next-browser open http://localhost:3000 --cookies-json cookies.json
# Cookie file format: [{"name":"authorization","value":"Bearer ..."}]
```

---

### Navigation

| Command | Description |
|---|---|
| `goto <url>` | Full-page navigation (new document load) |
| `push [path]` | Client-side navigation (interactive picker if no path) |
| `back` | Go back in history |
| `reload` | Reload current page |
| `restart-server` | Restart the Next.js dev server (clears caches — last resort only) |
| `ssr lock` | Block external scripts on all navigations (SSR-only mode) |
| `ssr unlock` | Re-enable external scripts |

---

### Inspection

| Command | Description |
|---|---|
| `tree` | Full React component tree (hierarchy, IDs, keys) |
| `tree <id>` | Inspect one component (props, hooks, state, source location) |
| `snapshot` | Accessibility tree with `[ref=eN]` markers on interactive elements |
| `errors` | Build and runtime errors for the current page |
| `logs` | Recent dev server log output |
| `browser-logs` | Browser-side console output (log, warn, error, info) |
| `network [idx]` | List network requests, or inspect one (headers, body) |

```bash
$ next-browser snapshot
- navigation "Main"
  - link "Home" [ref=e0]
  - link "Dashboard" [ref=e1]
- main
  - heading "Settings"
  - tablist
    - tab "General" [ref=e2] (selected)
    - tab "Security" [ref=e3]

$ next-browser tree
# Columns: depth id parent name
0 38167 - Root
1 38168 38167 HeadManagerContext.Provider
...
224 46375 46374 DeploymentsProvider

$ next-browser tree 46375
path: Root > ... > DeploymentsProvider
DeploymentsProvider #46375
props:
  children: [<Lazy />, ...]
hooks:
  Router: undefined (2 sub)
source: app/.../context.tsx:180:10
```

---

### Interaction

| Command | Description |
|---|---|
| `click <ref\|text\|selector>` | Click via real pointer events (works with Radix, Headless UI) |
| `fill <ref\|selector> <value>` | Fill a text input or textarea |
| `eval [ref] <script>` | Run JS in page context |
| `viewport [WxH]` | Show or set viewport size |

```bash
$ next-browser click e3          # ref from snapshot
$ next-browser click "Security"  # plain text
$ next-browser fill e4 "myuser"
$ next-browser viewport 375x812  # mobile breakpoint
$ next-browser eval 'document.title'
```

---

### Performance & PPR

| Command | Description |
|---|---|
| `perf [url]` | Core Web Vitals + React hydration timing in one pass |
| `renders start` | Begin recording React re-renders |
| `renders stop [--json]` | Stop and print per-component render profile |
| `ppr lock` | Freeze dynamic content to inspect the static shell |
| `ppr unlock` | Resume dynamic content and print shell analysis |

```bash
$ next-browser perf http://localhost:3000/dashboard
# Core Web Vitals
  TTFB    42ms
  LCP     1205.3ms
  CLS     0.03
# React Hydration — 65.5ms

$ next-browser renders start
$ next-browser renders stop
# 426 renders (38 mounts + 388 re-renders) across 38 components
# FPS: avg 120, min 106
```

---

### Screenshots

| Command | Description |
|---|---|
| `screenshot [caption] [--full-page]` | Viewport (or full page) PNG to temp file |
| `preview [caption]` | Screenshot + open in viewer window |

---

### Next.js MCP

| Command | Description |
|---|---|
| `page` | Route segments for the current URL |
| `project` | Project root and dev server URL |
| `routes` | All app router routes |
| `action <id>` | Inspect a server action by ID |

---

## Scenarios

### Debug a visual or interaction issue

1. `open http://localhost:3000`
2. `snapshot` — discover interactive elements
3. `click eN` / `fill eN value` — interact
4. `errors` — check for runtime errors
5. `screenshot "before fix"` — document state
6. Make the code change, HMR picks it up
7. `screenshot "after fix"` — verify

### Profile performance

```bash
$ next-browser perf http://localhost:3000/dashboard
# → Core Web Vitals + hydration timing

$ next-browser renders start
# reproduce slow interaction
$ next-browser renders stop
# → per-component render counts, self time, change reasons
```

### Debug PPR shell

```bash
$ next-browser ppr lock
$ next-browser goto http://localhost:3000/page
$ next-browser screenshot "PPR shell — locked"
$ next-browser ppr unlock
# → Shell analysis: which Suspense boundaries are holes and why
```

### Test responsive layout

```bash
$ next-browser viewport 375x812   # mobile
$ next-browser screenshot "mobile"
$ next-browser viewport 1440x900  # desktop
$ next-browser screenshot "desktop"
```

### Debug component re-renders

1. `renders start`
2. `goto` the page (captures hydration)
3. Reproduce the interaction
4. `renders stop` — read Mounts vs Re-renders, Self time, change reasons
5. `tree <id>` the expensive component — check source, props, hooks
6. Fix, HMR picks it up, re-run `renders start/stop` to verify

## ./AGENTS.md
﻿
---

# GitHub Copilot Customisation

<table>
<tr>
<th width="50%">DE Deutsch</th>
<th width="50%">EN English</th>
</tr>
<tr>
<td>Dieses Projekt nutzt GitHub Copilot Agents und Instructions, um eine strukturierte und produktive KI-gestützte Entwicklung zu ermöglichen.</td>
<td>This project uses GitHub Copilot Agents and Instructions to enable a structured and productive AI-assisted development workflow.</td>
</tr>
</table>

---

## Übersicht / Overview

| Typ / Type | Pfad / Path | Beschreibung / Description |
|---|---|---|
| **Global Instructions** | `.github/copilot-instructions.md` | Projektweite Basisregeln für alle Copilot-Interaktionen. / Project-wide baseline rules for all Copilot interactions. |
| **Instructions** | `.github/instructions/*.instructions.md` | Dateimuster-spezifische Coding-Richtlinien. / File-pattern-scoped coding guidelines. |
| **Agents** | `.github/agents/*.agent.md` | Spezialisierte KI-Personas für verschiedene Phasen. / Specialised AI personas for different development phases. |

---

## `copilot-instructions.md` — Globale Basisregeln / Global Baseline Rules

**Pfad / Path:** `.github/copilot-instructions.md`

<table>
<tr>
<th width="50%">DE Was ist das?</th>
<th width="50%">EN What is it?</th>
</tr>
<tr>
<td>Diese Datei wird bei <strong>jeder</strong> Copilot-Interaktion automatisch geladen. Sie definiert den Tech-Stack, kritische Regeln (async APIs, Server Components, Supabase), und Code-Stil-Konventionen für das gesamte Projekt.</td>
<td>This file is automatically loaded on <strong>every</strong> Copilot interaction. It defines the tech stack, critical rules (async APIs, Server Components, Supabase), and code style conventions for the whole project.</td>
</tr>
</table>

<table>
<tr>
<th width="50%">DE Wann bearbeiten?</th>
<th width="50%">EN When to edit?</th>
</tr>
<tr>
<td>
- Tech-Stack ändert sich (z. B. neue Bibliothek wird Standardmuster)<br>
- Neue projektweite Regeln sollen für alle Copilot-Interaktionen gelten<br>
- Team-Konventionen ändern sich<br>
<strong>⚠️ Kurz halten</strong> — diese Datei wird immer in den Kontext geladen.
</td>
<td>
- Tech stack changes (e.g. a new library becomes a standard pattern)<br>
- New project-wide rules need to apply to all Copilot interactions<br>
- Team conventions change<br>
<strong>⚠️ Keep it short</strong> — this file is always loaded into context.
</td>
</tr>
</table>

---

## Instructions — Datei-spezifische Richtlinien / File-Scoped Guidelines

<table>
<tr>
<th width="50%">DE Was sind Instructions?</th>
<th width="50%">EN What are Instructions?</th>
</tr>
<tr>
<td>Instructions sind Richtlinien, die automatisch geladen werden, wenn Copilot an Dateien arbeitet, die dem <code>applyTo</code>-Glob-Muster entsprechen. Sie ergänzen die globalen Regeln mit datei-spezifischen Details.</td>
<td>Instructions are guidelines that are automatically loaded when Copilot works on files matching the <code>applyTo</code> glob pattern. They extend the global rules with file-type-specific details.</td>
</tr>
</table>

### Vorhandene Instructions / Available Instructions

| Datei / File | `applyTo` | Zweck / Purpose |
|---|---|---|
| `nextjs.instructions.md` | `**/*.tsx, **/*.ts, **/*.jsx, **/*.js` | Next.js 16 App Router Patterns, async APIs, Server/Client Components |
| `tailwind.instructions.md` | `**/*.tsx, **/*.jsx, **/*.css` | Tailwind CSS v4 Utility-First Patterns, Responsive Design |
| `typescript.instructions.md` | `**/*.ts, **/*.tsx` | TypeScript Strict Mode, Naming Conventions, Type Safety |
| `supabase.instructions.md` | `**/supabase/**, **/*supabase*` | Supabase Client Setup, RLS, Migrations, Schema Awareness |

### Eigene Instructions erstellen / Creating Custom Instructions

<table>
<tr>
<th width="50%">DE Anleitung</th>
<th width="50%">EN Guide</th>
</tr>
<tr>
<td>Erstelle eine <code>.instructions.md</code>-Datei in <code>.github/instructions/</code> mit YAML-Frontmatter. Das <code>applyTo</code>-Feld akzeptiert Glob-Muster und bestimmt, für welche Dateien die Regeln gelten.</td>
<td>Create a <code>.instructions.md</code> file in <code>.github/instructions/</code> with YAML frontmatter. The <code>applyTo</code> field accepts glob patterns and determines which files the rules apply to.</td>
</tr>
</table>

```yaml
---
applyTo: "**/*.tsx"
---
# Your instructions here
```

---

## Agents — KI-Personas / AI Personas

<table>
<tr>
<th width="50%">DE Was sind Agents?</th>
<th width="50%">EN What are Agents?</th>
</tr>
<tr>
<td>Agents sind spezialisierte KI-Personas. Du rufst sie in VS Code über das Chat-Panel mit <code>@agent-name</code> auf. Jeder Agent hat eine klar definierte Rolle, einen Workflow und Regeln.</td>
<td>Agents are specialised AI personas. You invoke them in VS Code via the Chat panel using <code>@agent-name</code>. Each agent has a clearly defined role, workflow, and rules.</td>
</tr>
</table>

---

### `@planner` — Planer

<table>
<tr>
<th width="50%">DE</th>
<th width="50%">EN</th>
</tr>
<tr>
<td><strong>Zweck:</strong> Technische Planung und Anforderungsanalyse vor dem Coding.</td>
<td><strong>Purpose:</strong> Technical planning and requirements analysis before coding.</td>
</tr>
<tr>
<td><strong>Wann verwenden:</strong><br>— Vor dem Start eines neuen Features<br>— Wenn die Anforderungen unklar sind<br>— Vor Refactoring oder Migrations-Aufgaben</td>
<td><strong>When to use:</strong><br>— Before starting a new feature<br>— When requirements are unclear<br>— Before refactoring or migration tasks</td>
</tr>
<tr>
<td><strong>Was er tut:</strong><br>✔ Codebase erkunden und Kontext sammeln<br>✔ Betroffene Dateien und Module identifizieren<br>✔ Nummerierten Implementierungsplan erstellen<br>✘ <strong>Schreibt keinen Code</strong></td>
<td><strong>What it does:</strong><br>✔ Explore codebase and gather context<br>✔ Identify affected files and modules<br>✔ Create a numbered implementation plan<br>✘ <strong>Does not write code</strong></td>
</tr>
<tr>
<td><strong>Verwendung:</strong> <code>@planner Füge eine Authentifizierungsseite hinzu</code></td>
<td><strong>Usage:</strong> <code>@planner Add an authentication page</code></td>
</tr>
</table>

---

### `@developer` — Entwickler

<table>
<tr>
<th width="50%">DE</th>
<th width="50%">EN</th>
</tr>
<tr>
<td><strong>Zweck:</strong> Strukturierte Feature-Implementierung mit Qualitätsprüfungen.</td>
<td><strong>Purpose:</strong> Structured feature implementation with quality checks.</td>
</tr>
<tr>
<td><strong>Wann verwenden:</strong><br>— Nach der Planung mit <code>@planner</code><br>— Zur Implementierung von Features, Seiten, API-Routen<br>— Für Datenbankänderungen und Migrationen</td>
<td><strong>When to use:</strong><br>— After planning with <code>@planner</code><br>— To implement features, pages, API routes<br>— For database changes and migrations</td>
</tr>
<tr>
<td><strong>Vierphasiger Prozess:</strong><br>1. <strong>Vorbereitung</strong> — Plan lesen, Code verstehen<br>2. <strong>Implementierung</strong> — Schrittweise, Fehler sofort beheben<br>3. <strong>Verifikation (PFLICHT)</strong> — <code>typecheck</code> → <code>lint</code> → <code>build</code> → lokal testen<br>4. <strong>Dokumentation</strong> — Handbook aktualisieren</td>
<td><strong>Four-phase process:</strong><br>1. <strong>Preparation</strong> — Read plan, understand code<br>2. <strong>Implementation</strong> — Step by step, fix errors immediately<br>3. <strong>Verification (MANDATORY)</strong> — <code>typecheck</code> → <code>lint</code> → <code>build</code> → test locally<br>4. <strong>Documentation</strong> — Update handbook</td>
</tr>
<tr>
<td><strong>Verwendung:</strong> <code>@developer Implementiere diesen Plan: [Plan einfügen]</code></td>
<td><strong>Usage:</strong> <code>@developer Implement this plan: [paste plan]</code></td>
</tr>
</table>

---

### `@reviewer` — Code-Reviewer

<table>
<tr>
<th width="50%">DE</th>
<th width="50%">EN</th>
</tr>
<tr>
<td><strong>Zweck:</strong> Code-Review und Qualitätssicherung vor dem PR.</td>
<td><strong>Purpose:</strong> Code review and quality assurance before a PR.</td>
</tr>
<tr>
<td><strong>Wann verwenden:</strong><br>— Bevor ein PR erstellt wird<br>— Nach einer Implementierung zur Qualitätsprüfung<br>— Zur Sicherheits- und Performance-Analyse</td>
<td><strong>When to use:</strong><br>— Before creating a PR<br>— After implementation for quality check<br>— For security and performance audits</td>
</tr>
<tr>
<td><strong>Was er tut:</strong><br>✔ Strukturierte Checkliste (Qualität, Next.js 16, Supabase-Sicherheit, Styling)<br>✔ Alle Checks ausführen (typecheck, lint, build)<br>✔ Review-Bericht mit Status (✅ / ⚠️ / ❌)</td>
<td><strong>What it does:</strong><br>✔ Structured checklist (quality, Next.js 16, Supabase security, styling)<br>✔ Run all checks (typecheck, lint, build)<br>✔ Review report with status (✅ / ⚠️ / ❌)</td>
</tr>
<tr>
<td><strong>Verwendung:</strong> <code>@reviewer Überprüfe die Änderungen in src/app/dashboard/</code></td>
<td><strong>Usage:</strong> <code>@reviewer Review the changes in src/app/dashboard/</code></td>
</tr>
</table>

---

## Tools

| Tool | Pfad / Path | Zweck / Purpose |
|---|---|---|
| **next-browser** | `.github/skills/next-browser/SKILL.md` | CLI that exposes React DevTools and the Next.js dev overlay as shell commands — component trees, props, errors, performance, screenshots — structured output for AI agents. |
| **anforderungsdokument** | `.github/skills/anforderungsdokument/SKILL.md` | WAMOCON-Dokumentgenerator: erstellt professionelle Word-Dokumente (.docx) — Anforderungsdokument (neue App-Ideen, 10 Kapitel) oder Analyse-Dokument (Technologie/Strategie, 14 Kapitel). |

### `next-browser` — AI-Driven Browser for Next.js

<table>
<tr>
<th width="50%">DE Was ist das?</th>
<th width="50%">EN What is it?</th>
</tr>
<tr>
<td><code>@vercel/next-browser</code> ist ein CLI-Tool, das React DevTools und das Next.js Dev-Overlay als Shell-Befehle bereitstellt. Agents können den Browser steuern, Komponenten inspizieren, Fehler lesen und Performance prüfen — ohne manuell durch DevTools zu klicken.</td>
<td><code>@vercel/next-browser</code> is a CLI tool that exposes React DevTools and the Next.js dev overlay as shell commands. Agents can drive the browser, inspect components, read errors, and check performance — without manually clicking through DevTools.</td>
</tr>
<tr>
<td><strong>Wann verwenden:</strong><br>— Nach der Implementierung zur visuellen Verifikation<br>— Debugging von Runtime-Fehlern oder Re-Render-Problemen<br>— Performance-Analyse (Core Web Vitals, Hydration)<br>— PPR-Shell-Debugging</td>
<td><strong>When to use:</strong><br>— After implementation for visual verification<br>— Debugging runtime errors or re-render issues<br>— Performance analysis (Core Web Vitals, hydration timing)<br>— PPR shell debugging</td>
</tr>
<tr>
<td><strong>Installation:</strong><br><code>npm install -g @vercel/next-browser</code><br><code>playwright install chromium</code><br>Benötigt Node >= 20</td>
<td><strong>Install:</strong><br><code>npm install -g @vercel/next-browser</code><br><code>playwright install chromium</code><br>Requires Node >= 20</td>
</tr>
<tr>
<td><strong>Wichtigste Befehle:</strong><br><code>next-browser open &lt;url&gt;</code> — Browser starten<br><code>next-browser snapshot</code> — Accessibility-Tree + klickbare Refs<br><code>next-browser errors</code> — Build- und Runtime-Fehler<br><code>next-browser perf</code> — Core Web Vitals + Hydration<br><code>next-browser screenshot</code> — Viewport als PNG<br><code>next-browser tree</code> — React-Komponentenbaum</td>
<td><strong>Key commands:</strong><br><code>next-browser open &lt;url&gt;</code> — launch browser<br><code>next-browser snapshot</code> — accessibility tree + clickable refs<br><code>next-browser errors</code> — build and runtime errors<br><code>next-browser perf</code> — Core Web Vitals + hydration timing<br><code>next-browser screenshot</code> — viewport as PNG<br><code>next-browser tree</code> — React component tree</td>
</tr>
<tr>
<td><strong>Vollständige Dokumentation:</strong> <code>.github/skills/next-browser/SKILL.md</code></td>
<td><strong>Full documentation:</strong> <code>.github/skills/next-browser/SKILL.md</code></td>
</tr>
</table>

---

### `anforderungsdokument` — WAMOCON Anforderungs- und Analysedokument

<table>
<tr>
<th width="50%">DE Was ist das?</th>
<th width="50%">EN What is it?</th>
</tr>
<tr>
<td>Der Skill erstellt ein <strong>vollstaendiges Word-Dokument</strong> (<code>.docx</code>) mit allen 15 Kapiteln: Marktanalyse, Wettbewerb, Zielgruppe, Loesungsoptionen, Anforderungen V1, Kostenanalyse, Umsetzungsplan und Expertenempfehlung. Der Nutzer traegt seine App-Idee in <code>IDEA.md</code> ein — der Skill erledigt den Rest. Das Dokument dient als Freigabedokument fuer die Geschaeftsfuehrung. <strong>Erst nach Freigabe beginnt die Implementierung.</strong></td>
<td>The skill creates one <strong>complete Word document</strong> (<code>.docx</code>) with all 15 chapters: market analysis, competition, target audience, solution options, requirements v1, cost analysis, implementation plan, and expert recommendation. The user fills in their idea in <code>IDEA.md</code> — the skill does the rest. The document serves as an approval document for management. <strong>Implementation only starts after approval.</strong></td>
</tr>
</table>

<table>
<tr>
<th width="50%">DE Workflow (Schritt fuer Schritt)</th>
<th width="50%">EN Workflow (Step by Step)</th>
</tr>
<tr>
<td>
<ol>
<li><strong>IDEA.md ausfullen</strong> — Projektname, Beschreibung, Zielgruppe, Kernproblem, Funktionen, Ersteller, Empfaenger eintragen. Die Vorlage liegt im Projekt-Root.</li>
<li><strong>Skill aufrufen</strong> — Im GitHub Copilot Chat oder Claude (Prompt unten)</li>
<li><strong>KI generiert das Dokument</strong> — Skript wird erstellt und ausgefuehrt</li>
<li><strong>Ausgabe:</strong> <code>public/Anforderungsdokument_[Name].docx</code></li>
<li><strong>Pruefen</strong> — Dokument oeffnen, Zahlen und Quellen verifizieren</li>
<li><strong>Einreichen</strong> — An Empfaenger in der Geschaeftsfuehrung</li>
<li><strong>Nach Freigabe:</strong> <code>@planner</code> aufrufen und Implementierung starten</li>
</ol>
</td>
<td>
<ol>
<li><strong>Fill in IDEA.md</strong> — Enter project name, description, target audience, core problem, features, creator, recipient. Template is in the project root.</li>
<li><strong>Invoke skill</strong> — In GitHub Copilot Chat or Claude (see prompt below)</li>
<li><strong>AI generates the document</strong> — Script is created and executed</li>
<li><strong>Output:</strong> <code>public/Anforderungsdokument_[Name].docx</code></li>
<li><strong>Review</strong> — Open document, verify numbers and sources</li>
<li><strong>Submit</strong> — To the recipient in management</li>
<li><strong>After approval:</strong> Call <code>@planner</code> and start implementation</li>
</ol>
</td>
</tr>
</table>

<table>
<tr>
<th width="50%">DE Prompt — GitHub Copilot Chat</th>
<th width="50%">EN Prompt — GitHub Copilot Chat</th>
</tr>
<tr>
<td>

Oeffne den Copilot Chat in VS Code (<code>Ctrl+Alt+I</code>) und schreibe:

```
@workspace Du bist der Senior Business Analyst bei WAMOCON GmbH.
Lies IDEA.md im Projekt-Root und .github/skills/anforderungsdokument/SKILL.md.
Erstelle das vollstaendige WAMOCON Anforderungs- und Analysedokument:
- Alle 15 Pflichtkapitel in der richtigen Reihenfolge
- Echte Marktdaten mit deutschen Quellen (Bitkom, IfM, BMWK, Statista)
- Vollstaendige Anforderungstabellen (K- und B-Praefix)
- Loesungsoptionen-Vergleich, Amortisierungsrechnung als Tabellen
- Ausschliesslich Deutsch mit echten Umlauten (ae oe ue NEIN, ä ö ü JA)
Skript: scripts/generate-anforderungsdokument.mjs
Ausgabe: public/Anforderungsdokument_[ProjektName].docx
```

</td>
<td>

Open Copilot Chat in VS Code (<code>Ctrl+Alt+I</code>) and write:

```
@workspace You are the Senior Business Analyst at WAMOCON GmbH.
Read IDEA.md in the project root and .github/skills/anforderungsdokument/SKILL.md.
Create the complete WAMOCON Requirements and Analysis Document:
- All 15 mandatory chapters in order
- Real market data with German sources (Bitkom, IfM, BMWK, Statista)
- Full requirements tables (K- and B-prefix)
- Solution options comparison, amortization table
- German only, real umlauts (ae oe ue NEIN, ä ö ü JA)
Script: scripts/generate-anforderungsdokument.mjs
Output: public/Anforderungsdokument_[ProjectName].docx
```

</td>
</tr>
</table>

<th width="50%">DE Prompt — Claude Code</th>
<th width="50%">EN Prompt — Claude Code</th>
</tr>
<tr>
<td>

```
Du bist der Senior Business Analyst bei WAMOCON GmbH.
1. Lies IDEA.md im Projekt-Root
2. Lies .github/skills/anforderungsdokument/SKILL.md fuer alle Regeln
3. Erstelle scripts/generate-anforderungsdokument.mjs mit echtem Content
4. Fuehre aus: node scripts/generate-anforderungsdokument.mjs
5. Bestaetigung: public/Anforderungsdokument_[ProjektName].docx
```

</td>
<td>

```
You are the Senior Business Analyst at WAMOCON GmbH.
1. Read IDEA.md in the project root
2. Read .github/skills/anforderungsdokument/SKILL.md for all rules
3. Create scripts/generate-anforderungsdokument.mjs with real content
4. Run: node scripts/generate-anforderungsdokument.mjs
5. Confirm: public/Anforderungsdokument_[ProjectName].docx
```

</td>
</tr>
</table>

<table>
<tr>
<th width="50%">DE Wo stehen welche Dateien?</th>
<th width="50%">EN Where are which files?</th>
</tr>
<tr>
<td>

| Datei | Zweck |
|---|---|
| <code>IDEA.md</code> | Projektidee ausfuellen (Nutzer) |
| <code>.github/skills/anforderungsdokument/SKILL.md</code> | Alle Regeln und Kapitelstruktur (KI liest das) |
| <code>scripts/generate-anforderungsdokument.mjs</code> | Generator-Skript (KI schreibt das) |
| <code>public/Anforderungsdokument_[Name].docx</code> | Fertige Word-Datei (Ausgabe) |

</td>
<td>

| File | Purpose |
|---|---|
| <code>IDEA.md</code> | Fill in project idea (user) |
| <code>.github/skills/anforderungsdokument/SKILL.md</code> | All rules and chapter structure (AI reads this) |
| <code>scripts/generate-anforderungsdokument.mjs</code> | Generator script (AI writes this) |
| <code>public/Anforderungsdokument_[Name].docx</code> | Finished Word file (output) |

</td>
</tr>
</table>

<table>
<tr>
<th width="50%">DE Voraussetzungen</th>
<th width="50%">EN Prerequisites</th>
</tr>
<tr>
<td>
— Node.js >= 18<br>
— <code>npm install docx</code> (einmalig, falls noch nicht installiert)<br>
— Ausfuehren: <code>npm run gen:anforderungsdokument</code>
</td>
<td>
— Node.js >= 18<br>
— <code>npm install docx</code> (once, if not already installed)<br>
— Run: <code>npm run gen:anforderungsdokument</code>
</td>
</tr>
</table>

<table>
<tr>
<th width="50%">DE Vollstaendige Dokumentation</th>
<th width="50%">EN Full Documentation</th>
</tr>
<tr>
<td><strong>Skill-Datei:</strong> <code>.github/skills/anforderungsdokument/SKILL.md</code><br>Enthaelt Workflow, alle 15 Kapitel, Formatregeln, Code-Boilerplate, Prompt-Vorlagen und Qualitaetsstandards.</td>
<td><strong>Skill file:</strong> <code>.github/skills/anforderungsdokument/SKILL.md</code><br>Contains the workflow, all 15 chapters, formatting rules, code boilerplate, prompt templates and quality standards.</td>
</tr>
</table>

---

## Empfohlener Workflow / Recommended Workflow

<table>
<tr>
<th width="50%">DE Schritt</th>
<th width="50%">EN Step</th>
</tr>
<tr>
<td><strong>Phase 0 — Anforderungen (neues Projekt):</strong><br>
1. <code>IDEA.md</code> im Projekt-Root ausfullen<br>
2. <strong><code>anforderungsdokument</code></strong> Skill aufrufen: Dokument generieren lassen<br>
3. Dokument pruefen und zur Freigabe einreichen<br>
4. Nach Freigabe: Implementierung starten</td>
<td><strong>Phase 0 — Requirements (new project):</strong><br>
1. Fill in <code>IDEA.md</code> in the project root<br>
2. Call <strong><code>anforderungsdokument</code></strong> skill: let it generate the document<br>
3. Review document and submit for approval<br>
4. After approval: start implementation</td>
</tr>
<tr>
<td><strong>Phase 1 — Planung:</strong><br>
<strong><code>@planner</code></strong> — Aufgabe analysieren und Implementierungsplan erstellen</td>
<td><strong>Phase 1 — Planning:</strong><br>
<strong><code>@planner</code></strong> — Analyse the task and create an implementation plan</td>
</tr>
<tr>
<td><strong>Phase 2 — Implementierung:</strong><br>
<strong><code>@developer</code></strong> — Plan entgegennehmen und schrittweise implementieren</td>
<td><strong>Phase 2 — Implementation:</strong><br>
<strong><code>@developer</code></strong> — Receive plan and implement step by step</td>
</tr>
<tr>
<td><strong>Phase 3 — Qualitaetspruefung:</strong><br>
<strong><code>@reviewer</code></strong> — Code pruefen, bevor ein PR erstellt wird</td>
<td><strong>Phase 3 — Quality review:</strong><br>
<strong><code>@reviewer</code></strong> — Review code before creating a PR</td>
</tr>
</table>

---

## Eigene Agents erstellen / Creating Custom Agents

<table>
<tr>
<th width="50%">DE Anleitung</th>
<th width="50%">EN Guide</th>
</tr>
<tr>
<td>Erstelle eine <code>.agent.md</code>-Datei in <code>.github/agents/</code> mit YAML-Frontmatter. Definiere Rolle, Workflow und Regeln.</td>
<td>Create a <code>.agent.md</code> file in <code>.github/agents/</code> with YAML frontmatter. Define role, workflow, and rules.</td>
</tr>
</table>

```yaml
---
name: MyAgent
description: >
  Description of what this agent does.
---
# Agent: MyAgent

## Role
...

## Workflow
...
```

---

## Wenn ein Agent schlecht antwortet / When an Agent Responds Poorly

<table>
<tr>
<th width="50%">DE Problem & Lösung</th>
<th width="50%">EN Problem & Solution</th>
</tr>
<tr>
<td><strong>Agent ignoriert Regeln aus der Instructions-Datei</strong><br>→ Prüfe das <code>applyTo</code>-Glob-Muster — stimmt es mit der Datei überein, die du bearbeitest? Teste mit: <code>**/*.ts</code> statt <code>src/**/*.ts</code></td>
<td><strong>Agent ignores rules from an Instructions file</strong><br>→ Check the <code>applyTo</code> glob pattern — does it match the file you are editing? Try broader patterns: <code>**/*.ts</code> instead of <code>src/**/*.ts</code></td>
</tr>
<tr>
<td><strong>Agent befolgt den Workflow nicht (z. B. überspringt typecheck)</strong><br>→ Öffne die <code>.agent.md</code>-Datei und mache die Anweisung strikter. Ersetze "sollte" durch "muss". Füge am Ende eine Zusammenfassung hinzu: <em>"Bevor du antwortest, liste alle abgeschlossenen Schritte auf."</em></td>
<td><strong>Agent does not follow the workflow (e.g. skips typecheck)</strong><br>→ Open the <code>.agent.md</code> file and make the instruction stricter. Replace "should" with "must". Add a reminder at the end: <em>"Before responding, list all completed steps."</em></td>
</tr>
<tr>
<td><strong>Agent schreibt schlechten Next.js-Code (z. B. falsche API-Nutzung)</strong><br>→ Füge ein konkretes Beispiel in die <code>nextjs.instructions.md</code> ein. Copilot folgt Beispielen besser als abstrakten Regeln.</td>
<td><strong>Agent writes bad Next.js code (e.g. wrong API usage)</strong><br>→ Add a concrete code example to <code>nextjs.instructions.md</code>. Copilot follows examples better than abstract rules.</td>
</tr>
<tr>
<td><strong>Agent "vergisst" den Kontext nach langen Gesprächen</strong><br>→ Starte ein neues Chat-Fenster. Langer Kontext verdrängt Instructions. Übergib den Plan explizit: <em>"Hier ist der Plan: [Plan]. Bitte implementiere Schritt 3."</em></td>
<td><strong>Agent "forgets" context after long conversations</strong><br>→ Start a new chat window. Long context pushes out instructions. Pass the plan explicitly: <em>"Here is the plan: [plan]. Please implement step 3."</em></td>
</tr>
<tr>
<td><strong>Agent antwortet auf Englisch statt Deutsch (oder umgekehrt)</strong><br>→ Füge in <code>copilot-instructions.md</code> eine Sprachanweisung hinzu: <em>"Antworte immer auf Deutsch."</em> — oder sprich den Agent in der gewünschten Sprache an.</td>
<td><strong>Agent responds in German instead of English (or vice versa)</strong><br>→ Add a language instruction to <code>copilot-instructions.md</code>: <em>"Always respond in English."</em> — or address the agent in your preferred language.</td>
</tr>
<tr>
<td><strong>Agent überschreitet den Plan / macht ungebetene Änderungen</strong><br>→ Füge in der <code>.agent.md</code> unter "Rules" hinzu: <em>"Ändere nur Dateien, die explizit im Plan genannt sind. Keine ungebetenen Refactors."</em></td>
<td><strong>Agent exceeds the plan / makes unrequested changes</strong><br>→ Add to the <code>.agent.md</code> under "Rules": <em>"Only modify files explicitly listed in the plan. No unrequested refactoring."</em></td>
</tr>
</table>

---

## Referenzen / References

- [GitHub Copilot Customisation Docs](https://docs.github.com/en/copilot/customizing-copilot)
- [awesome-copilot](https://github.com/github/awesome-copilot) — Beispiele und Best Practices / Examples and best practices

## ./Anforderungsdokument_Vertragsmanager_v1.0.md
﻿# Anforderungsdokument Vertragsmanager
## Softwareprojekt Welle 6

---

| | |
|---|---|
| **Projekt** | Vertragsmanager |
| **Unternehmen** | WAMOCON GmbH |
| **App Version** | 1 |
| **Erstellt von** | Daniel Moretz |
| **Eingereicht an** | Waleri Moretz (GeschÃ¤ftsfÃ¼hrung) |
| **Datum** | 14. April 2026 |
| **Vertraulichkeit** | Intern vertraulich |
| **Status** | Zur Freigabe eingereicht |

---

## 1. Zusammenfassung

### 1.1 Die Idee

**Vertragsmanager** ist eine webbasierte SaaS-Plattform fÃ¼r kleine und mittlere Unternehmen im DACH-Raum. Sie lÃ¶st ein strukturelles und tÃ¤glich spÃ¼rbares Problem: Wer ein Unternehmen fÃ¼hrt oder verwaltet, hat Dutzende laufende Abonnements, VertrÃ¤ge und Lizenzen â€” verteilt auf E-Mail-PostfÃ¤cher, PDF-Ordner, Schubladenarchive und das GedÃ¤chtnis einzelner Mitarbeiter. KÃ¼ndigungsfristen werden verpasst, Abos laufen jahrelang ungenutzt weiter, und niemand hat einen vollstÃ¤ndigen Ãœberblick Ã¼ber die tatsÃ¤chlichen monatlichen Kosten.

Vertragsmanager lÃ¶st dieses Problem mit einem klaren Dreischritt: Ein Dokument fotografieren oder hochladen, automatisch die Kerndaten per OCR extrahieren lassen, und sofort in einer Ã¼bersichtlichen Zentrale verwalten â€” inklusive proaktiver Erinnerungen vor KÃ¼ndigungsfristen, konfigurierbarer Kosten-Dashboards und rechtskonformer KÃ¼ndigungsvorlagen fÃ¼r den DACH-Raum.

### 1.2 Warum jetzt?

Im Jahr 2025 nutzt ein durchschnittliches deutsches KMU mit 10 bis 250 Mitarbeitenden im Median **34 verschiedene SaaS-Abonnements** gleichzeitig â€” Tendenz stark steigend (Bitkom, 2025). Die Gesamtausgaben fÃ¼r Software-Abonnements im DACH-KMU-Segment werden fÃ¼r 2025 auf **18,7 Milliarden Euro** geschÃ¤tzt.

Das Kernproblem ist gut dokumentiert: Branchenstudien zeigen, dass Unternehmen im Schnitt 17 bis 23 Prozent ihrer Abo-Ausgaben fÃ¼r ungenutzte oder redundante Dienste aufwenden (C+R Research, 2024). Bei einem typischen KMU mit 50.000 Euro Jahresausgaben fÃ¼r Software sind das bis zu 11.500 Euro verbranntes Budget â€” ohne jedes strategische Bewusstsein.

Gleichzeitig haben GroÃŸanbieter wie Adobe, Microsoft und Salesforce ihre Lizenzpreise 2024/25 um 10 bis 25 Prozent erhÃ¶ht. Konjunktureller Druck und explodierende Softwarekosten zwingen KMU dazu, Ausgaben aktiv zu steuern. Ein DSGVO-natives, DACH-fokussiertes Tool, das dieses Problem elegant und ohne Einarbeitungsaufwand lÃ¶st, findet heute einen reifen, zahlungsbereiten Markt.

Parallel dazu treiben regulatorische Rahmen wie die CSRD und das Lieferkettensorgfaltspflichtengesetz (LkSG) den Bedarf nach strukturierter Vertragsdokumentation in die HÃ¶he. Unternehmen mÃ¼ssen nachweisen kÃ¶nnen, welche Dienstleister sie nutzen, unter welchen Bedingungen und seit wann. Vertragsmanager liefert diese Basis.

---

## 2. Marktanalyse: Fokus DACH

### 2.1 KMU-Softwarelandschaft und Zielgruppe

Deutschland hat rund **3,5 Millionen KMU** â€” definiert als Unternehmen mit weniger als 250 Mitarbeitenden. Sie beschÃ¤ftigen 55 Prozent aller sozialversicherungspflichtig BeschÃ¤ftigten und erzielen 35 Prozent des gesamten Umsatzes in Deutschland. Ã–sterreich und die Schweiz ergÃ¤nzen den DACH-Raum mit weiteren 600.000 KMU.

Nicht alle 3,5 Millionen KMU sind sofortige Zielkunden. Wer kein einziges SaaS-Abo hat, braucht Vertragsmanager nicht. Wer ausschlieÃŸlich einmalige Softwarelizenzen nutzt und keine laufenden VertrÃ¤ge verwaltet, hat keinen unmittelbaren Leidensdruck. Die realistische Kernzielgruppe sind KMU, die **mindestens fÃ¼nf laufende digitale Abonnements oder VertrÃ¤ge** gleichzeitig verwalten â€” das betrifft heute schÃ¤tzungsweise **2,1 bis 2,6 Millionen Unternehmen** im DACH-Raum, wachsend auf 3 bis 4 Millionen bis 2028.

Quellen: Bitkom Digitalatlas 2025, Statistisches Bundesamt 2025, IFM Bonn 2024

### 2.2 Wachstum des SaaS-Marktes und was das bedeutet

Der globale SaaS-Markt wuchs 2024 auf 317 Milliarden US-Dollar und wird bis 2030 auf Ã¼ber 820 Milliarden US-Dollar prognostiziert (Gartner, 2025). Im DACH-Raum steigt die SaaS-Adoptionsrate jÃ¤hrlich um 18 bis 22 Prozent.

Was das fÃ¼r Vertragsmanager bedeutet: Jedes neue SaaS-Abo ist ein weiterer Eintrag, der in eine Vertragsliste gehÃ¶rt. Je mehr Abonnements ein KMU abschlieÃŸt, desto dringlicher wird die Verwaltungsfrage. Vertragsmanager wird mit jeder neuen SoftwareneueinfÃ¼hrung im Markt wertvoller â€” nicht weniger. Das ist eine strukturelle Wachstumsdynamik, die unabhÃ¤ngig von einzelnen Projekten wirkt.

Gleichzeitig steigen die Stornierungskosten: Adobe, Microsoft und Salesforce haben Jahresabos mit automatischer VerlÃ¤ngerung und KÃ¼ndigungsfristen von 30 bis 90 Tagen als Standard etabliert. Ein verpasstes KÃ¼ndigungsdatum bedeutet unmittelbar einen weiteren Jahresbeitrag â€” oft zwischen 500 und 5.000 Euro je Abo.

Quellen: Gartner Forecast 2025, Bitkom SaaS-Studie DACH 2024, Statista Cloud-Software-Markt 2025

### 2.3 Das Kernproblem: Fragmentiertes Vertragsmanagement

Fragmentierung bezeichnet den Zustand, in dem zusammengehÃ¶rige Information auf viele unverbundene Orte verteilt ist. Im KMU-Vertragsmanagement bedeutet das: Jeder Vertrag kommt von einem anderen Anbieter, landet in einem anderen E-Mail-Postfach und wird in einer anderen Schublade oder auf einem anderen Laufwerk abgelegt. Es gibt keine Zentrale.

Das erzeugt sechs konkrete Probleme fÃ¼r jede UnternehmensfÃ¼hrung:

**Intransparente Gesamtkosten:** Die meisten KMU-Inhaber kÃ¶nnen nicht aus dem Stegreif sagen, was sie monatlich fÃ¼r Software ausgeben. Addiert man alle Einzel-Abos, ist das Ergebnis regelmÃ¤ÃŸig Ã¼berraschend â€” im negativen Sinne.

**Verpasste KÃ¼ndigungsfristen:** Abonnements mit automatischer VerlÃ¤ngerung und 30- bis 90-tÃ¤gigen Fristen verlÃ¤ngern sich still, wenn niemand aktiv widerruft. Forrester (2023) dokumentiert, dass Unternehmen im ersten Monat nach EinfÃ¼hrung eines CLM-Tools durchschnittlich 2,3 bis dahin unbekannte oder ungenutzte VertrÃ¤ge entdecken.

**Veraltete Lizenzzahlen:** Mitarbeiterbewegungen fÃ¼hren dazu, dass fÃ¼r ausgeschiedene oder nie aktive Mitarbeitende weiterhin Lizenzen bezahlt werden. Gartner (2024) schÃ¤tzt, dass Unternehmen durchschnittlich nur 47 Prozent der Softwarelizenzen nutzen, fÃ¼r die sie zahlen.

**Fehlende Dokumentation:** Regulatorische Anforderungen (DSGVO, LkSG, CSRD) verlangen, dass Unternehmen nachweisen kÃ¶nnen, welche Dienstleister sie beauftragen und unter welchen Datenschutzbedingungen. Diese Nachweise liegen selten strukturiert vor.

**PersonenabhÃ¤ngiges Wissen:** Wer ist fÃ¼r welchen Vertrag zustÃ¤ndig? Wenn die Office-Managerin das Unternehmen verlÃ¤sst, geht das institutionelle Vertragswissen mit ihr. Ein Tool schafft Institutionalisierung.

**Mangelnde Verhandlungsbasis:** Wer nicht weiÃŸ, seit wann er wie viel zahlt, kann nicht kompetent nachverhandeln. Vertragsmanager liefert die Datenbasis fÃ¼r jedes PreisgesprÃ¤ch.

Zum Vergleich: Die kaufmÃ¤nnische Buchhaltung war vor 30 Jahren genauso fragmentiert. Erst ERP-Systeme und dann cloudbasierte Buchhaltungstools lÃ¶sten das. Im Vertragsmanagement stehen KMU heute genau an diesem Wendepunkt.

Quellen: Gartner CLM-Studie 2024, Forrester TEI-Studie 2023, C+R Research Consumer Subscription Survey 2024

### 2.4 Regulatorischer RÃ¼ckenwind

Mehrere regulatorische Entwicklungen treiben den Bedarf nach strukturiertem Vertragsmanagement direkt:

**DSGVO (seit 2018, BuÃŸgelddurchsetzung ab 2020):** Jedes Unternehmen muss nachweisen kÃ¶nnen, welche Drittanbieter personenbezogene Daten verarbeiten. AuftragsverarbeitungsvertrÃ¤ge (AVV) mÃ¼ssen gefÃ¼hrt und aktuell gehalten werden. Ohne Vertragszentrale ist dieser Nachweis kaum erbringbar.

**LkSG (in Kraft seit Januar 2023):** Unternehmen ab 1.000 Mitarbeitenden mÃ¼ssen Sorgfaltspflichten in der Lieferkette nachweisen, was den Druck auf Lieferanten aller GrÃ¶ÃŸen erhÃ¶ht, Vertragsbeziehungen strukturiert zu dokumentieren.

**CSRD (Corporate Sustainability Reporting Directive, ab 2025 fÃ¼r groÃŸe, ab 2026 fÃ¼r mittlere Unternehmen):** Nachhaltigkeitsberichterstattung erfordert eine vollstÃ¤ndige Ãœbersicht Ã¼ber Dienstleister und deren Konditionen â€” das setzt strukturiertes Vertragsmanagement voraus.

**EU AI Act (vollstÃ¤ndig anwendbar ab August 2026):** Wer KI-basierte Dienstleistungen einkauft oder einsetzt, muss deren vertragliche Grundlage und Risikoklassifikation dokumentieren kÃ¶nnen.

Keiner dieser regulatorischen Treiber erfordert das Kauf eines CLM-Tools. Aber jeder einzelne setzt voraus, was ein fehlendes Tool systematisch verhindert: einen vollstÃ¤ndigen, zuverlÃ¤ssigen Ãœberblick Ã¼ber alle Vertragsbeziehungen.

Quellen: EU-DSGVO Art. 30, LkSG Â§3, CSRD-Richtlinie 2022/2464, EU AI Act 2024

---

## 3. Wettbewerb

### 3.1 Spezialisierte CLM-Anbieter: StÃ¤rken und strukturelle SchwÃ¤chen

Spezialisierte Anbieter fÃ¼r Contract Lifecycle Management existieren, sind aber entweder auf Enterprise-Kundschaft ausgerichtet oder decken zentrale Funktionen wie OCR oder proaktive Fristerinnerungen nicht ab. Das schafft einen strukturellen Interessenkonflikt: Ihre KomplexitÃ¤t und ihr Preismodell passen nicht zu KMU.

| Anbieter | Reichweite | StÃ¤rke des Anbieters | SchwÃ¤che und Chance fÃ¼r Vertragsmanager |
|---|---|---|---|
| **DocuSign CLM** | Global, Enterprise | MarktfÃ¼hrer E-Signatur, starke Vertragsworkflows, Salesforce-Integration | Ab 25 USD pro Nutzer/Monat, komplexe Einrichtung, kein OCR-Upload, keine KMU-Positionierung, US-Hosting |
| **Ironclad** | US-fokussiert | Starke KI-Klauselanalyse, modernes UX | Enterprise-only (ab 2.000 USD/Monat), kein DACH-Fokus, kein DSGVO-natives Hosting, keine KÃ¼ndigunsvorlagen DE/AT |
| **Vertragsheld (DE)** | DACH | Deutschsprachig, Vertragsvorlagen, bekannt bei Kanzleien | Kein proaktives Fristmanagement, keine Kostenanalytik, kein OCR-Upload, veraltet in UX und Funktionsumfang |
| **PandaDoc** | Global | Einfache Nutzung, E-Signatur, Templates | US-Hosting (DSGVO-Grauzone), kein DACH-spezifisches Rechtsformat, kein Fristmanagement, kein KostenÃ¼berblick |

Das gemeinsame Muster: Kein einziger Anbieter kombiniert OCR-basierten Schnellerfassung, proaktives KÃ¼ndigungsfrist-Management und Kostenanalytik in einem DSGVO-nativ betriebenen, auf DACH-KMU zugeschnittenen Produkt. Das ist die MarktÃ¶ffnung fÃ¼r Vertragsmanager.

### 3.2 Indirekte Anbieter: Warum sie die LÃ¼cke nicht schlieÃŸen

Indirekte Anbieter decken Teilprobleme ab, aber keiner schlieÃŸt die LÃ¼cke vollstÃ¤ndig:

| Anbieter | Was er bietet | Was fehlt und warum das eine Chance ist |
|---|---|---|
| **Spendflo / Tronity** | SaaS-Spend Management, KostenÃ¼bersicht | Enterprise-Preis (ab 500 EUR/Monat), kein OCR, kein proaktives Fristmanagement, US-Hosting |
| **Genuity** | IT-Spend-Ãœbersicht fÃ¼r KMU (US) | Nur auf US-Markt, keine DSGVO-KonformitÃ¤t, keine KÃ¼ndigungsvorlagen, keine DACH-Lokalisierung |
| **Lexoffice / sevDesk** | Buchhaltung mit rudimentÃ¤ren Vertragsfeldern | Kein dediziertes Vertragsmanagement, kein OCR-Upload, kein Fristmanagement, kein Kostendashboard |
| **Microsoft SharePoint** | Dokumentenverwaltung | Keine intelligente Extraktion, keine Erinnerungen, kein Kostentracking â€” erfordert manuelle Pflege |
| **Excel / Google Sheets** | Flexibel, bekannt | FehleranfÃ¤llig, kein OCR, keine Automatisierung, keine Mandantentrennung, kein Rollenkonzept |

Die MarktlÃ¼cke: Es gibt heute kein DSGVO-natives, DACH-fokussiertes Produkt, das OCR-basierte Dokumentenerfassung, proaktives Fristmanagement mit KÃ¼ndigungsvorlagen und strukturierte Kostenanalytik in einer intuitiv bedienbaren KMU-LÃ¶sung vereint. Vertragsmanager schlieÃŸt diese LÃ¼cke.

Quellen: G2-Vergleich CLM-Software April 2026, Capterra-MarktÃ¼bersicht 2025, Gartner Magic Quadrant CLM 2024

---

## 4. Zielgruppe

### 4.1 PrimÃ¤re Zielgruppe (B2B KMU) und Marktvolumen

Die primÃ¤re Zielgruppe sind EntscheidungstrÃ¤ger und Verwalter in kleinen und mittleren Unternehmen, die laufende Abonnements, VertrÃ¤ge und Lizenzen verwalten mÃ¼ssen. Sie bilden den grÃ¶ÃŸeren Markt, haben hohen tÃ¤glichen Handlungsbedarf und sind am wenigsten durch bestehende Anbieter bedient.

**Warum KMU priorisiert werden:** Sie sind die Mehrheit der 2,1 bis 2,6 Millionen Zielkunden im DACH-Raum. Entscheidungen werden schnell getroffen â€” oft durch eine Einzelperson. Ein Tool mit klar messbarem ROI (Kostenersparnis durch vermiedene Auto-Renewals) gewinnt diese Gruppe ohne komplexen Vertriebszyklus. Enterprise-Kunden haben lÃ¤ngere Entscheidungszyklen, eigene IT-Abteilungen und dedizierte CLM-Beauftragungen.

**Vier Nutzerprofile:**

**Ãœberlasteter GeschÃ¤ftsfÃ¼hrer:** FÃ¼hrt ein KMU mit 5 bis 30 Mitarbeitenden. Hat keine dedizierte Person fÃ¼r Vertragsmanagement. Zahlt seit Monaten fÃ¼r ungenutzte Abos, weil die KÃ¼ndigung im Alltagsstress vergessen wurde. Braucht Ãœberblick ohne Einarbeitungsaufwand.

**Office-Managerin / Assistenz der GeschÃ¤ftsfÃ¼hrung:** Verantwortlich fÃ¼r alle laufenden VertrÃ¤ge, Lizenzen und Rechnungen. Muss monatlich an den GeschÃ¤ftsfÃ¼hrer berichten, was das Unternehmen ausgibt. Braucht ein Tool, das automatisch Kostenreports generiert.

**IT-Verantwortlicher im Mittelstand:** Verwaltet Microsoft- und Adobe-Lizenzen fÃ¼r 50 bis 200 Mitarbeitende. Muss Lizenzzahlen aktuell halten, Ãœber- und Unterlizenzen erkennen und Compliance nachweisen.

**SelbststÃ¤ndige und Freelancer:** Verwalten 5 bis 15 eigene Abonnements fÃ¼r Tools, Plattformen und Dienste. Keine Zeit fÃ¼r manuelle Pflege, aber direktes finanzielles Interesse an Kostenkontrolle.

### 4.2 SekundÃ¤re Zielgruppe (Kanzleien, Steuerberater, Dienstleister)

Steuerberater, Anwaltskanzleien und Unternehmensberater, die mehrere Mandanten betreuen, sind eine attraktive, aber separate Zielgruppe. Sie brauchen Multi-Mandanten-Verwaltung, Rollen-Trennung zwischen Kanzlei und Mandant sowie automatische Berichte. Das ist ein Premium-Segment mit hÃ¶herer Zahlungsbereitschaft.

FÃ¼r Version 1 ist diese Zielgruppe nicht im Fokus. Die Infrastruktur (Organisations-Multi-Tenant-Architektur, Rollenkonzept, Audit-Log) wird aber so angelegt, dass eine Multi-Mandanten-Erweiterung in Version 2 ohne Architekturwechsel mÃ¶glich ist.

### 4.3 Nicht Zielgruppe

Unternehmen ohne laufende digitale Abonnements oder ausschlieÃŸlich mit EinmalkÃ¤ufen (klassische Perpetual-Lizenzen) haben keinen unmittelbaren Nutzen aus Vertragsmanager. Das betrifft einen schrumpfenden Teil des Marktes, da die Softwareindustrie branchenweit auf Abonnementmodelle umgestellt hat. Diese Gruppe wird mit jeder weiteren SaaS-Adoption potenzieller Zielkunde.

---

## 5. Nutzen

### 5.1 Nutzen fÃ¼r Kunden

| Problem heute | LÃ¶sung durch Vertragsmanager | Konkreter Vorteil |
|---|---|---|
| Abonnements liegen in E-Mails, PDFs und Schreibtischschubladen | Foto oder Upload â†’ automatische OCR-Extraktion â†’ zentrale Ãœbersicht | Alle VertrÃ¤ge erfasst in Minuten, nicht Stunden |
| KÃ¼ndigungsfristen werden verpasst, Abos verlÃ¤ngern sich automatisch | Konfigurierbarer Erinnerungs-Workflow 90/60/30/14 Tage vor Frist | Kein ungewolltes Auto-Renewal mehr |
| Niemand weiÃŸ, was das Unternehmen monatlich fÃ¼r Software ausgibt | Echtzeit-Kostendashboard mit Monats-/Jahresauswertung und Kategorie-Breakdown | Fundierte Entscheidungsbasis fÃ¼r Budget-GesprÃ¤che |
| KÃ¼ndigungsschreiben mÃ¼ssen jedes Mal manuell formuliert werden | Vorlagenbibliothek mit Auto-BefÃ¼llung, Download als PDF/DOCX | KÃ¼ndigung in unter zwei Minuten, rechtskonform |
| Lizenzanzahlen stimmen nicht mit Mitarbeiterbestand Ã¼berein | Lizenzfelder je Vertrag, Warnung bei Abweichung | Keine Ãœber- oder Unterlizenzen |
| DatenschutzmÃ¤ngel bei US-Tools â€” Vertragsdaten in amerikanischen Clouds | EU-Only Hosting (Supabase EU Frankfurt, Vercel fra1), DSGVO-nativ | Datenschutz als echtes Kaufargument |
| Vertragsdokumentation fÃ¼r DSGVO-AVV-Nachweise fehlt | Alle Vertragspartner mit Kategorie und Dokumenten strukturiert gespeichert | Audit-Ready in Minuten |

Quellen: Forrester TEI CLM 2023, Gartner SAM-Studie 2024, C+R Research 2024

### 5.2 Nutzen fÃ¼r die WAMOCON GmbH

Vertragsmanager ist ein Produkt mit tÃ¤glich wiederkehrender Nutzung. Jede Verwaltungshandlung â€” eine FristprÃ¼fung, ein Kostenreport, eine KÃ¼ndigung â€” ist ein Kontaktpunkt mit echtem Mehrwert. WAMOCON baut damit eine direkte, wiederkehrende Kundenbeziehung zu einer stark wachsenden Nutzergruppe auf.

**Eigenes Produkt mit Alleinstellungsmerkmal:** WAMOCON tritt als Softwareanbieter in einem strukturell wachsenden Markt auf. Das schafft MarkenprÃ¤senz unabhÃ¤ngig von Einzelprojekten.

**Mehrere Einnahmequellen:** Free- und bezahlte Tiers (Starter, Pro, Business), Jahresabonnements mit Rabatt und langfristig White-Label-Kooperationen mit Steuerberatern, Unternehmensberatern oder IHK-nahen VerbÃ¤nden als Reseller-Kanal.

**Wachstumshebel:** Der SaaS-Markt verdoppelt bis verdreifacht sich bis 2030. Wer jetzt Nutzer gewinnt, profitiert von Switching-Costs und NutzerloyalitÃ¤t â€” wer seine VertrÃ¤ge einmal strukturiert hat, wechselt das Tool nicht leichtfertig.

**Regulatorischer Moat:** DSGVO-native Infrastruktur ist fÃ¼r US-Wettbewerber strukturell schwer zu replizieren. Das schÃ¼tzt den Marktanteil langfristig.

---

## 6. AbhÃ¤ngigkeiten und Machbarkeit

Vertragsmanager hat drei externe AbhÃ¤ngigkeiten: einen OCR-Dienst fÃ¼r Dokumentenanalyse, einen transaktionalen E-Mail-Dienst fÃ¼r Erinnerungen und einen Zahlungsdienstleister fÃ¼r das Abonnementmodell. Alle drei sind ohne exklusive Partnerschaft lÃ¶sbar. Wichtig: Alle kÃ¶nnen bei Bedarf ausgetauscht werden, da der OCR-Layer als abstrakte Schnittstelle implementiert wird.

### 6.1 Datenzugang und technische Services

| Service | Was er liefert | FÃ¼r welche Funktion | AbhÃ¤ngigkeit | Kosten |
|---|---|---|---|---|
| **Azure Document Intelligence** | OCR-Extraktion aus PDFs, Fotos, strukturierten Dokumenten, 85â€“93 % Genauigkeit mit Review-Flow | Dokumentenerfassung, SchlÃ¼sselfeld-Extraktion | Kein Exklusivvertrag, nutzungsbasiert | Ca. 0,001â€“0,005 EUR/Seite |
| **Google Cloud Vision** (Fallback) | OCR-Extraktion, multilinguale Texterkennung | Fallback-OCR-Provider | Kein Exklusivvertrag, nutzungsbasiert | Ca. 0,0015 EUR/Anfrage |
| **Supabase (EU, Frankfurt)** | PostgreSQL-Datenbank, Auth, Row Level Security, Object Storage | Alle Daten, Authentifizierung, Dokumentenspeicher | DPA erforderlich, kein Exklusivvertrag | Ab 25 USD/Monat Pro-Plan |
| **Vercel (fra1 Frankfurt)** | Hosting, Deployment, Edge-Netzwerk | Frontend und API-Hosting | Kein Exklusivvertrag | Ab 20 USD/Monat Pro-Plan |
| **Resend oder Brevo (EU)** | Transaktionale E-Mails | Erinnerungen, Einladungen, Willkommens-E-Mails | Kein Exklusivvertrag | Ab 0 EUR (Free Tier ausreichend fÃ¼r MVP) |

### 6.2 Abrechnung und ZahlungsmodalitÃ¤ten

| Option | Wie es funktioniert | AbhÃ¤ngigkeit | Empfehlung |
|---|---|---|---|
| **Stripe** | Branchenstandard fÃ¼r SaaS-Abonnements, unterstÃ¼tzt SEPA, Kreditkarte, automatische VerlÃ¤ngerungen | Stripe-Account, kein Exklusivvertrag, PCI-konform | Empfohlen ab Version 1 |
| **Paddle** | All-in-one Merchant of Record, Ã¼bernimmt EU-MwSt | Kein eigenes MwSt-Setup nÃ¶tig | Alternative fÃ¼r internationalen Rollout |
| **Manuell (Rechnungsstellung)** | Manuelle Abonnementverwaltung via Supabase-DB und SEPA-Ãœberweisung | Kein Drittanbieter | Nicht empfohlen â€” zu aufwÃ¤ndig |

**Empfehlung:** Stripe ab Version 1 integrieren. Stripe ist PCI-DSS-konform, unterstÃ¼tzt SEPA-Lastschrift fÃ¼r DACH-Kunden und bietet den Billing-Portal-Baustein fÃ¼r Self-Service-Kundenverwaltung ohne eigene Implementierung.

### 6.3 Gesamtbewertung

Version 1 hat keine kritischen externen AbhÃ¤ngigkeiten, die einen Start blockieren kÃ¶nnten. Azure Document Intelligence und Supabase sind Ã¶ffentlich verfÃ¼gbar, erfordern keine Genehmigungen und kÃ¶nnen innerhalb weniger Stunden eingerichtet werden. Die OCR-Schicht wird provider-agnostisch als abstraktes Interface implementiert â€” ein Providerwechsel erfordert nur eine neue Implementierungsklasse, keine Datenbankmigrationen.

Der einzige risikobehaftete externe Faktor ist die OCR-QualitÃ¤t bei unstrukturierten Handschriftenscans und schlecht digitalisierten PDFs. Diese wird durch den verpflichtenden Review-Flow nach jeder Extraktion mitigiert: Der Nutzer prÃ¼ft und korrigiert die extrahierten Felder vor dem Speichern. Fehlertoleranz ist damit baulich in das Produkt integriert.

---

## 7. Anforderungen Version 1

Version 1 ist bewusst auf den Kern fokussiert: vollstÃ¤ndige Vertragserfassung, proaktives Fristmanagement, KostenÃ¼berblick und KÃ¼ndigungsunterstÃ¼tzung. Keine KI-Empfehlungen, keine Bankkonto-Integration, keine nativen Mobile-Apps. Stattdessen: zuverlÃ¤ssige OCR-Erfassung, fehlertoleranter Review-Flow, konfigurierbare Erinnerungen und ein ehrlicher KostenÃ¼berblick.

### 7.1 Hauptprozesse

#### 7.1.1 Dokumentenerfassung per OCR und manueller Eingabe

Datengrundlage: Azure Document Intelligence als primÃ¤rer OCR-Provider, Google Cloud Vision als Fallback. Beide hinter einer abstrakten Service-Schnittstelle. Review-Flow nach jeder Extraktion verpflichtend â€” kein automatisches Speichern ohne NutzerbestÃ¤tigung.

| ID | Anforderung | PrioritÃ¤t | Status |
|---|---|---|---|
| DO-01 | Upload von PDF-, PNG-, JPEG- und HEIC-Dateien per Drag-Drop im Browser | Muss | Neu |
| DO-02 | Kamera-Upload auf MobilgerÃ¤ten (Mobile Browser, kein nativer App-Install) | Muss | Neu |
| DO-03 | OCR-Extraktion: Vertragspartner / Anbieter-Name | Muss | Neu |
| DO-04 | OCR-Extraktion: Vertragsbeginn und Vertragsende | Muss | Neu |
| DO-05 | OCR-Extraktion: KÃ¼ndigungsfrist (z. B. â€ž3 Monate zum Jahresende") | Muss | Neu |
| DO-06 | OCR-Extraktion: Zahlungsbetrag und Zahlungsintervall (monatlich / jÃ¤hrlich / quartalsweise) | Muss | Neu |
| DO-07 | OCR-Extraktion: Anzahl Lizenzen / Nutzeranzahl (sofern vorhanden) | Soll | Neu |
| DO-08 | OCR-Extraktion: Vertragskategorie als automatischer Vorschlag (manuell anpassbar) | Soll | Neu |
| DO-09 | Review-Flow nach jeder OCR-Extraktion: strukturiertes Formular zeigt extrahierte Felder zur BestÃ¤tigung oder Korrektur | Muss | Neu |
| DO-10 | VollstÃ¤ndig manueller Erfassungsweg ohne Dokument-Upload als gleichwertiger Onboarding-Pfad | Muss | Neu |
| DO-11 | Originaldokument optional verschlÃ¼sselt in Supabase Storage speichern (AES-256, EU-Region) | Soll | Neu |
| DO-12 | Nutzerkorrektur im Review-Flow wird im Audit-Log gespeichert (Vorher/Nachher-Diff) | Soll | Neu |

#### 7.1.2 Vertrags- und AboÃ¼bersicht

| ID | Anforderung | PrioritÃ¤t | Status |
|---|---|---|---|
| VE-01 | Dashboard in Karten- und Listen-Ansicht mit umschaltbarer Darstellung | Muss | Neu |
| VE-02 | Status-Badge je Vertrag: Aktiv / KÃ¼ndigung fÃ¤llig bald / Abgelaufen / GekÃ¼ndigt | Muss | Neu |
| VE-03 | Freitext-Suche Ã¼ber alle Vertragsfelder (Anbieter, Name, Notizen) | Muss | Neu |
| VE-04 | Filter nach Kategorie, Zahlungsintervall, Anbieter, Status | Muss | Neu |
| VE-05 | Sortierung nach monatlichen Kosten, nÃ¤chster Frist und Anbieter-Name | Muss | Neu |
| VE-06 | Detail-Ansicht je Vertrag mit vollstÃ¤ndiger BearbeitungsmÃ¶glichkeit aller Felder | Muss | Neu |
| VE-07 | Systemkategorien (Software & SaaS, Kommunikation, Versicherungen, Miete & Immobilien, Fahrzeuge & Leasing, Marketing & Werbung, Sonstiges) | Muss | Neu |
| VE-08 | Benutzerdefinierte Kategorien: Nutzer kann eigene Kategorien mit Name und Farbe anlegen | Soll | Neu |
| VE-09 | Lizenzfeld je Vertrag: Anzahl bezahlter vs. aktiv genutzter Lizenzen eintragbar | Soll | Neu |

#### 7.1.3 Frist- und Erinnerungsmanagement

| ID | Anforderung | PrioritÃ¤t | Status |
|---|---|---|---|
| FR-01 | Automatische Berechnung des KÃ¼ndigungsdatums aus Vertragslaufzeit und KÃ¼ndigungsfrist | Muss | Neu |
| FR-02 | Anzeige des berechneten KÃ¼ndigungsdatums prominent in der Detail-Ansicht und auf der Karte | Muss | Neu |
| FR-03 | Konfigurierbare Erinnerungszeitpunkte pro Vertrag: 90 / 60 / 30 / 14 / 7 Tage vor KÃ¼ndigungsfrist | Muss | Neu |
| FR-04 | Automatischer E-Mail-Versand zu den konfigurierten Zeitpunkten mit: Vertragsname, Anbieter, Fristdatum, direktem App-Link | Muss | Neu |
| FR-05 | In-App-Benachrichtigungs-Inbox mit ungelesenen-Badge in der Navigationsleiste | Muss | Neu |
| FR-06 | Fristenkalender-Ansicht: Monats- und JahresÃ¼bersicht aller bevorstehenden Fristen | Soll | Neu |
| FR-07 | .ics-Kalender-Export fÃ¼r alle Fristen (Outlook, Google Calendar, Apple Calendar kompatibel) | Soll | Neu |
| FR-08 | Erinnerungen kÃ¶nnen jederzeit deaktiviert, reaktiviert oder angepasst werden | Muss | Neu |

#### 7.1.4 KostenÃ¼bersicht und Analytik

| ID | Anforderung | PrioritÃ¤t | Status |
|---|---|---|---|
| KO-01 | Gesamtausgaben pro Monat: jÃ¤hrliche Abos werden auf monatliche Basis umgerechnet und addiert | Muss | Neu |
| KO-02 | Projektierte Jahreskosten auf Basis aller aktiven VertrÃ¤ge | Muss | Neu |
| KO-03 | Donut-Chart / Tortendiagramm der Ausgaben nach Kategorie | Muss | Neu |
| KO-04 | Liste der Top-10-Anbieter nach Ausgaben in aufsteigender Sortierung | Soll | Neu |
| KO-05 | Zahlungskalender: chronologische Ansicht der nÃ¤chsten 12 Monatszahlungen (Cash-Flow-Sicht) | Soll | Neu |
| KO-06 | Monatsvergleich: aktuelle Monatskosten vs. Vormonat mit Delta-Anzeige | Soll | Neu |
| KO-07 | CSV-Export aller Vertragsdaten und Kosten als strukturierte Tabelle | Muss | Neu |
| KO-08 | PDF-Kostenbericht mit GesamtÃ¼bersicht, Kategorie-Breakdown und Liste aller VertrÃ¤ge | Soll | Neu |

#### 7.1.5 KÃ¼ndigungsvorlagen

| ID | Anforderung | PrioritÃ¤t | Status |
|---|---|---|---|
| KV-01 | Vorlagenbibliothek mit professionellen KÃ¼ndigungsschreiben in Deutsch (DE), Ã–sterreichisch-Deutsch (AT) und Englisch (EN) | Muss | Neu |
| KV-02 | Template-Typen: Ordentliche KÃ¼ndigung zum Vertragsende / KÃ¼ndigung unter Berufung auf KÃ¼ndigungsfrist / NichtverlÃ¤ngerungs-BestÃ¤tigung fÃ¼r Jahresabos | Muss | Neu |
| KV-03 | AuÃŸerordentliche KÃ¼ndigung als separates Template mit markiertem Hinweis zur RechtsprÃ¼fung | Soll | Neu |
| KV-04 | Auto-BefÃ¼llung der Vorlage mit Vertragsdaten: Anbieter-Name, Vertragsdatum, Kundennummer (wenn hinterlegt), Firmendaten des Nutzers | Muss | Neu |
| KV-05 | Download der ausgefÃ¼llten Vorlage als PDF und DOCX | Muss | Neu |
| KV-06 | Klar sichtbarer rechtlicher Disclaimer: â€žDiese Vorlage ist unverbindlich. Im Zweifelsfall konsultieren Sie einen Rechtsanwalt." | Muss | Neu |

---

### 7.2 BasisfunktionalitÃ¤ten

#### 7.2.1 Rollen, Anmeldung und Registrierung

| ID | Anforderung | PrioritÃ¤t | Status |
|---|---|---|---|
| RB-01 | Drei Rollen: Admin (Vollzugriff inkl. Nutzerverwaltung), Editor (alle Funktionen auÃŸer Nutzerverwaltung), Viewer (nur lesen) | Muss | Neu |
| RB-02 | Registrierung per E-Mail mit Passwort und Double-Opt-In-BestÃ¤tigung via Supabase Auth | Muss | Neu |
| RB-03 | Anmeldung per E-Mail/Passwort sowie Social Login via Google und Microsoft | Soll | Neu |
| RB-04 | Passwort zurÃ¼cksetzen per E-Mail-Link | Muss | Neu |
| RB-05 | Organisations-Account: ein gemeinsamer Account fÃ¼r eine GmbH / ein Unternehmen mit mehreren Nutzern | Muss | Neu |
| RB-06 | Row Level Security (RLS) in Supabase: Nutzer sehen ausschlieÃŸlich die Daten ihrer eigenen Organisation | Muss | Neu |

#### 7.2.2 Organisationsverwaltung und Audit

| ID | Anforderung | PrioritÃ¤t | Status |
|---|---|---|---|
| OV-01 | Admin kann Nutzer per E-Mail-Einladung mit Rollenauswahl (Admin / Editor / Viewer) einladen | Muss | Neu |
| OV-02 | Eingeladener erhÃ¤lt E-Mail mit Aktivierungslink und wird nach Annahme der Organisation zugeordnet | Muss | Neu |
| OV-03 | Admin kann Rollen Ã¤ndern und Nutzer jederzeit aus der Organisation entfernen | Muss | Neu |
| OV-04 | Audit-Log: Protokoll aller Ã„nderungen mit Nutzer, Zeitstempel, Aktion und Vorher/Nachher-Diff | Soll | Neu |

#### 7.2.3 Navigation, Benachrichtigungen und Darstellung

| ID | Anforderung | PrioritÃ¤t | Status |
|---|---|---|---|
| NV-01 | Breadcrumb-Navigation auf allen Unterseiten | Muss | Neu |
| NV-02 | Dashboard als Startseite: KostenÃ¼bersicht, anstehende Fristen (nÃ¤chste 30 Tage), zuletzt bearbeitete VertrÃ¤ge | Muss | Neu |
| NV-03 | Dunkel/Hell-Modus in den Einstellungen umschaltbar | Soll | Neu |
| NV-04 | Responsives Design: vollstÃ¤ndig nutzbar auf Desktop, Tablet und Mobilbrowser | Muss | Neu |
| BN-01 | E-Mail-Benachrichtigung bei bevorstehenden Fristen gemÃ¤ÃŸ konfigurierter Erinnerungsregel | Muss | Neu |
| BN-02 | In-App-Benachrichtigungs-Inbox mit ungelesenen-Badge in der Navigation | Muss | Neu |

#### 7.2.4 Spracheinstellungen und Profilseite

| ID | Anforderung | PrioritÃ¤t | Status |
|---|---|---|---|
| SP-01 | Sprachumschaltung Deutsch und Englisch in den Einstellungen ohne Seitenneuladung | Muss | Neu |
| SP-02 | Datum- und WÃ¤hrungsformat richten sich nach der gewÃ¤hlten Sprache (DE: dd.mm.yyyy / EN: mm/dd/yyyy) | Soll | Neu |
| PR-01 | Profilseite: Name, E-Mail-Adresse, Unternehmensname, Sprache, Passwort Ã¤ndern | Muss | Neu |
| PR-02 | DSGVO-Rechte: Self-Service Konto- und DatenlÃ¶schung sowie vollstÃ¤ndiger Datenexport (JSON/CSV) auf Anfrage (Art. 17 und 20 DSGVO) | Muss | Neu |

#### 7.2.5 Rechtliches und GeschÃ¤ftsmodell

| ID | Anforderung | PrioritÃ¤t | Status |
|---|---|---|---|
| RE-01 | AGB, Impressum und DatenschutzerklÃ¤rung als eigene, verlinkbare Seiten in der App | Muss | Neu |
| RE-02 | Cookie-Banner bei erstem Aufruf, DSGVO-konform; Empfehlung: Plausible Analytics (cookiefrei) statt Google Analytics | Muss | Neu |
| RE-03 | Vier-Tier-Preismodell: Free (5 VertrÃ¤ge, 1 Nutzer) / Starter (â‚¬19/Monat, 30 VertrÃ¤ge, 3 Nutzer) / Pro (â‚¬49/Monat, unbegrenzt, 10 Nutzer) / Business (â‚¬99/Monat, unbegrenzt, 25 Nutzer) | Muss | Neu |
| RE-04 | Upgrade von Free auf bezahlte Tiers direkt in der App via Stripe | Muss | Neu |
| RE-05 | Server ausschlieÃŸlich in der EU: Supabase EU-Region (eu-central-1, Frankfurt), Vercel fra1 (Frankfurt) | Muss | Neu |
| RE-06 | Data Processing Agreement (DPA) mit allen Drittanbietern (Supabase, Vercel, Azure, E-Mail-Service) vor Launch abgeschlossen | Muss | Neu |

---

### 7.3 Scope: Was ist Version 1 und was nicht

| In Scope â€” Version 1 | Out of Scope â€” ab Version 2 und spÃ¤ter |
|---|---|
| OCR-Upload (PDF, Foto) mit Review-Flow | E-Mail-Postfach-Scan (IMAP-Integration) |
| Manueller Erfassungsweg | KI-basierte Vertragskostenempfehlungen |
| Vertrags- und AboÃ¼bersicht (Karten- und Listenansicht) | Bankkonto-Integration (PSD2/Open Banking) |
| Proaktive Fristerinnerungen per E-Mail und In-App | Native iOS/Android-App |
| KostenÃ¼bersicht, Donut-Chart, Monatsvergleich | Automatisches KÃ¼ndigen per API |
| CSV- und PDF-Export | Multi-Mandanten-Modul fÃ¼r Kanzleien |
| KÃ¼ndigungsvorlagen DE/AT/EN (PDF + DOCX) | Buchhaltungs-Integration (DATEV, Lexoffice) |
| Multi-User: Admin / Editor / Viewer | Branchenvergleich (Benchmark-Daten) |
| Organisationseinladung per E-Mail | White-Label / Reseller-Programm |
| Stripe-Abonnementverwaltung | Mehrsprachige Vorlagen (FR, ES, IT) |
| DSGVO-Rechte: DatenlÃ¶schung und -export | KI-Chat-Interface Ã¼ber eigene Vertragssammlung |
| AGB, DatenschutzerklÃ¤rung, Cookie-Banner | Vertragsverhandlungs-Assistent |
| Dunkel/Hell-Modus, Sprachumschaltung DE/EN | Semantische Suche (pgvector RAG) |

---

## 8. Chancen und Risiken

### 8.1 Chancen

| Chance | BegrÃ¼ndung |
|---|---|
| Strukturell wachsende Zielgruppe | SaaS-Abos verdoppeln sich bis 2028 im KMU-Markt. Jedes neue Abo ist ein weiterer Nutzergrund. |
| DSGVO als dauerhafter Selektionsvorteil | US-Hosting ist fÃ¼r DACH-KMU mit sensiblen Vertragsdaten strukturell unattraktiv. Ein EU-native Anbieter gewinnt dieses Argument dauerhaft. |
| Regulatorik schafft Zwangsnachfrage | DSGVO, LkSG, CSRD und EU AI Act erzwingen strukturierte Vertragsdokumentation â€” ohne dass Vertragsmanager aktiv verkauft werden muss. |
| Kein neutraler Vollservice-Anbieter im DACH-KMU-Segment | Die Kombination aus OCR-Erfassung, proaktivem Fristmanagement und Kostenanalytik in einem DSGVO-nativen Produkt gibt es nicht. Die LÃ¼cke ist offen und zugÃ¤nglich. |
| Messbar schneller ROI | Forrester (2023): Im ersten Monat nach EinfÃ¼hrung entdecken Nutzer durchschnittlich 2,3 unbekannte oder ungenutzte VertrÃ¤ge. Eine Ersparnis von einem Jahresabo amortisiert das Jahresabonnement in der Regel vollstÃ¤ndig. |
| Version 1 ohne kritische AbhÃ¤ngigkeiten | Kein Partnervertrag, keine BehÃ¶rdengenehmigung nÃ¶tig. Start ist unmittelbar mÃ¶glich. Kein Gatekeeper kann den Marktzugang blockieren. |

### 8.2 Risiken

| Risiko | Warum es eintreten kann | GegenmaÃŸnahme |
|---|---|---|
| OCR-QualitÃ¤t bei unstrukturierten Dokumenten | Handschriften, schlecht gescannte PDFs, nicht-standardisierte Layouts kÃ¶nnen Extraktion auf 60â€“70 % Genauigkeit reduzieren | Review-Flow als Pflichtabschnitt nach jeder Extraktion â€” OCR ist Entwurf, nicht Endergebnis. Fehlertoleranz ist baulich integriert. |
| Cold-Start-Problem: Produkt hat erst nach 5+ EintrÃ¤gen Wert | Leeres Dashboard liefert keinen unmittelbaren Nutzen. FrÃ¼her Churn, wenn Onboarding scheitert | Guided Onboarding mit Fortschrittsanzeige, E-Mail-Sequenz in den ersten 14 Tagen, erster messbarer Mehrwert nach 3 erfassten VertrÃ¤gen sichtbar |
| Zahlungsbereitschaft fÃ¼r â€žVertragsmanagement" | KMU ordnen Vertragsmanagement als â€žnice to have" ein, nicht als Pflichtausgabe | Freemium-Einstieg mit 5 kostenlosen VertrÃ¤gen senkt HÃ¼rde auf null. Conversion Ã¼ber messbaren Mehrwert (entdeckte Ersparnisse). |
| Wechsel zwischen OCR-Providern (PreiserhÃ¶hung, Dienstaustritt) | AbhÃ¤ngigkeit von einem einzigen Anbieter schafft Preisrisiko | Abstrakte OCR-Service-Schnittstelle von Tag 1 â€” Providerwechsel erfordert nur neue Klasse, keine Datenbankmigration |
| Etablierte Anbieter schlieÃŸen die LÃ¼cke | DocuSign oder ein europÃ¤ischer Wettbewerber baut OCR + KÃ¼ndigungsvorlagen nach | DSGVO-Positionierung ist fÃ¼r US-Anbieter strukturell schwer replizierbar. First-Mover-Vorteil durch schnellen Launch. |
| Regulatorische Anforderungen beim Zahlungsmodell | Als Zahlungsabwickler greifen PSD2-Anforderungen | Stripe Ã¼bernimmt gesamte PSD2-Compliance als zertifizierter Zahlungsdienstleister. Kein eigenes Lizenzerwerb nÃ¶tig. |

---

## 9. Umsetzungsplan Version 1

### 9.1 Entwicklungsansatz

Die Entwicklung erfolgt mit GitHub Copilot als aktivem Implementierungswerkzeug. GitHub Copilot schreibt den GroÃŸteil des Codes, der Entwickler Ã¼bernimmt Review, Architekturentscheidungen und QualitÃ¤tssicherung. Das ermÃ¶glicht die Umsetzung von Version 1 in 5 Werktagen.

**Technologiestack fÃ¼r Version 1:**

- **Frontend:** Next.js 15 (App Router), Tailwind CSS, TypeScript, shadcn/ui Komponenten
- **Backend / Datenbank:** Supabase (PostgreSQL mit Row Level Security, Auth, Object Storage) â€” EU-Region Frankfurt
- **OCR-Engine:** Azure Document Intelligence (primÃ¤r), Google Cloud Vision (Fallback), provider-agnostisch hinter eigenem Interface
- **E-Mail-Service:** Resend oder Brevo (EU-Anbieter, DSGVO-konform)
- **Payment:** Stripe (Abonnementverwaltung, SEPA, Kreditkarte)
- **Hosting und Deployment:** Vercel (fra1 Frankfurt, automatisches Deployment aus GitHub)
- **Analytics:** Plausible Analytics (EU-gehostet, cookiefrei, kein DSGVO-Banner nÃ¶tig)

### 9.2 Umsetzungsplan 5 Werktage

| Tag | Fokus | Inhalt | Wer / womit |
|---|---|---|---|
| Tag 1 | Anforderungen erfassen | Anforderungsdokument finalisieren und mit Stakeholdern abstimmen, Datenbankschema vollstÃ¤ndig entwerfen (Tabellen, RLS-Regeln, Indizes), Komponentenstruktur und Routing-Konzept (Next.js App Router) festlegen, OCR-Provider-Entscheidung treffen und API-Zugang beantragen, alle technischen AbhÃ¤ngigkeiten und deren Versionen fixieren | Entwickler und GitHub Copilot |
| Tag 2 | Rahmenbedingungen und erster Prototyp (OCR) | Vercel-Deployment-Pipeline aus GitHub einrichten (fra1 Frankfurt), Supabase-Projekt in EU-Region aufsetzen (Schema, RLS-Policies, Auth-Konfiguration), Strato-Domain konfigurieren und mit Vercel verknÃ¼pfen, Basis-UI-Framework aufsetzen (Next.js 15, Tailwind, shadcn/ui), Azure Document Intelligence hinter Abstract-Interface anbinden, Upload-Komponente (Drag-Drop + Kamera-Mobile), Review-Flow-UX (FeldbestÃ¤tigung und -korrektur), optionale Dokumentenspeicherung Supabase Storage, manueller Erfassungsweg. Zwischenstand: lauffÃ¤higer OCR-Upload mit Review-Flow | Entwickler und GitHub Copilot |
| Tag 3 bis 4 | Alle Kernfunktionen | Dashboard (Karten- und Listenansicht), Status-Badge-Logik, Freitext-Suche, Filter und Sortierung, Detail-Ansicht mit Bearbeitungsmodus, Kategorieverwaltung (System + benutzerdefiniert). Fristberechnungslogik (Vertragslaufzeit + KÃ¼ndigungsfrist â†’ KÃ¼ndigungsdatum), tÃ¤glicher Cron-Job fÃ¼r FristprÃ¼fung, E-Mail-Versand (Resend/Brevo), In-App-Benachrichtigungs-Inbox, Fristenkalender-Ansicht, .ics-Export. Monats-/Jahresauswertung (anteilige Umrechnung), Donut-Chart nach Kategorie, Top-Anbieter-Liste, Zahlungskalender, Monatsvergleich, CSV-Export, PDF-Kostenbericht. Template-Engine fÃ¼r DE/AT/EN-Vorlagen, Auto-BefÃ¼llung mit Vertragsdaten, PDF- und DOCX-Generierung, rechtlicher Disclaimer. Organisations-Einladungsflow, Rollen-Rechtemodell (Admin/Editor/Viewer), Audit-Log. GitHub Copilot schreibt den Code, Entwickler reviewt und korrigiert | Entwickler und GitHub Copilot |
| Tag 5 | QA und Launch | Manuelles Testen aller Kernfunktionen (OCR-Upload, Fristberechnung, E-Mail-Erinnerung, KostenÃ¼bersicht, KÃ¼ndigungsvorlage, Rollen), Fehlerkorrektur und Edge-Cases, Plausible Analytics einbinden, direkter Service-Launch | Entwickler und GitHub Copilot |

**Hinweis:** Die 5-Tage-Umsetzung produziert einen lauffÃ¤higen Dienst mit allen Kernfunktionen. Das ist kein Endprodukt, aber eine nutzbare Version 1, die echter Nutzung standhalten kann. Payment-Integration und rechtliche Dokumente (AGB, DatenschutzerklÃ¤rung) werden in einem separaten Folgeschritt nach dem Launch ergÃ¤nzt. QualitÃ¤tssicherung und Stabilisierung laufen nach dem 5-Tage-Sprint weiter.

---

## Quellenverzeichnis

| Quelle | Inhalt |
|---|---|
| Bitkom Digitalatlas 2025 | KMU-SaaS-Adoptionsrate, 34 Abos/Unternehmen im Median, bitkom.org |
| Gartner Technology Insights for Software Asset Management, 2024 | Nur 47 % der bezahlten Softwarelizenzen werden genutzt, 25â€“30 % Kostenreduktion durch SAM, gartner.com |
| Gartner Forecast: Public Cloud Services, Worldwide, 2025 | SaaS-Markt 317 Mrd. USD (2024), 820 Mrd. USD bis 2030, gartner.com |
| Forrester Research â€” The Total Economic Impact of Contract Lifecycle Management Solutions, 2023 | ROI 337 % Ã¼ber 3 Jahre, durchschnittlich 2,3 unbekannte VertrÃ¤ge im ersten Monat entdeckt, forrester.com |
| C+R Research â€” Subscription Economy Consumer Survey, 2024 | 17â€“23 % der Abo-Ausgaben fÃ¼r ungenutzte Dienste, crresearch.com |
| Statistisches Bundesamt, Unternehmensregister, 2025 | 3,5 Millionen KMU in Deutschland, destatis.de |
| IFM Bonn â€” KMU-Definition und Marktdaten, 2024 | KMU beschÃ¤ftigen 55 % der Arbeitnehmer, erzielen 35 % des Umsatzes, ifm-bonn.org |
| Vakul Agarwal et al. â€” Contract Analytics with Natural Language Processing, ACM Digital Library, 2022 | NLP-Vertragsextraktion erreicht 85â€“93 % Genauigkeit mit Human-in-the-Loop, ohne Review nur 68â€“75 %, dl.acm.org |
| McKinsey Global Survey â€” The State of AI in 2024 | 65 % der Unternehmen nutzen GenAI, Top-Anwendungsfall Dokumentenverarbeitung 43 %, mckinsey.com |
| European Commission â€” SME Performance Review: Digital Transformation, 2023 | 22 % der EU-KMU nutzen Cloud-CLM, 58 % noch mit Office-Dokumenten, ec.europa.eu |
| EU-DSGVO (VO 2016/679), insb. Art. 5, 6, 17, 20, 28, 30, 32, 33 | Rechtsgrundlagen, Betroffenenrechte, Auftragsverarbeitung, Datenpannenmeldung, eur-lex.europa.eu |
| EU AI Act (Verordnung 2024/1689) | Risikoklassifikation KI-Systeme, Transparenzpflichten, eur-lex.europa.eu |
| Lieferkettensorgfaltspflichtengesetz (LkSG), Â§3 ff., Deutschland 2023 | Sorgfaltspflichten und Dokumentationsanforderungen, gesetze-im-internet.de |
| CSRD-Richtlinie 2022/2464 | Nachhaltigkeitsberichterstattung ab 500 MA (2025) und 250 MA (2026), eur-lex.europa.eu |
| G2 â€” CLM Software Market Overview, April 2026 | Konkurrenzvergleich DocuSign CLM, Ironclad, PandaDoc, g2.com |
| Gartner Magic Quadrant for Contract Life Cycle Management, 2024 | Marktpositionierung Enterprise-Anbieter, KMU-LÃ¼cke dokumentiert, gartner.com |

---

*Dokument erstellt: 14. April 2026 | Version 1.0 | Status: Zur Freigabe eingereicht*
*Alle Marktdaten basieren auf zum Erstellungszeitpunkt Ã¶ffentlich verfÃ¼gbaren Quellen. FÃ¼r bindende rechtliche Aussagen (insbesondere DSGVO-KonformitÃ¤t, KÃ¼ndigungsvorlagen) ist juristischer Beistand erforderlich.*



## ./HOWTO.md
# HOWTO — Project Setup & Deployment Guide

> 🎉 **Herzlichen Glückwunsch zur Einrichtung! / Congratulations on setting up!**
> Du kannst dieses Dokument nach Abschluss der Einrichtung löschen oder als Referenz behalten.
> You can delete this HOWTO document once your setup is complete, or keep it for future reference.

> 📖 **Lies die [AGENTS.md](AGENTS.md) Datei, um GitHub Copilot optimal und produktiv zu nutzen.**
> **Read the [AGENTS.md](AGENTS.md) file to use GitHub Copilot in an optimised and productive way.**

---

## DE Deutsch

---

### Prozessübersicht

Folge diesen Schritten in der angegebenen Reihenfolge, um vom Template zur Produktion zu gelangen:

1. **GitHub Repo erstellen** — Nutze dieses Template, um ein neues Repository in der Wamocon GitHub Organisation zu erstellen.
2. **Repo klonen** — Klone es auf deinen Rechner und führe `npm install` aus.
3. **Pre-commit Hook installieren** — Führe `bash hooks/install.sh` aus. Einmalig nach dem Klonen — schützt dich davor, Geheimnisse versehentlich zu pushen.
4. **GitHub Workflow-Datei überprüfen** — Öffne `.github/workflows/deploy.yml` und `.github/workflows/pr-pipeline.yml`, überprüfe, dass `Wamocon/github_workflow` die korrekte Referenz ist.
5. **`.env.local` aktualisieren & Supabase verbinden** — Kopiere `.env.example` → `.env.local`, erstelle ein Remote-Supabase-Projekt und trage die Zugangsdaten ein.
6. **Lokal starten & Entwicklung beginnen** — Führe `npm run dev` aus und beginne mit der Entwicklung.
7. **GitHub-Organisationseinstellungen: Vercel-App Zugriff gewähren** — Gehe zu den Organisationseinstellungen auf GitHub, suche die Vercel GitHub-App und gewähre ihr Zugriff auf dein neues Repo. Nur so erscheint das Repo in der Vercel-Importliste.
8. **Repo vorübergehend öffentlich machen & bei Vercel importieren** — Stelle das Repo temporär auf öffentlich und importiere es in Vercel.
9. **Vercel Project ID holen** — Kopiere die Vercel Project ID aus den Vercel-Projekteinstellungen.
10. **Vercel Project ID zu GitHub Secrets hinzufügen** — Füge `VERCEL_PROJECT_ID` zu den GitHub Actions Secrets deines Repos hinzu.
11. **Repo auf intern umstellen** — Ändere die Sichtbarkeit des Repos zurück auf intern.
12. **Erstes manuelles Deployment** — Gehe zu `Actions → "Deploy to Vercel" → Run workflow` und wähle `production`.

> 📖 **Für detaillierte Informationen, lies weiter unten.**

---

### 1. Klonen & Einrichten

```bash
# Repository klonen
git clone https://github.com/Wamocon/<dein-repo-name>.git
cd <dein-repo-name>

# Abhängigkeiten installieren
npm install

# Umgebungsvariablen kopieren
cp .env.example .env.local

# Pre-commit Hook installieren (einmalig, direkt nach npm install)
bash hooks/install.sh

# Entwicklungsserver starten (mit Turbopack für schnelles Hot-Reload)
npm run dev
```

Öffne [http://localhost:3000](http://localhost:3000) in deinem Browser.

**Verfügbare Skripte:**

| Befehl | Beschreibung |
|---|---|
| `npm run dev` | Startet den Dev-Server mit Turbopack (Hot Reload) |
| `npm run build` | Erstellt den Produktions-Build |
| `npm run start` | Startet den Produktionsserver |
| `npm run lint` | Führt ESLint aus |
| `npm run typecheck` | Führt TypeScript-Typprüfung aus |

---

### 1b. Git-Workflow & Branch-Strategie

> 💡 **Arbeite nach dem ersten Push immer auf einem Branch — nie direkt auf `main`/`master`.**

**Empfohlener Ablauf:**

```bash
# Einmalig: Ersten Stand auf main/master pushen
git add .
git commit -m "chore: initial setup"
git push origin <main-oder-master>

# Ab jetzt: Immer auf einem Feature-Branch arbeiten
git checkout -b feature/mein-feature

# Änderungen lokal testen, dann committen
npm run dev        # testen
npm run typecheck  # Typfehler prüfen
npm run lint       # Lint prüfen

git add .
git commit -m "feat: beschreibung der Änderung"
git push origin feature/mein-feature
```

**Wichtige Regeln:**

- **Lokal testen, bevor du pushst.** Führe `npm run typecheck` und `npm run lint` aus, bevor du Änderungen pushst.
- **Alle Änderungen vor dem PR-Öffnen pushen.** Jeder neue Push auf einen offenen PR triggert automatisch die GitHub Actions — das verbraucht GitHub Actions-Minuten.
- **Nur einen PR auf einmal offen halten**, bis er gemergt ist.

> ⚠️ **Warum das wichtig ist:** Jeder Push auf einen offenen PR löst die PR Pipeline aus (Auto-Fix + Typecheck + Lint). Teste erst lokal — dann push, dann PR.

---

### 1c. Pre-commit Hook — Geheimnis-Scanner

Der Pre-commit Hook verhindert, dass API-Schlüssel, Tokens, Passwörter oder andere Geheimnisse versehentlich in GitHub eingecheckt werden. Der Hook scannt alle für den Commit vorgesehenen Dateien, **bevor** der Commit abgeschlossen wird.

#### Installation

```bash
# Einmalig nach dem Klonen ausführen — nie wieder nötig:
bash hooks/install.sh
```

> ✅ **Wie oft?** Genau **einmal** — direkt nach `npm install` beim ersten Einrichten des Repos. Nach der Installation läuft der Hook automatisch vor **jedem** `git commit`.

#### Was der Hook prüft

| Muster | Beispiel |
|--------|---------|
| AWS-Schlüssel | `AKIA...`, `aws_secret_access_key = ...` |
| GitHub-Tokens | `ghp_...`, `github_pat_...` |
| Vercel-Token | `VERCEL_TOKEN = abc123...` |
| Private Keys (PEM) | `-----BEGIN PRIVATE KEY-----` |
| Datenbankverbindungen | `postgres://user:password@host` |
| Generische Tokens | `api_key =`, `client_secret =`, `password =` |
| JWT-Tokens | `eyJ...` (vollständige `header.payload.signature`-Form) |
| Supabase Service Role | `service_role = eyJ...` |
| `.env`-Dateien | `.env`, `.env.local`, `.env.production` |

#### Wenn ein Commit blockiert wird

```
[pre-commit] BLOCKED — potential secrets detected
  ✖ src/lib/config.ts:12
    Reason: Generic API key
    Content: const API_KEY = "abc123xyz789..."
```

**Lösung:**
1. Entferne das Geheimnis aus der Datei.
2. Speichere es in deiner `.env.local` (ist bereits in `.gitignore` eingetragen).
3. Nutze GitHub Actions Secrets für CI/CD-Werte.

**Bei einem Falsch-Positiv** (z.B. ein Beispielwert in der Doku):
Füge den Kommentar `# notsecret` am Ende der betroffenen Zeile hinzu.

**Notfall-Bypass** (nur im absoluten Ausnahmefall — niemals für echte Geheimnisse!):
```bash
git commit --no-verify -m "deine Nachricht"
```

---

### 2. Supabase Setup & Warnung

> ⚠️ **KRITISCH: Sobald du einen Supabase-Account hast, speichere alle Testdaten direkt dort.**
>
> **Lege Testdaten NICHT als lokale Dateien im Projektverzeichnis ab** (z.B. JSON-Fixtures, SQL-Dumps). Füge Daten stattdessen direkt in dein Supabase-Projekt ein — über das Dashboard, per MCP-Tool oder per Migrations-Skript.

**Schritte:**

1. Gehe zu [supabase.com](https://supabase.com) und erstelle ein neues Projekt.
2. Kopiere die **Project URL**, den **Anon Key** und den **Service Role Key** aus  
   `Project Settings → API`.
3. Trage sie in deine `.env.local` Datei ein:
   ```
   NEXT_PUBLIC_SUPABASE_URL=https://your-project-ref.supabase.co
   NEXT_PUBLIC_SUPABASE_ANON_KEY=dein-anon-key
   SUPABASE_SERVICE_ROLE_KEY=dein-service-role-key
   ```

---

### 2b. Lokales Supabase Setup via Docker (Optional)

> 💡 **Dieser Schritt ist optional.** Standardmäßig verbindest du dich direkt mit einem Remote-Supabase-Projekt (siehe Abschnitt 2). Nur wenn du lieber vollständig lokal entwickelst, ohne Internetverbindung zu Supabase, verwende dieses Setup.

Für ein vollständig lokales Supabase-Setup mit Docker und einer migrations-basierten Entwicklung, kopiere diesen Prompt in deinen KI-Assistenten (z.B. GitHub Copilot in VS Code):

```
Act as an expert DevOps and database engineer. I am developing an app locally. I need to set up a local Supabase instance via Docker and establish a strict migration-based workflow. Please execute the following:

Setup: Initialize a local Supabase environment using Docker and the Supabase CLI. Check automatically before every task if Docker and the Supabase containers are running. When they are down, start them autonomously.

Initial Schema: Create the initial database schema for the app and save it as a formal Supabase migration file.

Environment Variables: Automatically extract the local Supabase connection details (API URL, anon key, service role key, DB URL) and append them to my local .env file.

Autonomous Development & Schema Versioning: You will develop the entire application based on my prompts. You must autonomously create and update tables, generate schemas, and create timestamped migration files for every change. Ensure strict version history and guarantee that absolutely no functions fail due to database inconsistencies.

Production Deployment via MCP: I use the Supabase MCP to push data to production tables. Do not provide manual push commands. Your only job for production is to verify that all local migrations are completely up to date and error-free so the MCP can handle the live deployment seamlessly.

Automated Testing & Verification: Once the Docker setup is running and the schema is applied, automatically write and execute a small integration test. This test must verify that the app can successfully read from and write to the local Docker database. If any errors or bugs are detected, diagnose and fix them immediately on your own.
```

#### Lokale Supabase-Oberfläche im Browser öffnen

Sobald der lokale Docker-Stack läuft (`npx supabase start`), stellt Supabase eine vollständige Studio-Oberfläche bereit:

| Service | URL | Beschreibung |
|---|---|---|
| **Supabase Studio** | `http://localhost:54323` | Datenbank-UI: Tabellen, SQL-Editor, Auth, Storage |
| **REST API** | `http://localhost:54321` | PostgREST API (dein App-Endpoint) |
| **PostgreSQL** | `localhost:54322` | Direktzugriff (z.B. via pgAdmin, TablePlus) |
| **Inbucket (E-Mail)** | `http://localhost:54324` | Lokaler E-Mail-Dienst für Auth-Mails |

> 💡 Die exakten Ports werden nach `npx supabase start` auch in der Konsole ausgegeben. Der Befehl `npx supabase status` zeigt sie jederzeit an.

**Lokale Umgebungsvariablen:**
```
NEXT_PUBLIC_SUPABASE_URL=http://localhost:54321
NEXT_PUBLIC_SUPABASE_ANON_KEY=<aus supabase status kopieren>
SUPABASE_SERVICE_ROLE_KEY=<aus supabase status kopieren>
SUPABASE_DB_URL=postgresql://postgres:postgres@localhost:54322/postgres
```

---

### 3. Supabase MCP & Schemas

#### Supabase MCP für Migrationen verwenden

Der Supabase MCP (Model Context Protocol) Server ermöglicht es deinem KI-Coding-Assistenten (z.B. GitHub Copilot, Cursor), Migrationen und Tabellen direkt zu erstellen und zu verwalten.

- Migrationen werden in `supabase/migrations/` gespeichert.
- Nutze die MCP-Tools, um Tabellen zu erstellen, Schemas zu ändern und Indizes zu verwalten.

#### Arbeiten mit mehreren Schemas

Standardmäßig macht Supabase nur das `public`-Schema über die API verfügbar. Wenn du eigene Schemas brauchst:

**1. Schema erstellen:**
```sql
CREATE SCHEMA IF NOT EXISTS app;
```

**2. Berechtigungen vergeben:**
```sql
GRANT USAGE ON SCHEMA app TO anon, authenticated, service_role;
GRANT ALL ON ALL TABLES IN SCHEMA app TO anon, authenticated, service_role;
ALTER DEFAULT PRIVILEGES IN SCHEMA app
  GRANT ALL ON TABLES TO anon, authenticated, service_role;
GRANT ALL ON ALL SEQUENCES IN SCHEMA app TO anon, authenticated, service_role;
ALTER DEFAULT PRIVILEGES IN SCHEMA app
  GRANT ALL ON SEQUENCES TO anon, authenticated, service_role;
```

**3. Schema über die Supabase API zugänglich machen:**
Gehe zu `Project Settings → API → Exposed schemas` und füge deinen Schema-Namen hinzu.

---

### 4. GitHub Workflow Konfiguration

Dieses Projekt nutzt den **zentralen Wamocon CI/CD-Workflow** aus [`Wamocon/github_workflow`](https://github.com/Wamocon/github_workflow).

#### Übersicht der drei Workflows

| Workflow | Datei | Auslöser | Was es tut |
|---|---|---|---|
| **PR Pipeline** | `pr-pipeline.yml` | **Automatisch** bei jedem PR auf `main`/`master` | Auto-Fix (ESLint + Prettier) → Typecheck + Lint |
| **Deploy** | `deploy.yml` | **Manuell** — kein Auto-Trigger | Baut das Projekt via Vercel CLI und deployed auf Vercel |
| **LP Generator** | `lp-generator.yml` | **Manuell** — kein Auto-Trigger | Generiert eine Landing Page in einem neuen Repo |

> ⚠️ **Kein automatisches Deployment:** Weder ein PR noch ein Push auf `main` startet automatisch ein Deployment. Alle Deployments werden manuell über den Actions-Tab gestartet.

#### Was du tun musst

1. Überprüfe, dass `Wamocon/github_workflow` die korrekte Org/Repo-Referenz ist.
2. Aktiviere Workflow-Schreibrechte:  
   `Settings → Actions → General → Workflow permissions → Allow GitHub Actions to create and approve pull requests`
3. Füge **ein einziges Secret** zu deinem Repository hinzu:
   - Gehe zu `Repository → Settings → Secrets and variables → Actions → New repository secret`
   - Name: `VERCEL_PROJECT_ID`
   - Wert: *(siehe Abschnitt 5 unten)*

> ✅ **Alle anderen benötigten Secrets** (`VERCEL_TOKEN`, `VERCEL_ORG_ID`) sind **bereits auf GitHub-Organisationsebene konfiguriert**.

#### Manuelles Deployment starten

```
Repository auf GitHub → Actions → "Deploy to Vercel" → Run workflow
→ Environment auswählen: production oder preview
→ Run workflow klicken
```

---

### 5. Vercel Deployment-Strategie

#### Schritt 1: GitHub-Org Vercel-App Zugriff gewähren (Einmalig, Kritisch)

> ⚠️ **Dieser Schritt muss VOR dem Vercel-Import durchgeführt werden.** Ohne ihn erscheint dein Repo nicht in der Vercel-Importliste — auch wenn es öffentlich ist.

1. Gehe zu **GitHub → Wamocon Organisation → Settings**  
   `https://github.com/organizations/Wamocon/settings/installations`
2. Suche die **Vercel**-App in der Liste der installierten GitHub Apps.
3. Klicke auf **Configure**.
4. Scrolle zu **Repository access**.
5. Wähle **"Only select repositories"** und füge dein neues Repo hinzu.
6. Klicke **Save**.

#### Schritt 2: Erstmalige Bereitstellung (Einmalig)

1. **Repo vorübergehend öffentlich machen**  
   `Repository → Settings → General → Change visibility → Public`
2. Gehe zu [vercel.com](https://vercel.com) → **Add New Project** → **Import** → dein Repo auswählen.
3. Deploye das Projekt (Vercel erkennt Next.js automatisch).
4. **Vercel Project ID kopieren:**  
   `Vercel Project → Settings → General → Project ID` → ID kopieren.
5. **Zu GitHub Secrets hinzufügen:**  
   `Repository → Settings → Secrets and variables → Actions → New repository secret`  
   Name: `VERCEL_PROJECT_ID` | Wert: die kopierte ID.
6. **Repo auf intern zurücksetzen:**  
   `Repository → Settings → General → Change visibility → Internal`

> 💡 **Nach diesem einmaligen Setup** deployt die GitHub Action via Vercel CLI — Vercel sieht nicht mehr, wer committed, und es gibt keine Team-Seat-Fehler bei einem privaten Hobby-Account.

#### Schritt 3: Umgebungsvariablen in Vercel eintragen

> ⚠️ **WICHTIG:** Gehe zu `Vercel Project → Settings → Environment Variables` und füge **alle** Variablen aus deiner `.env.local` hinzu. Ohne diese wird der Build **fehlschlagen**.

#### Deployment-Ablauf (nach dem Setup)

| Auslöser | Ergebnis |
|---|---|
| PR auf `main`/`master` | PR Pipeline läuft automatisch (kein Deployment) |
| Merge auf `main`/`master` | Kein automatisches Deployment |
| **Manuelles Workflow-Start** | Deployment nach Auswahl: `production` oder `preview` |

**Manuell deployen:**

```
GitHub → Actions → "Deploy to Vercel" → Run workflow
→ environment: production  (für die Live-URL)
→ environment: preview     (für eine Test-URL)
→ db_schema: (leer lassen — Vercel-Umgebungsvariablen werden genutzt)
```

---

### 6. Domain-Verwaltung

1. **Sichere eine Domain** für deine Anwendung über [Strato](https://www.strato.de).
2. Gehe in den Vercel-Projekteinstellungen zu `Settings → Domains` und füge deine Domain hinzu.
3. Konfiguriere die DNS-Einträge bei Strato wie von Vercel angegeben (typischerweise CNAME oder A-Record).

---

### 7. Projekt-Checkliste

- [ ] Landing Page
- [ ] Handbuch / Manual
- [ ] Hauptprozess (Kernfunktion)
- [ ] Video (Demo / Tutorial)
- [ ] Domain (über Strato gesichert)

---

### 8. Landing Page Generator

Das Workflow-File `.github/workflows/lp-generator.yml` generiert eine Landing Page für diese App.

#### Was der Workflow tut

- Liest den Inhalt dieses Repos aus
- Verwendet ein konfigurierbares **Design-Template** als optische Basis
- Lässt GitHub Copilot eine Landing Page generieren und ein neues Repo dafür anlegen

#### Was du ändern kannst

| Eingabe | Standard | Beschreibung |
|---|---|---|
| `custom_template` | `Wamocon/hochzeitsrechner_lp` | Das GitHub-Repo, das als Design-Template dient |
| `custom_name` | `generated_lp` | Name des neu erstellten Landing-Page-Repos |
| `custom_prompt` | *(allgemeiner Prompt)* | Detaillierte Anweisungen für Copilot |

> 💡 **Tipp:** Je spezifischer dein Prompt, desto besser die generierte Landing Page.

#### Wann du den Workflow starten sollst

Starte diesen Workflow **erst dann**, wenn dein Projekt weitgehend fertig dokumentiert ist:
- Handbuch / Manual
- README mit Produktbeschreibung, Features und Zielgruppe
- Rechtstexte / relevante Seiten

> ✅ **Empfehlung:** In der Regel reicht **ein einmaliger Lauf** am Projektende.

#### Manuell ausführen

`Repository → Actions → "🚀 lp-generator" → Run workflow`

1. Wähle in der linken Liste den Workflow **"🚀 lp-generator"**.
2. Klicke rechts auf **"Run workflow"**.
3. Passe Template, Name und Prompt an.
4. Bestätige mit **"Run workflow"**.

#### Wenn der Workflow fehlschlägt

Wenn bereits ein Repository mit gleichem Namen existiert:
1. Ändere den Namen in `custom_name` und starte erneut.
2. Oder lösche das bestehende Ziel-Repo und starte erneut.

---
---

## EN English

---

### Process Overview

Follow these steps in order to go from template to production:

1. **Create GitHub Repo** — Use this template to create a new repository in the Wamocon GitHub organisation.
2. **Clone Repo** — Clone to your local machine and run `npm install`.
3. **Install pre-commit hook** — Run `bash hooks/install.sh`. One-time setup after cloning — protects you from accidentally pushing secrets.
4. **Check GitHub Workflow files** — Open `.github/workflows/deploy.yml` and `.github/workflows/pr-pipeline.yml`, verify that `Wamocon/github_workflow` is the correct reference.
5. **Update `.env.local` & connect Supabase** — Copy `.env.example` → `.env.local`, create a remote Supabase project, and fill in the credentials.
6. **Run locally & start development** — Run `npm run dev` and start building.
7. **GitHub Organisation Settings: Grant Vercel app access** — Go to the organisation settings on GitHub, find the Vercel GitHub App, and grant it access to your new repo. Without this, the repo will not appear in Vercel's import list.
8. **Make repo temporarily public & import in Vercel** — Set the repo to public temporarily and import it in Vercel.
9. **Get Vercel Project ID** — Copy the Vercel Project ID from the Vercel project settings.
10. **Add Vercel Project ID to GitHub secrets** — Add `VERCEL_PROJECT_ID` to your repository's GitHub Actions secrets.
11. **Make repo internal** — Revert the repository visibility to internal.
12. **First manual deployment** — Go to `Actions → "Deploy to Vercel" → Run workflow` and choose `production`.

> 📖 **For detailed information, read below.**

---

### 1. Clone & Setup

```bash
# Clone the repository
git clone https://github.com/Wamocon/<your-repo-name>.git
cd <your-repo-name>

# Install dependencies
npm install

# Copy environment variables
cp .env.example .env.local

# Install pre-commit hook (once, right after npm install)
bash hooks/install.sh

# Start the development server (with Turbopack for fast refresh)
npm run dev
```

Open [http://localhost:3000](http://localhost:3000) in your browser.

**Available scripts:**

| Command | Description |
|---|---|
| `npm run dev` | Start dev server with Turbopack (hot reload) |
| `npm run build` | Create production build |
| `npm run start` | Start production server |
| `npm run lint` | Run ESLint |
| `npm run typecheck` | Run TypeScript type checking |

---

### 1b. Git Workflow & Branching Strategy

> 💡 **After the first push, always work on a branch — never directly on `main`/`master`.**

**Recommended flow:**

```bash
# One-time: push the initial state to main/master
git add .
git commit -m "chore: initial setup"
git push origin <main-or-master>

# From now on: always work on a feature branch
git checkout -b feature/my-feature

# Test locally, then commit
npm run dev        # test in browser
npm run typecheck  # catch type errors
npm run lint       # catch lint issues

git add .
git commit -m "feat: description of change"
git push origin feature/my-feature
```

**Key rules:**

- **Test locally before pushing.** Always run `npm run typecheck` and `npm run lint` before pushing.
- **Push all changes before opening the PR.** Every new push to an open PR triggers the GitHub Actions pipeline — this consumes GitHub Actions minutes.
- **Keep only one PR open at a time** until it is merged.

> ⚠️ **Why this matters:** Every push to an open PR triggers the PR Pipeline (Auto-Fix + Typecheck + Lint). Test locally first — then push, then open the PR.

---

### 1c. Pre-commit Hook — Secret Scanner

The pre-commit hook prevents API keys, tokens, passwords, or other secrets from accidentally being checked into GitHub. The hook scans all files staged for commit **before** the commit is finalised.

#### Installation

```bash
# Run ONCE after cloning — never needs to run again:
bash hooks/install.sh
```

> ✅ **How often?** Exactly **once** — right after `npm install` when first setting up the repo. After installation, the hook runs automatically before **every** `git commit`.

#### What the hook checks

| Pattern | Example |
|--------|---------|
| AWS keys | `AKIA...`, `aws_secret_access_key = ...` |
| GitHub tokens | `ghp_...`, `github_pat_...` |
| Vercel token | `VERCEL_TOKEN = abc123...` |
| Private keys (PEM) | `-----BEGIN PRIVATE KEY-----` |
| Database connection strings | `postgres://user:password@host` |
| Generic tokens | `api_key =`, `client_secret =`, `password =` |
| JWT tokens | `eyJ...` (full `header.payload.signature` form) |
| Supabase service role | `service_role = eyJ...` |
| `.env` files | `.env`, `.env.local`, `.env.production` |

#### When a commit is blocked

```
[pre-commit] BLOCKED — potential secrets detected
  ✖ src/lib/config.ts:12
    Reason: Generic API key
    Content: const API_KEY = "abc123xyz789..."
```

**How to fix:**
1. Remove the secret from the file.
2. Store it in your `.env.local` (already in `.gitignore`).
3. Use GitHub Actions secrets for CI/CD values.

**False positive** (e.g. an example value in documentation):  
Add the comment `# notsecret` at the end of that line.

**Emergency bypass** (absolute last resort — never for real secrets!):
```bash
git commit --no-verify -m "your message"
```

---

### 2. Supabase Setup & Warning

> ⚠️ **CRITICAL: Once you have a Supabase account, store all test data there directly.**
>
> **Do NOT store test data as local files** (e.g. JSON fixtures, SQL dumps). Insert data directly into your Supabase project — via the Dashboard, an MCP tool, or a migration script.

**Steps:**

1. Go to [supabase.com](https://supabase.com) and create a new project.
2. Copy the **Project URL**, **Anon Key**, and **Service Role Key** from  
   `Project Settings → API`.
3. Paste them into your `.env.local` file:
   ```
   NEXT_PUBLIC_SUPABASE_URL=https://your-project-ref.supabase.co
   NEXT_PUBLIC_SUPABASE_ANON_KEY=your-anon-key
   SUPABASE_SERVICE_ROLE_KEY=your-service-role-key
   ```

---

### 2b. Local Supabase Setup via Docker (Optional)

> 💡 **This step is optional.** By default you connect directly to a remote Supabase project (see section 2). Only use this setup if you prefer developing fully offline.

For a fully local Supabase setup with Docker and a migration-based development workflow, copy this prompt into your AI assistant (e.g. GitHub Copilot in VS Code):

```
Act as an expert DevOps and database engineer. I am developing an app locally. I need to set up a local Supabase instance via Docker and establish a strict migration-based workflow. Please execute the following:

Setup: Initialize a local Supabase environment using Docker and the Supabase CLI. Check automatically before every task if Docker and the Supabase containers are running. When they are down, start them autonomously.

Initial Schema: Create the initial database schema for the app and save it as a formal Supabase migration file.

Environment Variables: Automatically extract the local Supabase connection details (API URL, anon key, service role key, DB URL) and append them to my local .env file.

Autonomous Development & Schema Versioning: You will develop the entire application based on my prompts. You must autonomously create and update tables, generate schemas, and create timestamped migration files for every change. Ensure strict version history and guarantee that absolutely no functions fail due to database inconsistencies.

Production Deployment via MCP: I use the Supabase MCP to push data to production tables. Do not provide manual push commands. Your only job for production is to verify that all local migrations are completely up to date and error-free so the MCP can handle the live deployment seamlessly.

Automated Testing & Verification: Once the Docker setup is running and the schema is applied, automatically write and execute a small integration test. This test must verify that the app can successfully read from and write to the local Docker database. If any errors or bugs are detected, diagnose and fix them immediately on your own.
```

#### Viewing the local database in the browser

| Service | URL | Description |
|---|---|---|
| **Supabase Studio** | `http://localhost:54323` | Database UI: tables, SQL editor, Auth, Storage |
| **REST API** | `http://localhost:54321` | PostgREST API (your app endpoint) |
| **PostgreSQL** | `localhost:54322` | Direct DB access (e.g. pgAdmin, TablePlus) |
| **Inbucket (email)** | `http://localhost:54324` | Local email service for Auth emails |

**Local environment variables:**
```
NEXT_PUBLIC_SUPABASE_URL=http://localhost:54321
NEXT_PUBLIC_SUPABASE_ANON_KEY=<copy from supabase status output>
SUPABASE_SERVICE_ROLE_KEY=<copy from supabase status output>
SUPABASE_DB_URL=postgresql://postgres:postgres@localhost:54322/postgres
```

---

### 3. Supabase MCP & Schemas

#### Using Supabase MCP for Migrations

The Supabase MCP (Model Context Protocol) server allows your AI coding assistant to create and manage migrations and tables directly.

- Migrations are stored in `supabase/migrations/`.
- Use the MCP tools to create tables, alter schemas, and manage indexes.

#### Working with Multiple Schemas

By default, Supabase only exposes the `public` schema via the API. If you need custom schemas:

**1. Create the schema:**
```sql
CREATE SCHEMA IF NOT EXISTS app;
```

**2. Grant permissions:**
```sql
GRANT USAGE ON SCHEMA app TO anon, authenticated, service_role;
GRANT ALL ON ALL TABLES IN SCHEMA app TO anon, authenticated, service_role;
ALTER DEFAULT PRIVILEGES IN SCHEMA app
  GRANT ALL ON TABLES TO anon, authenticated, service_role;
GRANT ALL ON ALL SEQUENCES IN SCHEMA app TO anon, authenticated, service_role;
ALTER DEFAULT PRIVILEGES IN SCHEMA app
  GRANT ALL ON SEQUENCES TO anon, authenticated, service_role;
```

**3. Expose the schema via the Supabase API:**
Go to `Project Settings → API → Exposed schemas` and add your custom schema name.

---

### 4. GitHub Workflow Configuration

This project uses the **centralized Wamocon CI/CD workflow** from [`Wamocon/github_workflow`](https://github.com/Wamocon/github_workflow).

#### Overview of the three workflows

| Workflow | File | Trigger | What it does |
|---|---|---|---|
| **PR Pipeline** | `pr-pipeline.yml` | **Automatic** on every PR to `main`/`master` | Auto-Fix (ESLint + Prettier) → Typecheck + Lint |
| **Deploy** | `deploy.yml` | **Manual only** — no auto-trigger | Builds and deploys to Vercel via CLI |
| **LP Generator** | `lp-generator.yml` | **Manual only** — no auto-trigger | Generates a landing page in a new repo |

> ⚠️ **No automatic deployment:** Neither a PR nor a push to `main` triggers a deployment automatically. All deployments are started manually from the Actions tab.

#### What you need to do

1. Verify that `Wamocon/github_workflow` is the correct org/repo reference.
2. Enable workflow write permissions:  
   `Settings → Actions → General → Workflow permissions → Allow GitHub Actions to create and approve pull requests`
3. Add **one single secret** to your repository:
   - Go to `Repository → Settings → Secrets and variables → Actions → New repository secret`
   - Name: `VERCEL_PROJECT_ID`
   - Value: *(see section 5 below)*

> ✅ **All other required secrets** (`VERCEL_TOKEN`, `VERCEL_ORG_ID`) are **already configured at the GitHub Organisation level**.

#### Triggering a manual deployment

```
GitHub repo → Actions → "Deploy to Vercel" → Run workflow
→ Choose environment: production or preview
→ Click "Run workflow"
```

---

### 5. Vercel Deployment Strategy

#### Step 1: Grant Vercel GitHub App access in Org Settings (One-time, Critical)

> ⚠️ **This step must be done BEFORE importing in Vercel.** Without it, your repo will not appear in Vercel's import list — even if it is public.

1. Go to **GitHub → Wamocon Organisation → Settings → Installed GitHub Apps**  
   `https://github.com/organizations/Wamocon/settings/installations`
2. Find the **Vercel** app in the list.
3. Click **Configure**.
4. Scroll to **Repository access**.
5. Select **"Only select repositories"** and add your new repo.
6. Click **Save**.

#### Step 2: Initial Deployment (One-Time)

1. **Make the repo public** temporarily  
   `Repository → Settings → General → Change visibility → Public`
2. Go to [vercel.com](https://vercel.com) → **Add New Project** → **Import** → select your repo.
3. Deploy the project (Vercel detects Next.js automatically).
4. **Copy the Vercel Project ID:**  
   `Vercel Project → Settings → General → Project ID` → copy the value.
5. **Add to GitHub secrets:**  
   `Repository → Settings → Secrets and variables → Actions → New repository secret`  
   Name: `VERCEL_PROJECT_ID` | Value: the ID you copied.
6. **Revert the repo to internal:**  
   `Repository → Settings → General → Change visibility → Internal`

> 💡 **After this one-time setup**, the GitHub Action deploys via Vercel CLI — Vercel no longer sees who committed, and there are no team seat errors with a personal Hobby account.

#### Step 3: Add environment variables in Vercel

> ⚠️ **CRUCIAL:** Go to `Vercel Project → Settings → Environment Variables` and add **all** variables from your `.env.local`. Without these, the build **will fail**.

#### Deployment flow (after setup)

| Trigger | Result |
|---|---|
| PR targeting `main`/`master` | PR Pipeline runs automatically (no deployment) |
| Merge / push to `main`/`master` | No automatic deployment |
| **Manual workflow run** | Deploys to the chosen environment: `production` or `preview` |

**How to deploy manually:**

```
GitHub → Actions → "Deploy to Vercel" → Run workflow
→ environment: production  (for the live URL)
→ environment: preview     (for a temporary test URL)
→ db_schema: (leave empty — Vercel env vars are used)
```

---

### 6. Domain Management

1. **Secure a domain** for your application via [Strato](https://www.strato.de).
2. In the Vercel project settings, go to `Settings → Domains` and add your domain.
3. Configure the DNS records at Strato as shown by Vercel (typically a CNAME or A record).

---

### 7. Project Checklist

- [ ] Landing Page
- [ ] Handbuch / Manual
- [ ] Main Process (core feature)
- [ ] Video (demo / tutorial)
- [ ] Domain (secured via Strato)

---

### 8. Landing Page Generator

The workflow file `.github/workflows/lp-generator.yml` generates a landing page for this app by calling a central, reusable Wamocon workflow.

#### What the workflow does

- Reads the contents of this repository
- Uses a configurable **design template** as a visual base
- Lets GitHub Copilot generate a landing page and create a new repository for it

#### What you can change

| Input | Default | Description |
|---|---|---|
| `custom_template` | `Wamocon/hochzeitsrechner_lp` | The GitHub repo used as the design template |
| `custom_name` | `generated_lp` | Name for the newly created landing page repository |
| `custom_prompt` | *(generic prompt)* | Detailed instructions for Copilot |

> 💡 **Tip:** The more specific your prompt, the better the generated landing page.

#### When you should run this workflow

Run this workflow **only after** your project is mostly complete:
- Handbook / Manual
- README with product description, features, and target audience
- Legal pages / relevant links

> ✅ **Recommendation:** In most cases, you only need to run it **once** at the end of the project.

#### Running manually

`Repository → Actions → "🚀 lp-generator" → Run workflow`

1. Select **"🚀 lp-generator"** from the left workflow list.
2. Click **"Run workflow"** on the right.
3. Adjust the input fields (template, name, prompt) as needed.
4. Confirm with **"Run workflow"**.

#### If the workflow fails (repository name already exists)

1. Change the `custom_name` value and run the workflow again.
2. Or delete the existing target repository (if no longer needed) and run again.

## ./IDEA.md
# Projektidee

> Fuell diese Datei aus, bevor du den Skill `anforderungsdokument` aufrufst.
> Je mehr Details du hier schreibst, desto besser wird das generierte Dokument.
> Anleitung: `.github/skills/anforderungsdokument/SKILL.md`

---

## Projektname

[Name der App, z.B. "Universal Inventory Manager"]

---

## Beschreibung

[Was macht die App? Welches Problem loest sie? Sei konkret.

Beispiel:
"Eine webbasierte Lagerverwaltungs-App fuer kleine und mittelgrosse Unternehmen in
Deutschland. Sie ersetzt Excel-basierte Bestandslisten durch ein strukturiertes System
mit Echtzeit-Uebersicht, QR-Code-Scanfunktion und automatisierten Benachrichtigungen.
Das Ziel ist es, manuelle Fehler bei der Lagerhaltung um mindestens 80 % zu reduzieren
und den Zeitaufwand fuer Inventurprozesse auf ein Minimum zu senken."]

---

## Zielgruppe

[Wer soll die App nutzen? Branche, Unternehmensgroesse, Rolle.

Beispiel:
"Lager- und Logistikleiter sowie Mitarbeiter in KMU mit 10 bis 200 Angestellten,
insbesondere im Einzel-, Gross- und Onlinehandel in Deutschland und dem
deutschsprachigen Europa."]

---

## Kernproblem

[Das konkrete Problem, das OHNE die App besteht.

Beispiel:
"Lagerfehler durch manuelle Erfassung in Excel kosten KMU durchschnittlich
14 % des Jahresumsatzes. Bestehende Enterprise-Loesungen wie SAP EWM sind
fuer kleinere Betriebe zu komplex und zu teuer."]

---

## Gewuenschte Hauptfunktionen (Version 1)

[Liste der wichtigsten Funktionen. Beispiel:]

- Artikel anlegen, bearbeiten, loeschen
- Lagerbestand in Echtzeit einsehen
- QR-Code-Etiketten generieren und scannen
- Rollenbasierter Zugriff (Admin, Manager, Mitarbeiter)
- Mehrsprachige Oberflaeche (Deutsch und Englisch)
- Dashboard mit Bestandsuebersicht

---

## Tech-Stack (optional, wenn bekannt)

[Bekannte oder gewuenschte Technologien. Standardempfehlung falls leer:
Next.js, React, TypeScript, Supabase, Tailwind CSS, Vercel]

---

## Marktsegment und Branche

[In welchem Markt soll die App operieren?

Beispiel: "Lagerverwaltungssoftware (WMS) fuer KMU in Deutschland, Segment Mittelstand"]

---

## Bekannte Wettbewerber (optional)

[Bestehende Loesungen, die aehnliches tun.

Beispiel: SAP EWM, Sage Wawi, Pickware, Odoo Inventory, Lightspeed]

---

## Ersteller

[Vollstaendiger Name]

---

## Empfaenger (Geschaeftsfuehrung)

[Name und Titel des Entscheidungstraegers, z.B. "waleri moretz, CEO WAMOCON GmbH"]

---

## Welle und Version

1

---

## Zusaetzliche Hinweise (optional)

[Besondere Anforderungen, Einschraenkungen, Deadlines oder andere relevante
Informationen, die im Dokument beruecksichtigt werden sollen.]

## ./docs/PROGRESS.md
﻿# Vertragsmanager â€” Implementierungsfortschritt V1

**Stand:** 2025-07-15  
**Status:** V1 Kernfunktionen implementiert â€” bereit zum manuellen Testen

---

## Zusammenfassung

Alle V1-Module aus dem Anforderungsdokument (Abschnitt 7) wurden als funktionsfÃ¤hige Next.js-Seiten implementiert. Die App nutzt eine lokale Supabase-Instanz (Docker) mit vollstÃ¤ndigem Datenbankschema, RLS-Policies und realistischen Testdaten.

---

## Technologie-Stack

| Komponente | Technologie | Version |
|---|---|---|
| Framework | Next.js (App Router, Turbopack) | 16.2.1 |
| Sprache | TypeScript (strict) | 5.x |
| Styling | Tailwind CSS | v4 |
| Backend/DB | Supabase (lokal, Docker) | CLI 2.91.2 |
| i18n | next-intl | DE + EN |
| Icons | lucide-react | â€” |
| Charts | recharts | â€” |
| Datumsberechnung | date-fns | â€” |

---

## Lokale Supabase-Konfiguration

Die lokale Instanz lÃ¤uft auf eigenen Ports, um Konflikte mit anderen Supabase-Projekten zu vermeiden:

| Service | Port |
|---|---|
| API (PostgREST) | 54351 |
| PostgreSQL | 54352 |
| Studio | 54353 |
| Inbucket (E-Mail) | 54354 |
| Analytics | 54357 |
| Inspector | 8093 |

**Zugang:**
- Studio: http://127.0.0.1:54353
- Inbucket (E-Mail): http://127.0.0.1:54354
- API: http://127.0.0.1:54351

---

## Implementierte Module (V1)

### âœ… Vertragsverwaltung (VE-01 bis VE-09)
- VertragsÃ¼bersicht mit Karten- und Listenansicht
- Suche nach Name, Anbieter, Kategorie
- Filter nach Kategorie und Status
- Sortierung (Name, Kosten, Frist, Anbieter)
- Vertragsdetailseite mit allen Feldern
- Vertrag erstellen, bearbeiten, lÃ¶schen
- Erinnerungen pro Vertrag aktivieren/deaktivieren
- **Seiten:** `/contracts`, `/contracts/new`, `/contracts/[id]`

### âœ… Dashboard & Analytik (NV-02, KO-01 bis KO-08)
- KPI-Karten: GesamtvertrÃ¤ge, Monats-/Jahreskosten, anstehende Fristen
- Kosten nach Kategorie (PieChart)
- Top-Anbieter (Balkendiagramm)
- Zahlungskalender (12 Monate)
- CSV-Export der Analysedaten
- **Seiten:** `/dashboard`, `/analytics`

### âœ… Fristen & Erinnerungen (FR-01 bis FR-08)
- Fristenliste gruppiert nach Dringlichkeit (Ã¼berfÃ¤llig/diese Woche/dieser Monat/spÃ¤ter)
- Tage bis Frist berechnet und angezeigt
- .ics Kalender-Export fÃ¼r einzelne Fristen
- Farbcodierung nach Dringlichkeit
- **Seite:** `/deadlines`

### âœ… KÃ¼ndigungsvorlagen (KV-01 bis KV-06)
- 4 Vorlagentypen: Ordentliche KÃ¼ndigung, SonderkÃ¼ndigung, NichtverlÃ¤ngerung, AuÃŸerordentliche KÃ¼ndigung
- Vertrag auswÃ¤hlen â†’ Vorlage wird automatisch befÃ¼llt
- Download als Textdatei
- Rechtlicher Hinweis zu juristischer Beratung
- **Seite:** `/templates`

### âœ… Benachrichtigungen (FR-05, BN-01, BN-02)
- In-App-Benachrichtigungszentrale
- Einzeln als gelesen markieren
- Alle als gelesen markieren
- Unread-Badge in Sidebar
- **Seite:** `/notifications`

### âœ… Profil & Datenschutz (PR-01, PR-02, DS-01 bis DS-05)
- Profilbearbeitung (Name, Firma)
- DSGVO-Datenexport (JSON)
- **Seite:** `/profile`

### âœ… Einstellungen (NV-03, SP-01)
- Theme-Umschaltung: Hell / Dunkel / System
- Sprachanzeige
- **Seite:** `/settings`

### âœ… Organisationsverwaltung (OV-01 bis OV-04)
- Mitgliederliste mit Rollen (Admin/Member/Viewer)
- Mitglieder per E-Mail einladen
- Rollen Ã¤ndern
- Mitglieder entfernen
- Ausstehende Einladungen anzeigen
- Audit-Log (letzte Aktionen)
- **Seite:** `/organization`

### âœ… Authentifizierung (AU-01 bis AU-04)
- Login mit E-Mail/Passwort (Supabase Auth)
- Registrierung mit automatischer Demo-Daten-Erstellung
- Middleware-basierter Routenschutz
- Logout Ã¼ber Sidebar
- **Seiten:** `/login`, `/register`

### âœ… Rechtliche Seiten (RE-01)
- Impressum (WAMOCON GmbH)
- DatenschutzerklÃ¤rung (DSGVO-konform)
- AGB
- **Seiten:** `/legal/imprint`, `/legal/privacy`, `/legal/terms`

### âœ… Landingpage
- Hero-Section mit CTA
- Feature-Grid (6 Features)
- Preistabelle (4 Stufen: Free/Starter/Pro/Business)
- Footer mit rechtlichen Links
- **Seite:** `/`

---

## Datenbankschema

### Tabellen (9)
| Tabelle | Zweck |
|---|---|
| `organizations` | Organisationen (Multi-Tenant) |
| `organization_members` | Mitglieder mit Rollen |
| `profiles` | Benutzerprofile (via Trigger bei Registrierung) |
| `categories` | Vertragskategorien (7 System-Kategorien) |
| `contracts` | VertrÃ¤ge mit allen V1-Feldern |
| `reminders` | Erinnerungen pro Vertrag |
| `notifications` | In-App-Benachrichtigungen |
| `invitations` | E-Mail-Einladungen |
| `audit_log` | Aktionsprotokoll |

### RLS-Policies
Alle Tabellen haben Row-Level-Security aktiviert. Zugriff ist auf die eigene Organisation beschrÃ¤nkt.

### Trigger
- `handle_new_user`: Erstellt automatisch ein Profil bei Registrierung
- `compute_cancellation_deadline`: Berechnet KÃ¼ndigungsfrist bei VertragsÃ¤nderung
- `auto_update_contract_status`: Aktualisiert Vertragsstatus basierend auf Enddatum
- `create_default_reminders`: Erstellt Standard-Erinnerungen bei neuem Vertrag

### Seed-Daten
Die Funktion `setup_demo_data(user_id, org_name)` wird bei der Registrierung aufgerufen und erstellt:
- 1 Organisation mit dem Benutzer als Admin
- 7 System-Kategorien (Software, Cloud, Telekommunikation, Versicherung, Immobilien, Fahrzeuge, Marketing)
- 15 realistische Demo-VertrÃ¤ge (Microsoft 365, Adobe CC, Slack, GitHub, Jira, Vercel, Telekom, Vodafone, Allianz, HDI, BÃ¼ro-Miete, BMW Leasing, Google Workspace, HubSpot, DATEV)

---

## Build-Status

| Check | Status |
|---|---|
| TypeScript (`npm run typecheck`) | âœ… 0 Fehler |
| ESLint (`npm run lint`) | âœ… 0 Fehler (4 Warnings) |
| Build (`npm run build`) | âœ… Erfolgreich |
| Dev Server (`npm run dev`) | âœ… LÃ¤uft auf Port 3001 |

---

## Bekannte EinschrÃ¤nkungen & Offene Punkte

### Noch nicht implementiert (V1-Scope, aber komplexere Integration nÃ¶tig)

| Feature | Anforderungs-ID | Grund |
|---|---|---|
| Dokumenten-Upload mit OCR | DO-01 bis DO-12 | Erfordert Azure Document Intelligence API-Key und Storage-Bucket-Konfiguration |
| E-Mail-Erinnerungen | FR-04 | Erfordert E-Mail-Service (z. B. Resend, SendGrid) |
| PDF/DOCX-Generierung fÃ¼r KÃ¼ndigungsvorlagen | KV-05, KV-06 | `docx`-Package installiert, aber Export-Logik noch nicht verdrahtet (aktuell nur .txt) |
| Stripe-Zahlungsintegration | RE-04 | Erfordert Stripe-Account und API-Keys |
| Cookie-Banner | RE-02 | Kann als separate Komponente nachgerÃ¼stet werden |
| Zwei-Faktor-Authentifizierung | AU-05 | Supabase MFA-Konfiguration erforderlich |

### Technische Hinweise

1. **Middleware-Deprecation:** Next.js 16 hat `middleware.ts` zugunsten von `proxy.ts` deprecated. Die aktuelle Middleware funktioniert, sollte aber bei Gelegenheit migriert werden.
2. **Supabase-Typen:** Die Supabase-Client-Erstellung nutzt aktuell keine generischen DB-Typen (`Database`), um KompatibilitÃ¤tsprobleme mit `@supabase/ssr` zu vermeiden. Typsicherheit wird Ã¼ber die Interface-Definitionen in `src/types/database.ts` gewÃ¤hrleistet.
3. **Theme-Switching:** Implementiert via `localStorage` und `dark`-CSS-Klasse. Kein serverseitiges Cookie â€” bei Seitenneuladen kann es zu einem kurzen Flash kommen.

---

## Testanleitung

### Voraussetzungen
- Docker lÃ¤uft
- Lokale Supabase ist gestartet (`npx supabase start` im Projektverzeichnis)

### Schritte
1. `npm run dev` â€” Dev-Server starten
2. http://localhost:3001 Ã¶ffnen â€” Landingpage prÃ¼fen
3. "Registrieren" klicken â†’ E-Mail und Passwort eingeben
4. Inbucket Ã¶ffnen (http://127.0.0.1:54354) â†’ BestÃ¤tigungsmail finden und Link klicken
5. Einloggen â†’ Dashboard mit 15 Demo-VertrÃ¤gen sollte erscheinen
6. Alle Module durchklicken und FunktionalitÃ¤t prÃ¼fen

### Supabase Studio
- http://127.0.0.1:54353 â€” Tabellen, Daten, Auth-Benutzer direkt einsehen

---

## Dateistruktur (Ãœbersicht)

```
src/
â”œâ”€â”€ app/
â”‚   â”œâ”€â”€ (app)/                    # Authentifizierter Bereich
â”‚   â”‚   â”œâ”€â”€ analytics/            # Kostenanalyse
â”‚   â”‚   â”œâ”€â”€ contracts/            # Vertragsverwaltung
â”‚   â”‚   â”‚   â”œâ”€â”€ [id]/             # Vertragsdetail
â”‚   â”‚   â”‚   â””â”€â”€ new/              # Neuer Vertrag
â”‚   â”‚   â”œâ”€â”€ dashboard/            # Dashboard
â”‚   â”‚   â”œâ”€â”€ deadlines/            # Fristenverwaltung
â”‚   â”‚   â”œâ”€â”€ notifications/        # Benachrichtigungen
â”‚   â”‚   â”œâ”€â”€ organization/         # Organisationsverwaltung
â”‚   â”‚   â”œâ”€â”€ profile/              # Profil
â”‚   â”‚   â”œâ”€â”€ settings/             # Einstellungen
â”‚   â”‚   â”œâ”€â”€ templates/            # KÃ¼ndigungsvorlagen
â”‚   â”‚   â””â”€â”€ layout.tsx            # App-Shell mit Sidebar
â”‚   â”œâ”€â”€ legal/                    # Rechtliche Seiten
â”‚   â”œâ”€â”€ login/                    # Login
â”‚   â”œâ”€â”€ register/                 # Registrierung
â”‚   â”œâ”€â”€ layout.tsx                # Root-Layout
â”‚   â”œâ”€â”€ page.tsx                  # Landingpage
â”‚   â””â”€â”€ globals.css               # Globale Styles
â”œâ”€â”€ components/
â”‚   â”œâ”€â”€ layout/
â”‚   â”‚   â””â”€â”€ Sidebar.tsx           # Navigation
â”‚   â””â”€â”€ ui/                       # Wiederverwendbare UI-Komponenten
â”œâ”€â”€ lib/
â”‚   â”œâ”€â”€ supabase/                 # Supabase-Client-Utilities
â”‚   â””â”€â”€ utils.ts                  # cn() Helper
â”œâ”€â”€ messages/
â”‚   â”œâ”€â”€ de.json                   # Deutsche Ãœbersetzungen
â”‚   â””â”€â”€ en.json                   # Englische Ãœbersetzungen
â”œâ”€â”€ i18n/
â”‚   â””â”€â”€ request.ts                # next-intl Konfiguration
â”œâ”€â”€ types/
â”‚   â””â”€â”€ database.ts               # TypeScript-Interfaces
â””â”€â”€ middleware.ts                  # Route-Schutz
supabase/
â”œâ”€â”€ config.toml                   # Lokale Supabase-Konfiguration
â”œâ”€â”€ migrations/
â”‚   â””â”€â”€ 20260416120000_initial_schema.sql  # Datenbankschema
â””â”€â”€ seed.sql                      # Demo-Daten-Funktion
```



## ./docs/TESTPLAN.md
﻿# Vertragsmanager â€” Testplan

## Testbenutzer

| E-Mail | Passwort | Rolle | Unternehmen |
|---|---|---|---|
| `superadmin@Vertragsmanager.de` | `Test1234!` | Superadmin | â€” |
| `admin@wamocon.de` | `Test1234!` | Unternehmensadmin | WAMOCON GmbH |
| `manager@wamocon.de` | `Test1234!` | Vertragsmanager | WAMOCON GmbH |
| `leser@wamocon.de` | `Test1234!` | Leser | WAMOCON GmbH |
| `admin@techcorp.de` | `Test1234!` | Unternehmensadmin | TechCorp AG |
| `manager@techcorp.de` | `Test1234!` | Vertragsmanager | TechCorp AG |
| `leser@techcorp.de` | `Test1234!` | Leser | TechCorp AG |

---

## A. Automatisierte Komponententests (Unit/Integration)

Voraussetzung: `npm install -D vitest @testing-library/react @testing-library/jest-dom`

### A.1 UI Komponenten

| ID | Komponente | Test | Erwartung |
|---|---|---|---|
| A1.1 | `Button` | Render mit variant `primary`, `secondary`, `danger`, `ghost` | Korrekte CSS-Klassen, onClick aufgerufen |
| A1.2 | `Input` | Render mit/ohne `error`-Prop | Fehlermeldung wird angezeigt, roter Border |
| A1.3 | `Select` | Render mit Options-Array | Alle Optionen sichtbar, onChange funktioniert |
| A1.4 | `Card` | Render mit `CardHeader`, `CardTitle`, `CardContent` | Korrekt verschachtelt |
| A1.5 | `StatusBadge` | Render mit `active`, `expired`, `expiring_soon`, `cancelled` | Richtige Farbe und Text |
| A1.6 | `Sidebar` | Render mit `unreadCount > 0` | Badge mit Zahl sichtbar |
| A1.7 | `AdminSidebar` | Render | Alle 3 Nav-Links vorhanden (Dashboard, Unternehmen, Benutzer) |

### A.2 GeschÃ¤ftslogik

| ID | Funktion | Test | Erwartung |
|---|---|---|---|
| A2.1 | `computeMonthlyAmount` | `monthly=100` | `100` |
| A2.2 | `computeMonthlyAmount` | `quarterly=300` | `100` |
| A2.3 | `computeMonthlyAmount` | `yearly=1200` | `100` |
| A2.4 | `computeMonthlyAmount` | `one_time=500` | `0` |
| A2.5 | Vertragsfilter | Suche "Adobe" | Nur VertrÃ¤ge mit "Adobe" im Namen/Anbieter |
| A2.6 | Vertragsfilter | Status "active" | Nur aktive VertrÃ¤ge |
| A2.7 | Vertragsfilter | Kategorie "Software" | Nur Software-VertrÃ¤ge |
| A2.8 | Admin-Benutzerfilter | Rolle "reader" | Nur Leser angezeigt |
| A2.9 | Admin-Benutzerfilter | Rolle "reader" + Suche "Weber" | 0 Ergebnisse + Hinweis "Filter aktiv" |
| A2.10 | Admin-Benutzerfilter | Filter zurÃ¼cksetzen | Alle Benutzer wieder sichtbar |

### A.3 Supabase RLS (Integrationstests)

| ID | Szenario | Test | Erwartung |
|---|---|---|---|
| A3.1 | Org-Isolation | WAMOCON-User liest TechCorp-VertrÃ¤ge | Leere Ergebnisse (kein Fehler) |
| A3.2 | Org-Isolation | TechCorp-User liest WAMOCON-Mitglieder | Leere Ergebnisse |
| A3.3 | RollenprÃ¼fung | Reader versucht Contract INSERT | RLS-Fehler |
| A3.4 | RollenprÃ¼fung | Reader versucht Contract DELETE | RLS-Fehler |
| A3.5 | RollenprÃ¼fung | Manager kann Contract INSERT | Erfolg |
| A3.6 | RollenprÃ¼fung | Manager versucht Member DELETE | RLS-Fehler |
| A3.7 | Superadmin | Superadmin liest alle Profiles | Alle 7 Profile sichtbar |
| A3.8 | Profil-Sichtbarkeit | User sieht Profil von Org-Kollegen | Sichtbar |
| A3.9 | Profil-Sichtbarkeit | User sieht Profil aus fremder Org | Nicht sichtbar |

---

## B. Manueller E2E-Testplan

### B.1 Authentifizierung & Registrierung

| ID | Prozess | Schritte | Erwartung |
|---|---|---|---|
| B1.1 | Login (Unternehmensadmin) | 1. Ã–ffne `/login` 2. `admin@wamocon.de` / `Test1234!` 3. Klick "Anmelden" | Weiterleitung zu `/dashboard`, 10 WAMOCON-VertrÃ¤ge sichtbar |
| B1.2 | Login (Superadmin) | 1. Ã–ffne `/login` 2. `superadmin@Vertragsmanager.de` / `Test1234!` | Weiterleitung zu `/admin` (Superadmin Dashboard) |
| B1.3 | Login (Reader) | `leser@wamocon.de` â†’ `/dashboard` | Dashboard sichtbar, kein "Vertrag erstellen"-Button |
| B1.4 | Login (Manager) | `manager@wamocon.de` â†’ `/dashboard` | Dashboard sichtbar |
| B1.5 | Login falsch | Falsches Passwort eingeben | Fehlermeldung "Invalid login credentials" |
| B1.6 | Registrierung | 1. Ã–ffne `/register` 2. Alle Felder ausfÃ¼llen 3. Absenden | Erfolgsseite mit Inbucket-Link (Port 54354) |
| B1.7 | E-Mail-BestÃ¤tigung | 1. Registrieren 2. Inbucket Ã¶ffnen 3. BestÃ¤tigungslink klicken | Weiterleitung zu `/dashboard`, Org wird automatisch erstellt |
| B1.8 | Logout | Sidebar â†’ Logout-Button | Weiterleitung zu `/login` |

### B.2 Dashboard

| ID | Prozess | User | Erwartung |
|---|---|---|---|
| B2.1 | Ãœbersicht | admin@wamocon.de | KPI-Karten: Aktive VertrÃ¤ge, Monatl. Kosten, Ablaufende, Benachrichtigungen |
| B2.2 | Letzte VertrÃ¤ge | admin@wamocon.de | Liste der neuesten VertrÃ¤ge angezeigt |
| B2.3 | Ungelesene Benachrichtigungen | leser@wamocon.de | Badge in Sidebar zeigt "1" (Willkommens-Nachricht) |
| B2.4 | Org-Isolation | admin@techcorp.de | Nur TechCorp-VertrÃ¤ge sichtbar (5 StÃ¼ck) |

### B.3 Vertragsverwaltung

| ID | Prozess | User | Schritte | Erwartung |
|---|---|---|---|---|
| B3.1 | VertrÃ¤ge ansehen | admin@wamocon.de | Ã–ffne `/contracts` | 10 VertrÃ¤ge, Karten- und Listenansicht umschaltbar |
| B3.2 | Suche | admin@wamocon.de | Suche "Adobe" | 1 Ergebnis: Adobe Creative Cloud |
| B3.3 | Filter Status | admin@wamocon.de | Filter "Aktiv" | Nur aktive VertrÃ¤ge |
| B3.4 | Filter Kategorie | admin@wamocon.de | Filter "Software & SaaS" | Nur Software-VertrÃ¤ge |
| B3.5 | Sortierung | admin@wamocon.de | Sortiere nach "Kosten" | HÃ¶chste Kosten zuerst |
| B3.6 | Vertrag erstellen | admin@wamocon.de | `/contracts/new` â†’ Formular ausfÃ¼llen â†’ Speichern | Neuer Vertrag in Liste, Erinnerungen automatisch erstellt |
| B3.7 | Vertrag bearbeiten | admin@wamocon.de | Vertrag Ã¶ffnen â†’ "Bearbeiten" â†’ Ã„ndern â†’ Speichern | Ã„nderungen Ã¼bernommen |
| B3.8 | Manager-Sicherheitshinweis | manager@wamocon.de | Vertrag Ã¶ffnen â†’ "Bearbeiten" â†’ Speichern | Sicherheitshinweis: "Ã„nderungen werden protokolliert" â†’ BestÃ¤tigen |
| B3.9 | Vertrag lÃ¶schen | admin@wamocon.de | Vertrag Ã¶ffnen â†’ "LÃ¶schen" â†’ BestÃ¤tigen | Vertrag entfernt |
| B3.10 | LÃ¶schen (Reader) | leser@wamocon.de | Vertrag Ã¶ffnen | Kein "Bearbeiten" und kein "LÃ¶schen" Button |
| B3.11 | LÃ¶schen (Manager) | manager@wamocon.de | Vertrag Ã¶ffnen | "Bearbeiten" sichtbar, KEIN "LÃ¶schen" |
| B3.12 | Vertrag erstellen (Reader) | leser@wamocon.de | Ã–ffne `/contracts` | Kein "Vertrag erstellen" Button, `/contracts/new` leitet zu `/contracts` weiter |
| B3.13 | Vertragsdetails | admin@wamocon.de | Vertrag anklicken | Details, Erinnerungen, Kommentare angezeigt |

### B.4 Kommentarsystem

| ID | Prozess | User | Schritte | Erwartung |
|---|---|---|---|---|
| B4.1 | Kommentar schreiben | leser@wamocon.de | Vertrag Ã¶ffnen â†’ Kommentar eintippen â†’ Senden | Kommentar erscheint mit Name und Zeitstempel |
| B4.2 | Kommentar schreiben (Admin) | admin@wamocon.de | Vertrag Ã¶ffnen â†’ Kommentar schreiben | Kommentar erscheint |
| B4.3 | Benachrichtigung | leser@wamocon.de kommentiert | Admin + Manager erhalten Benachrichtigung | Benachrichtigung in `/notifications` |

### B.5 KÃ¼ndigungsfristen & Erinnerungen

| ID | Prozess | User | Erwartung |
|---|---|---|---|
| B5.1 | Fristen-Seite | admin@wamocon.de â†’ `/deadlines` | KalenderÃ¼bersicht mit farbcodierten Fristen |
| B5.2 | Erinnerungen ein/aus | admin@wamocon.de â†’ Vertrag â†’ Erinnerungen | Erinnerung aktivieren/deaktivieren mÃ¶glich |

### B.6 Organisation & Mitgliederverwaltung

| ID | Prozess | User | Schritte | Erwartung |
|---|---|---|---|---|
| B6.1 | Org-Seite | admin@wamocon.de â†’ `/organization` | WAMOCON GmbH, 3 Mitglieder sichtbar mit Rollen |
| B6.2 | Mitglied einladen | admin@wamocon.de | E-Mail + Rolle eingeben â†’ Einladen | Einladung in Tabelle, E-Mail versendet |
| B6.3 | Rollenzuweisung | admin@wamocon.de | Rolle eines Mitglieds Ã¤ndern | Rolle aktualisiert |
| B6.4 | Org-Seite (Reader) | leser@wamocon.de â†’ `/organization` | Org sichtbar, KEIN Einladen/Ã„ndern-Button |

### B.7 Analytics

| ID | Prozess | User | Erwartung |
|---|---|---|---|
| B7.1 | Ãœbersicht | admin@wamocon.de â†’ `/analytics` | Monatliche Kosten, Verteilung nach Kategorie/Status, VerlÃ¤ngerungs-Prognose |
| B7.2 | Nur eigene Org | admin@techcorp.de â†’ `/analytics` | Nur TechCorp-Analytics (5 VertrÃ¤ge) |

### B.8 Profil & Einstellungen

| ID | Prozess | User | Erwartung |
|---|---|---|---|
| B8.1 | Profil | admin@wamocon.de â†’ `/profile` | Name, E-Mail, Sprache, Firma angezeigt |
| B8.2 | Name Ã¤ndern | admin@wamocon.de â†’ Name aktualisieren | Gespeichert, sofort sichtbar |
| B8.3 | Theme | `/settings` â†’ Dark Mode | Gesamte App im Dark Mode |
| B8.4 | Theme zurÃ¼ck | `/settings` â†’ Light Mode | Light Mode wieder aktiv |

### B.9 Benachrichtigungen

| ID | Prozess | User | Erwartung |
|---|---|---|---|
| B9.1 | Benachrichtigungsliste | admin@wamocon.de â†’ `/notifications` | Willkommens-Benachrichtigung sichtbar |
| B9.2 | Als gelesen markieren | Benachrichtigung anklicken/markieren | Badge-ZÃ¤hler sinkt |

### B.10 Vorlagen

| ID | Prozess | User | Erwartung |
|---|---|---|---|
| B10.1 | E-Mail-Vorlagen | admin@wamocon.de â†’ `/templates` | KÃ¼ndigungsbrief-Vorlagen fÃ¼r aktive VertrÃ¤ge sichtbar |

---

## C. Superadmin E2E-Tests

### C.1 Admin-Dashboard

| ID | Prozess | Schritte | Erwartung |
|---|---|---|---|
| C1.1 | Dashboard | Login als superadmin â†’ `/admin` | KPI-Karten: Gesamt-Unternehmen, Aktive, Gesamt-Benutzer, Inaktive |
| C1.2 | Letzte Unternehmen | `/admin` | Liste der zuletzt registrierten Unternehmen |

### C.2 Unternehmensverwaltung

| ID | Prozess | Schritte | Erwartung |
|---|---|---|---|
| C2.1 | UnternehmensÃ¼bersicht | `/admin/companies` | 2 Unternehmen: WAMOCON GmbH (3 Mitglieder, 10 VertrÃ¤ge), TechCorp AG (3 Mitglieder, 5 VertrÃ¤ge) |
| C2.2 | Unternehmensdetail | Klick auf WAMOCON | Benutzer nach Rolle gruppiert (Unternehmensadmin: Anna Weber, Vertragsmanager: Max MÃ¼ller, Leser: Lisa Schmidt), VertrÃ¤gsliste |
| C2.3 | Unternehmen deaktivieren | "Deaktivieren" klicken | Status wechselt zu "Inaktiv", Button wechselt zu "Aktivieren" |
| C2.4 | Unternehmen aktivieren | "Aktivieren" klicken | Status wechselt zu "Aktiv" |

### C.3 Benutzerverwaltung

| ID | Prozess | Schritte | Erwartung |
|---|---|---|---|
| C3.1 | BenutzerÃ¼bersicht | `/admin/users` | 6 Benutzer (ohne Superadmin selbst), mit Name, E-Mail, Unternehmen, Rolle, Status |
| C3.2 | Suche | "Weber" eingeben | 1 Ergebnis: Anna Weber |
| C3.3 | Filter: Rolle | "Leser" auswÃ¤hlen | 2 Ergebnisse: Lisa Schmidt, Peter Braun |
| C3.4 | Filter: Unternehmen | "WAMOCON GmbH" auswÃ¤hlen | 3 Ergebnisse |
| C3.5 | Filter: Status | "Aktiv" auswÃ¤hlen | Nur aktive Benutzer |
| C3.6 | Filter + Suche | Filter "TechCorp" + Suche "Weber" | 0 Ergebnisse + Hinweis: "es gibt Treffer fÃ¼r Weber, aber diese werden durch aktive Filter ausgeblendet" |
| C3.7 | Filter zurÃ¼cksetzen | "Filter zurÃ¼cksetzen" klicken | Alle 6 Benutzer sichtbar |
| C3.8 | Benutzer deaktivieren | "Deaktivieren" bei einem User | Status wechselt zu "Inaktiv" |
| C3.9 | Passwort zurÃ¼cksetzen | "Passwort zurÃ¼cksetzen" â†’ neues PW eingeben â†’ OK | BestÃ¤tigung, User kann mit neuem PW anmelden |

### C.4 Rollenbasierte Zugangskontrolle (Admin-Bereich)

| ID | Prozess | Schritte | Erwartung |
|---|---|---|---|
| C4.1 | Admin-URL als normaler User | `admin@wamocon.de` â†’ `/admin` | Weiterleitung zu `/dashboard` |
| C4.2 | Superadmin auf App-URL | `superadmin@Vertragsmanager.de` â†’ `/dashboard` | Weiterleitung zu `/admin` |
| C4.3 | Nicht eingeloggt â†’ `/admin` | Ohne Login `/admin` aufrufen | Weiterleitung zu `/login` |

---

## D. Cross-Cutting Concerns

| ID | Thema | Test | Erwartung |
|---|---|---|---|
| D1 | Dark Mode | Alle Seiten im Dark Mode durchklicken | Kein WeiÃŸblitz, konsistente Farben |
| D2 | Responsive Mobile | Browser auf 375px Breite | Sidebar eingeklappt, Tabellen scrollbar |
| D3 | i18n DE | App auf Deutsch | Alle Labels, Buttons, Fehlermeldungen auf Deutsch |
| D4 | Legal Footer | Homepage â†’ Footer | Links zu Impressum, Datenschutz, AGB funktionieren |
| D5 | 404-Seite | `/nonexistent` aufrufen | 404-Seite angezeigt |
| D6 | Org-Isolation gesamt | Als WAMOCON-User: keine TechCorp-Daten sichtbar | Strikt getrennt in allen Modulen |




## ./legal-docs/agb.md
# Allgemeine Geschäftsbedingungen (AGB)

Stand: {{MONAT}} {{JAHR}}

## § 1 Geltungsbereich

(1) Diese Allgemeinen Geschäftsbedingungen (nachfolgend „AGB") der WAMOCON GmbH, Mergenthalerallee 79 - 81, 65760 Eschborn (nachfolgend „Anbieter"), gelten für alle Verträge über die Nutzung der Software-as-a-Service-Plattform {{PROJEKTNAME}} (nachfolgend „Plattform"), die über die Website {{DOMAIN}} bereitgestellt wird.

(2) Die Plattform richtet sich an Unternehmen und gewerbliche Nutzer (nachfolgend „Auftraggeber") sowie deren Benutzer (nachfolgend „Nutzer"). Es handelt sich um ein B2B-Angebot. Verbraucher im Sinne des § 13 BGB sind nicht Zielgruppe dieses Angebots.

(3) Abweichende, entgegenstehende oder ergänzende AGB des Auftraggebers werden nicht Vertragsbestandteil, es sei denn, der Anbieter stimmt deren Geltung ausdrücklich schriftlich zu.

## § 2 Vertragsschluss

(1) Die Darstellung der Plattform und ihrer Funktionen auf der Website stellt kein verbindliches Angebot im Sinne des § 145 BGB dar, sondern eine Aufforderung zur Abgabe eines Angebots (invitatio ad offerendum).

(2) Der Auftraggeber gibt ein verbindliches Angebot zum Abschluss eines Nutzungsvertrages ab, indem er den Registrierungsprozess auf der Plattform abschließt und diese AGB akzeptiert.

(3) Der Vertrag kommt zustande, wenn der Anbieter das Angebot des Auftraggebers durch Freischaltung des Zugangs annimmt.

## § 3 Leistungsbeschreibung

(1) Der Anbieter stellt dem Auftraggeber die Plattform als Software-as-a-Service (SaaS) über das Internet zur Verfügung.

(2) Der genaue Funktionsumfang ergibt sich aus der jeweils aktuellen Leistungsbeschreibung auf der Website.

(3) Der Anbieter ist berechtigt, die Plattform weiterzuentwickeln, zu erweitern und anzupassen. Wesentliche Einschränkungen des Funktionsumfangs werden dem Auftraggeber vorab mitgeteilt.

## § 4 Nutzungsrechte

(1) Der Anbieter räumt dem Auftraggeber für die Vertragslaufzeit ein einfaches, nicht übertragbares, nicht unterlizenzierbares Recht zur Nutzung der Plattform ein.

(2) Der Auftraggeber darf die Plattform nur für eigene betriebliche Zwecke nutzen.

## § 5 Pflichten des Auftraggebers

(1) Der Auftraggeber ist verpflichtet, seine Zugangsdaten geheim zu halten und vor dem Zugriff Dritter zu schützen.

(2) Der Auftraggeber stellt sicher, dass die Nutzung der Plattform im Einklang mit geltendem Recht erfolgt.

## § 6 Verfügbarkeit

(1) Der Anbieter bemüht sich um eine Verfügbarkeit der Plattform von 99,5 % im Jahresmittel.

(2) Nicht als Ausfallzeit gelten geplante Wartungsarbeiten, die vorab angekündigt werden.

## § 7 Datenschutz

Die Verarbeitung personenbezogener Daten erfolgt gemäß der Datenschutzerklärung des Anbieters und den Bestimmungen der DSGVO.

## § 8 Haftung

(1) Der Anbieter haftet unbeschränkt für Schäden aus der Verletzung des Lebens, des Körpers oder der Gesundheit sowie bei Vorsatz und grober Fahrlässigkeit.

(2) Im Übrigen ist die Haftung auf den vertragstypischen, vorhersehbaren Schaden begrenzt.

## § 9 Vertragslaufzeit und Kündigung

(1) Der Vertrag wird auf unbestimmte Zeit geschlossen und kann von beiden Parteien mit einer Frist von einem Monat zum Monatsende gekündigt werden.

(2) Das Recht zur außerordentlichen Kündigung aus wichtigem Grund bleibt unberührt.

## § 10 Schlussbestimmungen

(1) Es gilt das Recht der Bundesrepublik Deutschland unter Ausschluss des UN-Kaufrechts.

(2) Gerichtsstand ist Eschborn, sofern der Auftraggeber Kaufmann ist.

(3) Sollten einzelne Bestimmungen dieser AGB unwirksam sein, bleibt die Wirksamkeit der übrigen Bestimmungen davon unberührt.

---

> **Hinweis:** Dies ist ein Muster. Ersetze alle `{{PLATZHALTER}}` mit den projektspezifischen Informationen. Lass die AGB vor Veröffentlichung von einem Juristen prüfen.

## ./legal-docs/company-stamp.md
# Firmenstempel / Company Stamp

## Deutsch

### WAMOCON GmbH

**Geschäftsführer:** Dipl.-Ing. Waleri Moretz

**Anschrift:**
Mergenthalerallee 79 - 81
65760 Eschborn
Deutschland

**Kontakt:**
Telefon: +49 6196 5838311
E-Mail: info@wamocon.com

**Registrierung:**
Handelsregister: Eschborn HRB 123666
USt-IdNr.: DE344930486

---

## English

### WAMOCON GmbH

**Managing Director:** Dipl.-Ing. Waleri Moretz

**Address:**
Mergenthalerallee 79 - 81
65760 Eschborn
Germany

**Contact:**
Phone: +49 6196 5838311
Email: info@wamocon.com

**Registration:**
Commercial Register: Eschborn HRB 123666
VAT ID: DE344930486

## ./legal-docs/datenschutzerklaerung.md
# Datenschutzerklärung

Stand: {{MONAT}} {{JAHR}}

## 1. Verantwortlicher

Verantwortlicher im Sinne der Datenschutz-Grundverordnung (DSGVO) und anderer nationaler Datenschutzgesetze ist:

WAMOCON GmbH
Mergenthalerallee 79 - 81
65760 Eschborn
Telefon: +49 6196 5838311
E-Mail: info@wamocon.com
Projektkontakt: {{PROJEKT_EMAIL}}
Geschäftsführer: Dipl.-Ing. Waleri Moretz
Handelsregister: Eschborn HRB 123666
USt-ID: DE344930486

## 2. Überblick über die Datenverarbeitung

Diese Datenschutzerklärung gilt für die Website und Webanwendung {{PROJEKTNAME}} ({{DOMAIN}}).

Wir verarbeiten personenbezogene Daten unserer Nutzer grundsätzlich nur, soweit dies zur Bereitstellung einer funktionsfähigen Plattform sowie unserer Inhalte und Leistungen erforderlich ist.

## 3. Rechtsgrundlagen der Verarbeitung

- **Einwilligung** – Art. 6 Abs. 1 lit. a DSGVO
- **Vertragserfüllung** – Art. 6 Abs. 1 lit. b DSGVO
- **Rechtliche Verpflichtung** – Art. 6 Abs. 1 lit. c DSGVO
- **Berechtigtes Interesse** – Art. 6 Abs. 1 lit. f DSGVO

## 4. Hosting und Infrastruktur

### Vercel Inc.
Die Website und Webanwendung werden über Vercel gehostet. Dabei verarbeitet Vercel technisch notwendige Verbindungsdaten (IP-Adresse, Zeitstempel, Browserinformationen). Rechtsgrundlage: Art. 6 Abs. 1 lit. f DSGVO.

### Supabase Inc.
Für Datenbank, Authentifizierung, Dateispeicher und Teile der Backend-Infrastruktur nutzen wir Supabase. Verarbeitet werden Authentifizierungsdaten, Session-Informationen, Projektdaten sowie gespeicherte Medien. Rechtsgrundlage: Art. 6 Abs. 1 lit. b DSGVO.

## 5. Erhebung personenbezogener Daten

### Registrierung und Nutzerkonto
Bei der Registrierung erheben wir: Name, E-Mail-Adresse und Passwort. Diese Daten sind zur Vertragserfüllung erforderlich (Art. 6 Abs. 1 lit. b DSGVO).

### Server-Logfiles
Bei jedem Zugriff auf unsere Plattform werden automatisch folgende Daten erfasst: IP-Adresse, Datum und Uhrzeit, aufgerufene Seite, Referrer-URL, Browser-Typ und -Version. Rechtsgrundlage: Art. 6 Abs. 1 lit. f DSGVO.

## 6. Cookies und Tracking

Die Plattform verwendet technisch notwendige Cookies für Session-Management und Authentifizierung. Rechtsgrundlage: Art. 6 Abs. 1 lit. f DSGVO.

## 7. Rechte der betroffenen Personen

Sie haben das Recht auf:
- **Auskunft** (Art. 15 DSGVO)
- **Berichtigung** (Art. 16 DSGVO)
- **Löschung** (Art. 17 DSGVO)
- **Einschränkung der Verarbeitung** (Art. 18 DSGVO)
- **Datenübertragbarkeit** (Art. 20 DSGVO)
- **Widerspruch** (Art. 21 DSGVO)
- **Widerruf** erteilter Einwilligungen (Art. 7 Abs. 3 DSGVO)
- **Beschwerde** bei einer Aufsichtsbehörde (Art. 77 DSGVO)

## 8. Datensicherheit

Wir setzen technische und organisatorische Sicherheitsmaßnahmen nach dem Stand der Technik ein, um Ihre Daten gegen zufällige oder vorsätzliche Manipulation, Verlust, Zerstörung oder den Zugriff unberechtigter Personen zu schützen.

## 9. Änderung der Datenschutzerklärung

Wir behalten uns vor, diese Datenschutzerklärung anzupassen, um sie an geänderte Rechtslagen oder bei Änderungen der Plattform anzupassen.

---

> **Hinweis:** Dies ist ein Muster. Ersetze alle `{{PLATZHALTER}}` mit den projektspezifischen Informationen. Lass die Datenschutzerklärung vor Veröffentlichung von einem Juristen prüfen.

## ./legal-docs/impressum.md
# Impressum

Stand: {{MONAT}} {{JAHR}}

## WAMOCON GmbH

Mergenthalerallee 79 - 81
65760 Eschborn
Deutschland

## Kontakt

Telefon: +49 6196 5838311
E-Mail: info@wamocon.com
Projektkontakt: {{PROJEKT_EMAIL}}

## Vertretungsberechtigter Geschäftsführer

Dipl.-Ing. Waleri Moretz

## Registereintrag

Sitz der Gesellschaft: Eschborn
Handelsregister: Eschborn HRB 123666
Umsatzsteuer-Identifikationsnummer: DE344930486

## Angaben zum Angebot

{{PROJEKTNAME}} ist eine webbasierte Software-as-a-Service-Plattform für {{BESCHREIBUNG_DES_ANGEBOTS}}. Das Angebot richtet sich primär an Unternehmen und gewerbliche Nutzer.

---

> **Hinweis:** Ersetze alle `{{PLATZHALTER}}` mit den projektspezifischen Informationen.

## ./legal-docs/imprint.md
# Imprint (Legal Notice)

As of: {{MONTH}} {{YEAR}}

## WAMOCON GmbH

Mergenthalerallee 79 - 81
65760 Eschborn
Germany

## Contact

Phone: +49 6196 5838311
Email: info@wamocon.com
Project Contact: {{PROJECT_EMAIL}}

## Authorized Managing Director

Dipl.-Ing. Waleri Moretz

## Registration

Registered Office: Eschborn
Commercial Register: Eschborn HRB 123666
VAT Identification Number: DE344930486

## About the Service

{{PROJECT_NAME}} is a web-based Software-as-a-Service platform for {{SERVICE_DESCRIPTION}}. The service is primarily aimed at businesses and commercial users.

---

> **Note:** Replace all `{{PLACEHOLDERS}}` with your project-specific information.

## ./legal-docs/privacy-policy.md
# Privacy Policy

As of: {{MONTH}} {{YEAR}}

## 1. Data Controller

The Data Controller as defined by the General Data Protection Regulation (GDPR) is:

WAMOCON GmbH
Mergenthalerallee 79 - 81
65760 Eschborn, Germany
Phone: +49 6196 5838311
Email: info@wamocon.com
Project Contact: {{PROJECT_EMAIL}}
Managing Director: Dipl.-Ing. Waleri Moretz
Commercial Register: Eschborn HRB 123666
VAT ID: DE344930486

## 2. Overview of Data Processing

This Privacy Policy applies to the website and web application {{PROJECT_NAME}} ({{DOMAIN}}).

We process personal data of our users only insofar as this is necessary to provide a functional platform and our content and services.

## 3. Legal Basis for Processing

- **Consent** – Art. 6(1)(a) GDPR
- **Performance of Contract** – Art. 6(1)(b) GDPR
- **Legal Obligation** – Art. 6(1)(c) GDPR
- **Legitimate Interest** – Art. 6(1)(f) GDPR

## 4. Hosting and Infrastructure

### Vercel Inc.
The website and web application are hosted via Vercel. Vercel processes technically necessary connection data (IP address, timestamp, browser information). Legal basis: Art. 6(1)(f) GDPR.

### Supabase Inc.
We use Supabase for database, authentication, file storage, and parts of the backend infrastructure. Authentication data, session information, project data, and stored media are processed. Legal basis: Art. 6(1)(b) GDPR.

## 5. Collection of Personal Data

### Registration and User Account
During registration we collect: name, email address, and password. This data is necessary for the performance of the contract (Art. 6(1)(b) GDPR).

### Server Log Files
Each access to our platform automatically records: IP address, date and time, page accessed, referrer URL, browser type and version. Legal basis: Art. 6(1)(f) GDPR.

## 6. Cookies and Tracking

The platform uses technically necessary cookies for session management and authentication. Legal basis: Art. 6(1)(f) GDPR.

## 7. Rights of Data Subjects

You have the right to:
- **Access** (Art. 15 GDPR)
- **Rectification** (Art. 16 GDPR)
- **Erasure** (Art. 17 GDPR)
- **Restriction of processing** (Art. 18 GDPR)
- **Data portability** (Art. 20 GDPR)
- **Object** (Art. 21 GDPR)
- **Withdraw consent** (Art. 7(3) GDPR)
- **Lodge a complaint** with a supervisory authority (Art. 77 GDPR)

## 8. Data Security

We employ state-of-the-art technical and organisational security measures to protect your data against accidental or intentional manipulation, loss, destruction, or access by unauthorised persons.

## 9. Changes to this Privacy Policy

We reserve the right to amend this Privacy Policy to reflect changes in the law or changes to the platform.

---

> **Note:** This is a template. Replace all `{{PLACEHOLDERS}}` with your project-specific information. Have the Privacy Policy reviewed by a lawyer before publication.

## ./legal-docs/terms-and-conditions.md
# Terms and Conditions

As of: {{MONTH}} {{YEAR}}

## § 1 Scope

(1) These Terms and Conditions (hereinafter "T&C") of WAMOCON GmbH, Mergenthalerallee 79 - 81, 65760 Eschborn (hereinafter "Provider"), apply to all contracts for the use of the Software-as-a-Service platform {{PROJECT_NAME}} (hereinafter "Platform"), provided via the website {{DOMAIN}}.

(2) The Platform is aimed at businesses and commercial users (hereinafter "Client") and their respective users (hereinafter "Users"). This is a B2B offering. Consumers within the meaning of § 13 BGB (German Civil Code) are not the target audience.

(3) Deviating, conflicting, or supplementary terms and conditions of the Client shall not become part of the contract unless the Provider expressly agrees in writing.

## § 2 Conclusion of Contract

(1) The presentation of the Platform and its features on the website does not constitute a binding offer within the meaning of § 145 BGB, but rather an invitation to submit an offer (invitatio ad offerendum).

(2) The Client submits a binding offer by completing the registration process on the Platform and accepting these T&C.

(3) The contract is concluded when the Provider accepts the Client's offer by activating access.

## § 3 Service Description

(1) The Provider makes the Platform available to the Client as Software-as-a-Service (SaaS) via the Internet.

(2) The precise scope of features is set out in the current service description on the website.

(3) The Provider is entitled to further develop, expand, and adapt the Platform. Material restrictions to the scope of features will be communicated to the Client in advance.

## § 4 Usage Rights

(1) The Provider grants the Client a simple, non-transferable, non-sublicensable right to use the Platform for the duration of the contract.

(2) The Client may only use the Platform for its own business purposes.

## § 5 Client Obligations

(1) The Client shall keep its access credentials confidential and protect them from third-party access.

(2) The Client shall ensure that use of the Platform complies with applicable law.

## § 6 Availability

(1) The Provider shall endeavour to maintain an annual average availability of 99.5%.

(2) Scheduled maintenance windows announced in advance shall not count as downtime.

## § 7 Data Protection

The processing of personal data is governed by the Provider's Privacy Policy and the provisions of the GDPR.

## § 8 Liability

(1) The Provider shall be fully liable for damages arising from injury to life, body, or health, as well as for intent and gross negligence.

(2) Otherwise, liability is limited to foreseeable, typically occurring damages.

## § 9 Contract Duration and Termination

(1) The contract is concluded for an indefinite period and may be terminated by either party with one month's notice to the end of a calendar month.

(2) The right to extraordinary termination for good cause remains unaffected.

## § 10 Final Provisions

(1) The law of the Federal Republic of Germany shall apply, excluding the UN Convention on Contracts for the International Sale of Goods.

(2) The place of jurisdiction is Eschborn, provided the Client is a merchant.

(3) Should individual provisions of these T&C be invalid, the validity of the remaining provisions shall remain unaffected.

---

> **Note:** This is a template. Replace all `{{PLACEHOLDERS}}` with your project-specific information. Have the T&C reviewed by a lawyer before publication.

## package.json
{
  "name": "vertragsmanager",
  "version": "0.1.0",
  "private": true,
  "scripts": {
    "dev": "next dev --turbopack",
    "build": "next build",
    "start": "next start",
    "lint": "eslint .",
    "lint:fix": "eslint . --fix",
    "typecheck": "tsc --noEmit",
    "verify": "npm run typecheck && npm run lint && npm run build",
    "gen:anforderungsdokument": "node scripts/generate-anforderungsdokument.mjs"
  },
  "dependencies": {
    "@supabase/ssr": "^0.10.2",
    "@supabase/supabase-js": "^2.103.2",
    "clsx": "^2.1.1",
    "date-fns": "^4.1.0",
    "docx": "^9.6.1",
    "lucide-react": "^1.8.0",
    "next": "16.2.1",
    "next-intl": "^4.9.1",
    "react": "19.2.4",
    "react-dom": "19.2.4",
    "recharts": "^3.8.1",
    "tailwind-merge": "^3.5.0"
  },
  "devDependencies": {
    "@tailwindcss/postcss": "^4",
    "@types/node": "^20",
    "@types/react": "^19",
    "@types/react-dom": "^19",
    "eslint": "^9",
    "eslint-config-next": "16.2.1",
    "tailwindcss": "^4",
    "typescript": "^5"
  }
}

