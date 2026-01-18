# Ace Project Management

View and manage your projects.

## Usage

```
/projects                      Show active projects by domain
/projects work                 Show work projects (all statuses)
/projects personal             Show personal projects
/projects status active        Filter by status
/projects status next_up       Show next up projects
/projects category health      Filter by category
/projects search "fireplace"   Search project names
/projects create "New thing"   Create a new project
/projects update [name]        Update a project
```

## Valid Statuses

- `active` - Currently working on (limited)
- `next_up` - Ready to start when capacity
- `incubating` - Needs more thinking
- `someday_maybe` - Might do someday
- `completed` - Done
- `dropped` - Decided not to do
- `merged` - Combined into another

Note: Projects can optionally be linked to a Goal. Use `/goals` to manage goals.

## Instructions for Claude

Use the `ace` CLI for all database operations. The CLI can be invoked via `npx ace-assistant-cli` or directly as `ace` if globally installed.

### Default View (Active Projects)

```bash
ace projects active --json
```

Returns JSON with active projects by domain.

### Status Filter

```bash
ace projects list --status active --json
ace projects list --status next_up --json
ace projects list --status incubating --json
```

### Domain Filter

```bash
ace projects list --domain Work --json
ace projects list --domain Personal --json
```

### Search

```bash
ace projects list --search "search term" --json
```

### Get Project Details

```bash
ace projects get <id> --json
```

### Create Project

1. Ask for: Name (outcome-focused), Category, Status (default: incubating)
2. Use CLI:
   ```bash
   ace projects add "Project name" --status incubating --category <id>
   ```

### Update Project

1. Find the project
2. Ask what to update: Status, One Thing, Category
3. Use CLI:
   ```bash
   ace projects update <id> --status active
   ace projects update <id> --the-one-thing "Next action description"
   ```

### Activate Project

```bash
ace projects activate <id>
```

### Complete Project

```bash
ace projects complete <id>
```

### Mark Reviewed

```bash
ace projects mark-reviewed <id>
```

### List Projects Due for Review

```bash
ace projects review --json
```

## Example

```
/projects

ACTIVE PROJECTS

Work: 1/3
Positioning for the plan is honed
   Category: On the business
   One Thing: Get feedback from 2 advisors on pitch
   Review due: Dec 20

Personal: 0/3
(no active personal projects)

You have capacity! Consider promoting from Next Up.
Run /projects status next_up to see candidates.
```

```
/projects status next_up

NEXT UP PROJECTS (156 total)

Work (showing first 10):
  - home automation for fireplace is explored (Home)
  - Clean up the garage (Home)
  - tread desk legs are stained (Hobby)
  ...

Personal (showing first 10):
  - Health checkups are scheduled
  - ...

Use /projects update "name" to change status.
```
