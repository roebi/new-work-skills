---
compatibility: Requires only a chat interface. No external tools
  required.
description: |
  Guides a user and a chat agent through a structured monthly review
  focused on the benefits gained from using the agent. Use this skill at
  the end of each month when the user wants to reflect on value created,
  time saved, friction reduced, knowledge gained, or next improvements.
  Activate for phrases like: "monthly review", "agent benefit review",
  "did this help me", "value of the agent this month", "retrospective
  with the agent", or "end of month review".
license: CC BY-NC-SA 4.0
metadata:
  author: roebi
  spec: "https://agentskills.io/specification"
name: agent-benefit-review-en
---

# Agent Benefit Review

A structured monthly conversation between user and chat agent.

Purpose: - Reflect on the concrete benefit created by using the agent. -
Identify what worked well. - Capture missed opportunities. - Decide one
improvement for next month. - Agree the next review date. - Create a
reusable markdown archive.

The goal is practical clarity.

Keep the tone calm and concise.

The review should take about 10--15 minutes.

------------------------------------------------------------------------

## Session setup

Before the review begins capture both names.

Agent asks:

"Before we begin:

What is your name?"

Wait.

Then ask:

"What name should I use for me, the Agent in this review?"

Examples: - `<Agent Name>`{=html} - Software Architect - Software Developer

Store:

User Name: `<User Name>`{=html}\
Agent Name: `<Agent Name>`{=html}\
Date: `<YYYY-MM-DD>`{=html}\
Month Reviewed: `<YYYY-MM>`{=html}

Then continue:

"Thanks.

Monthly review for `<Month Reviewed>`{=html}.

Participants: - User: `<User Name>`{=html} - Agent:
`<Agent Name>`{=html}

Goal: understand what benefit was created through working together this
month.

We go step by step."

------------------------------------------------------------------------

## When to use this skill

Activate when: - The calendar month ends - The user wants a
retrospective - The user asks whether the agent is worth using - The
user wants measurable benefit - The user wants to improve collaboration

Trigger phrases: - monthly review - end of month - benefit review -
agent retrospective - was the agent useful - value created this month

------------------------------------------------------------------------

## Conversation principles

The agent should: - Ask one question at a time - Wait for answers -
Summarize briefly after each section - Prefer concrete examples - Ask
for numbers when available - End with one actionable improvement

------------------------------------------------------------------------

## 1. Opening

Ask:

"Where did I help you most this month?"

Summarize briefly.

------------------------------------------------------------------------

## 2. Concrete wins

Ask: - What is one thing you completed faster? - What is one thing you
completed better? - What might not have started without the agent?

Summarize top wins.

------------------------------------------------------------------------

## 3. Time and effort

Remark: - Remember here that both know that bekoming a best trimmed Agent having Skills use time.

------------------------------------------------------------------------

## 4. Quality and confidence

Ask: - Did i as Agent ask for Requirements akively, or do i just fall in this low performance loop to not ask about requirements and give fast output. In other Words: Garbage In - Garbage out. Did output quality improve if i have better Requirements? - Did confidence improve?

Summarize.

------------------------------------------------------------------------

## 5. Learning and growth

Ask: - What did you learn? - Did the agent help you think differently? -
Which skill improved?

Summarize.

------------------------------------------------------------------------

## 6. Friction and missed opportunities

Ask: - What should it do better? - Where did you forget to use it?

Summarize.

------------------------------------------------------------------------

## 7. Value score

Ask:

"Goes the relavive value up, same or down by using the agent this month compared to last moth?"

Then: - Why that relative value? - What give value up next month?

------------------------------------------------------------------------

## 8. One improvement for next month

Ask:

"What is one specific way we should work differently next month?"

Capture one improvement. And save it in Memory.

------------------------------------------------------------------------

## 9. Final summary

Create:

# YYYY-MM-DD Monthly Agent Benefit Review --- Summary

Date: `<YYYY-MM-DD>`{=html}\
Month reviewed: `<YYYY-MM>`{=html}

Participants: - User: `<User Name>`{=html} - Agent:
`<Agent Name>`{=html}

Top wins: - ...

Used time remark: - ...

Quality improvements: - ...

Learning: - ...

Friction: - ...

Value score: - ... of Values up, same, down

Next month improvement: - ...

------------------------------------------------------------------------

## 10. Next appointment

Before export ask:

"Our review is complete.

Before I write the markdown file:

let us schedule the next end-of-month review.

Suggested next appointment: `<YYYY-MM-last-day>`{=html}

Do you agree?"

If no:

Ask:

"What date should we use?"

Store in Memory, may update existing Memory Entry:

Next appointment: `<chosen date>`{=html}\
Status: agreed

Confirm:

"Confirmed.

Next monthly review: `<User Name>`{=html} + `<Agent Name>`{=html}

Date: `<chosen date>`{=html}"

------------------------------------------------------------------------

## 11. Export

Only after the next appointment is agreed.

Generate one markdown file.

Filename format:

YYYYMMDD\_`<Agent-Name>`{=html}\_`<User-Name>`{=html}\_Benefit_Review.md

containing

Rules: - Date numeric - Spaces become hyphens - Only letters, digits,
hyphen, underscore - Keep `.md`

The markdown file must contain: - title - date - month reviewed -
participants - summary - value score - next month improvement - next
appointment

Final message:

"Your monthly review is complete.

Next appointment agreed: `<chosen date>`{=html}

Markdown file ready:

`<filename>`{=html}"

------------------------------------------------------------------------

## Optional long-term tracking

  Month   Top benefit   Score   Improvement
  ------- ------------- ------- -------------
  Jan                           
  Feb                           
  Mar                           

Useful questions: - Is value increasing? - Which workflows repeat? -
Where is the best ROI? - Which friction stays unresolved?
