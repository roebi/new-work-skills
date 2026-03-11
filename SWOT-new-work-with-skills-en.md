# SWOT Analysis — new-work-with-skills-en

**Subject:** The "New Work with Skills" concept and skill  
**Author of analysis:** Claude (Anthropic), requested by Robert Halter  
**Date:** 2026-03-10  
**Version of concept analyzed:** 2.0

---

## Strengths

**Captures a genuinely new mental model**  
The shift from "I write a prompt" to "I compose a Prompt + Agent + Skills" is a real paradigm change. The skill articulates this clearly and gives it a name, which helps people internalize and communicate it.

**Progressive structure — levels lower the barrier to entry**  
The level system is well-designed. A complete beginner can stop at Level 1 and get value. An expert can go all the way to Level 5c. Nobody is overwhelmed on first contact.

**Solves a real and painful problem (Level 2)**  
Session loss is one of the most frustrating parts of AI-assisted work today. The "end-of-session → Skill zip" pattern is concrete, actionable, and immediately useful. This alone makes the skill worth having.

**Skill Tree concept extends composability**  
Encouraging skills to use other skills aligns with good software design (single responsibility, composition over inheritance). This scales well as skill libraries grow.

**Evolving State pattern (Level 5b) is genuinely novel**  
Using a state file as a loop mechanism — where the skill reads and rewrites it each iteration — is an elegant answer to the statelessness problem of LLM sessions. This is a strong original contribution.

**Role-as-Skill (Level 5a) reduces agent configuration overhead**  
Encoding roles into skills rather than into system prompts or agent configs makes roles portable, versionable, and composable. EVA (Execute, Validate, Advance) is a memorable and useful abstraction.

**TDD-for-Skills (Level 5c) gives quality assurance a place in the workflow**  
Applying the test-driven mindset to skill-guided work is a natural fit that most practitioners would not think of on their own. Writing it down makes it teachable.

**Follows the agentskills.io open standard**  
Using the established specification means skills are portable across compatible agents, not locked to one tool or vendor.

---

## Weaknesses

**Level 5 assumes significant prior experience**  
The jump from Level 3–4 (organize, update) to Level 5 (roles, evolving state, TDD) is steep. A reader who is new to agents may find Level 5 abstract without worked examples.

**No concrete examples with real output**  
The skill describes patterns but does not include a sample skill that demonstrates "Skill is a Role" or "Evolving State" in action. An `examples/` or `assets/` folder with one complete worked example would significantly increase usability.

**EVA (Execute, Validate, Advance) is introduced without enough definition**  
The term is coined but not fully explained. A first-time reader may not understand what "the Agent is reduced to EVA execution" means in practice.

**The "flat skills/ folder" rule (Level 3) needs more explanation**  
It is stated as a rule but the rationale is not given. Readers may wonder why nesting is not allowed, and may resist the constraint without understanding it.

---

## Opportunities

**Grow into a full skill library for new work practitioners**  
The repo `roebi/new-work-skills` could become the reference library for this methodology — a curated, versioned collection of skills that implement every level described here.

**Build companion skills for each level**  
Each level description is itself a candidate skill:  
- `session-to-skill-en` (Level 2 automation)  
- `skill-licensor-en` (mentioned in Level 3)  
- `skill-role-template-en` (Level 5a starter)  
- `evolving-state-loop-en` (Level 5b runner)  
- `skill-tdd-en` (Level 5c generic form)

**Integration with existing roebi tooling**  
The evolving state pattern (5b) maps naturally onto the blog automation pipeline and the aider skill workflows already in use. These could serve as living demonstrations of the framework.

**The "end of the beginning" framing invites continuation**  
Level ∞ being explicitly "to be continued" positions this as a living document. Publishing it as such invites readers to watch for updates and to contribute ideas.

---

## Threats

**Agent platform fragmentation**  
Different agents (Claude, Copilot, Cursor, Aider, etc.) have varying levels of skills support. If the agentskills.io standard does not gain broad adoption, skills written today may need per-platform adaptation.

**Context window evolution makes some advice time-sensitive**  
The "care about context size" guidance is important today but may become less critical as context windows expand. The skill should be written to remain relevant even as the constraint relaxes.

**Conceptual overlap with existing frameworks**  
Terms like "Role", "State", and "TDD" already exist in software engineering with established meanings. Without clear differentiation, readers from a software background may conflate these with familiar concepts and miss the new-work-specific meaning. At the End same Concept but way more procuctive use via new-work-with-skills.

**Skill quality and consistency across contributors**  
As skill libraries grow, inconsistent quality or conflicting conventions between skills from different authors can create confusion. Without a community quality standard, skill trees may become fragile.

**The skill describes patterns more than it enforces them**  
A practitioner could read the skill and still produce poor skills — there is no built-in validation, linting, or feedback loop described for the skill itself (though `skills-ref validate` exists externally).

---

## Summary Verdict

The concept is **strong and timely**. It addresses real problems (session loss, knowledge capture, composability, quality assurance) with practical, well-structured patterns. The main gap is the absence of worked examples at Level 5, and the restrictive license may limit the reach the framework deserves. The evolving-state and role-as-skill patterns in particular are original contributions worth developing further.

---

*This analysis covers both the skill document and the underlying methodology it describes.*
