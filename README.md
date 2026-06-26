# Meeting Briefing Loop

A daily AI briefing that reads your meetings, calendar, and Slack to surface what you need to know before your first call.

## What it does

Every morning (via cron), the loop:
1. Reads Fathom transcripts from the past 24 hours
2. Pulls your calendar for the day
3. Scans Slack channels you care about
4. Produces a structured briefing: decisions made, action items, who needs follow-up, what's coming today

## Output example

```
BRIEFING — June 26

DECISIONS MADE YESTERDAY
- Platform team: confirmed rollback of staging deploy pending QA
- Kevin: approved Q3 CX headcount request

ACTION ITEMS OPEN (assigned to you)
- Follow up with Jenny on AI wishlist before Thursday session
- Send Ryan the coverage plan for July PTO

TODAY
- 10am: CX Builders kickoff — Jenny + Kevin + Chad
- 2pm: Eric 1:1

WHAT TO WATCH
- Platform release delayed — affects 3 customer onboarding calls this week
```

## Setup

### 1. What you need

- [Fathom](https://fathom.video) account (for meeting transcripts)
- Slack API token with read access to your channels
- A cron-capable machine (Mac with launchd, Linux with cron, or any server)
- Claude API key

### 2. Configure

Copy `.env.example` to `.env` and fill in:

```env
ANTHROPIC_API_KEY=your_key
FATHOM_API_KEY=your_fathom_key
SLACK_USER_TOKEN=your_slack_token
SLACK_CHANNELS=C0ABC123,C0DEF456   # channel IDs to monitor
TELEGRAM_BOT_TOKEN=optional        # to receive briefing on Telegram
TELEGRAM_CHAT_ID=optional
```

### 3. Run

```bash
# Run once
python3 briefing.py

# Schedule daily at 7am
# Add to crontab: 0 7 * * * /path/to/python3 /path/to/briefing.py
```

### 4. Make it yours

Edit `briefing_prompt.py` to change:
- Which sources to include
- What goes in each section
- How long it is
- Where it's delivered (email, Slack, Telegram)

## The pattern

This is an example of a **monitoring loop** — a loop that runs with frequency, reads a set of sources, and produces a structured output.

The same pattern works for:
- Customer health briefing (reads CRM + support tickets + usage data)
- Engineering standup prep (reads GitHub + Jira + deploy logs)
- Weekly leadership digest (reads team activity across all functions)

## Related

- [prd-loops](https://github.com/kaykas/prd-loops) — turn a problem into a spec
- [executive-coach-platform](https://github.com/kaykas/executive-coach-platform) — coaching from your meetings
- [content-pipeline-loop](https://github.com/kaykas/content-pipeline-loop) — weekly content from team activity
