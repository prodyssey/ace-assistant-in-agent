# Ace Quick Capture

Capture a new item to your inbox. The item will be saved and committed to git for sync.

## Usage

Just tell me what you want to capture. For example:
- "Buy new running shoes"
- "Call mom about Christmas plans"
- "Research SQLite full-text search"

You can optionally include:
- **Contexts/tags** with @ prefix: `@errands`, `@low-energy`, `@home`
- **Project hints** with # prefix: `#health`, `#home-projects`

## What happens

1. Parse any @ contexts and # project hints
2. Add it to the database as an inbox item via the CLI

## Instructions for Claude

Use the `ace` CLI for all database operations. The CLI can be invoked via `npx ace-assistant-cli` or directly as `ace` if globally installed.

When the user invokes /capture, do the following:

1. Parse the user's input to extract:
   - The main task/item text
   - Any @context tags
   - Any #project hints

2. Use the CLI to add to inbox:
   ```bash
   ace actions add "Task name" --status inbox
   ```

   If a project is specified, link it:
   ```bash
   ace actions add "Task name" --status inbox --project <project-id>
   ```

3. Confirm to the user what was captured.

## Example

User: `/capture Call dentist to schedule cleaning @phone @low-energy`

Claude:
- Runs: `ace actions add "Call dentist to schedule cleaning" --status inbox`
- Responds: "Captured: Call dentist to schedule cleaning [@phone, @low-energy]"
