# Ace Weekly Review

Guided weekly review to stay on top of your projects and actions.

## Review Checklist

1. **Inbox Zero** - Process all inbox items
2. **Active Projects** - Review each, update One Thing
3. **Next Up** - Evaluate for promotion (respect limits)
4. **Goals** - Review active goals and their progress
5. **Due for Review** - Check items past their review date
6. **Waiting For** - Follow up on blocked items
7. **Completed This Week** - Celebrate wins
8. **Set One Things** - Plan next week's priorities

## Instructions for Claude

Use the `ace` CLI for all database operations. The CLI can be invoked via `npx ace-assistant-cli` or directly as `ace` if globally installed.

When the user invokes /review:

1. **Step through each section interactively.**

### Section 1: Inbox

```bash
ace actions count
```

If > 0, ask if they want to process now or skip.

### Section 2: Active Projects

```bash
ace projects active --json
```

For each active project:
- Show current One Thing
- Ask: "What's the One Thing for next week?"
- Update if changed:
  ```bash
  ace projects update <id> --the-one-thing "New one thing"
  ```
- Ask: "Any blockers or status changes?"
- Mark as reviewed:
  ```bash
  ace projects mark-reviewed <id>
  ```

### Section 3: Next Up

```bash
ace projects list --status next_up --json
ace projects active --json  # for capacity check
```

For each Next Up project:
- Ask: "Ready to activate this? [Yes / Not yet / Drop]"
- If yes and under limit, promote to active:
  ```bash
  ace projects update <id> --status active
  ```

### Section 4: Goals

```bash
ace goals active --json
```

For each active goal:
- Show linked projects:
  ```bash
  ace goals get <id> --projects --json
  ```
- Ask: "How is progress toward this goal? Any actions needed?"
- If actions mentioned, offer to capture them
- Mark as reviewed:
  ```bash
  ace goals mark-reviewed <id>
  ```

### Section 5: Due for Review

```bash
ace projects review --json
```

For each:
- Show project with status
- Ask: "Still relevant? [Yes / Move to someday / Drop]"
- Mark as reviewed:
  ```bash
  ace projects mark-reviewed <id>
  ```

### Section 6: Waiting For

```bash
ace actions waiting --json
```

For each (or batched):
- Ask: "Any update on this? [Still waiting / Received / Drop]"
- If received, move to active or complete:
  ```bash
  ace actions update <id> --status active
  # or
  ace actions complete <id>
  ```

### Section 7: Completed This Week

```bash
ace projects completed --json
ace actions completed --json
```

Show wins and celebrate!

### Section 8: Wrap Up

- Summarize what was reviewed
- Show updated active project status:
  ```bash
  ace projects active
  ```

## Example Flow

```
/review

ACE WEEKLY REVIEW - Week of Dec 9-13, 2024

Starting your weekly review. We'll go through 8 sections.

SECTION 1/8: INBOX

You have 277 items in your inbox.

Would you like to:
[Process now] [Skip for now]

User: Skip for now

OK, we'll come back to that. Moving on...

SECTION 2/8: ACTIVE PROJECTS (1 project)

Positioning for the plan is honed
   Category: On the business (Work)
   Current One Thing: Draft elevator pitch v2
   Last reviewed: Dec 6

What's the One Thing for next week?
[Keep current] [Update]

User: Update -> "Get feedback from 2 advisors on pitch"

Updated. Any blockers or status changes?
[No changes] [Mark complete] [Move to incubating]

User: No changes

Reviewed.

SECTION 3/8: NEXT UP (156 projects)

Work: 1/3 active (capacity available)
Personal: 0/3 active (capacity available)

Top candidates:
1. Home automation for fireplace is explored (Home)
2. Clean up the garage (Home)
3. Tread desk legs are stained (Hobby)

Would you like to activate any? [Yes / Skip]

...continues through all sections...
```
