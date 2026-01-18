# Ace CLI Template for Claude Code

This repository provides slash commands for using [Ace](https://aceisyourassistant.com), a personal productivity assistant, directly within [Claude Code](https://docs.anthropic.com/en/docs/claude-code).

## What is Ace?

Ace is a personal productivity system that helps you:

- **Capture** ideas and tasks to an inbox for later processing
- **Organize** work into projects with clear outcomes
- **Track** actions with contexts, due dates, and statuses
- **Review** regularly to stay on top of commitments
- **Focus** with capacity limits (3 active projects per domain)

## Prerequisites

1. **An Ace account** - Sign up at [aceisyourassistant.com](https://aceisyourassistant.com)
2. **Claude Code** - Install from [Anthropic](https://docs.anthropic.com/en/docs/claude-code)
3. **Node.js 18+** - Required for the CLI

## Quick Start

### 1. Install the Ace CLI

```bash
npm install -g ace-assistant-cli
```

Or use `npx` without installing:

```bash
npx ace-assistant-cli auth login
```

### 2. Authenticate

```bash
ace auth login
```

This opens a browser window to authorize the CLI. Credentials are stored in `~/.ace/credentials.json`.

### 3. Clone or Fork This Repository

```bash
git clone https://github.com/prodyssey/ace-assistant-in-agent.git
cd ace-assistant-in-agent
```

Or click "Use this template" on GitHub to create your own copy.

### 4. Open with Claude Code

```bash
claude
```

Now you can use the slash commands!

## Available Slash Commands

| Command | Description |
|---------|-------------|
| `/capture` | Quick capture to inbox |
| `/process` | Process inbox items one-by-one |
| `/process-fast` | Batch process inbox items (optimized for mobile) |
| `/orient` | Daily orientation dashboard |
| `/actions` | View and manage actions |
| `/projects` | View and manage projects |
| `/goals` | View and manage high-level goals |
| `/log` | Log accomplishments |
| `/activity` | View activity history |
| `/review` | Guided weekly review |

## Usage Examples

### Quick Capture

```
/capture Buy groceries @errands
/capture "Review Q4 report" #quarterly-planning
```

### Daily Start

```
/orient
```

Shows your active goals, project limits, due items, and focus recommendations.

### Process Inbox

```
/process
```

Walks through each inbox item with options: delete, do now, defer, delegate, etc.

### Batch Processing (Mobile-Friendly)

```
/process-fast
```

Shows numbered inbox items, then accepts batch commands like:
- `d 1,3,5` - Delete items 1, 3, and 5
- `s 2,4` - Move items 2 and 4 to someday/maybe
- `x 6` - Mark item 6 as done

### Weekly Review

```
/review
```

Guided 8-step review covering inbox, projects, goals, and planning.

## CLI Commands Reference

The slash commands use the `ace` CLI under the hood. You can also use it directly:

```bash
# Authentication
ace auth login          # Authenticate with Ace
ace auth status         # Check auth status

# Actions
ace actions inbox       # List inbox items
ace actions list        # List all actions
ace actions add "Task"  # Create new action
ace actions complete ID # Mark action complete

# Projects
ace projects active     # List active projects
ace projects list       # List all projects
ace projects review     # Projects due for review

# Goals
ace goals active        # List active goals
ace goals list          # List all goals

# Dashboard
ace orient              # Daily orientation
ace orient --json       # JSON output for AI
```

Add `--json` to any command for machine-readable output.

## Customization

### Adding Your Own Commands

Create new markdown files in `.claude/commands/` to add custom slash commands. See the existing commands for the expected format.

### Using in Any Directory

You can copy the `.claude/` directory to any project to have Ace commands available there:

```bash
cp -r .claude ~/my-other-project/
```

## Concepts

### Statuses

**Actions:**
- `inbox` - Uncaptured, needs processing
- `active` - Ready to work on
- `waiting` - Blocked on someone/something
- `someday` - Might do eventually
- `completed` / `dropped`

**Projects:**
- `active` - Currently working on (max 3 per domain)
- `next_up` - Ready when capacity available
- `incubating` - Needs more thinking
- `someday_maybe` - Might do someday
- `completed` / `dropped` / `merged`

**Goals:**
- `active` - Currently pursuing
- `paused` - On hold
- `completed` / `dropped`

### Domains

Ace separates work and personal:
- **Work** - Professional projects and tasks
- **Personal** - Everything else

Each domain has a soft limit of 3 active projects to maintain focus.

### Contexts

Contexts like `@home`, `@phone`, `@low-energy` help filter actions based on your current situation.

## Troubleshooting

### "Not authenticated" errors

Run `ace auth login` to authenticate, or `ace auth status` to check your current auth state.

### Commands not found

Make sure you're in a directory with the `.claude/commands/` folder, or that Claude Code can find it.

### API errors

Check your internet connection. The CLI communicates with `https://aceisyourassistant.com/api`.

## Links

- [Ace Web App](https://aceisyourassistant.com)
- [Ace CLI on npm](https://www.npmjs.com/package/ace-assistant-cli)
- [Claude Code Documentation](https://docs.anthropic.com/en/docs/claude-code)

## License

MIT
