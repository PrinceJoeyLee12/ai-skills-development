# Financial Planning — Claw ClickUp Sub-Skill

This reference governs all financial planning, budgeting, expense tracking, income monitoring,
cash flow analysis, and debt management across Joey's ClickUp workspace.

**Trigger phrases:** "budget", "expenses", "income", "cash flow", "financial summary",
"monthly rollup", "how much did I spend", "what do I owe", "payment status", "financial health",
"money", "debt", "unpaid", "budget vs actual"

---

## Financial Task Architecture

Financial tasks live across multiple domains. Each domain has specific lists for financial data:

| Domain                | Financial List(s)             | List ID(s)                                     |
| --------------------- | ----------------------------- | ---------------------------------------------- |
| Pickpacer — Fin & Ops | F&O Main, Expenses, Wages     | `901604353987`, `901604354127`, `901604388639` |
| Pickpacer — Sales     | Sales List                    | `901611468224`                                 |
| Wife/Love             | Love Financial List           | `901605680981`                                 |
| Hardlix               | HardlixHardware Business List | `901605645075`                                 |
| Personal              | Personal List, Others List    | `901604362025`, `901604409717`                 |
| Family                | Family List                   | `901604361973`                                 |

**All financial tasks must have:**

- A month tag (e.g., `march 2026`) — lowercase, financial tasks only
- At least one currency field set (Budget, Income, Actual Spend, or Partial Payment)
- ABC Category and Categorize fields (mandatory on all tasks)

---

## Currency Custom Fields

| Field           | Field ID                               | When to Set                                              |
| --------------- | -------------------------------------- | -------------------------------------------------------- |
| Budget          | `72b72734-93dd-411f-a33f-3058b4d79875` | Planned/expected cost — set at task creation             |
| Actual Spend    | `82595661-6cb3-46b1-99df-e57a1ec842b6` | Confirmed expense — update when payment is made          |
| Income          | `b3ddf778-72a7-4863-9eea-b11e25c4fda0` | Revenue received — Pickpacer sales, Hardlix income, etc. |
| Partial Payment | `482687ed-96ad-46d7-9636-4229fcfcb87f` | Staged/installment payments — track partial amounts      |
| Is Paid         | `0b5aeafb-e514-40fd-878f-517516fa5d46` | Checkbox — mark `true` when fully paid                   |
| In Debt To      | `bdf3bf3f-5f76-4789-8c92-5c8bfffb3239` | Text — name of person/entity owed                        |

---

## Financial Operations

### 1. Monthly Budget Plan

**When Joey says:** "Create my budget for [month]", "plan next month's budget", "set up April finances"

**Procedure:**

1. Ask Joey which domains to include (or default to all financial lists)
2. For each domain, gather existing tasks tagged with the target month
3. If no tasks exist yet, ask Joey to list planned expenses and income
4. Create tasks in the correct financial lists with:
   - Descriptive name (e.g., "Office Rent — April 2026")
   - Budget field set to planned amount
   - Month tag (e.g., `april 2026`)
   - ABC Category based on priority
   - Categorize based on necessity
   - Due date set to expected payment date
5. After creating all tasks, show the budget summary

**Budget Plan Output:**

```
📊 Budget Plan — [Month Year]

💼 Pickpacer
• [Task Name] — ₱XX,XXX (due [date])
• [Task Name] — ₱XX,XXX (due [date])
Subtotal: ₱XX,XXX

🏠 Personal / Family
• [Task Name] — ₱XX,XXX (due [date])
Subtotal: ₱XX,XXX

💕 Wife / Love
• [Task Name] — ₱XX,XXX (due [date])
Subtotal: ₱XX,XXX

🔧 Hardlix
• [Task Name] — ₱XX,XXX (due [date])
Subtotal: ₱XX,XXX

━━━━━━━━━━━━━━━━━━
Total Planned Budget: ₱XXX,XXX
Expected Income:      ₱XXX,XXX
Projected Net:        ₱±XX,XXX [🟢 surplus / 🔴 deficit]
```

---

### 2. Expense Tracking & Budget vs Actual

**When Joey says:** "How much have I spent?", "budget vs actual", "am I over budget?", "expense report"

**Procedure:**

1. Pull all tasks with the Budget field set, filtered by the relevant month tag
2. For each task, compare Budget vs Actual Spend
3. Flag any task where:
   - Actual Spend > Budget by more than 20% → 🔴 **Over budget**
   - Budget is set but Actual Spend is missing → ⚠️ **Needs update**
   - Actual Spend is set but no Budget → ⚠️ **Unplanned expense**
4. Calculate totals per domain and overall

**Expense Report Output:**

```
💰 Budget vs Actual — [Month Year]

✅ On Track ([count])
• [Task Name] — Budget ₱X,XXX · Spent ₱X,XXX (XX%)

🔴 Over Budget ([count])
• [Task Name] — Budget ₱X,XXX · Spent ₱X,XXX (+₱X,XXX over, XX%)

⚠️ Needs Update ([count])
• [Task Name] — Budget ₱X,XXX · No spend recorded

📊 Totals
Total Budget:  ₱XXX,XXX
Total Spent:   ₱XXX,XXX (XX% of budget)
Variance:      ₱±XX,XXX [🟢 under / 🔴 over]
```

---

### 3. Income Tracking

**When Joey says:** "How much income this month?", "revenue summary", "what came in?"

**Procedure:**

1. Pull all tasks with the Income field set, filtered by month tag
2. Group by domain (Pickpacer Sales, Hardlix, etc.)
3. Sum income per domain and overall
4. Compare against planned budget if available

**Income Report Output:**

```
💵 Income Summary — [Month Year]

💼 Pickpacer
• [Task Name] — ₱XX,XXX [✅ received / ⏳ pending]
Subtotal: ₱XX,XXX

🔧 Hardlix
• [Task Name] — ₱XX,XXX [✅ received / ⏳ pending]
Subtotal: ₱XX,XXX

━━━━━━━━━━━━━━━━━━
Total Income (received): ₱XXX,XXX
Total Income (pending):  ₱XX,XXX
```

---

### 4. Cash Flow Analysis

**When Joey says:** "Cash flow", "net position", "am I positive this month?", "money in vs money out"

**Procedure:**

1. Pull all income tasks for the month → sum received + pending
2. Pull all expense tasks for the month → sum actual spend + committed budget
3. Calculate net position
4. Project remaining month cash flow based on unpaid commitments

**Cash Flow Output:**

```
💹 Cash Flow — [Month Year]

Money In:
  Received:    ₱XXX,XXX
  Pending:     ₱XX,XXX
  Total In:    ₱XXX,XXX

Money Out:
  Paid:        ₱XXX,XXX
  Committed:   ₱XX,XXX (budgeted, not yet paid)
  Total Out:   ₱XXX,XXX

━━━━━━━━━━━━━━━━━━
Net Position:  ₱±XX,XXX [🟢 positive / 🔴 negative]
Remaining commitments this month: ₱XX,XXX
Projected end-of-month: ₱±XX,XXX
```

---

### 5. Debt & Payment Status

**When Joey says:** "What do I owe?", "unpaid tasks", "debt summary", "payment status"

**Procedure:**

1. Pull all tasks where Is Paid = `false` and (Budget > 0 or Actual Spend > 0)
2. Pull all tasks with the "In Debt To" field set
3. Group by creditor (In Debt To value) and by domain
4. Show total outstanding, sorted by due date

**Debt Report Output:**

```
🏦 Debt & Payment Status

Unpaid ([count]):
• [Task Name] — ₱X,XXX · due [date] · owed to [name]
• [Task Name] — ₱X,XXX · due [date]

Overdue Payments ([count]):
• [Task Name] — ₱X,XXX · was due [date] ⚠️

Partial Payments ([count]):
• [Task Name] — ₱X,XXX paid of ₱X,XXX total (XX%)

━━━━━━━━━━━━━━━━━━
Total Outstanding: ₱XXX,XXX
Total Overdue:     ₱XX,XXX
```

---

### 6. Financial Health Check

**When Joey says:** "Financial health", "how are my finances?", "financial overview"

This is a combined view. Run all of the above and present a unified dashboard:

```
🏥 Financial Health — [Month Year]

📊 Budget Execution: XX% spent of ₱XXX,XXX budget
💵 Income: ₱XXX,XXX received (XX% of expected)
💹 Net Position: ₱±XX,XXX [🟢/🔴]
🏦 Outstanding Debt: ₱XX,XXX across [N] tasks

🚨 Alerts:
• [N] tasks over budget by >20%
• [N] financial tasks with no spend recorded
• [N] overdue unpaid tasks
• [N] tasks missing Is Paid status

Top 3 Actions:
1. [Highest priority financial action]
2. [Second priority]
3. [Third priority]
```

---

## MCP Tool Chain for Financial Operations

### Pull financial tasks for a month:

```
Step 1: clickup_search — query tasks tagged with "[month] [year]" across financial lists
Step 2: For each task from search results, clickup_get_task — retrieve full custom fields
        (Budget, Actual Spend, Income, Partial Payment, Is Paid, In Debt To)
Step 3: Aggregate and compute totals, variances, and flags
```

### Create a financial task:

```
Step 1: clickup_create_task — in the correct financial list with:
        - name, due_date, assignee
        - custom fields: ABC Category, Categorize, Budget (and/or Income)
Step 2: clickup_add_tag_to_task — add month tag (e.g., "march 2026")
Step 3: clickup_add_tag_to_task — add "needs budget" if applicable
```

### Update a financial task (payment confirmed):

```
Step 1: clickup_get_task — verify current state
Step 2: clickup_update_task — set Actual Spend, mark Is Paid = true
Step 3: Confirm update to Joey with before/after values
```

---

## Alert Thresholds

| Check                                         | Threshold              | Alert Level |
| --------------------------------------------- | ---------------------- | ----------- |
| Actual Spend > Budget                         | > 20% variance         | 🔴 Critical |
| Budget set, no Actual Spend                   | Any with past due date | ⚠️ Warning  |
| Income expected, not logged                   | Any with past due date | ⚠️ Warning  |
| Is Paid = false, overdue                      | Any                    | 🔴 Critical |
| Partial Payment < 50% of Budget, close to due | Within 7 days          | ⚠️ Warning  |
| Total monthly spend > total monthly income    | Any month              | 🔴 Critical |

---

## Financial Task Creation Defaults

When creating financial tasks, apply these defaults unless Joey specifies otherwise:

| Field        | Default                                                                   |
| ------------ | ------------------------------------------------------------------------- |
| ABC Category | `B Task - 1` (adjust up to A if large amount or business-critical)        |
| Categorize   | `Non-negotiable` for rent/utilities/wages, `Negotiable` for discretionary |
| Tags         | `[month] [year]` always, plus `needs budget` if amount is TBD             |
| Due date     | Payment due date, or end of month if unspecified                          |
| Is Paid      | `false` (until confirmed)                                                 |
