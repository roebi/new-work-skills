---
name: new-work-with-skills-en
description: Teaches the new thinking of how to do new work with Agent Skills. Use this skill when the user wants to understand how to organize work using skills, how to save sessions as skills, how to use skill trees, how to continuously improve skills, how to use skills as roles, how to use evolving state with skills, or how to apply test-driven thinking to skill workflows. Trigger when the user asks about new work methods, agent skills workflows, skill organization, session capture, evolving state, or skill-based role patterns.
license: CC-BY-NC-SA-4.0
metadata:
  author: roebi, Robert Halter, Switzerland
  version: "1.0"
  date: "2026-03-10"
  repo: roebi/new-work-skills
---

# New Work with Skills

> **Your Prompt in combination with an Agent** is the new unit of work.

A practical guide to working with Agent Skills across increasing levels of mastery.  
See [references/levels.md](references/levels.md) for full detail on each level.

---

## Quick Map of Levels

| Level | Title | Core Idea |
|-------|-------|-----------|
| 1 | Understand & Use Skills | A HowTo for Humans is a Skill for Agents |
| 2 | Save Single Work as a Skill | Capture sessions — don't let work get lost |
| 3 | Organize Your Skills | Git repos, flat folder, license, group freely |
| 4 | Continuous Skill Improvement | Live updates while you work |
| 5 | Skill with Special Techniques | Role skills, evolving state, TDD for skills |
| ∞ | This is the End of the Beginning | To be continued… |

---

## Level 1 — Understand Skills and Use Skills

**A HowTo for Humans is a Skill for Agents and Humans.**

- What is a Skill: [https://agentskills.io/specification](https://agentskills.io/specification)
- A Skill can use other Skills → **Skill Tree**
- Care about Context Size: Read a Skill in 3 Steps  
  See: [https://agentskills.io/what-are-skills#how-skills-work](https://agentskills.io/what-are-skills#how-skills-work)

### The 3-Step Reading Pattern (Context-Efficient)
1. **Discovery** — Agent loads only `name` + `description` (~100 tokens per skill)
2. **Activation** — When task matches, agent reads full `SKILL.md` body
3. **Execution** — Agent loads referenced files or executes scripts only as needed

---

## Level 2 — Save Single Work as a Skill

**The Problem:** You worked on something. Your session ends. Your work is lost.

**Old Solution:** Save a summary as a `.md` file manually.

**New Thinking:** At the end of your session, use this prompt:

```
For this session write me a Skill named <my-new-skill> using
https://github.com/roebi/agent-skills/blob/main/skills/create-agent-skill-en/SKILL.md
Output is a Zip file ending with .zip that I can download.
It contains the new Skill and all its related files.
```

This turns every working session into a reusable, shareable, versioned Skill.

---

## Level 3 — Organize Your Skills

- Store skills in one or more **git repositories**
- In a git repo, only a **flat `skills/` folder** is allowed — no nested skill folders
- **Group** by whatever fits: your team, your projects, your company
- **License** your skills — use the `license` frontmatter field  
  → Consider writing a dedicated `skill-licensor` skill for consistent licensing

```
my-skills-repo/
└── skills/
    ├── my-skill-a/
    │   └── SKILL.md
    ├── my-skill-b/
    │   └── SKILL.md
    └── new-work-with-skills-en/
        └── SKILL.md
```

---

## Level 4 — Continuous Skill Improvement

You work in your **project git repo** using an Agent, with your skills in a **parallel folder** `<my-skills>`.

While you work, when you notice a skill needs updating or extending, use this prompt pattern:

```
Update/Extend in the Skill <my-skill> this <whatever needs to change>.
```

This keeps skills alive and evolving alongside your actual work — without interrupting your flow.

---

## Level 5 — Skill with Special Techniques

See [references/special-techniques.md](references/special-techniques.md) for full detail.

### 5a — Skill is a Role

**Old Thinking:** Agent has Roles. Agent uses Skills.

**New Work Thinking:**

> **Your Prompt + Agent** → Let write a Skill that IS a Role,  
> using its own Skill, which may use other Skills.

The Role Skill can act as:
- **Mentor** — guides the process
- **Safeguard** — validates decisions
- **Process Guard** — enforces workflow

The Agent is reduced to pure **EVA execution** (Execute, Validate, Advance).

### 5b — Skill Having Evolving State

Instruct your Skill to **output an evolving work result state** `.md` file.

Loop:
1. Skill reads your prompt + the evolving state file
2. Skill processes
3. Skill writes a new evolved state file

Repeat until the evolving work result state matches your expectations.

### 5c — Skill Until Requirement is Fulfilled

Think **Test-Driven Development** — implemented with Skills.

Write a Skill in generic form. Then combine it with:
- A simple Skill
- A Skill that is a Role (5a)
- A Skill having Evolving State (5b)

The skill loop runs until the defined requirement is fulfilled.

---

## This is the End of the Beginning

*To be continued…*

---

**Author:** Robert Halter, Switzerland  
**Version:** 2.0  
**Date:** 2026-03-10  
**License:** All rights reserved by author.
