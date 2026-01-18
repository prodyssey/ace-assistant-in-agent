# Ace Activity Log Viewer

View your activity log - what you've accomplished.

## Usage

```
/activity                      Show recent activity
/activity today                Show today's activity
/activity week                 Show this week's activity
/activity source manual        Filter by source (manual, git, calendar, etc.)
```

## Instructions for Claude

Use the `ace` CLI for all database operations. The CLI can be invoked via `npx ace-assistant-cli` or directly as `ace` if globally installed.

### Recent Activity (default view)

```bash
ace activity list --limit 20 --json
```

### Filter by Source

```bash
ace activity list --source manual --json
ace activity list --source git --json
```

### Adjust Limit

```bash
ace activity list --limit 50 --json
```

## Example

```
/activity

TODAY'S ACTIVITY - December 13, 2024

(no activity logged yet today)

Use /log to record what you've accomplished.
Example: /log Finished the API integration
```

```
/activity week

THIS WEEK'S ACTIVITY (Dec 9-13)

Summary:
  - 12 manual logs
  - 5 completed actions

Friday, Dec 13
  (nothing yet)

Thursday, Dec 12
  14:30 [manual] Finished client presentation
        -> Project: TPP client care
  11:00 [manual] Weekly team sync
        -> Project: smg

Wednesday, Dec 11
  16:00 [manual] Deployed new feature
        -> Project: Boost dev
  ...
```
