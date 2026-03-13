# Example Prompt Answers — new-work-with-skills-en

Demonstrating the 3 key prompts from the skill, answered by an Agent using  
`new-work-with-skills-en` (roebi/new-work-skills).

**Skill version used:** 2.0 — 2026-03-10  
**Agent:** Claude Sonnet 4.6

---

## Example 1 — Level 2: Save Single Work as a Skill

### Prompt used

> For this session write me a Skill named `gradle-8-to-9-migration-en` using  
> https://github.com/roebi/agent-skills/blob/main/skills/create-agent-skill-en/SKILL.md  
> Output is a Zip file ending with .zip that I can download.  
> It contains the new Skill and all its related files.

*Context: the session covered migrating a multi-repo Gradle 8 project to Gradle 9 using aider and Claude Code.*

---

### Agent Output

**Skill activated:** `new-work-with-skills-en` → Level 2 pattern applied.

The session is captured as a new skill. Here is the generated `SKILL.md`:

```
gradle-8-to-9-migration-en/
├── SKILL.md
└── references/
    └── breaking-changes.md
```

**`SKILL.md` content:**

```markdown
---
name: gradle-8-to-9-migration-en
description: Migrate a Gradle 8 project to Gradle 9 across one or more Git
  repositories using aider and Claude Code. Use when the user wants to upgrade
  Gradle, fix deprecated API warnings, update wrapper files, or validate a
  Gradle 9 build. Trigger on: Gradle upgrade, Gradle 9, build migration,
  wrapper update, deprecated Gradle API.
license: CC BY-NC-SA 4.0. Author Robert Halter, Switzerland.
metadata:
  author: Robert Halter, Switzerland
  version: "1.0"
  date: "2026-03-10"
---

# Gradle 8 to 9 Migration

## When to use this skill
Use this skill when upgrading one or more Git repositories from Gradle 8 to 9.
Works best in combination with aider or Claude Code as the executing agent.

## Pre-flight Checks
1. Confirm current Gradle version: `./gradlew --version`
2. Confirm Java version compatibility (Gradle 9 requires Java 17+)
3. Commit or stash all uncommitted changes before starting

## Migration Steps

### Step 1 — Update the Gradle Wrapper
~~~bash
./gradlew wrapper --gradle-version 9.0.0
./gradlew wrapper  # apply
~~~

### Step 2 — Fix Deprecated APIs
Run the build with `--warning-mode all` and address each warning:
~~~bash
./gradlew build --warning-mode all
~~~
See [references/breaking-changes.md](references/breaking-changes.md) for
common Gradle 8 → 9 breaking changes and their fixes.

### Step 3 — Update Plugin Declarations
Replace legacy `apply plugin:` syntax with the `plugins {}` block.

### Step 4 — Validate
~~~bash
./gradlew clean build
./gradlew test
~~~
All tests must pass before the migration is considered complete.

## Multi-Repo Strategy
For multiple repos, apply Steps 1–4 to each repo in sequence.
Use a feature branch per repo: `git checkout -b gradle-9-migration`
Open a PR per repo for review.
```

**Output:** `gradle-8-to-9-migration-en.zip` — ready to download and place in your `skills/` folder.

**What happened here:** Instead of losing the migration knowledge from this session, it is now a reusable, versioned Skill. The next time this migration is needed — in another repo, by a colleague, or in 6 months — the Skill is ready.

---

## Example 2 — Level 4: Continuous Skill Improvement

### Prompt used

> Update/Extend in the Skill `gradle-8-to-9-migration-en` this:  
> Add a step for handling deprecated API warnings that come from third-party  
> plugins, not from our own code — these need a different fix strategy.

---

### Agent Output

**Skill activated:** `new-work-with-skills-en` → Level 4 pattern applied.

Reading `gradle-8-to-9-migration-en/SKILL.md`...  
Identifying insertion point: after Step 2 (Fix Deprecated APIs).

**Change applied — new section inserted:**

```markdown
### Step 2b — Deprecated APIs from Third-Party Plugins

If `--warning-mode all` shows deprecations originating from a plugin
(not your own build code), the fix strategy is different:

1. **Check for a plugin update first:**
   ~~~
   ./gradlew dependencyUpdates
   ~~~
   If a newer version of the plugin exists, update it in `build.gradle`:
   ~~~groovy
   id 'com.example.myplugin' version '2.x.x'  // update here
   ~~~

2. **If no update is available:** suppress the warning with a comment
   and open a tracking issue:
   ~~~groovy
   // TODO: suppress until plugin-name releases Gradle 9 support
   // Tracking: https://github.com/example/plugin/issues/123
   ~~~

3. **Do not patch plugin internals.** If the plugin is unmaintained and
   the deprecation becomes a build error in Gradle 9, consider replacing
   the plugin or forking it.
```

**Skill updated in place.** Version bumped from `1.0` to `1.1` in frontmatter.

**What happened here:** The improvement was made *in context*, while the real problem was fresh. No separate "skill maintenance" session needed. The skill now carries this knowledge forward for every future use.

---

## Example 3 — Level 5a: Skill is a Role

### Prompt used

> Write a Skill named `code-review-mentor` that acts as a senior code reviewer.  
> It should mentor the developer by explaining each issue found, suggest better  
> patterns, and guard that no PR is approved without addressing all critical issues.  
> The Skill uses `check-security-issues` and `check-code-style` sub-Skills.

---

### Agent Output

**Skill activated:** `new-work-with-skills-en` → Level 5a pattern applied.  
**Role type:** Mentor + Process Guard  
**Agent mode:** IPO (input – process – output)

Generated skill:

```
code-review-mentor/
└── SKILL.md
```

**`SKILL.md` content:**

```markdown
---
name: code-review-mentor
description: Acts as a senior code reviewer in a Mentor and Process Guard role.
  Explains every issue found, suggests better patterns, and enforces that no PR
  is approved until all critical issues are resolved. Uses check-security-issues
  and check-code-style sub-Skills. Trigger on: code review, PR review, pull
  request, review my code, check my implementation.
license: CC BY-NC-SA 4.0. Author Robert Halter, Switzerland.
metadata:
  author: Robert Halter, Switzerland
  version: "1.0"
  date: "2026-03-10"
---

# Code Review Mentor

> This Skill IS a Role. The Agent is reduced to IPO execution:
> input – process – output
> The Role (this Skill) mentors, safeguards, and guards the process.

## Role Definition

**I am a senior code reviewer.** My responsibilities:
- Find issues and **explain why** they are issues — not just flag them
- Suggest a better pattern for every issue found
- Enforce: no PR advances until all CRITICAL issues are resolved
- Treat every developer interaction as a teaching moment

## Step 1 — Execute: Run Sub-Skills

Invoke sub-Skills in this order:

1. Activate `check-security-issues` on the submitted code
2. Activate `check-code-style` on the submitted code
3. Collect all findings

## Step 2 — Validate: Classify Findings

For each finding, classify as:

| Severity | Definition | PR can advance? |
|----------|-----------|----------------|
| CRITICAL | Security vulnerability, data loss risk, broken contract | No |
| MAJOR | Logic error, significant performance issue, bad pattern | Only after fix or accepted trade-off |
| MINOR | Style, naming, readability | Yes, with comment |
| SUGGESTION | Better approach exists, not required | Yes |

## Step 3 — Mentor: Explain Each Finding

For every finding, output:

~~~
**[SEVERITY] — Short title**
What is wrong: <plain language explanation>
Why it matters: <consequence if not fixed>
Better pattern: <concrete suggestion or code example>
~~~

## Step 4 — Advance: Gate the PR

After presenting all findings:

- If any CRITICAL findings remain open → output:
  > "🔴 PR BLOCKED. The following critical issues must be resolved before approval: [list]"

- If no CRITICAL findings remain → output:
  > "🟢 PR may advance. Remaining items: [list MAJOR/MINOR/SUGGESTION]"

## Role Reminder

At no point does this Role skip the explanation step to save time.
Mentoring IS the work. Speed is not the goal — understanding is.
```

**What happened here:** The Agent did not *take on* a role via a system prompt. Instead, the Role lives in a Skill — portable, versioned, composable, and reusable across any session or agent that supports the agentskills.io format. The Agent simply executed IPO: followed the Skill instructions, validated outputs at each step, and advanced only when the Skill's conditions were met.

---

## Summary

| Prompt | Level | Pattern | Key outcome |
|--------|-------|---------|-------------|
| Session → Skill zip | 2 | Capture | Work is never lost again |
| Update/Extend Skill | 4 | Continuous improvement | Skill grows with real experience |
| Skill is a Role | 5a | Role + EVA | Role is portable, versionable, composable |

---

*These examples are part of the `roebi/new-work-skills` repository.*  
*Skill: `new-work-with-skills-en` v2.0 — Author: Robert Halter, Switzerland*  
*License: CC BY-NC-SA 4.0*
