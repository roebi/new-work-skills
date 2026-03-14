# Skill Class Model — Examples

Concrete skills from `roebi/agent-skills` mapped to each class.

---

## Role examples

| skill name | subrole-of |
|---|---|
| `role-sw-architect-en` | — |
| `role-cloud-architect-en` | `role-sw-architect-en` |
| `role-test-automation-engineer-en` | — |

---

## Topic examples

| skill name | requires-role | subtopic-of |
|---|---|---|
| `agentic-runtime-architecture-en` | `sw-architect` | — |
| `agent-skill-en` | — | — |

---

## Process examples (multi-step)

| skill name | requires-role | subprocess-of |
|---|---|---|
| `onboarding-employee-en` | `hr-manager, it-administrator` | — |
| `publish-content-en` | `content-author` | — |

---

## OneStepProcess examples

| skill name | crud-verb | topic | requires-role |
|---|---|---|---|
| `create-agent-skill-en` | create | agent-skill | — |
| `create-python-project-github-en` | create | python-project-github | — |
| `fix-unclosed-fenced-block-en` | update | fenced-block | — |
| `move-skills-to-new-nameconvention-license-en` | update | skills | — |
| `search-tags-en` | read | tags | — |

---

## Naming pattern verification

| Class | Pattern | Follows? |
|---|---|---|
| Role | `role-<n>-en` | ✓ |
| Topic | `<topic>-en` | ✓ |
| Process | `<n>-en` | ✓ |
| OneStepProcess | `<crud-verb>-<topic>-en` | ✓ |
