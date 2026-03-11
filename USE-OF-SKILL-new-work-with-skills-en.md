# Proof of Skill — new-work-with-skills-en

> **PoS — Prove of Skill**  
> This document demonstrates the skill `new-work-with-skills-en` in action  
> across three experience levels: Novice, Intermediate, and Senior.

**Skill used:** `new-work-with-skills-en` v2.0 — roebi/new-work-skills  
**Agent:** Claude Sonnet 4.6  
**Date:** 2026-03-10  
**License:** CC BY-NC-SA 4.0 — Author Robert Halter, Switzerland

---

## Novice Prompt — Explain me this skill

### Prompt

> Explain me this skill.

---

### Agent Answer

*(Skill `new-work-with-skills-en` activated — reading SKILL.md)*

Welcome. This skill teaches you a new way of working with AI Agents — step by step, at your own pace.

---

**The core idea in one sentence:**

> Your Prompt, combined with an Agent and a Skill, is the new unit of work.

---

**What is a Skill?**

A Skill is a folder with a file called `SKILL.md` inside it.  
That file contains:
- A name and a description (so the Agent knows when to use it)
- Instructions written in plain Markdown — step by step, like a recipe

A Skill is readable by both you (a human) and an Agent. The same document serves both.

```
my-skill/
└── SKILL.md   ← instructions for humans and agents
```

Think of it like this: if you have ever written a "how-to" note for a colleague, you already know how to write a Skill.

---

**What does this skill teach?**

It is organized in levels — you do not need to learn everything at once:

| Level | What you learn |
|-------|----------------|
| 1 | What a Skill is and how to use one |
| 2 | How to save your session work as a Skill so nothing gets lost |
| 3 | How to organize your Skills in a git repository |
| 4 | How to improve your Skills while you work |
| 5 | Advanced patterns: Skills as Roles, looping state, quality gates |

---

**Where do I start?**

Start at Level 1. Read the specification at [https://agentskills.io/specification](https://agentskills.io/specification).  
Then try Level 2 — use the session-to-skill prompt at the end of your next working session.  
That one change alone will make a big difference.

---

**The most important thing to remember:**

A HowTo you write for yourself is already a Skill.  
You just need to add a short frontmatter block at the top:

```yaml
---
name: my-first-skill
description: What this skill does and when to use it.
---
```

Then your notes become something an Agent can use too.

---

## Intermediate Prompt — How do I have to organize my skills?

### Prompt

> How do I have to organize my skills?

---

### Agent Answer

*(Skill `new-work-with-skills-en` activated — reading SKILL.md + references/levels.md → Level 3)*

Good question — organization is where many people get confused at first. Here are the rules and the reasoning behind them.

---

**Rule 1 — One git repository (or more), one flat `skills/` folder**

In any git repository that holds skills, you use exactly one folder called `skills/`.  
Inside it, each skill lives in its own subfolder. No nesting beyond that.

```
my-skills-repo/
└── skills/
    ├── gradle-migration-en/
    │   └── SKILL.md
    ├── code-review-mentor/
    │   ├── SKILL.md
    │   └── references/
    │       └── patterns.md
    └── new-work-with-skills-en/
        └── SKILL.md
```

Why flat? Because agents discover skills by scanning this folder. Nesting creates ambiguity about what is a skill and what is a sub-folder of a skill. Flat keeps it unambiguous.

---

**Rule 2 — Group by whatever fits your context**

There is no forced taxonomy. Group by what makes sense for you:

| Grouping strategy | Example repo name | Example skills inside |
|------------------|------------------|----------------------|
| By technology | `java-skills` | `gradle-migration-en`, `spring-boot-setup` |
| By team | `testautomation-skills` | `test-plan-creator`, `regression-checker` |
| By project | `project-x-skills` | `deploy-to-staging`, `release-checklist` |
| By company process | `company-skills` | `onboarding-new-dev`, `incident-response` |
| Personal / cross-domain | `my-skills` | anything you use regularly |

You can have multiple repositories. A personal `my-skills` repo plus a team `testautomation-skills` repo is a perfectly valid setup.

---

**Rule 3 — License every skill**

Add a `license` field to the frontmatter of every skill. Do not leave it empty.

```yaml
---
name: my-skill
description: ...
license: CC BY-NC-SA 4.0. Author Your Name.
---
```

If you have many skills, consider writing a `skill-licensor` skill — a skill that automatically applies your standard license when you create new skills. It saves you from forgetting.

---

**Rule 4 — Name skills with a language suffix**

If your skills may eventually be used by people in different languages, add a language code to the name:

```
gradle-migration-en/    ← English
gradle-migration-de/    ← German (future)
```

This makes internationalization clean from the start.

---

**Practical starting point for a new practitioner:**

1. Create one git repo: `my-skills`
2. Add a `skills/` folder
3. Put your first skill in it
4. Commit and push

Start simple. One repo, one skill. Grow from there.

---

**When to split into multiple repos:**

Split when:
- Skills for work and personal skills need different access rights
- A team wants its own repo with its own license and governance
- A project needs skills that should not travel with you when you leave

Keep together when:
- The skills are all yours, personal, cross-domain
- You want one place to look

---

## Senior Prompt — Enterprise amount of skills, context limit, divide and conquer

### Prompt

> Enterprise amount of skills: how do we solve not overflowing an agent in a session?  
> How do we divide and conquer?  
> Context Limit — similar to: Developer S works, then summarizes, then instructs Developer J.  
> Developer J overtakes, works, summarizes, then instructs Developer S.  
> Developer S overtakes. Describe the overtake loop.

---

### Agent Answer

*(Skill `new-work-with-skills-en` activated — reading SKILL.md + references/levels.md + references/special-techniques.md → Level 1 context pattern + Level 5b evolving state + enterprise extension)*

This is the most important operational challenge at enterprise scale.  
The answer lives at the intersection of three things: **Skill structure**, **Evolving State**, and the **Overtake Loop**.

---

### The Problem at Enterprise Scale

An enterprise skill library may have hundreds of skills across dozens of repositories. A single agent session has a finite context window. You cannot load everything. You must be deliberate about what enters context and when.

The same problem applies to long-running work that spans multiple sessions or multiple agents. Knowledge must be handed off cleanly — or it is lost.

---

### Part 1 — Skill Discovery Without Overflow

The 3-step progressive disclosure pattern (Level 1) is your first line of defense:

| Step | What enters context | Token cost |
|------|-------------------|------------|
| Discovery | `name` + `description` of ALL skills | ~100 tokens × N skills |
| Activation | Full `SKILL.md` of the ONE matched skill | < 5000 tokens |
| Execution | Only the referenced files actually needed | On demand |

At 100 skills: discovery costs ~10,000 tokens — manageable.  
At 1,000 skills: discovery costs ~100,000 tokens — you need to segment.

**Solution for large libraries — Skill Repositories as Domains:**

Split skills across multiple focused repositories. Load only the domain repo relevant to the current task:

```
enterprise-skills/
├── infra-skills/          ← loaded for infrastructure work
├── testing-skills/        ← loaded for test automation work
├── architecture-skills/   ← loaded for architecture decisions
└── process-skills/        ← loaded for workflow and governance
```

An agent starts a session by loading only the repo matching the task domain.  
This keeps discovery context bounded and relevant.

---

### Part 2 — The Overtake Loop (Divide and Conquer across context limits)

When a task exceeds what a single agent session can hold, use the **Overtake Loop** — inspired by how senior developers hand off work to each other in long-running projects.

---

#### The Overtake Loop — Described

```
┌─────────────────────────────────────────────────────────┐
│                   OVERTAKE LOOP                         │
│                                                         │
│  Developer S (Agent Session S)                          │
│  ┌──────────────────────────────┐                       │
│  │ 1. Read Handover-State.md    │                       │
│  │ 2. Read relevant Skills      │                       │
│  │ 3. WORK (until context ~80%) │                       │
│  │ 4. Write new Handover-State  │                       │
│  │ 5. Write Instruction for J   │                       │
│  └──────────────────────────────┘                       │
│               ↓ handover file                           │
│  Developer J (Agent Session J)                          │
│  ┌──────────────────────────────┐                       │
│  │ 1. Read Handover-State.md    │                       │
│  │ 2. Read relevant Skills      │                       │
│  │ 3. WORK (until context ~80%) │                       │
│  │ 4. Write new Handover-State  │                       │
│  │ 5. Write Instruction for S   │                       │
│  └──────────────────────────────┘                       │
│               ↓ handover file                           │
│  Developer S overtakes again → loop continues           │
└─────────────────────────────────────────────────────────┘
```

---

#### The Handover State File

The Handover State file is the single artifact that carries knowledge across the context boundary.  
It is a structured `.md` file written by the outgoing agent, read by the incoming agent.

**Handover-State.md structure:**

```markdown
# Handover State
**Session:** S-3  
**Date:** 2026-03-10 14:30  
**Outgoing agent:** S  
**Incoming agent:** J

## What was the goal
<one paragraph — the full task objective, unchanged>

## What has been done
<bulleted list — completed steps, decisions made, files changed>

## What is the current state
<description of the codebase / document / system right now>
<include relevant file names and their purpose>

## What was decided and why
<key decisions made, with reasoning — so J does not re-debate them>

## What is still open
<bulleted list — remaining tasks, ordered by priority>

## Known blockers or risks
<anything J needs to watch out for>

## Instruction for incoming agent J
<explicit, direct instruction: "Start by doing X, then Y, stop before Z">

## Skills to load for this session
<list the exact skill names J should activate>
```

---

#### The Overtake Loop — Step by Step

**Session S — work phase:**

1. Read `Handover-State.md` (or start fresh if first session)
2. Load only the skills listed in the state file
3. Work on the open items
4. When context reaches ~80% full — **stop working**
5. Write a new `Handover-State.md` with updated sections
6. Write explicit instruction for J: what to do next, what skills to load
7. Close session

**Session J — overtake:**

1. Read the `Handover-State.md` left by S
2. Load the skills listed
3. Work on the open items
4. When context reaches ~80% — **stop working**
5. Write a new `Handover-State.md`
6. Write explicit instruction for S: what to do next
7. Close session

**S overtakes again → loop until the task is complete.**

---

#### Why stop at ~80% context?

Because an agent approaching 100% context begins to degrade — it loses track of early instructions, makes contradictions, and produces lower quality work. Stopping at 80% and handing over cleanly is always better than pushing to 100% and degrading.

---

#### The Skill that implements this loop

This pattern is a natural candidate for a skill:

```
overtake-loop-en/
├── SKILL.md          ← describes the loop, the handover format, the stop rule
└── assets/
    └── handover-state-template.md   ← blank template S and J both use
```

The skill instructs the agent: when to stop, how to write the handover, what to include, which skills to carry forward. The agent does not need to invent the protocol — the skill defines it.

---

#### Relationship to Evolving State (Level 5b)

The Overtake Loop is the **multi-agent, multi-session extension** of the Evolving State pattern (Level 5b).

| Pattern | Scope | Who loops |
|---------|-------|-----------|
| Evolving State (5b) | Single session, single agent | One agent iterates on a state file |
| Overtake Loop | Multi-session, multi-agent | S and J alternate, each reads and writes the state |

The Handover State file IS the evolving state file — it just crosses a session and potentially an agent boundary.

---

#### Enterprise summary

| Challenge | Solution |
|-----------|----------|
| Too many skills to load | Domain repos — load only the relevant repo per session |
| Context overflow mid-task | Overtake Loop — stop at 80%, write handover, switch agent |
| Knowledge lost between sessions | Handover-State.md — structured, versioned, explicit |
| Agent forgets early instructions | Stop before degradation, hand over cleanly |
| Inconsistent handover quality | Write an `overtake-loop-en` skill — the protocol IS the skill |

---

*This document is part of the `roebi/new-work-skills` repository.*  
*Skill: `new-work-with-skills-en` v2.0 — Author: Robert Halter, Switzerland*  
*License: CC BY-NC-SA 4.0*
