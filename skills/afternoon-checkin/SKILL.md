---
name: afternoon-checkin
description: Afternoon check-in routine for Jon's Obsidian life vault. Reads today's daily note and weekly note, then outputs a focused summary of what still needs to get done today, what might have been forgotten, and only flags upcoming calendar items if action is required this afternoon. Does NOT update any notes — output only. Use when the user says "afternoon check-in", "afternoon review", or "midday check-in".
---

# Afternoon Check-In

## Steps

1. Read today's daily note: `Daily/YYYY-MM-DD.md`
2. Read this week's weekly note: `Weekly/YYYY-WXX.md`
3. Output the afternoon review — **do not edit any files**

## Output Format

### Still Needs to Get Done Today
- Pull from "Get done today" and any open action items in the daily note
- Note status if mentioned (e.g. "in testing", "waiting on X")
- Include personal/financial tasks (bills, errands) — these slip easily

### Likely Forgotten / Easy to Miss
- If current time is before 13:00: mention any lunch plans or leftovers noted for today
- Food that needs to be cooked tonight
- Checks, payments, or deliveries being watched for
- Bible reading streak — is it done yet?
- Anything promised to someone else

### Later Today (Only If Action Required Now)
- Only include if something happening later today needs prep *right now*
- Skip all future days — tomorrow, this weekend, next week are out of scope
- If afternoon meetings were noted as cancelled in the daily note, treat that block as free time

## Rules
- Output only — never write to or update any notes
- Keep it tight — this is a quick scan, not a full replanning session
- Don't surface the full calendar; only what's actionable this afternoon
- Check if cancelled meetings were noted and adjust accordingly
