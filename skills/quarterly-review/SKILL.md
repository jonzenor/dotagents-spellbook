---
name: quarterly-review
description: Quarterly review skill for Jon's Obsidian vault. Looks back over the last calendar quarter using weekly notes, sampled daily notes, completed OmniFocus projects, and Google Calendar patterns. Produces a saved review note with victories, lessons, large projects, and carry-forwards into the next quarter. Use when the user says "quarterly review", "Q1 review", "review last quarter", or similar.
---

# Quarterly Review

## What this does

Generates a retrospective for the just-completed calendar quarter and saves it to `Reference/Logs/Quarterly Reference/Logs/Quarterly Reviews/YYYY-QN.md`. Draws from weekly notes, daily notes, OmniFocus completed projects, and Google Calendar.

## Step 1 — Determine the quarter

From `currentDate`, identify the most recently completed calendar quarter:
- Q1: Jan 1 – Mar 31
- Q2: Apr 1 – Jun 30
- Q3: Jul 1 – Sep 30
- Q4: Oct 1 – Dec 31

If today is the first week of a new quarter, review the quarter just ended. Establish the exact date range (e.g. 2026-01-01 – 2026-03-31 for Q1).

## Step 2 — Read all weekly notes for the quarter

Weekly notes are named `Weekly/YYYY-WXX.md` (weeks start Sunday). Identify every week that falls within the quarter date range and read each one in full. Focus on:
- `## Focus This Week` — what mattered that week
- `## Active Action Items` — what was completed (`[x]`) vs. carried
- `## Notes & Reflections` — the curated narrative of each week
- `## Mentee Check-ins This Week` — who was met with
- `## Cooking This Week` — family rhythms (signals home life stability)

## Step 3 — Sample daily notes

Do not read all daily notes. Instead read selectively:
- The last daily note of each month in the quarter (e.g. Jan 31, Feb 28, Mar 31) — these often contain reflections on the month
- Any daily note explicitly referenced in a weekly "Notes & Reflections" entry as significant
- The last daily note of the quarter itself

Look for: recurring themes, health mentions, significant relationship moments, spiritual highlights, or anything the weekly notes only touched on briefly.

## Step 4 — Query OmniFocus for completed projects

Query for projects with `status: Done` and `completionDate` within the quarter date range. Also query for completed flagged tasks within the quarter — these represent things Jon marked as important and actually finished.

This surfaces what actually shipped vs. what just got carried week to week.

## Step 5 — Query Google Calendar for the quarter

Fetch events across all three calendars (`jonzenor@gmail.com`, `family18039985066750084653@group.calendar.google.com`, `danaezenor@gmail.com`) for the full quarter date range.

Look for:
- **Recurring meeting series** that appeared frequently — a recurring meeting around a project name signals significant work investment
- **One-off significant events** — medical appointments, travel, major life events, special church services
- **Patterns in commitments** — weeks with unusually heavy calendars vs. light ones
- **Cancelled or rescheduled patterns** — if the same commitment keeps moving, that's a trend worth naming
- **Recurring events that started mid-quarter** — a new recurring event means a new rhythm was added to Jon's life; worth naming and asking about if context isn't clear
- **Recurring events that disappeared mid-quarter** — a recurring event that ran for several weeks and then stopped signals a rhythm that ended; ask Jon whether it was intentional (finished, dropped) or something that just faded

These rhythm changes are often more significant than individual one-off events. When you spot one, either surface it in the review or ask Jon about it in the clarifying questions step.

Calendar is a supporting signal, not the primary narrative. Use it to catch things the vault didn't explicitly capture.

## Step 6 — Check for an existing review note

Check if `Reference/Logs/Quarterly Reviews/YYYY-QN.md` already exists. If so, read it before writing — either update it or append to it rather than overwriting. If the `Reviews/` folder doesn't exist, it will be created when writing the file.

## Step 7 — Write the quarterly review

Save to `Reference/Logs/Quarterly Reviews/YYYY-QN.md` (e.g. `Reviews/2026-Q1.md`).

---

## Output format

```markdown
# YYYY QN Review

*[Quarter date range — e.g. January 1 – March 31, 2026]*

## The Quarter in a Sentence

[1–2 honest sentences. What kind of quarter was it? What was the dominant theme or tone?]

## Victories

- [A real win — something completed, accomplished, or grown into. Be specific.]
- [Include both work wins and personal/spiritual/relational wins]
- [Completed OmniFocus projects and flagged tasks belong here]

## Large Projects

| Project | Status | Notes |
|---------|--------|-------|
| [Project name] | Completed / In progress | [Brief context — what it was, why it mattered] |

*Pull from completed OmniFocus projects and recurring calendar meeting series that indicate significant work investment.*

## Lessons Learned

- **[Theme]** — [What happened, what it taught, how it should change behavior going forward]
- [Only include genuine lessons — things that actually changed or should change how Jon operates]

## People & Relationships

- **Mentoring** — [Who was met with consistently, what growth was observed, what needs attention]
- **Family** — [Significant moments with Danae, Malikai; relationship health]
- **Community** — [Lighthouse, church, friendships — any notable developments]

## Faith & Spiritual Life

- **Bible reading** — [Patterns observed across the quarter — consistency, books studied, insights]
- **Prayer** — [Anything notable in the prayer life]
- **Community** — [Small group, Bible study, church highlights]

## What Kept Sliding

- [Things that appeared week after week in action items but never got done — name them honestly]
- [These are candidates for either a hard commitment in Q2 or a deliberate decision to drop]

## Trends to Watch in Q[N+1]

- **[Theme]** — [Pattern observed across the quarter. Why it matters. What to do differently or protect going forward.]

## Carry Into Q[N+1]

- [Unfinished projects or goals that should continue — with context on where they stand]
- [Relationships that need intentional attention]
- [Admin or financial items still outstanding]
- [One or two aspirations worth setting as Q[N+1] priorities]
```

---

## Section guidance

- **The Quarter in a Sentence**: Don't summarize the sections below — capture the *feeling* and *arc* of the quarter. Was it a rebuilding quarter? A momentum quarter? A survival quarter?
- **Victories**: Cast wide — include work deliverables, personal growth, spiritual wins, relationship moments. A quarter with only work victories is an incomplete picture.
- **Large Projects**: Cross-reference OmniFocus completed projects with calendar meeting patterns. If a project consumed many weekly meetings, it belongs here even if the OmniFocus project isn't marked done yet.
- **What Kept Sliding**: This is the most important diagnostic section. If the same item appeared in 8 of 13 weekly action lists, name it. Don't soften it. The pattern is the message.
- **Lessons Learned**: A lesson worth keeping changes future behavior. Skip insights that sound wise but don't connect to a specific decision.
- **Carry Into Q[N+1]**: Be selective — not everything unfinished belongs in the next quarter. Some things should be dropped. This section is about intentional carries, not just leftovers.

## Rules

- Read before writing — always check if a review file already exists before creating one.
- Output goes to `Reference/Logs/Quarterly Reviews/YYYY-QN.md` — create the file, don't just print to screen.
- Weekly notes are the primary source; calendar and OmniFocus are supporting signals.
- Be honest, not harsh. The purpose is clarity for the next quarter, not self-criticism.
- Omit sections that have nothing real to say — don't pad.
- Use `[[Person/Name]]` wikilink syntax when referencing people.

## Ask Before Writing

After gathering all sources, **ask Jon clarifying questions before drafting**. Do not infer a narrative from thin data — ask for it. At minimum, ask:

1. **"How would you describe the quarter in your own words?"** — Use his answer as the basis for "The Quarter in a Sentence." Do not construct this from calendar data alone.
2. **"What are you most proud of this quarter?"** — Anchors the Victories section in his actual experience.
3. **If vault data is sparse:** — "The vault only has data from [date range] — is this because you're just getting started? I don't want to fill the gaps with guesswork." The Obsidian vault was started in late March 2026; early quarters will have thin records. Sparse data ≠ a rough quarter.
4. **If a system appears to be restarting** (e.g. OmniFocus, GTD, journaling): — Ask how Jon characterizes it. A deliberate relaunch after a long gap is a victory, not a collapse.
5. **If something looks like it slid:** — Ask whether it was genuinely off-track or just didn't make it into the vault. "Taxes show up as outstanding — was that because life got disrupted, or has this been slipping for a while?"

**What NOT to do:**
- Do not fill data gaps with negative assumptions ("held together by willpower," "fell apart," etc.)
- Do not conflate sparse journaling with a difficult quarter
- Do not frame a deliberate restart as a system failure
- Do not bury a stated primary goal under caveats just because the vault doesn't confirm every detail

**Context about Jon's quarterly rhythm:**
- Jon uses a quarterly objectives system but stepped back from doing a full daily 4-quadrant battle plan (IC method) this quarter — intentionally. Having a single cooking focus this quarter was the plan, not a limitation.
- If the existing `Reference/Logs/Quarterly AARs.md` file has a self-written entry for the quarter, treat it as the most authoritative source for how the quarter went — it's Jon's own words.
