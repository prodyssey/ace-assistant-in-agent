# Ace Activity Log

Log what you've accomplished manually.

## Usage

```
/log Finished the API integration for TPP
/log Had a great call with Janet about ClusterTruck opportunity
/log Completed weekly review
```

## Instructions for Claude

Use the `ace` CLI for all database operations. The CLI can be invoked via `npx ace-assistant-cli` or directly as `ace` if globally installed.

When the user invokes /log with a message:

1. **Parse the log entry** to identify:
   - What was done (the summary)
   - Any project hints (look for project names in the text)
   - Any action references

2. **Try to match to a project** if mentioned:
   ```bash
   ace projects list --search "keyword" --json
   ```

3. **Log the activity:**
   ```bash
   ace activity log "Summary of what was done" --project <id>
   ```

4. **Optionally complete related action** if one was finished:
   ```bash
   ace actions complete <id>
   ```

5. **Confirm** what was logged.

## Example

```
/log Finished drafting the elevator pitch for The Plan

Logged: "Finished drafting the elevator pitch for The Plan"
   Linked to project: Positioning for the plan is honed
   Time: 2024-12-13 14:30

Did you complete an action? [Yes / No]

User: Yes

Which action?
1. Draft elevator pitch v2
2. Other

User: 1

Marked "Draft elevator pitch v2" as complete.
```

## Viewing Logs

You can also use /activity to view your activity log.
