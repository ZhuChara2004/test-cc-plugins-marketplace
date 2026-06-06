# Smithysoft CC Plugins

Smithysoft's internal Claude Code plugin marketplace. Contains reusable slash commands,
agents, and skills that any project can install with a single `/plugin install` command.

## Repository Structure

```
.claude-plugin/
  marketplace.json          # registry of all plugins in this repo
.claude/
  commands/
    plugin.md               # /plugin install|list|info command
  settings.json
plugins/
  <plugin-name>/
    .claude-plugin/
      plugin.json           # required manifest
    README.md
    commands/<name>.md      # slash commands (/name)
    agents/<name>.md        # sub-agents
    skills/<name>/
      SKILL.md              # ambient skill definition
      references/           # supplemental docs
```

## One-time setup

Register this repo as a named marketplace source so `/plugin install` can find it:

```sh
mkdir -p ~/.claude-plugin
echo '{ "cc-plugins": "/path/to/cc-plugins" }' > ~/.claude-plugin/sources.json
```

Replace `/path/to/cc-plugins` with the absolute path where you cloned this repo.

Then copy the `/plugin` command into any project you want to use it from:

```sh
cp /path/to/cc-plugins/.claude/commands/plugin.md <your-project>/.claude/commands/plugin.md
```

After that, inside any project you can run:

```
/plugin install ruby@cc-plugins
/plugin list @cc-plugins
/plugin info ruby@cc-plugins
```

## Plugin manifest (`plugin.json`)

```json
{
  "name": "plugin-name",
  "version": "0.1.0",
  "description": "What the plugin does",
  "author": { "name": "Smithysoft" },
  "keywords": ["relevant", "tags"]
}
```

Optional fields: `hooks`, `mcpServers`, `lspServers` — merged into the target project's
`.claude/settings.json` on install.

## Adding a plugin

1. Create `plugins/<name>/` following the structure above
2. Add a `plugin.json` manifest
3. Add a `README.md` with usage and requirements
4. Register it in `.claude-plugin/marketplace.json` with `name`, `source`, `description`, `category`, and `tags`
5. Open a PR

## Conventions

- Plugin names are lowercase kebab-case
- One focused responsibility per plugin — broad plugins can expose multiple commands
- No secrets, credentials, or personal config committed here
