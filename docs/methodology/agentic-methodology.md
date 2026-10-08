# Zorivest Agentic AI Development Methodology

> **This is the narrative companion — the *why*.** For the operational *how* (phase-by-phase steps, file references, human-intervention map, infrastructure), see the source-of-truth document `development-lifecycle.md`. This document explains the philosophy, economics, and design rationale; it deliberately does **not** restate the step mechanics, which the lifecycle doc owns. When the two disagree on mechanics, the lifecycle doc wins.
>
> **Audience:** Human developers, software architects, business executives, and AI agents  
> **Version:** 1.2 — June 2026  
> **Repository:** github.com/zorivest/zorivest  
> **Source of truth:** `project configuration/` — AGENTS.md, workflows, roles, skills, and context files

---

## Table of Contents

1. [Executive Summary](#1-executive-summary)
2. [Feature Research & Deep Prompt Engineering](#2-feature-research--deep-prompt-engineering)
3. [Build Planning & Decision-Tracking Indexes](#3-build-planning--decision-tracking-indexes)
4. [TDD & Anti-Practices for Agentic AI](#4-tdd--anti-practices-for-agentic-ai)
5. [The Dual-Agent Orchestration Model](#5-the-dual-agent-orchestration-model)
6. [Reflection-Driven Continuous Improvement](#6-reflection-driven-continuous-improvement)
7. [Issue Management & Triage](#7-issue-management--triage)
8. [Git Commit Discipline](#8-git-commit-discipline)
9. [Why Full Automation Fails](#9-why-full-automation-between-agents-fails)
10. [Context Degradation Vigilance](#10-context-degradation-vigilance)
11. [Designing for LLM Evolution](#11-designing-for-llm-evolution)
12. [Monetization-Aware Workflow Design](#12-monetization-aware-workflow-design)

---

## 1. Executive Summary

Zorivest is a trading portfolio analysis platform built using a **human-supervised, dual-agent orchestration model**. Two roles — the **primary driver** (`coordinator` / `builder` per tier — see `project configuration/docs/model-routing.md`) and the **external reviewer** (`independent_reviewer` primary; full chain in `project configuration/docs/model-routing.md`) — perform complementary roles under structured workflows, with a human orchestrator maintaining oversight at every critical gate.

The methodology has delivered **200+ Manageable Execution Units (MEUs)** across 11 phases — from domain entities through REST API, MCP server, Electron GUI, pipeline engine, and market data expansion — with a disciplined TDD-first approach that prevents the common failure modes of AI-generated code.

**Key principle:** AI agents are powerful but not autonomous. They require structured instructions, adversarial review, human decision gates, and continuous calibration through reflections to produce production-quality software.

---

## 2. Feature Research & Deep Prompt Engineering

### Philosophy

Before any code is written, features go through a structured research phase. The goal is to transform engineering problems into **agentic AI problems** — gathering reference implementations and teaching AI agents *patterns* rather than hardcoding every variation.

### Multi-Provider Deep Research

Each major feature or architectural decision begins with **custom-tailored deep research prompts** sent to multiple LLM providers. The prompts are crafted differently for each model's strengths:

| Provider | Prompt Style | Strength Leveraged |
|----------|-------------|-------------------|
| **Gemini** (Google) | Broad synthesis prompts with explicit output structure | Large context window, web-grounded reasoning, multi-source synthesis |
| **ChatGPT** (OpenAI) | Analytical prompts with reasoning-effort control | Deep reasoning (`xhigh` effort), structured report output, tool-use research mode |
| **Claude** (Anthropic) | Detailed constraint-heavy prompts with adaptive thinking | Extended thinking (up to 128K tokens), 6-step decomposition protocol, web search integration |

### Research Workflow (`/pre-build-research`)

The formal research pipeline follows six steps:

1. **Define Feature Scope** — 2-3 sentence description covering data consumed/produced, variations, and edge cases
2. **Identify Sources** — Official docs first, then GitHub reference implementations, then community patterns
3. **Extract Patterns** — Schema mappings, algorithm pseudocode, API interaction patterns from real repos
4. **Create AI Instruction Set** — Transform extracted patterns into reusable prompt templates with examples, schemas, edge cases, and validation rules
5. **Build Decision** — Classify as AI-driven (5+ references), Hybrid (1-4 references), Code-first (no prior art), or Code-first-with-AI-review (security-critical)

### Composite Synthesis

Results from all three providers are synthesized into a **composite research brief** that captures:

- Consensus patterns (where all providers agree)
- Unique insights (where one provider found something others missed)
- Contradictions (flagged for human resolution)
- Source-backed decisions with cited URLs and document paths

This composite synthesis becomes the authoritative input for the build plan, ensuring no single model's biases dominate the architecture.

---

## 3. Build Planning & Decision-Tracking Indexes

### The Index System

Every architectural decision, feature specification, and design choice is tracked in a hierarchy of canonical documents:

```
docs/
├── BUILD_PLAN.md                    # Hub/index — links to all phase files
├── build-plan/
│   ├── build-priority-matrix.md     # milestone ordering and dependencies
│   ├── 01-domain-layer.md           # Phase 1 spec
│   ├── 02-infrastructure.md         # Phase 2 spec
│   ├── ...                          # One file per phase
│   └── 09h-pipeline-markdown.md     # Sub-phase specs
├── execution/
│   ├── plans/                       # Per-project plan + task files
│   ├── reflections/                 # Post-execution reflection files
│   └── metrics.md                   # Session performance metrics
```

### Why Indexes Matter for AI Alignment

AI agents have **no persistent memory** across sessions. Without indexes:

- Agents re-invent decisions already made, creating contradictions
- Specifications drift as each session interprets requirements differently  
- Acceptance criteria lack traceability to original design intent

The index system solves this by providing a **single source of truth** that every agent session reads before taking action. Each decision is tagged with its source basis:

| Source Type | Meaning |
|-------------|---------|
| `Spec` | Explicitly stated in the target build-plan section |
| `Local Canon` | Documented in another canonical local file |
| `Research-backed` | Confirmed via web research against official/primary sources |
| `Human-approved` | Resolved by explicit human decision |

> **Critical rule:** `Best practice` alone is never an acceptable source. Every behavioral decision must cite a specific file, URL, or human approval.

### milestone Registry

The milestone (Manageable Execution Unit) Registry tracks every unit of work:

- **milestone ID and slug** — unique identifier
- **Build plan matrix reference** — links to the spec section
- **Description** — what the milestone delivers
- **Status** — ⬜ planned → 🟡 in-progress → ✅ approved
- **Execution order** — dependency graph per phase

With 200+ MEUs tracked, this registry is the authoritative record of what has been built and what remains.

### Emerging Standards

The `emerging-standards.md` file captures implementation standards discovered *during* development — patterns that emerged from real failures and were codified to prevent recurrence. Each standard includes:

- Severity rating and applicability scope
- The exact failure that surfaced the standard
- Bad example (what went wrong) and good example (correct approach)
- Enforcement checklist for new work

Examples: Schema field parity for MCP tools (M1), pagination response standards (P1), bug-fix TDD protocol (G19), form guard save system (G23).

---

## 4. TDD & Anti-Practices for Agentic AI

### The Core TDD Protocol

Zorivest enforces **tests-first, implementation-after** via the Feature Intent Contract (FIC) workflow:

1. **Write FIC** — Acceptance criteria (AC-1, AC-2, ...) with source labels before any code
2. **Red Phase** — Write ALL tests first. Every AC maps to at least one test. Run tests — confirm they FAIL
3. **Green Phase** — Write minimum code to make tests pass
4. **Refactor** — Clean up while keeping tests green
5. **Quality Checks** — `pyright` (type checking) + `ruff` (linting)

### Anti-Practices Codified in AGENTS.md

The `AGENTS.md` file (the master instruction file for all AI agents) codifies hard rules that prevent common AI coding failures:

#### Test Immutability
> Once tests are written in Red phase, do NOT modify test assertions or expected values in Green phase. If a test expectation is wrong, fix the *implementation*, not the *test*.

**Why:** AI agents exhibit a strong tendency to "fix" failing tests by weakening assertions rather than fixing the code.

#### Anti-Placeholder Enforcement
> Before declaring complete, run `rg "TODO|FIXME|NotImplementedError" packages/` and resolve any matches.

**Why:** AI agents frequently leave placeholder stubs and mark tasks as complete. The anti-placeholder scan is a hard gate.

#### No Vacuous Tests (Standard M6)
> Tests must fail if the bug they target is reintroduced. A test that passes regardless of the fix is vacuous.

**Why:** AI-generated tests often assert that code runs without error rather than asserting specific behavioral outcomes.

#### Bug-Fix TDD Protocol (Standard G19)
> Bug reports ALWAYS require Red→Green TDD. Write a failing test BEFORE touching production code.

**Why:** Without this rule, AI agents jump directly to editing production code, creating fixes with no regression guard.

#### No Scope Expansion During Execution
> If the user requests features outside the approved plan scope during execution, PAUSE and ask before proceeding.

**Why:** AI agents silently expand scope, causing handoff/review misalignment and wasted review cycles.

#### Boundary Input Validation
> Python `assert` and type annotations alone are NOT acceptable runtime boundary validation. Every write surface must have an explicit Pydantic/Zod schema.

**Why:** `assert` statements are stripped in optimized Python builds (`-O` flag).

---

## 5. The Dual-Agent Orchestration Model

### Agent Roles

```
┌────────────────────────────────────────────────────────────────────┐
│  PLANNING — Primary Driver (`coordinator` — see model-routing.md)    │
│  → Reads context files, scopes project, generates plan             │
│  → Post-review gate branches on plan_to_exec_gate                  │
│    (harness-profiles.md / model-routing.md / GUARDRAILS SIGN 1):   │
│    human → awaits explicit human go-ahead                          │
│    reviewer-auto → auto-continues to execution                     │
└──────────────┬─────────────────────────────────────────────────────┘
               ▼
┌────────────────────────────────────────────────────────────┐
│  PLAN VALIDATION — External Reviewer                        │
│  (`independent_reviewer` — see model-routing.md)            │
│  → Adversarial review of unstarted plan                    │
│  → Checks contract completeness, source traceability       │
│  → Verdict: approved or changes_required                   │
└──────────────┬─────────────────────────────────────────────┘
               ▼
┌────────────────────────────────────────────────────────────┐
│  EXECUTION — Primary Driver (`coordinator` / `builder` —    │
│  see model-routing.md)                                      │
│  → TDD cycle per milestone (FIC → Red → Green → Quality)        │
│  → Creates handoff artifact with evidence bundle           │
└──────────────┬─────────────────────────────────────────────┘
               ▼
┌────────────────────────────────────────────────────────────┐
│  EXECUTION VALIDATION — External Reviewer                   │
│  (`independent_reviewer` — see model-routing.md)            │
│  → Runs full test suite, adversarial checklist (AV-1..9)   │
│  → Checks: failing-then-passing proof, no bypass hacks,    │
│    changed paths exercised, no placeholders, source-backed  │
│  → Verdict: approved or changes_required                   │
└──────────────┬─────────────────────────────────────────────┘
               ▼
┌────────────────────────────────────────────────────────────┐
│  REFLECTION — Primary Driver (per harness profile)           │
│  → Friction/quality/workflow logs                           │
│  → Pattern extraction → design rules for next session      │
└────────────────────────────────────────────────────────────┘
```

### Step-by-Step Flow

The full nine-phase sequence — every step, its slash command, the convergence caps, and the human-intervention points — lives in the operational source of truth, `development-lifecycle.md`. In brief, the cycle is **plan → plan-review → execute (TDD) → execution-review → closeout → commit**, with the primary driver (`coordinator` / `builder` per `project configuration/docs/model-routing.md`) on planning/execution and the external reviewer (`independent_reviewer`) on adversarial review.

> **Model-economics refinement (June 2026) — a two-beat sequence.** Not every step belongs on the frontier tier, and adopting a cheaper class safely takes two moves. **Beat 1 — delegation:** a 2-week audit found mechanical findings (count reconciliation, boundary validation, token swaps) drove most of the 116 paid review rounds, so those are now **delegated to the `builder` class** at low/medium effort while the `coordinator` class + frontier effort stays on the *correctness* class. **Beat 2 — parity:** reviewing the `builder` class's first delegated work (milestone-tracking, with the tier then filled by Sonnet 4.6 <!-- model-slug-ok: historical incident record — milestone-tracking ran on Sonnet 4.6 -->) exposed style/methodology drift (a silent test guard, a `test.skip`, `fireEvent` vs `userEvent`), so the instruction set was made **model-agnostic and deterministically enforced** — eslint now *blocks* `test.skip`/`.only` (standards **G36**/**G37**), and AGENTS.md rules dropped their `(Opus 4.8)` qualifier <!-- model-slug-ok: historical record of the qualifier removed in June 2026 -->. The lesson: delegation only pays off once the shared rules no longer assume *which* model is reading them. See `project configuration/docs/model-routing.md` (canonical current routing), `model-delegation.md`, and the lifecycle doc's *Multi-Model Adoption → Delegation + Instruction Parity (`builder`)*.

---

## 6. Reflection-Driven Continuous Improvement

### The Reflection Feedback Loop

```
Session N Execution → Reflection File → Next Session Design Rules
                                              ↓
Session N+1 Planning ← Reads latest reflection → Applies RULE-{N} set
                                              ↓
Session N+1 Execution → New Reflection → Updated Rules
```

### Periodic Meta-Reviews (`/session-meta-review`)

After accumulating 10-15 milestone reflections, a structured retrospective is performed:

1. **Parse** — Segment session into labeled turns (human prompts, agent actions, tool calls)
2. **Analyze** — Sequential thinking across 5 categories: Prompt Clarity, Context Load, Tool Efficiency, Verification Gaps, Communication Quality
3. **Research** — Validate mitigations against published best practices via web search
4. **Synthesize** — Generate improvement rules with before/after examples

### The Improvement Flywheel

Each reflection cycle makes the next faster. Early MEUs required 4-11 review passes; recent MEUs average 2-3 passes. The Pre-Handoff Self-Review Protocol (8 steps, 10 recurring patterns) was distilled from 7 critical review handoffs spanning 37+ passes.

### From Reflections to Evolved Instructions (SkillOpt-Inspired)

For most of the project, folding stable reflection lessons back into the *permanent* instruction docs (AGENTS.md, skills, emerging standards) was a manual rewrite — easy to do carelessly, and prone to two failure modes: **context collapse** (a wholesale rewrite loses hard-won nuance) and **unsourced drift** (a vague "best practice" sneaks in without evidence).

The `/skill-optimize` loop (June 2026) makes this rigorous. Inspired by SkillOpt's "treat instructions like a trainable weight" idea but recreated natively and **on-demand**, it harvests past reflections + handoffs as a corpus, proposes only **bounded edits** (≤4 add/delete/replace ops — a deliberate "textual learning rate"), and gates each candidate through a **cross-vendor LLM judge** (`builder` optimizer vs `independent_reviewer` judge — see `project configuration/docs/model-routing.md`) scored against a **held-out** slice of digests. Why these constraints matter:

- **Bounded edits** prevent the context collapse that wholesale rewrites cause.
- **Held-out, cross-vendor judging** prevents an agent from grading its own homework — the same diversity principle behind the dual-agent review model (§5).
- **A negative-memory buffer** stops rejected edits from being re-proposed, avoiding self-reinforcing error.
- **Human adoption is terminal** — the tool only *stages* accepted edits into a protected `LEARNED` region; a human copies them into the production doc by hand. This keeps the §9 "no non-human gate can authorize a change to a governance doc" principle intact even for self-improvement.

The mechanics live in the lifecycle doc's *Reflection Harvest → Instruction Evolution* section; this is the *why* — it closes the loop from "reflections inform the next session" to "reflections, once proven against evidence, improve the instructions themselves."

---

## 7. Issue Management & Triage

### The Known Issues System

The framework maintains a living issue tracker in two paired files:

- **`known-issues.md`** — Active issues requiring attention (target: < 100 lines)
- **`known-issues-archive.md`** — Resolved issues preserved for historical reference

Unlike traditional GitHub Issues or Jira tickets, this system is **optimized for AI agent consumption** — every issue lives in a Markdown file that agents read at session start, ensuring they are aware of all known limitations, workarounds, and upstream blockers before writing any code.

### Issue Structure

Each issue follows a standardized template:

```markdown
### [SHORT-TITLE] — Brief description
- **Severity:** Critical / High / Medium / Low
- **Component:** core / infrastructure / api / ui / mcp-server
- **Discovered:** YYYY-MM-DD
- **Status:** Open / In Progress / Workaround Applied
- **Details:** What happens, how to reproduce
- **Workaround:** (if any)
```

### Issue Categories

Issues are classified into three sections by lifecycle stage:

| Section | Contains | Agent Action |
|---------|----------|-------------|
| **Active Issues** | Bugs and limitations with no workaround | Must be addressed by new MEUs |
| **Mitigated / Workaround Applied** | Problems with temporary solutions | Monitor; plan permanent fix when prioritized |
| **Archived** | Summary table linking to `known-issues-archive.md` | Reference only; no action needed |

### Real-World Issue Examples

The Zorivest known-issues file tracks several categories of real problems:

- **Upstream SDK bugs** (e.g., `[MCP-ZODSTRIP]` — TS-SDK silently strips arguments; workaround: startup assertion for non-empty input schemas)
- **Platform-specific limitations** (e.g., `[MCP-WINDETACH]` — Node.js `detached: true` broken on Windows since 2016; workaround: Windows Job Objects)
- **Technical debt** (e.g., `[STUB-RETIRE]` — legacy service stubs to be progressively replaced as real services are implemented)
- **Unofficial API risks** (e.g., `[MKTDATA-YAHOO-UNOFFICIAL]` — Yahoo Finance endpoints are unofficial with TOS violation risk)
- **Environment-specific test failures** (e.g., `[E2E-SANDBOX-NODISPLAY]` — Playwright fails in headless/sandboxed CI environments)

### The Issue Triage Workflow (`/issue-triage`)

When known issues accumulate, the `/issue-triage` workflow performs a structured classification pass. This is a **PLANNING-phase-only workflow** — it produces a triage report, never implementation.

#### Triage Steps

**Step 1: Validate & Archive Resolved Issues**

Before any classification, every issue is verified against current codebase state:

- Grep for fix indicators in production code
- Check test coverage for regression guards
- Cross-reference milestone completion status in the registry
- Check upstream issue/PR status for third-party bugs
- Verify workarounds are still in place and still needed

Confirmed-resolved issues are moved to the archive with a 1-line summary row left behind.

**Step 2: Classify Each Remaining Issue**

Every active issue is assigned exactly one classification code:

| Code | Category | Action Required |
|------|----------|----------------|
| `milestone-tracking` | New milestone Required | Create new milestone(s) within an existing build-plan section |
| `milestone-tracking` | Expand Existing milestone | Add scope to an already-planned milestone |
| `PLAN-NEW` | New Build Plan Section | Write new build-plan file + register new MEUs |
| `UPSTREAM` | External Dependency | No milestone action — blocked on third-party fix |
| `ARCH-DECISION` | Architecture Decision Needed | Requires ADR or human approval before scoping |
| `WORKAROUND-OK` | Workaround Sufficient | Current mitigation is adequate long-term |
| `BLOCKED` | Blocked on Other Work | Will resolve when a dependent milestone completes |
| `TECH-DEBT` | Technical Debt | Low-severity cleanup for batch debt reduction |

**Step 3: Prioritize Using Severity × Impact Matrix**

| Severity \ Impact | Blocks Other Work | Standalone |
|---|---|---|
| **Critical** | P0 — Immediate | P1 — Next session |
| **High** | P1 — Next session | P2 — Near term |
| **Medium** | P2 — Near term | P3 — Backlog |
| **Low** | P3 — Backlog | P4 — Opportunistic |

**Step 4: Group into Project Batches**

Actionable issues are grouped following the same principles as `/create-plan`: dependency order first, logical continuity within components, 2-5 MEUs per batch, and no mixing P0 with P4 issues.

**Step 5: Generate Triage Report — HARD STOP**

The triage report is written to `project configuration/context/issue-triage-report.md` and includes:

- Summary counts (reviewed, archived, actionable, upstream/deferred)
- Classification table with reasoning
- Recommended project batches with milestone mappings
- Architecture decisions requiring human input

After presenting the report, the agent **must stop and await human review**. Each approved batch then feeds into `/create-plan` as scoped input, following the standard planning → validation → execution lifecycle.

### Integration with the Development Lifecycle

```
Bugs / limitations discovered during development
        ↓
Logged in known-issues.md (with severity, component, workaround)
        ↓
Periodic /issue-triage → Classification → Priority matrix
        ↓
Approved batches → /create-plan → /tdd-implementation → /milestone-tracking
        ↓
Resolution confirmed → Archive with 1-line summary + date
```

This closed loop ensures that no issue is silently forgotten and that every fix follows the same TDD-first, plan-before-code discipline as feature work.

---

## 8. Git Commit Discipline

### Policy

- **Never auto-commit.** Agents propose conventional commit messages; humans approve.
- **Never use `--no-verify`.** Pre-commit hooks are safety nets — fix the underlying issue.
- **SSH signing** is mandatory — GPG would cause interactive prompts that hang the agent terminal.

### Workflow

The agent uses a dedicated script that validates SSH signing config, checks remote URL, stages changes, runs lint + tests, commits with `-m` flag (never opens editor), pushes, and verifies.

---

## 9. Why Full Automation Between Agents Fails

A fully automated coordinator↔independent-reviewer loop without human oversight **wastes tokens, time, and produces rework** for five reasons:

1. **Misalignment Amplification** — Both agents share similar reasoning biases; human review catches category errors that agent-to-agent review misses.
2. **Infinite Revision Loops** — Without a human circuit breaker, agents enter cycles where each "fix" introduces new findings. The 2-cycle maximum exists because this was observed in practice.
3. **Token Waste on Ceremony** — A single validation cycle consumes 50-100K tokens re-reading context. Unnecessary cycles multiply costs.
4. **Scope Drift** — Unsupervised agents expand scope to "improve" things. Standard G20 was created after an agent set its own verdict to `approved`.
5. **Quality vs. Speed** — Human monitoring ensures the *right* things are built, not just *passing* things.

### The Human-in-the-Loop Gates

| Gate | What Human Reviews |
|------|-------------------|
| Plan approval | Scope, milestone sequencing |
| Execution spot-checks | Plan adherence vs. drift |
| External-reviewer verdict review | Validation logic |
| Commit approval | Change readiness |
| Merge/release | Deployability |

---

## 10. Context Degradation Vigilance

LLMs exhibit measurable accuracy degradation above ~50% context fill. Mitigations:

| Mitigation | Implementation |
|-----------|---------------|
| **Post-checkpoint recovery** | Re-read `task.md` BEFORE any action after truncation |
| **Context compression rules** | Test output → only failures. Code → diffs only. No full file inlining |
| **Cache boundary markers** | `<!-- CACHE BOUNDARY -->` separates stable from variable content for KV cache reuse |
| **Verbosity tiers** | `summary` (~500 tokens), `standard` (~2,000), `detailed` (~5,000+) |
| **Anti-premature-stop gates** | Physical `view_file` re-read of `task.md` forced before completion claims |

In 3 documented incidents, agents generated completion summaries with **10-16 unchecked tasks remaining.** The mandatory re-read gate was added to prevent this.

---

## 11. Designing for LLM Evolution

Workflows reference **roles** (orchestrator, coder, tester, reviewer), not specific models — the concrete model/harness filling each role is a capability-flag lookup in `project configuration/docs/harness-profiles.md` (which harness) and `project configuration/docs/model-routing.md` (which model), never a hard-coded product name. The `project configuration/` folder contains 18 workflows, 6 roles, 15 skills — all in plain Markdown. When a new model is assigned, it reads the same instructions with no code changes.

| Role | Current Assignment | Could Be |
|------|-------------------|----------|
| Planner/Executor (primary driver) | `coordinator` / `builder` per tier | Any model with extended thinking |
| Validator/Reviewer (external reviewer) | `independent_reviewer` (primary); headless Claude (`claude -p`, `fallback_reviewer` class, flagged same-vendor) (only on a rate-limit receipt) | Any model with code execution |
| Researcher | Multiple (Gemini, ChatGPT, Claude) | Any model with web search |

Reflection-based calibration captures what changes when models upgrade, feeding adjustments back into AGENTS.md.

---

## 12. Monetization-Aware Workflow Design

Token efficiency is built into every workflow:

- **Context compression** reduces handoff size by 60-80%
- **Right-sized MEUs** balance context setup cost vs. degradation risk
- **2-cycle revision cap** prevents token-burning infinite loops
- **Structured prompts** over conversational exploration reduce overhead
- **Session discipline** — one project per session avoids context pollution
- **Aggressive caching** — handoffs, and the milestone registry are cheaper to re-read than regenerate
- **Graceful degradation** — hard-stop gates allow seamless continuation in new sessions if rate limits are hit

---

## Appendix: Key File Reference

| File | Purpose | GitHub |
|------|---------|--------|
| `AGENTS.md` | Master instruction file for all AI agents | View |
| `project configuration/workflows/create-plan.md` | Planning workflow | View |
| `project configuration/workflows/tdd-implementation.md` | TDD execution workflow | View |
| `project configuration/workflows/milestone-tracking.md` | Handoff protocol | View |
| `project configuration/workflows/validation-review.md` | Codex validation workflow | View |
| `project configuration/workflows/execution-critical-review.md` | Adversarial implementation review | View |
| `project configuration/workflows/plan-critical-review.md` | Adversarial plan review | View |
| `project configuration/workflows/pre-build-research.md` | Research-before-build workflow | View |
| `project configuration/workflows/session-meta-review.md` | Reflection meta-review | View |
| `project configuration/workflows/execution-session.md` | Full session lifecycle | View |
| `project configuration/workflows/issue-triage.md` | Issue classification & triage | View |
| `project configuration/context/known-issues.md` | Active bugs & limitations tracker | View |
| `project configuration/context/milestone-tracking.md` | milestone tracking (200+ units) | View |
| `project configuration/docs/emerging-standards.md` | 20+ codified implementation standards | View |
| `project configuration/docs/context-compression.md` | Token-efficient artifact rules | View |
| `project configuration/roles/orchestrator.md` | Orchestrator role definition | View |
| `project configuration/roles/reviewer.md` | Reviewer role definition | View |
| `project configuration/roles/researcher.md` | Researcher role definition | View |
| `project configuration/skills/git-workflow/SKILL.md` | Agent-safe git operations | View |
| `project configuration/docs/development-lifecycle.md` | **Operational source of truth** — phase-by-phase *how* | View |
| `project configuration/docs/model-delegation.md` | `builder` (mechanical) vs `coordinator` (correctness) work-class routing | View |
| `project configuration/docs/harness-profiles.md` | Harness capability flags (role, plan_to_exec_gate, injects_auto_approval, tool-name substitution) | View |
| `project configuration/docs/model-routing.md` | Canonical model/CLI routing matrix — which class fills each role | View |
| `project configuration/workflows/skill-optimize.md` | `/skill-optimize` — evolve instruction docs from reflection evidence | View |
| `project configuration/skills/skill-optimizer/SKILL.md` | Skill-optimizer edit format, judge rubric, LEARNED-region contract | View |
