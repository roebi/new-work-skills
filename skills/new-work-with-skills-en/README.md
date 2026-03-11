# new-work-skills

> **Your Prompt in combination with an Agent** is the new unit of work.

A curated collection of Agent Skills that teach and implement the **New Work with Skills** methodology — a practical framework for capturing, organizing, and continuously improving work using the [Agent Skills](https://agentskills.io) open standard.

---

## What is this repo?

`new-work-skills` is the home of skills that describe *how to work with skills* — a self-referential, living knowledge base.

The methodology is organized in levels, from first contact with the skills concept all the way to advanced patterns like role-as-skill, evolving state, and test-driven skill workflows.

---

## Skills in this repo

| Skill | Description |
|-------|-------------|
| [`new-work-with-skills-en`](skills/new-work-with-skills-en/SKILL.md) | The full New Work with Skills framework — 5 levels from beginner to advanced |

*More skills will be added as the methodology evolves.*

---

## Repository structure

```
new-work-skills/
├── README.md
└── skills/                          ← flat folder, one subfolder per skill
    └── new-work-with-skills-en/
        ├── SKILL.md                 ← main skill instructions
        └── references/
            ├── levels.md            ← full level detail
            └── special-techniques.md ← Level 5 quick reference
```

> **Rule:** Only a flat `skills/` folder is used. No nested skill folders.

---

## The 5 Levels

### Level 1 — Understand Skills and Use Skills
A HowTo for Humans is a Skill for Agents and Humans.  
Learn what a Skill is, how Skill Trees work, and how to read skills in 3 steps to manage context efficiently.

### Level 2 — Save Single Work as a Skill
Stop losing your sessions.  
At the end of any working session, use a single prompt to capture everything as a downloadable, reusable Skill zip.

### Level 3 — Organize Your Skills
Store skills in git repositories, use a flat `skills/` folder, group freely, and license your skills.

### Level 4 — Continuous Skill Improvement
Work in your project repo with your skills in a parallel folder.  
Update and extend skills live, in context, while the problem is fresh.

### Level 5 — Skill with Special Techniques
- **5a — Skill is a Role:** A Skill that acts as Mentor, Safeguard, or Process Guard. The Agent becomes pure EVA execution.
- **5b — Skill Having Evolving State:** A Skill that reads and writes a state file in a loop until expectations are met.
- **5c — Skill Until Requirement is Fulfilled:** Test-Driven Development implemented with Skills.

### Level ∞ — This is the End of the Beginning
*To be continued…*

---

## How to use these skills

### Option A — Download and install

1. Download the `.zip` from [Releases](../../releases) or directly from this repo
2. Unzip and place the skill folder in your local skills library:
   ```
   my-local-skills/
   └── skills/
       └── new-work-with-skills-en/
           └── SKILL.md
   ```
3. Point your agent at the `skills/` folder

### Option B — Reference directly

Reference the raw `SKILL.md` URL in your agent prompt:
```
Use the skill at:
https://github.com/roebi/new-work-skills/blob/main/skills/new-work-with-skills-en/SKILL.md
```

### Option C — Clone

```bash
git clone https://github.com/roebi/new-work-skills.git
```

---

## Related repos

| Repo | Purpose |
|------|---------|
| [roebi/agent-skills](https://github.com/roebi/agent-skills) | Core agent skill tooling and the `create-agent-skill-en` skill |
| [roebi/awesome-agent-skills](https://github.com/roebi/awesome-agent-skills) | Curated list of agent skills |
| [agentskills/agentskills](https://github.com/agentskills/agentskills) | The agentskills.io open standard and reference library |

---

## Skill specification

All skills in this repo follow the [agentskills.io specification](https://agentskills.io/specification):
- YAML frontmatter with `name` and `description`
- Markdown body with step-by-step instructions
- Progressive disclosure: metadata → SKILL.md body → references/scripts/assets on demand
- Validated with `skills-ref validate`

---

## License

Skills in this repository are licensed as specified in each skill's frontmatter `license` field.

`new-work-with-skills-en`: **All rights reserved.**  
Author: Robert Halter, Switzerland  
Version 2.0 — 2026-03-10

For permissions beyond this license, contact the author.

---

## Author

**Robert Halter** — Switzerland  
GitHub: [@roebi](https://github.com/roebi)

*New Work with Skills is a living framework. Expect new levels, new skills, and new patterns as the methodology evolves.*
