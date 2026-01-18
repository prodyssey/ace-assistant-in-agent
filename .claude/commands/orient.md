# Ace Daily Orientation

Quick start-of-day or on-demand orientation to help you focus on what matters most.

## What it shows

1. **Active Goals** - Current high-level objectives you're working toward
2. **Active Project Limits** - Are you within your 3/3 limits?
3. **The One Things** - Most important next action for each active project
4. **Due Today** - Actions with today's due date
5. **Overdue** - Actions past their due date
6. **Waiting For** - Things you're waiting on (follow-up needed?)
7. **Inbox Count** - How many items need processing
8. **Focus Recommendation** - 1-3 things to focus on right now

## Instructions for Claude

Use the `ace` CLI for all database operations. The CLI can be invoked via `npx ace-assistant-cli` or directly as `ace` if globally installed.

When the user invokes /orient:

1. **Get orientation data:**
   ```bash
   ace orient --json
   ```

   This returns JSON with:
   - `domains` - Active projects per domain vs limits
   - `activeProjects` - Projects with their One Things
   - `dueToday` - Actions due today
   - `overdue` - Overdue actions
   - `waiting` - Items being waited on
   - `inboxCount` - Number of inbox items

2. **Get active goals:**
   ```bash
   ace goals active --json
   ```

3. **Format and display** the orientation dashboard nicely, showing goals first for context.

4. **Recommend focus** based on:
   - Active goals (context for projects)
   - Overdue items (highest priority)
   - Due today items
   - One Things from active projects
   - Consider time of day if known

## Optional: Context Filtering

If the user provides context (e.g., "orient @low-energy" or "orient at home"), filter active actions by context:
```bash
ace actions list --status active --context <context-id> --json
```

To get context IDs, first list contexts:
```bash
ace contexts list --json
```

## Example Output

```
/orient

ACE DAILY ORIENTATION - Friday, Dec 13

ACTIVE GOALS

Work:
  - Launch MVP product -> Get to 10 beta users
  - Build consulting pipeline -> Close 2 clients

Personal:
  - Improve physical health -> Exercise 3x weekly

CAPACITY
   Work:     1/3 active projects
   Personal: 2/3 active projects

ACTIVE PROJECTS & ONE THINGS

Work:
  - Positioning for the plan is honed
    -> One Thing: Draft elevator pitch v2

Personal:
  - Health checkups are scheduled
    -> One Thing: Call dentist
  - Home automation for fireplace is explored
    -> One Thing: Research smart gas valve options

DUE TODAY
  (none)

OVERDUE
  (none)

WAITING FOR (5 items)
  - Waiting on Kels schedule of global entry interview
  - Waiting on IRS correspondence for S corp
  - [+3 more]

INBOX: 277 items (consider /process)

RECOMMENDED FOCUS

Based on your active projects, consider:
1. Draft elevator pitch v2 (work priority)
2. Call dentist (quick win)
3. Process 10 inbox items

What would you like to work on?
```
