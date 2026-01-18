# Ace - Personal Productivity Assistant

This repository provides Claude Code integration for Ace, a personal productivity system.

## CLI Tool

Use the `ace` CLI (published as `ace-assistant-cli` on npm) for all productivity operations:

```bash
# If globally installed
ace <command>

# Or via npx
npx ace-assistant-cli <command>
```

## Key Concepts

### Domains & Capacity Limits
- **Work**: 3 active projects max (soft limit)
- **Personal**: 3 active projects max (soft limit)

### Goals
Goals are high-level objectives that multiple projects can contribute toward.

- **Statuses**: `active`, `paused`, `completed`, `dropped`
- Goals belong to a domain (Work or Personal)
- Projects can be linked to a goal via `goalId`
- Review intervals: 7 days (active), 30 days (paused)

### Projects
Outcome-focused items ("You have X", "X is done").

- **Statuses**: `active`, `next_up`, `incubating`, `someday_maybe`, `completed`, `dropped`, `merged`
- Each project has a "One Thing" field - the most important next action
- Review intervals vary by status (7-90 days)

### Actions
Verb-focused tasks ("Call Bob", "Review document").

- **Statuses**: `inbox`, `active`, `waiting`, `someday`, `completed`, `dropped`
- Can be assigned to projects, contexts, and categories
- Support due dates and "waiting for" tracking

### Contexts
Tags for filtering actions by situation: `@home`, `@phone`, `@low-energy`, `@computer`, etc.

## CLI Commands

### Authentication

```bash
ace auth login      # Authenticate with Ace (opens browser)
ace auth logout     # Remove stored credentials
ace auth status     # Check authentication status
ace auth whoami     # Show current user
```

### Actions

```bash
# Listing
ace actions list                        # All actions
ace actions inbox                       # Inbox items
ace actions list --status active        # By status
ace actions list --context "@home"      # By context
ace actions list --project <id>         # By project
ace actions due                         # Due today
ace actions overdue                     # Past due
ace actions waiting                     # Waiting items

# Creating
ace actions add "Task name"             # Add to inbox
ace actions add "Task" --status active  # Add as active
ace actions add "Task" --project <id>   # Add to project
ace actions add "Task" --notes "..."    # With notes

# Updating
ace actions complete <id>               # Mark complete
ace actions delete <id>                 # Delete action
ace actions update <id> --status waiting --waiting "Bob"

# Metadata
ace actions count                       # Count by status
```

### Projects

```bash
# Listing
ace projects list                       # All projects
ace projects active                     # Active projects
ace projects list --status next_up      # By status
ace projects list --domain Work         # By domain
ace projects review                     # Due for review

# Creating & Updating
ace projects add "Project name"         # Create project
ace projects activate <id>              # Set to active
ace projects complete <id>              # Mark complete
ace projects update <id> --status next_up
ace projects mark-reviewed <id>         # Reset review timer
```

### Goals

```bash
# Listing
ace goals list                          # All goals
ace goals active                        # Active goals
ace goals list --status paused          # By status
ace goals review                        # Due for review

# Creating & Updating
ace goals add "Goal name"               # Create goal
ace goals update <id> --status paused
ace goals complete <id>                 # Mark complete
ace goals mark-reviewed <id>            # Reset review timer
```

### Daily Orientation

```bash
ace orient                              # Human-readable dashboard
ace orient --json                       # JSON for AI processing
```

### Activity Log

```bash
ace activity list                       # Recent activity
ace activity list --limit 50            # More items
ace activity log "Summary of work"      # Log accomplishment
ace activity log "Work" --project <id>  # Log with project
```

### Contexts & Categories

```bash
ace contexts list                       # List all contexts
ace categories list                     # List all categories
```

## JSON Output

Add `--json` to any list/read command for machine-readable output:

```bash
ace orient --json
ace actions inbox --json
ace projects active --json
```

This is essential for slash commands to parse and display data properly.

## Slash Commands

The `.claude/commands/` directory contains productivity slash commands:

| Command | Purpose |
|---------|---------|
| `/capture` | Quick capture to inbox with optional context/project hints |
| `/process` | Process inbox items one-by-one |
| `/process-fast` | Batch process inbox items (iOS/mobile optimized) |
| `/orient` | Daily orientation dashboard |
| `/actions` | View and manage actions |
| `/projects` | View and manage projects |
| `/goals` | View and manage goals |
| `/log` | Log accomplishments to activity log |
| `/activity` | View activity history |
| `/review` | Guided weekly review |

## Processing Workflow

When processing inbox items (`/process`), offer these options:

1. **Delete** - Not actionable, remove it
2. **Do Now** - Less than 2 minutes, complete immediately
3. **Defer** - Add to a project as an active action
4. **Someday/Maybe** - Might do eventually
5. **Waiting** - Blocked on someone, track who
6. **Delegate** - Hand off to someone else
7. **Clip** - For URLs/articles, open in browser for saving elsewhere

## Review Intervals

Projects and goals have review dates that advance based on status:

| Status | Interval |
|--------|----------|
| Active | 7 days |
| Next Up | 14 days |
| Incubating | 30 days |
| Someday/Maybe | 90 days |
| Paused (goals) | 30 days |

Use `ace projects review` and `ace goals review` to find items due for review.

## Tips for Claude

1. **Always use `--json`** when fetching data to parse, then format nicely for the user
2. **Respect capacity limits** - warn users when they're at 3/3 active projects
3. **Projects are outcomes** - help users phrase them as "X is done" not "Do X"
4. **Actions are verbs** - start with action words like "Call", "Review", "Send"
5. **One Thing matters** - each active project should have a clear next action
6. **Context awareness** - suggest filtering by context when relevant (@low-energy for tired users)
