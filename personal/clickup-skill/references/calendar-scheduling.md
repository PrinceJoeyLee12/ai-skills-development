# Calendar & Scheduling — Claw ClickUp Sub-Skill

This reference governs all calendar pulls, schedule summaries, daily/weekly/monthly views,
and schedule analysis across Joey's entire ClickUp workspace.

**Trigger phrases:** "calendar", "schedule", "what's on today", "what's due", "my tasks today",
"this week", "summary of my calendar", "what's coming up", "show me my day", "weekly view",
"what am I doing today", "agenda"

**Calendar URL:** `https://app.clickup.com/9016539972/v/c/8cpuyu4-5256`
**Timezone:** Asia/Manila (UTC+8) — all times displayed and compared in this timezone.

---

## Core Principle

The calendar summary from Claw must **match exactly** what Joey sees in his main ClickUp
calendar view. That means querying ALL spaces and lists assigned to him, not just one domain.
Every task with a due date that is assigned to Joey should appear in the summary.

---

## Complete List of Queryable Spaces & Lists

These are ALL the lists across Joey's workspace that feed into his calendar. When pulling
calendar data, query **every one of these** to ensure completeness.

### Personal (agent-personal)

| List                                     | ID                   |
| ---------------------------------------- | -------------------- |
| Personal List                            | `901604362025`       |
| Others List                              | `901604409717`       |
| Work Tasks (no dedicated space)          | `901604362284`       |
| Activity Log (folder — all lists inside) | Folder `90164060945` |

### Pickpacer (agent-pickpacer)

| List                           | ID             |
| ------------------------------ | -------------- |
| Prince Joey Lee List (General) | `901604342078` |
| Core General List              | `901604314839` |
| Website Dev List               | `901604318618` |
| Bug Tracking List              | `901604318441` |
| F&O Main List                  | `901604353987` |
| Expenses List                  | `901604354127` |
| Wages List                     | `901604388639` |
| Marketing Main List            | `901604832496` |
| Sales List                     | `901611468224` |
| HR List                        | `901605120956` |

### Running (agent-running)

| List     | ID             |
| -------- | -------------- |
| Run List | `901604361974` |

### Athletes (agent-athletes)

| List                 | ID             |
| -------------------- | -------------- |
| RunSmartWithLee List | `901604361967` |

### Family (agent-family)

| List                        | ID             |
| --------------------------- | -------------- |
| Family List                 | `901604361973` |
| Family Home Renovation List | `901604531431` |

### Wife/Love (agent-wife)

| List                 | ID             |
| -------------------- | -------------- |
| Love List            | `901604409689` |
| Apartment List       | `901604765860` |
| House - Binaliw List | `901605383401` |
| Love Financial List  | `901605680981` |

### Hardlix (agent-hardlix)

| List                          | ID             |
| ----------------------------- | -------------- |
| HardlixHardware Business List | `901605645075` |

**Total lists to query: 22**

---

## Calendar Pull Procedure

### Step-by-Step MCP Tool Chain

```
STEP 1 — Resolve Joey's Member ID
  Tool: clickup_find_member_by_name
  Input: name = "Joey" (or "Prince Joey Lee")
  Cache this ID for all subsequent queries.

STEP 2 — Determine Date Range
  Based on what Joey asked:
  - "today" → start of today 00:00 to end of today 23:59 (Manila time)
  - "tomorrow" → start of tomorrow to end of tomorrow
  - "this week" → Monday 00:00 to Sunday 23:59 (Manila time, current week)
  - "next week" → next Monday 00:00 to next Sunday 23:59
  - "this month" → 1st of month 00:00 to last day 23:59
  - Custom range → as specified by Joey
  Convert Manila time to UTC milliseconds for API calls.

STEP 3 — Query All Lists
  Tool: clickup_search
  Run a search across the workspace for tasks:
  - Assigned to Joey's member ID
  - With due dates falling within the target date range
  - Status NOT "complete" (unless Joey asks for completed tasks too)

  If clickup_search does not support full date-range + assignee filtering in
  a single call, iterate through the 22 lists above individually:

  For each list ID:
    Tool: clickup_search (filtered to that list, date range, assignee)
    Collect all matching tasks.

STEP 4 — Enrich Task Data
  For each task returned, check if it has custom field data (especially ABC Category).
  If custom fields are missing from search results:
    Tool: clickup_get_task (for each task ID)
    Extract: ABC Category, Categorize, status, start_date, due_date (with times),
             assignees, list name, custom fields (Budget, Income, Actual Spend,
             Partial Payment, Is Paid), and tags

STEP 4B — Fetch Subtasks
  For every task retrieved, check if it has subtasks.
  If yes → call clickup_get_task on the parent task with include_subtasks=true,
  or query each subtask ID returned in the task's subtask list.
  Extract from each subtask: name, status, start_date, due_date, ABC Category.
  Subtasks display indented beneath their parent task in the output.

STEP 4C — Financial Detection
  For each task (parent and subtask), check all currency fields:
    Budget (`72b72734-93dd-411f-a33f-3058b4d79875`)
    Income (`b3ddf778-72a7-4863-9eea-b11e25c4fda0`)
    Actual Spend (`82595661-6cb3-46b1-99df-e57a1ec842b6`)
    Partial Payment (`482687ed-96ad-46d7-9636-4229fcfcb87f`)
  Also scan the task name and description for currency indicators (₱, PHP, peso amounts).

  If ANY amount is detected:
  1. Ensure tag `needs budget` is present — if missing, add it via clickup_add_tag_to_task
  2. Ensure the current month tag is present (e.g., `march 2026`) — if missing, add it
  3. Ensure the Budget field is set — if the amount came from name/description but Budget
     field is empty, flag it in the output as ⚠️ Budget field not set
  4. All tags must be lowercase — validate before adding

STEP 5 — Deduplicate & Sort
  - Remove any duplicate tasks (same task ID)
  - Sort by date, then by start_date time within each date
  - Tasks without a specific time go to the end of that date's list
  - Subtasks inherit their parent's time block for sorting purposes

STEP 6 — Format & Display
  Use the appropriate output format below.
```

---

## Output Formats

### Daily View (Today / Tomorrow / Specific Date)

```
📅 [Day of Week], [Month Day, Year] — Joey's Schedule

🌅 Morning (before 12:00 PM)
• [ABC emoji] [start time – end time] - [Task Name] [status emoji]
  ↳ [Subtask Name] [subtask status emoji]
  ↳ [Subtask Name] [subtask status emoji]

🌤️ Afternoon (12:00 PM – 5:00 PM)
• [ABC emoji] [start time – end time] - [Task Name] [status emoji]
  ↳ [Subtask Name] [subtask status emoji]

🌙 Evening (after 5:00 PM)
• [ABC emoji] [start time – end time] - [Task Name] [status emoji]

📌 No Time Set
• [ABC emoji] no time - [Task Name] [status emoji]
  ↳ [Subtask Name] [subtask status emoji]

🔴 Overdue (carried over)
• [ABC emoji] [original due time or no time] - [Task Name] [status emoji]
  ↳ [Subtask Name] [subtask status emoji]

━━━━━━━━━━━━━━━━━━
Total: [N] tasks · [N] overdue · [N] high-priority (A Tasks)
```

**Task row format rules:**

- `[ABC emoji]` — ABC Category color (🔴 A Task · 🟡 B Task · 🔵 Routine · ➖ not set)
- `[start time – end time]` — use `start_date` time and `due_date` time from the task
  - If only due time is set (no start time): show `[due time]` without a range
  - If neither is set: show `no time`
  - Format: 12-hour with AM/PM in Manila time (e.g., `9:00 AM – 10:30 AM`)
- `[Task Name]` — exact task name from ClickUp
- `[status emoji]` — 🔵 to do · 🟡 in progress · 🟠 review · 🟢 complete · 🔴 blocked
- Subtasks are indented with `  ↳` under their parent task
- If a task has no subtasks, omit the `↳` lines entirely
- Financial flag: if task has a detected amount, append `💰` after the status emoji

### Weekly View

```
📅 Week of [Month Day] – [Month Day, Year]

**Monday, [Date]** ([N] tasks)
• [ABC emoji] [start time – end time] - [Task Name] [status emoji]
  ↳ [Subtask Name] [subtask status emoji]
• [ABC emoji] [start time – end time] - [Task Name] [status emoji]

**Tuesday, [Date]** ([N] tasks)
• [ABC emoji] [start time – end time] - [Task Name] [status emoji]
  ↳ [Subtask Name] [subtask status emoji]

**Wednesday, [Date]** ([N] tasks)
• [ABC emoji] no time - [Task Name] [status emoji]

[continue for each day]

━━━━━━━━━━━━━━━━━━
Week Total: [N] tasks · [N] overdue · [N] A-Tasks · [N] B-Tasks · [N] Routine

⚠️ Conflicts:
• [Day] [time]: [Task A] overlaps with [Task B]

📊 Load Distribution:
  Heaviest day: [Day] ([N] tasks)
  Lightest day: [Day] ([N] tasks)
```

### Monthly Overview

```
📅 [Month Year] — Monthly Overview

Week 1 ([date range]): [N] tasks · [N] A-Tasks
Week 2 ([date range]): [N] tasks · [N] A-Tasks
Week 3 ([date range]): [N] tasks · [N] A-Tasks
Week 4 ([date range]): [N] tasks · [N] A-Tasks
[Week 5 if applicable]

Highlights:
• [N] tasks total · [N] overdue · [N] high-priority
• Busiest week: Week [X] ([N] tasks)
• [N] financial tasks due this month (₱XXX,XXX total)

🔴 Overdue from Previous Months ([N]):
• [ABC emoji] [original due time or no time] - [Task Name] [status emoji]
  ↳ [Subtask Name] [subtask status emoji]
```

---

## Financial Detection Rule

Apply automatically during any calendar pull — no manual trigger needed.

**Detect a financial amount when:**

- Any of these custom fields are non-zero: Budget, Income, Actual Spend, Partial Payment
- The task name or description contains a currency indicator: `₱`, `PHP`, or a number
  followed by peso context (e.g., "5,000", "₱3,500", "PHP 12,000")

**When an amount is detected, Claw must:**

1. Verify the task has the tag `needs budget` — add it if missing (lowercase, always)
2. Verify the task has the current month-year tag (e.g., `march 2026`) — add if missing (lowercase)
3. Verify the `Budget` field (`72b72734-93dd-411f-a33f-3058b4d79875`) is set — if the amount
   was inferred from the task name/description but the Budget field is empty, flag it:
   `⚠️ Budget field not set — amount detected in task name`
4. Append `💰` after the status emoji in the task row to signal financial context

**Tag format rules (non-negotiable):**

- All tags lowercase: `needs budget`, `march 2026`, `routine`
- Month tag format: `[month name] [4-digit year]` → `march 2026` ✅ · `March 2026` ❌ · `mar 2026` ❌
- Never apply month tags to non-financial tasks

---

## Conflict Detection

When presenting any calendar view, automatically check for:

1. **Time overlap** — two tasks with specific times that overlap on the same day
2. **Overload** — more than 8 tasks in a single day (flag as heavy day)
3. **A-Task stacking** — more than 3 A-Tasks on the same day
4. **Empty gaps** — days with zero tasks in a work week (might indicate missing data)

Report conflicts at the bottom of the calendar view:

```
⚠️ Conflicts Detected:
• [Day]: [Task A] (9:00 AM – 10:00 AM) overlaps with [Task B] (9:30 AM – 11:00 AM)
• [Day]: Heavy load — [N] tasks scheduled
```

---

## Schedule Analysis

When Joey asks "analyze my schedule" or "am I overloaded", provide:

```
📊 Schedule Analysis — [Date Range]

Load by Domain:
• Pickpacer: [N] tasks (XX%)
• Personal: [N] tasks (XX%)
• Wife/Love: [N] tasks (XX%)
• [other domains...]

Priority Mix:
• A Tasks: [N] (XX%)
• B Tasks: [N] (XX%)
• Routine: [N] (XX%)
• Uncategorized: [N] (XX%)

⏰ Time Distribution:
• Morning tasks: [N]
• Afternoon tasks: [N]
• Evening tasks: [N]
• No time set: [N]

Recommendations:
• [Actionable insight based on data — e.g., "Tuesday has 12 tasks, consider moving 3 B-Tasks"]
• [Another insight]
```

---

## "Summary of My Calendar" — Default Behavior

When Joey says **"summary of my calendar"** without specifying a date range:

1. **Default to today** — show today's full daily view
2. **Include overdue** — always show overdue tasks carried over from past dates
3. **Include tomorrow preview** — show a brief count of tomorrow's tasks
4. **Include weekly context** — show total tasks remaining this week

```
📅 Calendar Summary — [Today's Full Date]

[Full daily view as defined above]

👀 Quick Look Ahead:
• Tomorrow: [N] tasks ([N] A-Tasks)
• Rest of this week: [N] tasks remaining
• Overdue backlog: [N] tasks need attention
```

---

## Timezone Handling

- **All API timestamps are in UTC (milliseconds)** — convert to Asia/Manila (UTC+8) for display
- **When creating/updating due dates** — convert Joey's Manila time input to UTC for the API
- **Date boundary:** A task due at `2026-03-18T16:00:00Z` (UTC) = `2026-03-19T00:00:00+08:00` (Manila) — this falls on March 19 in Joey's calendar, not March 18
- **"Today" calculation:** Current date in Manila time, not UTC
- **Week start:** Monday (ISO standard)

---

## Quick Commands Reference

| Joey Says                       | What Claw Does                                 |
| ------------------------------- | ---------------------------------------------- |
| "summary of my calendar"        | Today's full view + overdue + tomorrow preview |
| "what's on today"               | Today's daily view                             |
| "what's due this week"          | Weekly view (Mon–Sun)                          |
| "show my schedule for tomorrow" | Tomorrow's daily view                          |
| "what's coming up next week"    | Next week's view                               |
| "this month overview"           | Monthly overview with weekly rollups           |
| "am I overloaded"               | Schedule analysis with recommendations         |
| "any conflicts today"           | Conflict detection for today                   |
| "what's overdue"                | All overdue tasks across all domains           |
