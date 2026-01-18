# Ace Action Management

View and manage your actions (tasks).

## Usage

```
/actions                       Show active actions
/actions inbox                 Show inbox items
/actions waiting               Show waiting items
/actions due today             Show actions due today
/actions due week              Show actions due this week
/actions overdue               Show overdue actions
/actions @context              Filter by context (e.g., @low-energy)
/actions project "name"        Show actions for a project
/actions search "text"         Search action names
/actions complete [name]       Mark an action complete
/actions add "task"            Add a new action
```

## Valid Statuses

- `inbox` - Just captured, needs processing
- `active` - Ready to do
- `waiting` - Waiting on someone/something
- `someday` - Might do someday
- `completed` - Done
- `dropped` - Decided not to do

## Instructions for Claude

Use the `ace` CLI for all database operations. The CLI can be invoked via `npx ace-assistant-cli` or directly as `ace` if globally installed.

### Default View (Active Actions)

```bash
ace actions list --status active --json
```

### Inbox View

```bash
ace actions inbox --json
ace actions count
```

Show count and prompt for /process.

### Waiting View

```bash
ace actions waiting --json
```

### Due Today/Week/Overdue

```bash
# Due today
ace actions due --json

# Due this week
ace actions due --week --json

# Overdue
ace actions overdue --json
```

### Context Filter

To filter by context, first get the context ID:
```bash
ace contexts list --json
```

Then filter actions:
```bash
ace actions list --status active --context <context-id> --json
```

### Project Filter

```bash
ace actions list --project <project-id> --json
```

### Search

```bash
ace actions list --search "search term" --json
```

### Complete Action

```bash
ace actions complete <id>
```

Then ask if they want to add the next action for the project.

### Add Action

1. Parse input for @contexts and #project hints
2. Ask for project if not specified
3. Use CLI:
   ```bash
   ace actions add "Task name" --status inbox --project <id>
   ```

## Example

```
/actions

ACTIVE ACTIONS (showing 20 of 4428)

Due Today:
  (none)

No Due Date:
  - A lever I can flip or sign I can make...
    Project: be intentional / be effective
    @Personal Deep Work

  - convert book mindsweep checklist captured...
    Project: be intentional / be effective
    @Personal Deep Work

  - randomness in your productivity system...
    Project: be intentional / be effective
    @Personal Deep Work
  ...

Filter by context: /actions @low-energy
Filter by project: /actions project "health"
```

```
/actions @low-energy

ACTIONS TAGGED @low-energy (15 actions)

  - Schedule dentist appointment
  - Review bank statements
  - Update password manager
  ...
```
