# Smithysoft CC Plugins

This repository contains shared Claude Code configuration for the Smithysoft organization:
custom slash commands, hooks, and settings that every engineer can pull in.

## Structure

```
.claude/
  commands/     # Custom slash commands — one .md file per command
  settings.json # Shared permissions and hook definitions
CLAUDE.md       # This file — Claude reads it automatically
```

## Using this repo

### Option 1 — symlink into a project
```sh
ln -s /path/to/cc-plugins/.claude /path/to/your-project/.claude
```

### Option 2 — copy selectively
Copy individual files from `.claude/commands/` into your project's `.claude/commands/`.

## Adding a command

1. Create `.claude/commands/<command-name>.md`
2. Write a clear prompt describing what the command should do
3. Open a PR — include a short description of when to use it

## Adding a hook

Edit `.claude/settings.json` and add your hook under `"hooks"`.
See the [Claude Code hooks docs](https://docs.anthropic.com/en/docs/claude-code/hooks) for the schema.

## Conventions

- Command files use kebab-case names (`pr-review.md` → `/pr-review`)
- Keep commands focused — one job per command
- No secrets, credentials, or personal config committed here
