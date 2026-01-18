# Ace Inbox Processing

Process items in your inbox one at a time.

## Workflow

For each inbox item, help the user decide:

1. **Is it actionable?**
   - No -> Delete or move to reference/someday
   - Yes -> Continue

2. **Can it be done in < 2 minutes?**
   - Yes -> Do it now, log it, mark complete
   - No -> Continue

3. **What's the next action?**
   - **Defer to project** -> Link to existing project or create new one
   - **Someday/maybe** -> Move to someday status
   - **Waiting for** -> Mark as waiting, note who/what
   - **Delegate** -> Mark delegated, note to whom

4. **Add contexts** -> Which @tags apply?

5. **Set due date?** -> If time-sensitive

## Instructions for Claude

Use the `ace` CLI for all database operations. The CLI can be invoked via `npx ace-assistant-cli` or directly as `ace` if globally installed.

When the user invokes /process:

1. Query the database for inbox items:
   ```bash
   ace actions inbox --json
   ```

2. Show the first item with:
   - Name
   - Notes (if any, truncated)
   - When captured
   - Any existing context hints

3. Ask the user what they want to do using AskUserQuestion with options.

   **For forwarded articles/emails with links** (name starts with "Fwd:" or notes contain URLs to articles):
   - Clip to Obsidian (opens URL in browser for web clipper) - FIRST OPTION
   - Delete (not actionable)
   - Defer to project
   - Skip for now

   **For regular items:**
   - Delete (not actionable)
   - Do now (< 2 min)
   - Defer to project
   - Someday/maybe
   - Waiting for
   - Delegate
   - Skip for now

4. Based on their choice:

   **Clip to Obsidian:**
   - Extract URL from notes (look for https:// links, prefer article URLs over unsubscribe links)
   - Open URL in browser using `open "URL"`
   - Wait for user to confirm they've clipped it
   - Delete the item:
   ```bash
   ace actions delete <id>
   ```

   **Delete:**
   ```bash
   ace actions delete <id>
   ```

   **Do now:**
   - Ask what they did
   - Mark complete and log activity:
   ```bash
   ace actions complete <id>
   ace activity log "Summary of what was done" --action <id>
   ```

   **Defer to project:**
   - Show relevant projects:
   ```bash
   ace projects list --status active --json
   ```
   - Or offer to create new:
   ```bash
   ace projects add "Project name" --status incubating
   ```
   - Update action:
   ```bash
   ace actions update <id> --status active --project <project-id>
   ```

   **Someday/maybe:**
   ```bash
   ace actions update <id> --status someday
   ```

   **Waiting for:**
   - Ask who/what they're waiting for
   ```bash
   ace actions update <id> --waiting "Person or thing"
   ```

   **Delegate:**
   - Ask who it's delegated to
   ```bash
   ace actions update <id> --status waiting --waiting "Delegated to Person"
   ```

5. After processing, check inbox count:
   ```bash
   ace actions count
   ```
   - If items remain, ask if they want to continue
   - If zero, celebrate inbox zero!

## Clarification

If an item is unclear, ask the user:
- "What specifically needs to happen here?"
- "Is this still relevant?"

Update the action name if they clarify:
```bash
ace actions update <id> --name "Clarified task name"
```

## Example Session

```
/process

Inbox Item 1 of 15:
"Fwd: Business should serve your life, not consume it."
Captured: 2025-06-02
Notes: [Email forward from Justin Welsh newsletter]

This looks like a forwarded article.
[Clip to Obsidian] [Delete] [Defer to project] [Skip]

User: Clip to Obsidian

[Opens article in browser]
Let me know when you've clipped it.

User: done

Clipped and removed. 14 items remaining.

Inbox Item 2 of 14:
"Call dentist to schedule cleaning"
Captured: 2024-12-13
Contexts: @phone, @low-energy

What would you like to do with this?

User: Defer to project - health

Which project?
1. Health checkups are scheduled (active)
2. Create new project

User: 1

Added to "Health checkups are scheduled"
Any due date? [No / Pick date]

User: No

Processed. 13 items remaining. Continue? [Yes / No]
```
