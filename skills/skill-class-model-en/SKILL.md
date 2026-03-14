---
name: skill-class-model-en
license: CC BY-NC-SA 4.0
description: >
  Documents the Skill Class Model — the OO-based taxonomy that classifies every
  Agent Skill as one of: Role, Topic, Process, or OneStepProcess, each extending
  the base Skill class. Defines the v1 metadata extension fields for skill-class,
  crud-verb, topic, requires-role, subprocess-of, subtopic-of, and subrole-of.
  Use when an agent or author needs to classify a skill, assign metadata, understand
  the class hierarchy, or check naming conventions. Activate for phrases like:
  "what class is this skill", "how do I classify a skill", "skill taxonomy",
  "skill-class metadata", "is-a or has-a for skills", "what metadata does a Role need",
  or "naming convention for process skills".
metadata:
  author: roebi
  skill-class: topic
  requires-role: sw-architect
  spec: https://agentskills.io/specification
---

# Skill Class Model

The Skill Class Model applies object-oriented design to Agent Skills.
Every skill is an instance of a class that extends the base `Skill` class.
Classes are discriminated by the `metadata.skill-class` field (v1 extension).

## Class Hierarchy (is-a / inheritance)

```
Skill                          ← base class (agentskills.io)
├── Role                       ← extends Skill
├── Topic                      ← extends Skill
└── Process                    ← extends Skill
    └── OneStepProcess         ← extends Process
```

## Composition (has-a)

```
Topic          has  Subtopic        1:n
Topic          has  File            1:n

Role           has  Subrole         1:n

Process        needs  Role          1:n   (mandatory)
Process        has    Subprocess    1:n

OneStepProcess needs  Role          0..1  (optional)
```

**Key rule:** Process is higher than Role. A Process needs one or more Roles —
never the other way around.

## The Subject and the Verb

A `Topic` is the **subject** — the thing that gets operated on.
A `OneStepProcess` applies a **CRUD verb** to a Topic.

| crud | natural language equivalents |
|------|------------------------------|
| create | create, generate, scaffold, write, init |
| read | read, search, find, list, get, describe, show |
| update | update, fix, move, rename, migrate, refactor |
| delete | delete, remove, clean, prune |

## Naming Conventions

| Class | Pattern | Example |
|---|---|---|
| Role | `role-<name>-en` | `role-sw-architect-en` |
| Topic | `<topic>-en` or `<compound-topic>-en` | `agentic-runtime-architecture-en` |
| Process | `<name>-en` | `onboarding-employee-en` |
| OneStepProcess | `<crud-verb>-<topic>-en` | `create-agent-skill-en` |

## Metadata Fields — v1

See `references/metadata-v1.md` for the full field reference.

Quick summary:

```yaml
# All classes — discriminator (required for classified skills)
metadata:
  skill-class: role | topic | process | one-step-process

# Role adds:
  subrole-of: <role-name>           # optional

# Topic adds:
  subtopic-of: <topic-name>         # optional
  requires-role: <role-name>        # optional

# Process adds:
  requires-role: <role-name>        # 1:n comma-separated (mandatory)
  subprocess-of: <process-name>     # optional

# OneStepProcess adds:
  crud-verb: create | read | update | delete   # mandatory
  topic: <topic-name>                          # mandatory
  subprocess-of: <process-name>               # optional
  requires-role: <role-name>                  # optional 0..1
```

## Composability Chain

When an agent loads a skill it resolves the role chain automatically:

```
load skill
  → read requires-role
    → load role-<name>-en
      → agent has role context + task context
        → execute
```

## References

- `references/metadata-v1.md` — full field-by-field metadata reference
- `references/examples.md` — concrete skills mapped to each class
