---
name: onboarding
description: "Generate or regenerate ONBOARDING.md to help new contributors understand a codebase. Use when the user asks to 'create onboarding docs', 'generate ONBOARDING.md', 'document this project for new developers', 'write onboarding documentation', 'vonboard', 'vonboarding', 'prepare this repo for a new contributor', 'refresh the onboarding doc', or 'update ONBOARDING.md'. Also use when someone needs to onboard a new team member and wants a written artifact, or when a codebase lacks onboarding documentation and the user wants to generate one."
argument-hint: "[optional: focus area or scope hint]"
---

# Generate Onboarding Document

Crawl a repository and generate `ONBOARDING.md` at the repo root -- a document that helps new contributors understand the codebase without requiring the creator to explain it.

Onboarding is a general problem in software, but it is more acute in fast-moving codebases where code is written faster than documentation -- whether through AI-assisted development, rapid prototyping, or simply a team that ships faster than it documents. This skill reconstructs the mental model from the code itself.

This skill always regenerates the document from scratch. It does not read or diff a previous version. If `ONBOARDING.md` already exists, it is overwritten.

## Core Principles

1. **Write for humans first** -- Clear prose that a new developer can read and understand. Agent utility is a side effect of good human writing, not a separate goal.
2. **Show, don't just tell** -- Use ASCII diagrams for architecture and flow, markdown tables for structured information, and backtick formatting for all file paths, commands, and code references.
3. **Five sections, each earning its place** -- Every section answers a question a new contributor will ask in their first hour. No speculative sections.
4. **State what you can observe, not what you must infer** -- Do not fabricate design rationale or assess fragility. If the code doesn't reveal why a decision was made, don't guess.
5. **Never include secrets** -- The onboarding document is committed to the repository. Never include API keys, tokens, passwords, connection strings with credentials, or any other secret values. Reference environment variable *names* (`STRIPE_SECRET_KEY`), never their *values*. If a `.env` file contains actual secrets, extract only the variable names.
6. **Link, don't duplicate** -- When existing documentation covers a topic well, link to it inline rather than re-explaining.

## Focus Hint

<focus_hint> #$ARGUMENTS </focus_hint>

If a focus hint is provided, use it to prioritize which parts of the codebase to examine more deeply. The document still covers all five sections, but depth can shift toward the focus area.

## Execution Flow

### Phase 1: Gather Inventory

Run the bundled inventory script to get a structural map of the repository without reading every file:

```bash
node "${CLAUDE_PLUGIN_ROOT}/skills/onboarding/scripts/inventory.mjs" --root .
```

Parse the JSON output. This provides:
- Project name, languages, frameworks, package manager, test framework
- Directory structure (top-level + one level into source directories)
- Entry points per detected ecosystem
- Available scripts/commands
- Existing documentation files
- Test infrastructure
- Infrastructure and external dependencies (env files, docker services, detected integrations)
- Monorepo structure (if applicable)

If the script fails or returns an error field, report the issue to the user and stop. Do not attempt to write `ONBOARDING.md` from incomplete data.

### Phase 2: Read Key Files

Guided by the inventory, read files that are essential for understanding the codebase. Use the native file-read tool (not shell commands).

**Priority order -- read up to ~15 key files total:**

1. **README.md** (if exists) -- for project purpose and setup instructions
2. **Primary entry points** -- the files listed in `entryPoints` from the inventory. These reveal what the application does when it starts.
3. **Route/controller files** -- look for `routes/`, `app/controllers/`, `src/routes/`, `src/api/`, or similar directories from the inventory structure. Read the main route file to understand the primary flow.
4. **Configuration files that reveal architecture and external dependencies** -- `docker-compose.yml`, `.env.example`, `.env.sample`, database config, `next.config.*`, `vite.config.*`, or similar. These reveal what the system depends on externally. Only read these if they exist in the inventory. **Never read `.env` itself** -- only read `.env.example` or `.env.sample` templates. Extract variable names only, never values.
5. **AGENTS.md or CLAUDE.md** (if exists) -- for project conventions and patterns already documented.
6. **A sample of discovered documentation** -- scan titles and first paragraphs of files from the inventory's `docs` list. Read fully only those that appear directly relevant to the five sections.

Do not read files speculatively. Every file read should be justified by the inventory output.

### Phase 3: Write ONBOARDING.md

Synthesize the inventory data and key file contents into a document with exactly five sections. Write the file to the repo root.

**Writing style -- the document should read like a knowledgeable teammate explaining the project over coffee, not like generated documentation.**

Voice and tone:
- Write in second person ("you") -- speak directly to the new contributor
- Use active voice and present tense: "The router dispatches requests to handlers" not "Requests are dispatched by the router to handlers"
- Be direct. Lead sentences with what matters, not with setup: "Run `bun dev` to start the server" not "In order to start the development server, you will need to run the following command"
- Match the formality of the codebase. A scrappy prototype gets casual prose. An enterprise system gets more precise language. Read the README and existing docs for tone cues.

Clarity:
- Every sentence should teach the reader something or tell them what to do. Cut any sentence that doesn't.
- Prefer concrete over abstract: "`src/services/billing.ts` charges the customer's card" not "The billing module handles payment-related business logic"
- When introducing a term, define it immediately in context. Don't make the reader scroll to a glossary.
- Use the simplest word that's accurate. "Use" not "utilize." "Start" not "initialize." "Send" not "transmit."

What to avoid:
- Filler and throat-clearing: "It's important to note that", "As mentioned above", "In this section we will"
- Vague summarization: "This module handles various aspects of..." -- say specifically what it does
- Hedge words when stating facts: "This essentially serves as", "This is basically" -- if you know what it does, say it plainly
- Superlatives and marketing language: "robust", "powerful", "comprehensive", "seamless"
- Meta-commentary about the document itself: "This document aims to..." -- just do the thing

**Formatting requirements -- apply consistently throughout:**
- Use backticks for all file names (`package.json`), paths (`src/routes/`), commands (`bun test`), function/class names, environment variables, and technical terms
- Use markdown headers (`##`) for the five sections
- Use ASCII diagrams and markdown tables where specified below
- Use bold for emphasis sparingly
- Keep paragraphs short -- 2-4 sentences

#### Section 1: What Is This?

Answer: What does this project do, who is it for, and what problem does it solve?

Draw from `README.md`, manifest descriptions (e.g., `package.json` description field), and what the entry points reveal about the application's purpose.

If the project's purpose cannot be clearly determined from the code, state that plainly: "This project's purpose is not documented. Based on the code structure, it appears to be..."

Keep to 1-3 paragraphs.

#### Section 2: How Is It Organized?

Answer: What is the architecture, what are the key modules, how do they connect, and what does the system depend on externally?

This section covers both the **internal structure** and the **system boundary** -- what the application talks to outside itself.

**Internal structure** -- Include an ASCII directory tree showing the high-level layout:

```
project-name/
  src/
    routes/       # HTTP route handlers
    services/     # Business logic
    models/       # Data layer
  tests/          # Test suite
  config/         # Environment and app configuration
```

Annotate directories with a brief comment explaining their role. Only include directories that matter -- skip build artifacts, config files, and boilerplate.

When there are distinct modules or components with clear responsibilities, present them in a table:

```
| Module | Responsibility |
|--------|---------------|
| `src/routes/` | HTTP request handling and routing |
| `src/services/` | Core business logic |
| `src/models/` | Database models and queries |
```

Describe how the modules connect -- what calls what, where data flows between them.

**External dependencies and integrations** -- Surface everything the system talks to outside its own codebase. This is often the biggest blocker for new contributors trying to run the project. Look for signals in:
- `docker-compose.yml` (databases, caches, message queues)
- Environment variable references in config files or `.env.example`
- Import statements for client libraries (database drivers, API SDKs, cloud storage)
- The inventory's detected frameworks (e.g., Prisma implies a database)

Present as a table when there are multiple dependencies:

```
| Dependency | What it's used for | Configured via |
|-----------|-------------------|---------------|
| PostgreSQL | Primary data store | `DATABASE_URL` |
| Redis | Session cache and job queue | `REDIS_URL` |
| Stripe API | Payment processing | `STRIPE_SECRET_KEY` |
| S3 | File uploads | `AWS_*` env vars |
```

If no external dependencies are detected, state that: "This project appears self-contained with no external service dependencies."

#### Section 3: Key Concepts and Abstractions

Answer: What vocabulary and patterns does someone need to understand to talk about this codebase?

This section covers two things:

**Domain terms** -- The project-specific vocabulary: entity names, API resource names, database tables, configuration concepts, and jargon that a new reader would not immediately recognize.

**Architectural abstractions** -- The structural patterns in the codebase that shape how code is organized and how a contributor should think about making changes. These are especially important in codebases where the original author may not have consciously chosen these patterns -- they may have been introduced by an AI or adopted from a template without documentation.

Examples of architectural abstractions worth surfacing:
- "Business logic lives in the service layer (`src/services/`), not in route handlers"
- "Authentication runs through middleware in `src/middleware/auth.ts` before every protected route"
- "Database access uses the repository pattern -- each model has a corresponding repository class"
- "Background jobs are defined in `src/jobs/` and dispatched through a Redis-backed queue"

Present both domain terms and abstractions in a single table:

```
| Concept | What it means in this codebase |
|---------|-------------------------------|
| `Widget` | The primary entity users create and manage |
| `Pipeline` | A sequence of processing steps applied to incoming data |
| Service layer | Business logic lives in `src/services/`, not in route handlers |
| Middleware chain | Request processing flows through `src/middleware/` before reaching routes |
```

Aim for 5-15 entries. Include only concepts that would confuse a new reader or that represent non-obvious architectural decisions. Skip universally understood terms.

#### Section 4: Primary Flow

Answer: What happens when the main thing this app does actually happens?

Trace one concrete path through the system from the user's perspective. Start with the trigger ("When a user creates a new widget...") and walk through the code path.

Include an ASCII flow diagram:

```
User Request
  |
  v
src/routes/widgets.ts    -- validates input, extracts params
  |
  v
src/services/widget.ts   -- applies business rules, calls DB
  |
  v
src/models/widget.ts     -- persists to PostgreSQL
  |
  v
Response (201 Created)
```

At each step, reference the specific file path. Explain what happens at that step in one sentence.

Choose the flow that best demonstrates how the system's pieces connect. For a web app, this is usually the primary CRUD operation. For a CLI, the main command. For a library, the primary public API path.

#### Section 5: Where Do I Start?

Answer: How do I set up the project, run it, and make common changes?

Cover three things:

1. **Setup** -- Prerequisites, install steps, environment config. Draw from README and the inventory's scripts. Format commands in code blocks:
   ```
   bun install
   cp .env.example .env
   bun dev
   ```

2. **Running and testing** -- How to start the dev server, run tests, lint. Use the inventory's detected scripts.

3. **Common change patterns** -- Where to go for the 2-3 most common types of changes. For example:
   - "To add a new API endpoint, create a route handler in `src/routes/` and register it in `src/routes/index.ts`"
   - "To add a new database model, create a file in `src/models/` and run `bun migrate`"

#### Inline Documentation Links

While writing each section, check whether any file from the inventory's `docs` list is directly relevant to what the section explains. If so, link inline:

> Authentication uses token-based middleware -- see [`docs/solutions/auth-pattern.md`](docs/solutions/auth-pattern.md) for the full pattern.

Do not create a separate references or further-reading section. If no relevant docs exist for a section, the section stands alone -- do not mention their absence.

### Phase 4: Quality Check

Before writing the file, verify:

- [ ] Every section answers its question without padding or filler
- [ ] No secrets, API keys, tokens, passwords, or credential values anywhere in the document
- [ ] No fabricated design rationale ("we chose X because...")
- [ ] No fragility or risk assessments
- [ ] File paths referenced in the document correspond to real files from the inventory
- [ ] All file names, paths, commands, code references, and technical terms use backtick formatting
- [ ] ASCII diagrams are present in the architecture and primary flow sections
- [ ] External dependencies and integrations are surfaced in the architecture section (or explicitly noted as absent)
- [ ] Tables are used for module responsibilities, domain terms/abstractions, and external dependencies
- [ ] Markdown styling is consistent throughout (headers, bold, code blocks, tables)
- [ ] Existing docs are linked inline only where directly relevant
- [ ] Writing is direct and concrete -- no filler, no hedge words, no meta-commentary about the document
- [ ] Tone matches the codebase (casual for scrappy projects, precise for enterprise)

Write the file to the repo root as `ONBOARDING.md`.

### Phase 5: Present Result

After writing, inform the user that `ONBOARDING.md` has been generated. Offer next steps using the platform's blocking question tool when available (`AskUserQuestion` in Claude Code, `request_user_input` in Codex, `ask_user` in Gemini). Otherwise, present numbered options in chat.

Options:
1. Open the file for review
2. Commit the file
3. Done
