---
name: pulse-check
description: "Status check across all active projects — flags risks, stalled items, and what needs attention. Use when someone says 'pulse check', 'pulse', 'project status', 'how are things going', 'what needs attention', or 'run pulse check'."
user_invocable: true
---

# Pulse Check

Check the health of all Sarthak's active projects. Pull status from git, Slack, Gmail, and memory — then flag what's on track, what's at risk, and what needs a decision.

## Active Projects

These are Sarthak's projects (hardcoded to save tokens — update this list when projects change):

| Project | Path | What to Check |
|---------|------|---------------|
| **flo-analytics-llm** | `work/flo-analytics-llm/` | SDK pipeline, audit items, Render deploy |
| **myVoiceBooksAI** | `work/myVoiceBooksAI/` | Mobile app, Netlify deploy, voice agent |
| **myBillBook** | `.claude/projects/myBillBook/` + `work/myBillBook/` | Invoice animation feature, satellite project |
| **trading-monitor** | `personal/trading-monitor/` | 6E scanner, Sierra study, backtest |
| **content-listener** | `personal/content-listener/` | VidText, Chrome extension, Kindle sender |
| **claude-paglu** | `personal/claude-paglu/` | Zero-face AI reels pipeline |

## Data Sources

1. **Memory files** — `.claude/projects/-Users-sarthak-Claude/memory/project_*.md` for last known status
2. **Git logs** — `git log --oneline -10` in each project directory for recent activity
3. **Gmail** — Search for project-related threads from last 7 days: `gmail_search_messages` with queries like `flo-analytics OR myVoiceBooksAI OR myBillBook`
4. **Slack (project keywords)** — Search relevant channels for project mentions in last 7 days
5. **Slack (people asks)** — Search for DMs and mentions TO Sarthak in last 3 days: `to:me after:<3 days ago>`. These surface action items from colleagues (data pulls, reviews, decisions) that don't mention any project name but still need tracking. Include these as a separate "People Asks" section BEFORE the follow-ups.

## Workflow

### Step 1: Gather project status (parallel)

For each project in the table above, run in parallel:

1. **Read memory**: Read the corresponding `project_*.md` memory file for last known state
2. **Git activity**: Run `git log --oneline --since="7 days ago" -10` in the project directory (skip if not a git repo)
3. **Gmail scan**: Search for project-related emails from last 7 days
4. **Slack scan (projects)**: Search for project mentions in Slack
5. **Slack scan (people asks)**: Search `to:me after:<3 days ago>` to find DMs/mentions from colleagues asking Sarthak for data, reviews, decisions, or help. Filter out bot messages and calendar notifications. Group these by person with a summary of what they asked and whether Sarthak responded.

### Step 2: Assess each project

For each project, determine:
- **Status**: Active (commits in last 7 days) / Stalled (no commits in 7+ days) / Blocked (known blocker)
- **Last activity**: Most recent commit or update
- **Open items**: From memory files — what was pending last time?
- **Risks**: Anything overdue, stalled, or needing a decision

### Step 3: Classify follow-ups

Sort all follow-up items into three tiers:
- **High priority (at risk)**: Stalled for 7+ days, approaching deadlines, blocking other work, needs a decision
- **Medium priority**: Needs attention this week but not urgent
- **Looking good**: On track, no action needed

### Step 4: Generate output

Use this EXACT format:

```
## Pulse Check -- [Full Date]

*[X] days until end of [month]. Here's where everything stands.*

**[Project Name]** -- [STATUS]
- [Key detail] -- [status tag: IN PROGRESS / STALLED / DONE / BLOCKED]
- [Key detail] -- [status tag]
- Last commit: [date] — "[commit message]"

**[Project Name]** -- [STATUS]
- [Key detail] -- [status tag]
- Last commit: [date] — "[commit message]"

[...repeat for all active projects...]

---

### People Asks (unanswered or pending)

Actionable requests from colleagues found in Slack DMs/mentions. These are NOT project-specific but still need Sarthak's attention.

- **[Person Name]** -- [What they asked] -- [DONE / PENDING / NEEDS DATA]
  - Context: [Brief summary of the conversation]
  - Action: [What Sarthak needs to do]

---

### Follow-ups Needed

**High priority (at risk)**
- **[Project/Item]** -- [What's wrong and what decision or action is needed]
- **[Project/Item]** -- [Detail]

**Medium priority**
- **[Project/Item]** -- [What needs attention this week]

**Looking good**
- **[Project/Item]** -- [Brief confirmation it's on track]

> **Bottom line:** [One-sentence overall assessment — what's healthy, what needs attention]
```

## Rules

1. **Always include all 6 projects** — even if there's nothing new, say "No activity this week" rather than omitting.
2. **Bold all project/item names** before the `--` separator.
3. **Status tags in CAPS**: IN PROGRESS, STALLED, DONE, BLOCKED, TO DO.
4. **Include last commit date** for each project that has a git repo.
5. **Days until end of month**: Calculate from today's date. If it's the last week, add urgency.
6. **Tone**: Analytical, risk-flagging, direct. Call out stalled items explicitly — "No commits in 12 days. Is this parked or forgotten?"
7. **Bottom line**: Must be one sentence. Honest assessment, not cheerleading.
8. **If Gmail/Slack MCP unavailable**: Skip gracefully, note it, continue with git + memory data.
9. **Don't read full codebases**: Only git logs, memory files, and external sources. Keep it fast.
