# Custom Fields Reference — Claw ClickUp Skill

All custom fields across Joey's workspace. Use exact field IDs and option strings when
setting or reading these fields via MCP tools.

---

## Mandatory Fields (set on EVERY task)

### ABC Task Category

- **Field ID:** `241293c9-b8fb-4d0b-8563-7091c3c27eb7`
- **Type:** Dropdown
- **Options (exact strings):**
  - `A Task - 1` — High-value, top priority
  - `A Task - 2` — High-value, secondary
  - `B Task - 1` — Medium priority
  - `B Task - 2` — Medium, secondary
  - `routine - 1` — Recurring operations, primary
  - `routine - 2` — Recurring operations, secondary
- **Rule:** Always set on task creation. Never leave blank.
- **Season logic (March–June 2026):**
  - Pickpacer / Wife & Baby = A Task
  - Health / Running (maintenance) = B Task
  - Daily/weekly ops = routine

### Categorize

- **Field ID:** `ec149710-5cf6-42f0-b24e-3eb241182914`
- **Type:** Dropdown
- **Options (exact strings):**
  - `Non-negotiable` — Must be done by Joey, Big Three tasks
  - `Delegate` — Can be handed off to someone else
  - `Negotiable` — Flexible on timing or method
  - `Eliminate` — Should be removed from the system
- **Rule:** Always set on task creation. Use `Eliminate` aggressively for noise.

---

## Financial Fields (set on financial tasks)

### Budget

- **Field ID:** `72b72734-93dd-411f-a33f-3058b4d79875`
- **Type:** Currency (PHP ₱)
- **When:** Set at task creation for any planned expense
- **Rule:** If Budget is set, Actual Spend must be updated upon payment

### Actual Spend

- **Field ID:** `82595661-6cb3-46b1-99df-e57a1ec842b6`
- **Type:** Currency (PHP ₱)
- **When:** Update when expense is confirmed/paid
- **Alert:** Flag if Actual Spend > Budget by more than 20%

### Income

- **Field ID:** `b3ddf778-72a7-4863-9eea-b11e25c4fda0`
- **Type:** Currency (PHP ₱)
- **When:** Revenue or income tied to task (Pickpacer sales, Hardlix, freelance)

### Partial Payment

- **Field ID:** `482687ed-96ad-46d7-9636-4229fcfcb87f`
- **Type:** Currency (PHP ₱)
- **When:** Staged payments, invoicing milestones, installments

### Is Paid

- **Field ID:** `0b5aeafb-e514-40fd-878f-517516fa5d46`
- **Type:** Checkbox (true/false)
- **When:** Mark `true` once full payment confirmed
- **Default:** `false` on creation
- **Alert:** Surface all financial tasks where Is Paid = false and due date has passed

### In Debt To

- **Field ID:** `bdf3bf3f-5f76-4789-8c92-5c8bfffb3239`
- **Type:** Short Text
- **When:** Name of person or entity owed (personal finance, Hardlix suppliers, etc.)

---

## Progress Field

### Progress

- **Field ID:** `708a825f-8719-4e06-8322-4d1594636a02`
- **Type:** Number (0–100)
- **When:** Project tasks and milestones — not simple to-dos
- **Update:** On significant milestones, not every small step

---

## Dev / Engineering Fields (Pickpacer Core Space)

### Documentation

- **Field ID:** `e45d579a-0b6c-4a74-8102-366d9d169d41`
- **Type:** Task Link
- **When:** Link related documentation tasks in Core/Pickpacer dev work

### Testing Scripts

- **Field ID:** `db39025e-7b1f-4a9d-836d-0867a2d80a8e`
- **Type:** Task Link
- **When:** Link QA/testing tasks in Core Space

### Deployment

- **Field ID:** `34fd7270-a384-4872-993e-8af4607df370`
- **Type:** Task Link
- **When:** Link deployment tasks for release management

---

## Tags Reference

All tags **lowercase only**. No exceptions.

| Tag              | When to Apply                                               |
| ---------------- | ----------------------------------------------------------- |
| `routine`        | Any daily/weekly/monthly recurring task                     |
| `needs budget`   | Any task requiring budget approval before work starts       |
| `[month] [year]` | **Financial tasks only** — e.g., `march 2026`, `april 2026` |

**Month tag format:** `march 2026`, `april 2026`, `december 2026`
**Never use:** `March 2026`, `mar 2026`, `03/2026`, `Mar-2026`

### Tags by Domain

| Domain          | Common Tags                                    |
| --------------- | ---------------------------------------------- |
| agent-personal  | `routine`, `[month] [year]` on financial items |
| agent-pickpacer | `routine`, `needs budget`, `[month] [year]`    |
| agent-hardlix   | `needs budget`, `[month] [year]`               |
| agent-wife      | `routine`, `needs budget`, `[month] [year]`    |
| agent-family    | `routine`, `[month] [year]` on financial items |
| agent-running   | `routine`                                      |
| agent-athletes  | `routine`                                      |

---

## Financial Analysis Thresholds

| Check                          | Threshold      | Action                 |
| ------------------------------ | -------------- | ---------------------- |
| Budget vs Actual Spend         | > 20% variance | Flag 🔴                |
| Overdue unpaid financial task  | Any            | Surface immediately 🔴 |
| Budget set, no Actual Spend    | Past due date  | Flag ⚠️                |
| Income expected, not logged    | Past due date  | Flag ⚠️                |
| Monthly spend > monthly income | Any month      | Flag 🔴                |
| Partial Payment < 50% near due | Within 7 days  | Flag ⚠️                |

---

## Status Emoji Mapping

Use these when displaying task status in summaries:

| Status            | Emoji |
| ----------------- | ----- |
| to do             | 🔵    |
| in progress       | 🟡    |
| review / testing  | 🟠    |
| complete / done   | 🟢    |
| overdue / blocked | 🔴    |

## ABC Category Emoji Mapping

| Value                    | Display    |
| ------------------------ | ---------- |
| A Task - 1, A Task - 2   | 🔴 A Task  |
| B Task - 1, B Task - 2   | 🟡 B Task  |
| routine - 1, routine - 2 | 🔵 Routine |
| Not set / empty          | ➖         |
