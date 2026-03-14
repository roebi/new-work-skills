# Skill Class Model — Metadata v1 Field Reference

All fields live inside the `metadata:` block of the SKILL.md frontmatter.
They extend the base agentskills.io spec without breaking it.

---

## Field: `skill-class`

**Applies to:** All classes  
**Required:** Yes, for any classified skill  
**Type:** enum  

| Value | Class |
|---|---|
| `role` | Role |
| `topic` | Topic |
| `process` | Process |
| `one-step-process` | OneStepProcess |

Skills without `skill-class` are unclassified — treated as plain base Skill (legacy or untyped).

---

## Field: `crud-verb`

**Applies to:** OneStepProcess only  
**Required:** Yes (for OneStepProcess)  
**Type:** enum  

| Value | Meaning |
|---|---|
| `create` | create, generate, scaffold, write, init |
| `read` | read, search, find, list, get, describe, show |
| `update` | update, fix, move, rename, migrate, refactor |
| `delete` | delete, remove, clean, prune |

---

## Field: `topic`

**Applies to:** OneStepProcess only  
**Required:** Yes (for OneStepProcess)  
**Type:** string — skill name of the target Topic  

Identifies which Topic this OneStepProcess operates on.

```yaml
metadata:
  skill-class: one-step-process
  crud-verb: create
  topic: agent-skill
```

---

## Field: `requires-role`

**Applies to:** Topic (optional), Process (mandatory 1:n), OneStepProcess (optional 0..1)  
**Required:** Mandatory for Process. Optional for Topic and OneStepProcess.  
**Type:** string — single role name, or comma-separated list for Process  

Declares which Role skill(s) must be loaded before this skill executes.
An agent resolves this by loading `role-<value>-en` from the skill library.

```yaml
# Single role
requires-role: sw-architect

# Multiple roles (Process only)
requires-role: sw-architect, test-automation-engineer
```

---

## Field: `subprocess-of`

**Applies to:** Process, OneStepProcess  
**Required:** No  
**Type:** string — skill name of the parent Process  

Upward navigation: declares that this skill is a subprocess of a larger Process.

```yaml
metadata:
  skill-class: one-step-process
  subprocess-of: onboarding-employee-en
```

---

## Field: `subtopic-of`

**Applies to:** Topic  
**Required:** No  
**Type:** string — skill name of the parent Topic  

Upward navigation: declares that this Topic is a subtopic of a broader Topic.

```yaml
metadata:
  skill-class: topic
  subtopic-of: software-architecture-en
```

---

## Field: `subrole-of`

**Applies to:** Role  
**Required:** No  
**Type:** string — skill name of the parent Role  

Upward navigation: declares that this Role is a specialization of a broader Role.

```yaml
metadata:
  skill-class: role
  subrole-of: role-architect-en
```

---

## Complete Examples Per Class

### Role

```yaml
---
name: role-sw-architect-en
license: CC BY-NC-SA 4.0
description: >
  Defines the Software Architect role ...
metadata:
  author: roebi
  skill-class: role
  subrole-of: role-architect-en   # optional
---
```

### Topic

```yaml
---
name: agentic-runtime-architecture-en
license: CC BY-NC-SA 4.0
description: >
  Documents the agentic runtime architecture ...
metadata:
  author: roebi
  skill-class: topic
  requires-role: sw-architect
  subtopic-of: software-architecture-en   # optional
---
```

### Process

```yaml
---
name: onboarding-employee-en
license: CC BY-NC-SA 4.0
description: >
  Multi-step process for onboarding a new employee ...
metadata:
  author: roebi
  skill-class: process
  requires-role: hr-manager, it-administrator
  subprocess-of: people-operations-en    # optional
---
```

### OneStepProcess

```yaml
---
name: create-agent-skill-en
license: CC BY-NC-SA 4.0
description: >
  Creates a new Agent Skill ...
metadata:
  author: roebi
  skill-class: one-step-process
  crud-verb: create
  topic: agent-skill
  requires-role: sw-architect    # optional
---
```
