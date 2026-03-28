# gstack Best Practices Guide

> A comprehensive guide to Garry Tan's gstack skill package for Claude Code — from installation to mastery, for every audience.

**gstack version:** 0.12.5.0 | **Last updated:** 2026-03-27 | **License:** MIT

---

## Table of Contents

1. [What is gstack?](#1-what-is-gstack)
2. [Installation](#2-installation)
3. [The Sprint Model](#3-the-sprint-model)
4. [Section A — For Social Solidarity Economy Organizations](#section-a--for-social-solidarity-economy-organizations)
5. [Section B — For Tech Startups & Founders](#section-b--for-tech-startups--founders)
6. [Section C — For Learners of Skill Packages](#section-c--for-learners-of-skill-packages)
7. [Section D — For Instructors & Educators](#section-d--for-instructors--educators)
8. [Section E — For Staff Engineers & Tech Leads](#section-e--for-staff-engineers--tech-leads)
9. [Skill Reference](#skill-reference)
10. [Builder Ethos & Philosophy](#builder-ethos--philosophy)
11. [Troubleshooting](#troubleshooting)
12. [Resources](#resources)

---

## 1. What is gstack?

gstack is an open-source skill package for [Claude Code](https://docs.anthropic.com/en/docs/claude-code) created by [Garry Tan](https://x.com/garrytan), President & CEO of [Y Combinator](https://www.ycombinator.com/). It transforms a single AI assistant into a **virtual software development team** — 20 specialist roles and 8 power tools, all invoked as slash commands.

### The core idea

Instead of using Claude Code as a generic assistant, gstack gives it **structured roles**:

| Role | Slash Command | What it does |
|------|---------------|--------------|
| YC Office Hours | `/office-hours` | Reframes your product idea before you write code |
| CEO / Founder | `/plan-ceo-review` | Finds the 10-star product hiding in your request |
| Eng Manager | `/plan-eng-review` | Locks architecture, data flow, diagrams, edge cases |
| Senior Designer | `/plan-design-review` | Rates design dimensions 0-10, fixes the plan |
| Design Partner | `/design-consultation` | Builds a complete design system from scratch |
| Staff Engineer | `/review` | Finds bugs that pass CI but blow up in production |
| Debugger | `/investigate` | Systematic root-cause debugging |
| Designer Who Codes | `/design-review` | Live-site visual audit + fix loop |
| QA Lead | `/qa` | Tests your app, finds bugs, fixes them, re-verifies |
| QA Reporter | `/qa-only` | Same as /qa but report only, no code changes |
| Chief Security Officer | `/cso` | OWASP Top 10 + STRIDE threat model audit |
| Release Engineer | `/ship` | Sync, test, audit coverage, push, open PR |
| Release Engineer | `/land-and-deploy` | Merge the PR, wait for CI, verify production |
| SRE | `/canary` | Post-deploy monitoring for errors and regressions |
| Performance Engineer | `/benchmark` | Baseline and compare page load times, Web Vitals |
| Technical Writer | `/document-release` | Update all project docs to match what shipped |
| Eng Manager | `/retro` | Weekly retrospective with per-person breakdowns |
| QA Engineer | `/browse` | Real Chromium browser, real clicks, ~100ms/command |
| Session Manager | `/setup-browser-cookies` | Import cookies from real browser for auth testing |
| Review Pipeline | `/autoplan` | One-command fully reviewed plan |

### Power tools

| Tool | Command | Purpose |
|------|---------|---------|
| Second Opinion | `/codex` | Independent review from OpenAI Codex CLI |
| Safety Guardrails | `/careful` | Warns before destructive commands |
| Edit Lock | `/freeze` | Restrict file edits to one directory |
| Full Safety | `/guard` | `/careful` + `/freeze` combined |
| Unlock | `/unfreeze` | Remove the freeze boundary |
| Deploy Config | `/setup-deploy` | One-time setup for deployment |
| Self-Updater | `/gstack-upgrade` | Upgrade gstack to latest version |

### What makes it different

- **Process, not tools.** Skills run in sprint order: Think → Plan → Build → Review → Test → Ship → Reflect.
- **Each skill feeds the next.** `/office-hours` writes a design doc that `/plan-ceo-review` reads. `/plan-eng-review` writes a test plan that `/qa` picks up.
- **Real browser.** gstack runs a persistent headless Chromium daemon with ~100ms latency per command — the agent can see, click, and test real web pages.
- **Free and MIT licensed.** No premium tier, no waitlist.

---

## 2. Installation

### Prerequisites

- [Claude Code](https://docs.anthropic.com/en/docs/claude-code) installed
- [Git](https://git-scm.com/)
- [Bun](https://bun.sh/) v1.0+
- [Node.js](https://nodejs.org/) (required on Windows)

### Step 1: Install on your machine (30 seconds)

Open Claude Code and paste:

```
Install gstack: run git clone https://github.com/garrytan/gstack.git ~/.claude/skills/gstack && cd ~/.claude/skills/gstack && ./setup
```

Then add a `gstack` section to your project's `CLAUDE.md`:

```markdown
## gstack

Use the /browse skill from gstack for all web browsing. Never use mcp__claude-in-chrome__* tools.

Available skills: /office-hours, /plan-ceo-review, /plan-eng-review, /plan-design-review,
/design-consultation, /review, /ship, /land-and-deploy, /canary, /benchmark, /browse,
/qa, /qa-only, /design-review, /setup-browser-cookies, /setup-deploy, /retro,
/investigate, /document-release, /codex, /cso, /autoplan, /careful, /freeze, /guard,
/unfreeze, /gstack-upgrade.
```

### Step 2: Add to your repo for teammates (optional)

```
cp -Rf ~/.claude/skills/gstack .claude/skills/gstack && rm -rf .claude/skills/gstack/.git && cd .claude/skills/gstack && ./setup
```

Real files get committed to your repo — `git clone` just works. Everything lives inside `.claude/`.

### Step 3: Verify

Run `/office-hours` in Claude Code. If the skill activates and asks you about what you're building, you're set.

### For Codex, Gemini CLI, or Cursor

```bash
git clone https://github.com/garrytan/gstack.git .agents/skills/gstack
cd .agents/skills/gstack && ./setup --host codex
```

Or auto-detect your installed agents:

```bash
git clone https://github.com/garrytan/gstack.git ~/gstack
cd ~/gstack && ./setup --host auto
```

### Windows notes

gstack works on Windows 11 via Git Bash or WSL. Node.js is required in addition to Bun due to a known Bun bug with Playwright's pipe transport on Windows. The browse server automatically falls back to Node.js. Make sure both `bun` and `node` are on your PATH.

---

## 3. The Sprint Model

gstack is a **process**, not a collection of tools. The skills run in the order a real engineering sprint runs:

```
Think → Plan → Build → Review → Test → Ship → Reflect
```

### The complete workflow

```
1. /office-hours        — Define what you're actually building
2. /plan-ceo-review     — Challenge scope, find the 10-star version
3. /plan-design-review  — Catch design gaps before implementation
4. /plan-eng-review     — Lock architecture, data flow, test plan
5. [Build]              — Implement the plan
6. /review              — Find production bugs, auto-fix obvious ones
7. /cso                 — Security audit (OWASP + STRIDE)
8. /qa                  — Browser-based testing, bug fixes, regression tests
9. /ship                — Sync, test, push, open PR
10. /land-and-deploy    — Merge, deploy, verify production
11. /canary             — Post-deploy monitoring
12. /document-release   — Update all docs
13. /retro              — Weekly retrospective
```

### The key insight

Each skill **reads what the previous skill wrote**. The design doc from `/office-hours` feeds into `/plan-ceo-review`. The test plan from `/plan-eng-review` feeds into `/qa`. The review from `/review` feeds into `/ship`. Nothing falls through the cracks.

---

## Section A — For Social Solidarity Economy Organizations

### Why gstack matters for SSE organizations

Social solidarity economy (SSE) organizations — cooperatives, mutual aid networks, community land trusts, social enterprises, and nonprofit tech projects — typically operate with **limited engineering resources**. A single developer or a small volunteer team often carries the entire technology burden. gstack changes the economics of software development for these organizations.

### The multiplier effect

With gstack, a single developer can produce what previously required a team:

| Task | Traditional team | One person + gstack |
|------|-----------------|---------------------|
| Scaffolding & boilerplate | 2 days | 15 minutes |
| Test writing | 1 day | 15 minutes |
| Feature implementation | 1 week | 30 minutes |
| Bug fix + regression test | 4 hours | 15 minutes |
| Architecture & design | 2 days | 4 hours |

This compression ratio means SSE organizations can **build and maintain professional-grade software** without enterprise budgets.

### Recommended workflow for SSE projects

**1. Start with `/office-hours` to clarify your mission-driven product**

SSE organizations often build technology to serve a community need. `/office-hours` will challenge your assumptions about what the community actually needs versus what you think they need. Example:

- You say: "We need a member portal for our cooperative."
- gstack asks: "Who are the members? What's their tech literacy? What specific problem does the portal solve that email/WhatsApp doesn't?"
- The reframe might reveal: you're building a **democratic decision-making platform**, not a portal.

**2. Use `/plan-ceo-review` in Scope Reduction mode**

SSE projects benefit from shipping the narrowest wedge first. Use Scope Reduction mode to find the minimum viable version that serves your community immediately. Expand later based on real usage.

**3. Use `/careful` and `/guard` for production safety**

SSE organizations often have limited ops support. Use `/careful` to prevent destructive commands and `/guard` when working on production systems. These safety guardrails protect against mistakes that a small team can't afford.

**4. Use `/retro` to track volunteer contributions**

`/retro` produces per-person breakdowns and shipping streaks. For volunteer-driven organizations, this visibility helps recognize contributions and maintain momentum.

### Best practices for SSE

- **Prioritize accessibility.** Use `/plan-design-review` to catch accessibility gaps during planning. SSE software often serves diverse communities.
- **Document everything.** Run `/document-release` after every ship. When volunteers rotate, documentation is the continuity.
- **Security first.** Run `/cso` on every release. Community data is sacred. The OWASP + STRIDE audit catches vulnerabilities before they become incidents.
- **Open source your work.** gstack itself is MIT licensed. Consider open-sourcing your SSE tools so other organizations can benefit and contribute.

### Case: Community marketplace platform

```
/office-hours     — "We want a local exchange platform for our cooperative network"
                    Reframe: "You're building a trust-based marketplace where
                    reputation matters more than price"

/plan-ceo-review  — Scope Reduction mode: ship member directory + simple listings first
/plan-eng-review  — Architecture for multi-tenant cooperative data isolation
/cso              — Audit: member data protection, GDPR compliance for EU cooperatives
/qa               — Test accessibility on mobile (many members are mobile-first)
/ship             — PR with full test coverage
/retro            — Track which volunteers shipped what this week
```

---

## Section B — For Tech Startups & Founders

### Why gstack was built for you

Garry Tan built gstack for the way he works — and the way he's seen thousands of YC founders work. The premise: **a single builder with the right tooling can move faster than a traditional team.**

In Garry's words: *"600,000+ lines of production code in 60 days, 10,000-20,000 lines per day, part-time, while running YC full-time."*

### The founder's workflow

**Phase 1: Idea validation (30 minutes)**

```
/office-hours
```

This is where every startup should begin. Six forcing questions that come from how YC partners evaluate companies:

1. **Demand reality** — Is this a real problem or a hypothetical?
2. **Status quo** — How do people solve this today? (If they don't, is it actually painful?)
3. **Desperate specificity** — Can you name a specific person who needs this?
4. **Narrowest wedge** — What's the smallest thing you can ship to learn?
5. **Observation & surprise** — What have you noticed that others haven't?
6. **Future-fit** — Why is now the right time?

These questions are uncomfortable on purpose. They save you from building the wrong thing.

**Phase 2: Product review (20 minutes)**

```
/plan-ceo-review
```

Four modes to match your current needs:
- **Scope Expansion** — for when you need to dream big (fundraising, vision docs)
- **Selective Expansion** — hold current scope, but see what else is possible
- **Hold Scope** — maximum rigor on what you already planned
- **Scope Reduction** — find the MVP (most startups should start here)

**Phase 3: Technical planning (30 minutes)**

```
/plan-eng-review
```

Produces architecture diagrams, data flow, state machines, edge cases, and test matrices. Forces hidden assumptions into the open. This is the document your first engineer (or your AI) will execute against.

**Phase 4: Build + Ship (hours to days)**

```
[Implement]
/review → /cso → /qa → /ship → /land-and-deploy → /canary
```

One command at each stage. The pipeline catches bugs, security issues, and deployment failures before they reach users.

### Parallel sprints: The 10x multiplier

The real power of gstack emerges with parallel execution. Using [Conductor](https://conductor.build), you can run 10-15 Claude Code sessions simultaneously:

- Session 1: `/office-hours` on a new feature idea
- Session 2: Implementing a feature from yesterday's plan
- Session 3: `/review` on a completed PR
- Session 4: `/qa` on staging
- Session 5-10: More features, more branches

The sprint structure prevents chaos — each agent knows its role and when to stop.

### Best practices for startups

- **Ship the narrowest wedge.** `/office-hours` and `/plan-ceo-review` in Scope Reduction mode will help you resist the urge to build too much.
- **Test everything from day one.** `/ship` bootstraps test frameworks if you don't have one. 100% test coverage is the goal — tests make AI-assisted coding safe.
- **Run `/cso` before launch.** A security incident at the early stage can kill a startup. The OWASP + STRIDE audit takes minutes.
- **Use `/retro` weekly.** Even as a solo founder, the retro forces you to reflect on what's working and what's not. `/retro global` runs across all your projects.
- **Don't skip design.** Use `/design-consultation` before your first line of frontend code. A distinctive design system prevents the "every AI app looks the same" problem.

---

## Section C — For Learners of Skill Packages

### What you'll learn

gstack is one of the best-structured skill packages in the Claude Code ecosystem. By studying and using it, you'll learn:

1. **How AI agent workflows are structured** — the sprint model (Think → Plan → Build → Review → Test → Ship → Reflect)
2. **How SKILL.md files work** — the standard format for teaching Claude Code new behaviors
3. **How a persistent browser daemon integrates with an AI agent** — real-world architecture
4. **How to write effective prompts** — each skill is essentially a highly refined prompt

### Getting started: Your first 60 minutes

**Minutes 0-5: Install**

Follow the installation steps in [Section 2](#2-installation). Verify by running `/office-hours`.

**Minutes 5-20: Run the basic workflow**

```
1. /office-hours    — Describe a project you want to build (even a simple one)
2. /plan-ceo-review — See how it challenges your assumptions
3. /plan-eng-review — See the architecture it produces
```

Don't build anything yet. Just observe how each skill transforms your vague idea into a structured plan.

**Minutes 20-40: Explore the browser**

```
/browse
$B goto https://example.com
$B snapshot -i
$B click @e1
$B screenshot
```

This is where most learners have an "aha" moment — the AI can see and interact with real web pages.

**Minutes 40-60: Read the skill files**

Open `~/.claude/skills/gstack/office-hours/SKILL.md` and read it. This is the actual instruction set that makes `/office-hours` work. Notice:
- The structured prompt format
- How it references other skills
- The AskUserQuestion pattern
- How it writes artifacts that downstream skills consume

### The learning path

```
Level 1: User         — Run skills, follow the workflow
Level 2: Power User   — Customize CLAUDE.md, use /autoplan, run parallel sprints
Level 3: Contributor  — Read SKILL.md files, understand the architecture
Level 4: Creator      — Fork gstack, modify skills, create your own
```

### Understanding SKILL.md structure

Every gstack skill follows this pattern:

```markdown
---
name: skill-name
description: One-line description
...
---

[Preamble — shared setup across all skills]
[Skill-specific instructions]
[Workflow steps]
[Examples]
[Error handling guidance]
```

The preamble (injected via `{{PREAMBLE}}` template) handles:
- Update checks
- Session tracking (multi-window awareness)
- Contributor mode
- AskUserQuestion format standardization
- Search Before Building philosophy

### Key concepts to understand

**1. The ref system** — When the browser takes a snapshot, each interactive element gets a ref like `@e1`, `@e2`. The agent uses these refs to click, fill, and interact. No CSS selectors needed.

**2. The daemon model** — The browser stays alive between commands (~100ms response time). Cookies, tabs, and localStorage persist. This is what makes real QA testing possible.

**3. Artifact flow** — Skills write files to `~/.gstack/projects/` that other skills read. This is how `/office-hours` → `/plan-ceo-review` → `/plan-eng-review` form a pipeline without manual copy-paste.

**4. The Review Readiness Dashboard** — Tracks which reviews have been run. Eng Review is the required gate; CEO and Design reviews are recommended but optional.

### Exercises for learners

1. **Run the full sprint on a toy project.** Pick something simple (a to-do app, a personal blog). Go through every stage from `/office-hours` to `/retro`.
2. **Read three SKILL.md files.** Compare `/office-hours`, `/review`, and `/qa`. Notice the patterns.
3. **Modify a skill.** Fork gstack, change something in `/office-hours/SKILL.md` (e.g., add a seventh forcing question), and test it.
4. **Build your own skill.** Use the `SKILL.md.tmpl` template to create a new skill for your own workflow.

---

## Section D — For Instructors & Educators

### Teaching with gstack

gstack provides a structured, repeatable framework for teaching AI-assisted software development. Each skill is a **lesson in good engineering practice** wrapped in an AI-powered tool.

### Curriculum design

#### Module 1: Product Thinking (2 hours)

**Skills:** `/office-hours`, `/plan-ceo-review`

**Learning objectives:**
- Distinguish between features and products
- Apply the six YC forcing questions to validate ideas
- Practice scope management (Expansion vs. Reduction)

**Exercise:** Each student describes a project idea. They run `/office-hours` and present the reframe to the class. Discussion: what did the AI challenge? What did the student learn about their own assumptions?

**Assessment:** Students submit the design doc artifact from `/office-hours` along with a 500-word reflection on how their thinking changed.

#### Module 2: Technical Architecture (2 hours)

**Skills:** `/plan-eng-review`

**Learning objectives:**
- Read and create architecture diagrams
- Identify system boundaries, failure modes, and edge cases
- Write testable specifications

**Exercise:** Starting from Module 1's design doc, students run `/plan-eng-review` and analyze the diagrams it produces. They identify one assumption the AI missed and present it.

**Assessment:** Students annotate the engineering review with their own additions — edge cases, failure modes, or alternative architectures.

#### Module 3: Design Systems (2 hours)

**Skills:** `/design-consultation`, `/plan-design-review`

**Learning objectives:**
- Understand typography, color, spacing, and layout as system
- Distinguish between safe choices and creative risks
- Detect "AI slop" — generic AI-generated design patterns

**Exercise:** Students run `/design-consultation` for their project. They compare the "safe choices" vs. "risks" and write a justification for which risks they accept. Then run `/plan-design-review` on their plan to see the 0-10 ratings.

**Assessment:** Students present their design system and defend their creative choices.

#### Module 4: Code Quality & Security (2 hours)

**Skills:** `/review`, `/cso`

**Learning objectives:**
- Understand production-grade code review
- Learn OWASP Top 10 and STRIDE threat modeling
- Practice reading and responding to review feedback

**Exercise:** Students implement their feature, then run `/review` and `/cso`. They categorize each finding (auto-fixed, requires judgment, false positive) and explain their reasoning.

**Assessment:** Students submit a diff showing how they responded to review findings, with commentary.

#### Module 5: Testing & QA (2 hours)

**Skills:** `/qa`, `/browse`, `/benchmark`

**Learning objectives:**
- Understand browser-based QA testing
- Write and verify regression tests
- Measure performance baselines

**Exercise:** Students deploy to staging, run `/qa` on their URL, and analyze the bug report. They verify that auto-generated regression tests actually catch the bugs.

**Assessment:** Students present their QA report, including before/after evidence and performance baselines.

#### Module 6: Shipping & Reflection (2 hours)

**Skills:** `/ship`, `/land-and-deploy`, `/canary`, `/retro`

**Learning objectives:**
- Understand the full release pipeline
- Practice post-deploy monitoring
- Reflect on the development process

**Exercise:** Students ship their project through the full pipeline. They run `/canary` for 10 minutes post-deploy and run `/retro` to analyze their week.

**Assessment:** Students submit their `/retro` output and a reflective essay on what they'd do differently.

### Instructor tips

- **Start with `/careful`.** Activate safety guardrails for all students. It prevents destructive commands while they learn.
- **Use `/freeze` during live demos.** Lock edits to the demo directory so Claude can't accidentally modify other files.
- **Pair `/codex` with `/review`.** The cross-model analysis (Claude + OpenAI) teaches students that different AI models catch different issues — critical thinking about AI output.
- **Grade the artifacts, not just the code.** Design docs, review reports, QA reports, and retros are all assessable learning artifacts.
- **Encourage forking.** The best way to understand a skill is to modify it. Have advanced students create custom skills.

### Classroom safety

- gstack includes **opt-in** telemetry only. Default is off. Nothing is sent unless the student explicitly opts in.
- All processing is local — code never leaves the student's machine (beyond normal Claude Code API usage).
- Use `/guard` (which combines `/careful` + `/freeze`) for students working on shared staging environments.

---

## Section E — For Staff Engineers & Tech Leads

### gstack as an engineering process

For staff engineers and tech leads, gstack isn't about writing code faster — it's about **enforcing engineering rigor at every stage** without slowing down.

### The review pipeline

The Review Readiness Dashboard tracks every review:

```
+====================================================================+
|                    REVIEW READINESS DASHBOARD                       |
+====================================================================+
| Review          | Runs | Last Run            | Status    | Required |
|-----------------|------|---------------------|-----------|----------|
| Eng Review      |  1   | 2026-03-16 15:00    | CLEAR     | YES      |
| CEO Review      |  1   | 2026-03-16 14:30    | CLEAR     | no       |
| Design Review   |  0   | —                   | —         | no       |
+--------------------------------------------------------------------+
| VERDICT: CLEARED — Eng Review passed                                |
+====================================================================+
```

Eng Review is the only required gate. CEO and Design reviews are recommended for product and UI changes respectively. Configure with `gstack-config set skip_eng_review true` if needed.

### Best practices for tech leads

**1. Standardize the pipeline across your team**

Add gstack to your repo (Step 2 in installation). Every engineer gets the same skills, the same review standards, and the same QA methodology. This is process-as-code.

**2. Use `/autoplan` for faster reviews**

`/autoplan` runs CEO → Design → Eng review automatically with encoded decision principles. It only surfaces "taste decisions" (close approaches, borderline scope) for your approval. Saves 20-30 minutes of interactive review per plan.

**3. Require `/cso` before security-sensitive merges**

The OWASP Top 10 + STRIDE audit has 17 false positive exclusions and an 8/10+ confidence gate. Each finding includes a concrete exploit scenario — not vague warnings.

**4. Use `/codex` for cross-model analysis**

When both `/review` (Claude) and `/codex` (OpenAI Codex) review the same branch, you get a cross-model analysis showing overlapping and unique findings. Two independent AI models catching different bugs is more thorough than one.

**5. Monitor with `/canary` post-deploy**

Set up `/canary` to watch production after every deploy. It monitors console errors, performance regressions, and page failures. Catches issues that CI misses.

**6. Run `/retro global` weekly**

`/retro global` runs across all projects and AI tools. It produces per-person breakdowns, shipping streaks, test health trends, and growth opportunities. Use it in your weekly team sync.

### Architecture awareness

gstack's browser daemon uses a clean architecture:

- **Localhost-only HTTP server** with bearer token auth
- **Persistent Chromium** with ~100ms command latency
- **Ref system** using the accessibility tree (no DOM mutation, no CSP conflicts)
- **Ring buffer logging** (50K entries, O(1) push, async disk flush)
- **Crash recovery** via auto-restart (no self-healing complexity)

The state file at `.gstack/browse.json` contains PID, port, token, and binary version. Version auto-restart ensures stale binaries are never used.

### Integrating with CI/CD

- `/ship` auto-bootstraps test frameworks if missing
- Every `/qa` bug fix auto-generates a regression test
- `/document-release` is auto-invoked by `/ship` — docs stay current
- `/benchmark` baselines can be tracked per-PR for performance regression detection

---

## Skill Reference

### Quick reference card

| Stage | Skill | When to use |
|-------|-------|-------------|
| **Think** | `/office-hours` | Starting a new project or feature |
| **Plan** | `/plan-ceo-review` | Product/scope decisions |
| **Plan** | `/plan-design-review` | UI/UX planning |
| **Plan** | `/plan-eng-review` | Architecture & technical planning |
| **Plan** | `/autoplan` | Run all reviews in one command |
| **Design** | `/design-consultation` | Build a design system from scratch |
| **Build** | (your implementation) | Write the code |
| **Review** | `/review` | Code review for production bugs |
| **Review** | `/codex` | Cross-model second opinion |
| **Security** | `/cso` | OWASP + STRIDE audit |
| **Test** | `/qa` | Browser-based QA + bug fixing |
| **Test** | `/qa-only` | QA report without code changes |
| **Test** | `/browse` | Manual browser interaction |
| **Test** | `/benchmark` | Performance baselines |
| **Ship** | `/ship` | Test, push, open PR |
| **Deploy** | `/land-and-deploy` | Merge, deploy, verify |
| **Monitor** | `/canary` | Post-deploy monitoring |
| **Document** | `/document-release` | Update all docs |
| **Reflect** | `/retro` | Weekly retrospective |
| **Safety** | `/careful` | Destructive command warnings |
| **Safety** | `/freeze` | Restrict edits to one directory |
| **Safety** | `/guard` | `/careful` + `/freeze` |
| **Safety** | `/unfreeze` | Remove freeze boundary |
| **Setup** | `/setup-deploy` | Configure deployment |
| **Setup** | `/setup-browser-cookies` | Import browser cookies |
| **Utility** | `/gstack-upgrade` | Upgrade gstack |

---

## Builder Ethos & Philosophy

gstack embeds two core principles from Garry Tan's [Builder Ethos](https://github.com/garrytan/gstack/blob/main/ETHOS.md):

### 1. Boil the Lake

> "AI-assisted coding makes the marginal cost of completeness near-zero. When the complete implementation costs minutes more than the shortcut — do the complete thing. Every time."

- A **lake** is boilable — 100% test coverage, full feature implementation, all edge cases.
- An **ocean** is not — multi-quarter platform rewrites, entire system replacements.
- Boil lakes. Flag oceans as out of scope.

**Anti-patterns to avoid:**
- "Choose B — it covers 90% with less code." (If A is 70 lines more, choose A.)
- "Let's defer tests to a follow-up PR." (Tests are the cheapest lake to boil.)
- "This would take 2 weeks." (Say: "2 weeks human / ~1 hour AI-assisted.")

### 2. Search Before Building

> "The 1000x engineer's first instinct is 'has someone already solved this?' not 'let me design it from scratch.'"

Three layers of knowledge:
- **Layer 1: Tried and true.** Standard, battle-tested patterns. The risk: assuming the obvious answer is right.
- **Layer 2: New and popular.** Current best practices. The risk: humans are subject to mania.
- **Layer 3: First principles.** Original observations from reasoning about the specific problem. The most valuable of all.

The **eureka moment** happens when you search (Layers 1+2), understand the landscape, and then discover through first-principles reasoning (Layer 3) that the conventional approach is wrong. These moments are where breakthrough products come from.

---

## Troubleshooting

| Problem | Solution |
|---------|----------|
| Skill not showing up | `cd ~/.claude/skills/gstack && ./setup` |
| `/browse` fails | `cd ~/.claude/skills/gstack && bun install && bun run build` |
| Stale install | Run `/gstack-upgrade` or set `auto_upgrade: true` in `~/.gstack/config.yaml` |
| Claude can't see skills | Add gstack section to your project's `CLAUDE.md` (see [Installation](#2-installation)) |
| Windows: browse errors | Ensure both `bun` and `node` are on your PATH |
| Codex "invalid SKILL.md" | `cd ~/.codex/skills/gstack && git pull && ./setup --host codex` |

---

## Resources

### Official

- [gstack GitHub Repository](https://github.com/garrytan/gstack) — Source code, issues, and contributions
- [Skill Deep Dives](https://github.com/garrytan/gstack/blob/main/docs/skills.md) — Philosophy, examples, and workflow for every skill
- [Architecture](https://github.com/garrytan/gstack/blob/main/ARCHITECTURE.md) — Design decisions and system internals
- [Builder Ethos](https://github.com/garrytan/gstack/blob/main/ETHOS.md) — The philosophy behind gstack
- [Browser Reference](https://github.com/garrytan/gstack/blob/main/BROWSER.md) — Full command reference for `/browse`
- [Contributing](https://github.com/garrytan/gstack/blob/main/CONTRIBUTING.md) — Dev setup, testing, and contributor guide
- [Changelog](https://github.com/garrytan/gstack/blob/main/CHANGELOG.md) — What's new in every version

### Tutorials & Articles

- [GStack Tutorial: Garry Tan's Claude Code Workflow for 10K LOC/Week Development](https://www.sitepoint.com/gstack-garry-tan-claude-code/) — SitePoint
- [gstack: Installing Garry Tan's Claude Code Setup in One Click](https://www.sitepoint.com/gstack-claude-code-setup/) — SitePoint
- [Garry Tan's gstack: Running Claude Like an Engineering Team](https://agentnativedev.medium.com/garry-tans-gstack-running-claude-like-an-engineering-team-392f1bd38085) — Medium
- [Why Garry Tan's Claude Code setup has gotten so much love, and hate](https://techcrunch.com/2026/03/17/why-garry-tans-claude-code-setup-has-gotten-so-much-love-and-hate/) — TechCrunch
- [gstack Setup Install Guide](https://gstacks.org/gstack-setup-install-guide.html) — gstacks.org

### Community

- [gstack on Product Hunt](https://www.producthunt.com/products/gstack)
- [Awesome Agent Skills](https://github.com/VoltAgent/awesome-agent-skills) — Claude Code skills and 1000+ agent skills from the community

---

*This guide is part of the e-book project on agent skill packages. gstack is MIT licensed and maintained by Garry Tan and contributors.*
