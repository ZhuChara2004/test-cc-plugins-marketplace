Manage plugins from a cc-plugins-compatible marketplace: install, list, or inspect.

## Usage

- `/plugin install <name>@<source>` — copy a plugin into the current project
- `/plugin list [@<source>]` — list available plugins in a marketplace
- `/plugin info <name>@<source>` — show plugin manifest details

---

## Source resolution

Read `~/.claude-plugin/sources.json` to resolve `<source>` to a local path:
```json
{
  "cc-plugins": "/path/to/cc-plugins"
}
```
If the file doesn't exist or `<source>` is not registered, stop and tell the user:
> Source `<source>` is not registered. Add it to `~/.claude-plugin/sources.json`:
> `{ "<source>": "/absolute/path/to/repo" }`

---

## install

1. Resolve `<source>` → local path via `~/.claude-plugin/sources.json`
2. Read `<source-path>/.claude-plugin/marketplace.json` — verify `<name>` is listed
3. Read `<source-path>/plugins/<name>/.claude-plugin/plugin.json`
4. Copy files into the **current project**, creating directories as needed:
   - `plugins/<name>/commands/*.md` → `.claude/commands/`
   - `plugins/<name>/agents/*.md` → `.claude/agents/`
   - `plugins/<name>/skills/**` → `.claude/skills/`
5. If `plugin.json` has `hooks`: merge into the project's `.claude/settings.json` under `"hooks"`
6. If `plugin.json` has `mcpServers`: merge into `.claude/settings.json` under `"mcpServers"`
7. Confirm a summary of every file copied and every key merged.

If any destination file already exists, warn the user and ask before overwriting.

---

## list

1. Resolve `<source>` (if omitted, list all registered sources from `~/.claude-plugin/sources.json`)
2. Read `<source-path>/.claude-plugin/marketplace.json`
3. Print a table: **name**, **description**, **category**, **tags**

---

## info

1. Resolve `<source>` and read `<source-path>/plugins/<name>/.claude-plugin/plugin.json`
2. Print all fields: name, version, description, author, keywords
3. If optional fields are present (`hooks`, `mcpServers`, `lspServers`), list them with a short summary of what will be merged on install