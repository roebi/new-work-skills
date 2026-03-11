# Special Techniques Reference

Quick-access reference for Level 5 techniques.  
For background and context, see [levels.md](levels.md).

---

## Technique A — Skill is a Role

**Prompt template to create a Role Skill:**

```
Write a Skill named <role-skill-name> that acts as a <role-type>.
It should <describe mentor/safeguard/process-guard behavior>.
The Skill uses <list sub-Skills if any>.
```

**Role types:** Mentor | Safeguard | Process Guard

**Agent mode when using a Role Skill:** EVA (Execute → Validate → Advance)

---

## Technique B — Skill Having Evolving State

**State file naming convention:**
```
<topic>-state-v1.md
<topic>-state-v2.md
...
<topic>-state-vN.md
```

**Skill instruction block to include:**
```markdown
## Evolving State Instructions
1. Read prompt + current state file
2. Process according to skill instructions
3. Write new state file (increment version)
4. Report delta + next suggested action
```

---

## Technique C — Skill Until Requirement is Fulfilled

**Requirement checklist block to include in any Skill:**
```markdown
## Completion Requirements
- [ ] <Requirement 1>
- [ ] <Requirement 2>
Loop until all checked. Only then output final result.
```

---

## Combining Techniques

```
Role Skill (A)
  └── uses Evolving State (B)
        └── loops until Requirements fulfilled (C)
```

This is the most powerful combination: a Role guards the process, state evolves across iterations, and the loop exits only when all requirements are met.

---

**Author:** Robert Halter, Switzerland | Version 2.0 | 2026-03-10
