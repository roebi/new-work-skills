# New Work with Skills — Level Reference

Detailed reference for all levels described in the main SKILL.md.

---

## Level 1 — Understand Skills and Use Skills

### What is a Skill?

A Skill is a folder containing at minimum a `SKILL.md` file with YAML frontmatter (`name`, `description`) and Markdown instructions.

```
my-skill/
├── SKILL.md          # Required
├── scripts/          # Optional: executable code
├── references/       # Optional: documentation
└── assets/           # Optional: templates, resources
```

Full specification: [https://agentskills.io/specification](https://agentskills.io/specification)

### A HowTo for Humans is a Skill for Agents and Humans

The same document that instructs a human can instruct an Agent.  
Write skills as clear, step-by-step instructions — they serve both audiences.

### Skill Tree

A Skill can reference and use other Skills. This creates a **Skill Tree**:
- A high-level orchestrator Skill calls specialized sub-Skills
- Sub-Skills handle focused tasks (e.g., `validate-input`, `format-output`)
- Keeps each Skill focused and under the 500-line guideline

### Context Size — The 3-Step Read Pattern

| Step | What is loaded | Size |
|------|---------------|------|
| Discovery | `name` + `description` only | ~100 tokens |
| Activation | Full `SKILL.md` body | < 5000 tokens recommended |
| Execution | `scripts/`, `references/`, `assets/` files | On demand |

Reference: [https://agentskills.io/what-are-skills#how-skills-work](https://agentskills.io/what-are-skills#how-skills-work)

---

## Level 2 — Save Single Work as a Skill

### The Problem

Until now: your work is done, your session is lost.

### Old Solution

At end of session: let the Agent write a summary as a `.md` file that you save.

### New Thinking — The Session-to-Skill Prompt

At end of session use this prompt:

```
For this session write me a Skill named <my-new-skill> using
https://github.com/roebi/agent-skills/blob/main/skills/create-agent-skill-en/SKILL.md
Output is a Zip file ending with .zip that I can download.
It contains the new Skill and all its related files.
```

**What this achieves:**
- Work is captured in a structured, reusable format
- The skill follows the agentskills.io specification
- The zip contains everything needed to use the skill in a future session
- No knowledge is lost between sessions

---

## Level 3 — Organize Your Skills

### Repository Structure Rule

In a git repository, only a **flat `skills/` folder** is allowed:

```
my-skills-repo/
└── skills/
    ├── skill-alpha/
    │   └── SKILL.md
    └── skill-beta/
        ├── SKILL.md
        └── references/
            └── detail.md
```

No nested skill folders under `skills/`.

### Grouping Strategies

Group skills by whatever fits your context:
- **By domain**: `java-skill`, `python-skill`, `gradle-skill`
- **By team**: `backend-skill`, `frontend-skill`
- **By project**: `project-x-deploy`, `project-x-review`
- **By company process**: `onboarding-new-dev`, `release-checklist`

### Licensing Your Skills

Use the `license` frontmatter field in every skill:

```yaml
license: Apache-2.0
```

or for proprietary:

```yaml
license: All rights reserved. See LICENSE.txt
```

Consider writing a `skill-licensor` skill that automatically applies your standard license text when creating new skills.

---

## Level 4 — Continuous Skill Improvement

### The Working Context

```
your-computer/
├── project-repo/         ← where you work with the Agent
│   └── src/
└── my-skills/            ← parallel folder, your skill library
    └── skills/
        └── my-skill/
            └── SKILL.md
```

### Live Update Prompt Pattern

While working, when you notice a skill is incomplete or wrong:

```
Update/Extend in the Skill <my-skill> this:
<describe what needs to change or be added>
```

Examples:
```
Update/Extend in the Skill gradle-migration-en this:
Add a step for handling deprecated API warnings in Gradle 9.
```

```
Update/Extend in the Skill create-blog-from-issue this:
Add support for detecting MIT license from LICENSE.md files.
```

### Why This Matters

Skills improve **in the context of real work**, not in isolation.  
The improvement happens while the problem is fresh — no separate "skill maintenance" session needed.

---

## Level 5 — Skill with Special Techniques

### 5a — Skill is a Role

#### Old Thinking
- Agent has Roles
- Agent uses Skills

#### New Work Thinking

> **Your Prompt + Agent** → Skill IS a Role

Create a Skill that defines a Role. The Role Skill:
- Uses its own internal Skill
- May use other Skills (Skill Tree)
- Guides and safeguards the process

**Role types a Skill can implement:**

| Role | Behavior |
|------|----------|
| Mentor | Teaches and guides — explains decisions, suggests alternatives |
| Safeguard | Validates — checks outputs before proceeding |
| Process Guard | Enforces workflow — ensures steps are followed in order |

**The Agent becomes EVA:**
- **E** — Execute (follow Skill instructions)
- **V** — Validate (Skill-defined checks)
- **A** — Advance (move to next step only when validated)

#### Example Prompt to Create a Role Skill

```
Write a Skill named code-review-mentor that acts as a senior
code reviewer. It should mentor the developer by explaining
each issue found, suggest better patterns, and guard that
no PR is approved without addressing all critical issues.
The Skill uses check-security-issues and check-code-style
sub-Skills.
```

---

### 5b — Skill Having Evolving State

#### The Pattern

A Skill that maintains a state file across iterations:

```
session-work/
├── my-prompt.md          ← your input / goal
├── state-v1.md           ← first evolved state
├── state-v2.md           ← second evolved state
└── state-vN.md           ← final state (when expectations met)
```

#### Skill Instructions for Evolving State

Include in your Skill's SKILL.md:

```markdown
## Evolving State

1. Read the user prompt file
2. Read the current state file (if exists)
3. Process: apply skill instructions to current state
4. Write a new state file with incremented version
5. Report: what changed, what remains, suggested next step
```

#### Loop Until Done

```
[Prompt] + [State vN] → Skill → [State vN+1]
```

Repeat until the state file matches your expectations.  
This is particularly powerful for:
- Iterative document generation
- Progressive code refactoring
- Multi-step research synthesis

---

### 5c — Skill Until Requirement is Fulfilled (TDD for Skills)

#### Concept

Apply Test-Driven Development thinking to Skill workflows.

#### Generic TDD Skill Pattern

```markdown
## Requirement Check

Before completing, verify:
- [ ] Requirement 1: <describe>
- [ ] Requirement 2: <describe>
- [ ] Requirement N: <describe>

If any requirement is not met: loop back and address it.
Only output final result when ALL requirements are checked.
```

#### Combinations

| Combination | Effect |
|------------|--------|
| TDD Skill + Simple Skill | Validates simple task completion |
| TDD Skill + Role Skill | Role enforces requirements as process guard |
| TDD Skill + Evolving State Skill | Loops evolving state until requirements are met |
| All three combined | Full quality-gated, role-guided, iterative workflow |

---

## This is the End of the Beginning

*To be continued…*

The levels above represent a living framework. As Skills, Agents, and working practices evolve, new levels and patterns will emerge.

**Author:** Robert Halter, Switzerland  
**Version:** 2.0 — 2026-03-10
