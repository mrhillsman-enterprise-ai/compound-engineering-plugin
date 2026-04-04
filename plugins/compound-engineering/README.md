# Compounding Engineering Plugin

AI-powered development tools that get smarter with every use. Make each unit of engineering work easier than the last.

## Components

| Component | Count |
|-----------|-------|
| Agents | 50+ |
| Skills | 40+ |

## Skills

### Core Workflow

The primary entry points for engineering work, invoked as slash commands:

| Skill | Description |
|-------|-------------|
| `/ce-ideate` | Discover high-impact project improvements through divergent ideation and adversarial filtering |
| `/ce-brainstorm` | Explore requirements and approaches before planning |
| `/ce-plan` | Create structured plans for any multi-step task -- software features, research workflows, events, study plans -- with automatic confidence checking |
| `/ce-code-review` | Structured code review with tiered persona agents, confidence gating, and dedup pipeline |
| `/ce-work` | Execute work items systematically |
| `/ce-compound` | Document solved problems to compound team knowledge |
| `/ce-compound-refresh` | Refresh stale or drifting learnings and decide whether to keep, update, replace, or archive them |

### Git Workflow

| Skill | Description |
|-------|-------------|
| `ce-clean-gone-branches` | Clean up local branches whose remote tracking branch is gone |
| `ce-commit` | Create a git commit with a value-communicating message |
| `ce-commit-push-pr` | Commit, push, and open a PR with an adaptive description; also update an existing PR description |
| `ce-worktree` | Manage Git worktrees for parallel development |

### Workflow Utilities

| Skill | Description |
|-------|-------------|
| `/ce-changelog` | Create engaging changelogs for recent merges |
| `/ce-feature-video` | Record video walkthroughs and add to PR description |
| `/ce-reproduce-bug` | Reproduce bugs using logs and console |
| `/ce-report-bug` | Report a bug in the compound-engineering plugin |
| `/ce-resolve-pr-feedback` | Resolve PR review feedback in parallel |
| `/ce-test-browser` | Run browser tests on PR-affected pages |
| `/ce-test-xcode` | Build and test iOS apps on simulator using XcodeBuildMCP |
| `/ce-onboarding` | Generate `ONBOARDING.md` to help new contributors understand the codebase |
| `/ce-todo-resolve` | Resolve todos in parallel |
| `/ce-todo-triage` | Triage and prioritize pending todos |

### Development Frameworks

| Skill | Description |
|-------|-------------|
| `ce-agent-native-architecture` | Build AI agents using prompt-native architecture |
| `ce-andrew-kane-gem-writer` | Write Ruby gems following Andrew Kane's patterns |
| `ce-dhh-rails-style` | Write Ruby/Rails code in DHH's 37signals style |
| `ce-dspy-ruby` | Build type-safe LLM applications with DSPy.rb |
| `ce-frontend-design` | Create production-grade frontend interfaces |

### Review & Quality

| Skill | Description |
|-------|-------------|
| `ce-claude-permissions-optimizer` | Optimize Claude Code permissions from session history |
| `ce-doc-review` | Review documents using parallel persona agents for role-specific feedback |
| `ce-setup` | Reserved for future project-level workflow configuration; code review agent selection is automatic |

### Content & Collaboration

| Skill | Description |
|-------|-------------|
| `ce-every-style-editor` | Review copy for Every's style guide compliance |
| `ce-proof` | Create, edit, and share documents via Proof collaborative editor |
| `ce-todo-create` | File-based todo tracking system |

### Automation & Tools

| Skill | Description |
|-------|-------------|
| `agent-browser` | CLI-based browser automation using Vercel's agent-browser |
| `ce-gemini-imagegen` | Generate and edit images using Google's Gemini API |
| `ce-orchestrating-swarms` | Comprehensive guide to multi-agent swarm orchestration |
| `rclone` | Upload files to S3, Cloudflare R2, Backblaze B2, and cloud storage |

### Beta / Experimental

| Skill | Description |
|-------|-------------|
| `/lfg` | Full autonomous engineering workflow |
| `/slfg` | Full autonomous workflow with swarm mode for parallel execution |

## Agents

Agents are specialized subagents invoked by skills — you typically don't call these directly.

### Review

| Agent | Description |
|-------|-------------|
| `ce-agent-native-reviewer` | Verify features are agent-native (action + context parity) |
| `ce-api-contract-reviewer` | Detect breaking API contract changes |
| `ce-cli-agent-readiness-reviewer` | Evaluate CLI agent-friendliness against 7 core principles |
| `ce-cli-readiness-reviewer` | CLI agent-readiness persona for ce-code-review (conditional, structured JSON) |
| `ce-architecture-strategist` | Analyze architectural decisions and compliance |
| `ce-code-simplicity-reviewer` | Final pass for simplicity and minimalism |
| `ce-correctness-reviewer` | Logic errors, edge cases, state bugs |
| `ce-data-integrity-guardian` | Database migrations and data integrity |
| `ce-data-migration-expert` | Validate ID mappings match production, check for swapped values |
| `ce-data-migrations-reviewer` | Migration safety with confidence calibration |
| `ce-deployment-verification-agent` | Create Go/No-Go deployment checklists for risky data changes |
| `ce-dhh-rails-reviewer` | Rails review from DHH's perspective |
| `ce-julik-frontend-races-reviewer` | Review JavaScript/Stimulus code for race conditions |
| `ce-kieran-rails-reviewer` | Rails code review with strict conventions |
| `ce-kieran-python-reviewer` | Python code review with strict conventions |
| `ce-kieran-typescript-reviewer` | TypeScript code review with strict conventions |
| `ce-maintainability-reviewer` | Coupling, complexity, naming, dead code |
| `ce-pattern-recognition-specialist` | Analyze code for patterns and anti-patterns |
| `ce-performance-oracle` | Performance analysis and optimization |
| `ce-performance-reviewer` | Runtime performance with confidence calibration |
| `ce-reliability-reviewer` | Production reliability and failure modes |
| `ce-schema-drift-detector` | Detect unrelated schema.rb changes in PRs |
| `ce-security-reviewer` | Exploitable vulnerabilities with confidence calibration |
| `ce-security-sentinel` | Security audits and vulnerability assessments |
| `ce-testing-reviewer` | Test coverage gaps, weak assertions |
| `ce-project-standards-reviewer` | CLAUDE.md and AGENTS.md compliance |
| `ce-adversarial-reviewer` | Construct failure scenarios to break implementations across component boundaries |

### Document Review

| Agent | Description |
|-------|-------------|
| `ce-coherence-reviewer` | Review documents for internal consistency, contradictions, and terminology drift |
| `ce-design-lens-reviewer` | Review plans for missing design decisions, interaction states, and AI slop risk |
| `ce-feasibility-reviewer` | Evaluate whether proposed technical approaches will survive contact with reality |
| `ce-product-lens-reviewer` | Challenge problem framing, evaluate scope decisions, surface goal misalignment |
| `ce-scope-guardian-reviewer` | Challenge unjustified complexity, scope creep, and premature abstractions |
| `ce-security-lens-reviewer` | Evaluate plans for security gaps at the plan level (auth, data, APIs) |
| `ce-adversarial-document-reviewer` | Challenge premises, surface unstated assumptions, and stress-test decisions |

### Research

| Agent | Description |
|-------|-------------|
| `ce-best-practices-researcher` | Gather external best practices and examples |
| `ce-framework-docs-researcher` | Research framework documentation and best practices |
| `ce-git-history-analyzer` | Analyze git history and code evolution |
| `ce-issue-intelligence-analyst` | Analyze GitHub issues to surface recurring themes and pain patterns |
| `ce-learnings-researcher` | Search institutional learnings for relevant past solutions |
| `ce-repo-research-analyst` | Research repository structure and conventions |
| `ce-slack-researcher` | Search Slack for organizational context relevant to the current task |

### Design

| Agent | Description |
|-------|-------------|
| `ce-design-implementation-reviewer` | Verify UI implementations match Figma designs |
| `ce-design-iterator` | Iteratively refine UI through systematic design iterations |
| `ce-figma-design-sync` | Synchronize web implementations with Figma designs |

### Workflow

| Agent | Description |
|-------|-------------|
| `ce-bug-reproduction-validator` | Systematically reproduce and validate bug reports |
| `ce-lint` | Run linting and code quality checks on Ruby and ERB files |
| `ce-pr-comment-resolver` | Address PR comments and implement fixes |
| `ce-spec-flow-analyzer` | Analyze user flows and identify gaps in specifications |

### Docs

| Agent | Description |
|-------|-------------|
| `ce-ankane-readme-writer` | Create READMEs following Ankane-style template for Ruby gems |

## Browser Automation

This plugin uses **agent-browser CLI** for browser automation tasks. Install it globally:

```bash
npm install -g agent-browser
agent-browser install  # Downloads Chromium
```

The `agent-browser` skill provides comprehensive documentation on usage.

## Installation

```bash
claude /plugin install compound-engineering
```

## Version History

See the repo root [CHANGELOG.md](../../CHANGELOG.md) for canonical release history.

## License

MIT
