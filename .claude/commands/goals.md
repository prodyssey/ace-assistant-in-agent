# Ace Goal Management

View and manage your goals. Goals are higher-level objectives that multiple projects can contribute toward.

## Usage

```
/goals                       Show active goals by domain
/goals work                  Show work goals (all statuses)
/goals personal              Show personal goals
/goals status active         Filter by status
/goals status paused         Show paused goals
/goals create "New goal"     Create a new goal
/goals update [name]         Update a goal
```

## Valid Statuses

- `active` - Currently pursuing
- `paused` - Temporarily on hold
- `completed` - Achieved
- `dropped` - Decided not to pursue

## Instructions for Claude

Use the `ace` CLI for all database operations. The CLI can be invoked via `npx ace-assistant-cli` or directly as `ace` if globally installed.

### Default View (Active Goals)

```bash
ace goals active --json
```

### List All Goals

```bash
ace goals list --json
```

### Status Filter

```bash
ace goals list --status active --json
ace goals list --status paused --json
```

### Domain Filter

```bash
ace goals list --domain Work --json
ace goals list --domain Personal --json
```

### View Goal with Projects

```bash
ace goals get <id> --projects --json
```

### Create Goal

1. Ask for: Name, Domain (Work/Personal), Focus (the one thing)
2. Use CLI:
   ```bash
   ace goals add "Goal name" --domain Work --focus "The one thing focus"
   ```

### Update Goal

1. Find the goal
2. Ask what to update: Status, Focus (the one thing), Notes
3. Use CLI:
   ```bash
   ace goals update <id> --status paused
   ace goals update <id> --focus "New focus"
   ace goals update <id> --notes "Updated notes"
   ```

### Complete Goal

```bash
ace goals complete <id>
```

### Pause Goal

```bash
ace goals pause <id>
```

### Drop Goal

```bash
ace goals drop <id>
```

### Mark Reviewed

```bash
ace goals mark-reviewed <id>
```

### List Goals Due for Review

```bash
ace goals review --json
```

## Example Output

```
/goals

ACTIVE GOALS

Work
Positioning for the plan is honed
   Focus: Get to 10 beta users
   Projects: 3 linked
   Review due: Jan 15

Build sustainable consulting pipeline
   Focus: Close 2 new clients this quarter
   Projects: 1 linked
   Review due: Jan 18

Personal
Improve physical health
   Focus: Exercise 3x per week consistently
   Projects: 2 linked
   Review due: Jan 14

Use /goals update "name" to change status or focus.
Use /projects to see projects linked to each goal.
```

```
/goals status paused

PAUSED GOALS

Work:
  - Learn new programming language (paused Dec 15)

Personal:
  - Home renovation project (paused Jan 2)

Use /goals update "name" to resume.
```
