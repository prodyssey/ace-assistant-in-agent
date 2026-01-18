# Ace Fast Inbox Processing (iOS Optimized)

Process multiple inbox items at once to minimize network round-trips on mobile.

## Why This Exists

On iOS, each prompt triggers a git pull. Processing 10 items one-at-a-time = 10 network delays. This command batches decisions to reduce round-trips.

## Instructions for Claude

Use the `ace` CLI for all database operations. The CLI can be invoked via `npx ace-assistant-cli` or directly as `ace` if globally installed.

When the user invokes /process-fast:

1. **Fetch batch of inbox items:**
   ```bash
   ace actions inbox --json
   ```

2. **Also get total inbox count:**
   ```bash
   ace actions count
   ```

3. **Display ALL items in a numbered list:**
   ```
   ## Inbox (X total, showing 10)

   1. [id:123] "Call dentist" (Dec 13) @phone
   2. [id:124] "Fwd: Article about productivity" (Dec 14) - has URL
   3. [id:125] "Review quarterly report" (Dec 14)
   ...
   ```

4. **Ask for BATCH decisions in ONE prompt:**

   Present options like:
   ```
   Quick actions (comma-separated numbers):
   - DELETE: 1,3,5
   - CLIP (has URL): 2
   - SOMEDAY: 4
   - SKIP ALL (process individually later)

   Or type a number to process individually.
   ```

5. **Process ALL decisions from the single response:**

   Parse their response for batch operations. Examples:
   - "delete 1,2,3" -> Delete items 1,2,3
   - "someday 4,5" -> Move items 4,5 to someday
   - "clip 2" -> Open URL for item 2, mark for deletion
   - "3" -> Process item 3 individually (defer to project, etc.)

6. **Execute batch updates:**
   ```bash
   # For deletes (run for each item)
   ace actions delete <id>

   # For someday (run for each item)
   ace actions update <id> --status someday

   # For completions (run for each item)
   ace actions complete <id>
   ```

7. **Show summary and offer to continue:**
   ```
   Processed 6 items (4 deleted, 2 someday).
   X items remaining. Continue? [yes/no]
   ```

## Batch Commands Reference

| Command | Action |
|---------|--------|
| `delete 1,2,3` or `d 1,2,3` | Delete items |
| `someday 1,2` or `s 1,2` | Move to someday/maybe |
| `clip 1` or `c 1` | Open URL, then delete |
| `done 1,2` or `x 1,2` | Mark completed (quick tasks) |
| `skip` | Skip this batch |
| `3` | Process item 3 individually |

## Individual Processing (Fallback)

If user picks a single item number for defer/waiting/delegate:

1. Show item details
2. For **defer**: Show top 5 active projects + option to create new
   ```bash
   ace projects active --json
   ```
3. Update and return to batch view:
   ```bash
   ace actions update <id> --status active --project <project-id>
   ```

## Example Session

```
/process-fast

## Inbox (47 total, showing 10)

1. [456] "Fwd: Great article on focus" (Dec 12) - has URL
2. [457] "Buy batteries" (Dec 12) @errands
3. [458] "Research new laptop" (Dec 13)
4. [459] "Fwd: Newsletter about AI" (Dec 13) - has URL
5. [460] "Call mom" (Dec 13) @phone
6. [461] "Old todo - check if still relevant" (Nov 15)
7. [462] "Schedule car maintenance" (Dec 14)
8. [463] "Fwd: Article on sleep" (Dec 14) - has URL
9. [464] "Review insurance options" (Dec 14)
10. [465] "Random thought - probably delete" (Dec 14)

Batch commands: delete/d, someday/s, clip/c, done/x, skip
Or enter a number to process individually.

User: d 6,10 s 3,9 c 1,4,8

Opening URLs for clipping:
- https://example.com/focus-article
- https://example.com/ai-newsletter
- https://example.com/sleep-article

Let me know when done clipping. [done]

User: done

Processed 7 items:
- 2 deleted (6, 10)
- 2 to someday (3, 9)
- 3 clipped & deleted (1, 4, 8)

40 items remaining. Continue? [yes/no/process 2 individually]

User: 5

Processing item 5: "Call mom" @phone

[Delete] [Done now] [Defer to project] [Someday] [Waiting] [Back to batch]

User: defer to Family project

Added to "Family connection is maintained". Back to batch view.
```

## Tips for iOS

- Use shorthand: `d 1,2,3` instead of `delete 1,2,3`
- Batch similar items: clip all articles, delete all old items
- Skip complex items for later individual processing
- Aim to process 5-7 items per batch prompt
