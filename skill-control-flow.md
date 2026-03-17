# Skill Control Flow & Agentic Primitives

Skills are not just one-shot commands.
Combined with shell primitives, they become autonomous agents.
All of this runs via `/run` — no framework needed.

---

## Group 1 — Event Sources
*How does the agent wake up?*

### Heartbeat (cron)

A skill triggered on a time schedule.
The shell already has this: `cron` / `watch` / `sleep` loops.

```bash
# run aider-skills every hour via cron
0 * * * * aider --no-git \
  --read $(aider-skills tmpfile ~/skills) \
  --message "check project health and write report" \
  > ~/reports/$(date +"%Y%m%d_%H%M%S")_health.md
```

Or inside a skill:

```markdown
---
name: hourly-health-check
description: Run a project health check every hour and save a report
---
Schedule this skill using cron or run it in a heartbeat loop:
```bash
watch -n 3600 aider --no-git --message "check health" > report.md
```
```

**Use case:** nightly build reports, daily dependency audits, weekly summaries.

---

### Polling

Repeatedly check a condition until something changes.
Pure shell — no webhook server needed.

```bash
# poll a file for changes every 10 seconds
while true; do
  if [ -f /tmp/trigger.flag ]; then
    rm /tmp/trigger.flag
    aider --no-git \
      --read $(aider-skills tmpfile ~/skills) \
      --message "trigger file detected — process it"
    break
  fi
  sleep 10
done
```

Polling a REST API:

```bash
# poll until a CI job completes
while true; do
  STATUS=$(curl -s https://api.example.com/build/status | jq -r .status)
  if [ "$STATUS" = "success" ] || [ "$STATUS" = "failed" ]; then
    aider --no-git --message "CI finished with status: $STATUS — what should we do?"
    break
  fi
  sleep 30
done
```

**Use case:** wait for a PR to merge, wait for a deploy to finish, watch a log file for an error.

---

### Webhooks (become an event)

A webhook is an HTTP POST sent by an external system when something happens.
Run a minimal listener with `nc` or Python — no framework needed.

```bash
# minimal webhook listener on port 9000
while true; do
  REQUEST=$(nc -l -p 9000 -q 1)
  BODY=$(echo "$REQUEST" | tail -n 1)
  aider --no-git \
    --read $(aider-skills tmpfile ~/skills) \
    --message "webhook received: $BODY — analyse and respond"
done
```

Or with Python (one-liner):

```bash
python3 -c "
import http.server, subprocess
class H(http.server.BaseHTTPRequestHandler):
    def do_POST(self):
        body = self.rfile.read(int(self.headers['Content-Length'])).decode()
        subprocess.run(['aider','--no-git','--message', f'webhook: {body}'])
        self.send_response(200); self.end_headers()
http.server.HTTPServer(('',9000),H).serve_forever()
"
```

**Use case:** GitHub push event triggers a code review skill, Stripe payment triggers an invoice skill, monitoring alert triggers a diagnosis skill.

---

## Group 2 — The Agentic Loop
*How does the agent keep running?*

### While / Repeat Until

The fundamental agentic pattern: run until a goal is reached.

```bash
# agentic loop: keep improving until tests pass
MAX=5; COUNT=0
while [ $COUNT -lt $MAX ]; do
  COUNT=$((COUNT + 1))
  echo "==> Attempt $COUNT"

  aider --message "run tests, fix any failures, stop when all pass"

  # check exit condition
  if pytest --tb=no -q 2>&1 | grep -q "passed"; then
    echo "==> All tests pass after $COUNT attempt(s)"
    break
  fi

  echo "==> Tests still failing, retrying..."
done
```

As a skill:

```markdown
---
name: fix-until-green
description: Repeatedly attempt to fix failing tests until all pass or max attempts reached
---
Run the agentic fix loop:
```bash
MAX=5; COUNT=0
while [ $COUNT -lt $MAX ]; do
  COUNT=$((COUNT+1))
  aider --message "fix failing tests"
  pytest --tb=no -q && break
  sleep 2
done
```
Stop when pytest exits 0. Maximum 5 attempts.
```

**Use case:** self-healing code, retry on flaky tests, iterate until a document meets a quality threshold.

---

## Group 3 — Control Structures
*How does the agent decide what to do?*

### If / Else

Branch based on a condition.

```bash
# if the branch is main, run a full review; else run a quick lint
BRANCH=$(git branch --show-current)
if [ "$BRANCH" = "main" ]; then
  aider --no-git --message "run full code review and security audit"
else
  aider --no-git --message "run quick lint check only"
fi
```

In a skill:

```markdown
---
name: smart-review
description: Full review on main branch, quick lint on feature branches
---
Check current branch and apply the right review level:
```bash
BRANCH=$(git branch --show-current)
if [ "$BRANCH" = "main" ]; then
  aider --message "full review and security audit"
else
  aider --message "quick lint only"
fi
```
```

---

### For Loop

Iterate over a list of items.

```bash
# run a skill on every Python file that changed in the last commit
for FILE in $(git diff --name-only HEAD~1 HEAD -- '*.py'); do
  aider --no-git \
    --read "$FILE" \
    --message "review this file for code quality issues"
done
```

Process multiple services:

```bash
for SERVICE in auth payments notifications; do
  aider --no-git \
    --message "check $SERVICE service health and summarise" \
    >> health-report.md
done
```

---

### Switch / Case

Route to different skills based on a value.

```bash
EVENT_TYPE=$(cat /tmp/event.json | jq -r .type)

case $EVENT_TYPE in
  "pull_request")
    aider --no-git --read $(aider-skills tmpfile ~/skills/pr-review) \
      --message "review this pull request"
    ;;
  "deployment")
    aider --no-git --read $(aider-skills tmpfile ~/skills/deploy-check) \
      --message "verify deployment health"
    ;;
  "alert")
    aider --no-git --read $(aider-skills tmpfile ~/skills/incident) \
      --message "diagnose and suggest fix for this alert"
    ;;
  *)
    echo "Unknown event type: $EVENT_TYPE"
    ;;
esac
```

---

## Group 4 — Putting It All Together

A complete autonomous agent combining all primitives:

```bash
#!/usr/bin/env bash
# agent.sh — a production-style agentic loop
#
# Wakes on webhook → routes by event type → loops until resolved

SKILLS=$(aider-skills tmpfile ~/skills)

# Event source: webhook listener
while true; do
  EVENT=$(nc -l -p 9000 -q 1 | tail -n 1)
  TYPE=$(echo "$EVENT" | jq -r .type)
  PAYLOAD=$(echo "$EVENT" | jq -r .payload)

  echo "[$(date +"%Y%m%d_%H%M%S")] Event: $TYPE"

  # Control: route by type
  case $TYPE in
    "test_failure")
      # Agentic loop: fix until green or give up
      MAX=3; COUNT=0
      while [ $COUNT -lt $MAX ]; do
        COUNT=$((COUNT+1))
        aider --no-git --read "$SKILLS" \
          --message "fix this test failure: $PAYLOAD"
        pytest --tb=no -q && break
      done
      ;;

    "pr_opened")
      # Polling: wait for CI then review
      while true; do
        STATUS=$(curl -s "$PAYLOAD/status" | jq -r .ci)
        [ "$STATUS" = "success" ] && break
        sleep 30
      done
      aider --no-git --read "$SKILLS" \
        --message "review PR: $PAYLOAD"
      ;;

    "nightly")
      # Heartbeat: scheduled health check
      for SERVICE in auth payments notifications; do
        aider --no-git --read "$SKILLS" \
          --message "check $SERVICE health" >> nightly-report.md
      done
      ;;
  esac
done
```

---

## Summary

| Primitive | Shell construct | Use case |
|---|---|---|
| Heartbeat | `cron`, `watch`, `sleep` | Scheduled tasks |
| Polling | `while` + `sleep` + condition | Wait for external change |
| Webhook | `nc`, `python http.server` | React to external events |
| Agentic loop | `while` + exit condition | Retry until goal reached |
| If / Else | `if [ ] then else fi` | Branch on condition |
| For loop | `for x in list` | Iterate over items |
| Switch / Case | `case $VAR in` | Route by event type |

**The shell is the orchestrator.
`/run` is the bridge.
Skills are the knowledge.
The model is the reasoning engine.**

No framework. No MCP. No special runtime.
Just Unix.
