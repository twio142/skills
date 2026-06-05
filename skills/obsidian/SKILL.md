---
name: obsidian
description: Interact with the user's Obsidian vault using the obsidian CLI. Use this skill whenever the user references their notes or vault, wants to read/search/create/update notes, manage tasks, find backlinks, search by tag, or save a web article to the vault.
allowed-tools:
  - Bash(obsidian *)
  - Bash(vault *)
---

# Obsidian

## When to use which tool

The `obsidian` CLI requires an open Obsidian window. Prefer alternatives when possible:

- **Reading/writing content**: If inside the vault directory, use `Read`, `Edit`, `Write`, `Glob`, `Grep` directly.
- **Searching/reading notes**: Always prefer `vault` — it's semantic and works without Obsidian open. See [rules/vault-cli.md](rules/vault-cli.md).
- **`obsidian` CLI**: Only when the task requires Obsidian's own machinery: tags, properties, tasks, creating from a template.

To save a web article to the vault, follow [rules/save-article.md](rules/save-article.md).

## Useful CLI examples

Get the currently active file:
```
obsidian file active
```

Get the current editor selection (always includes the active file path for context):
```
obsidian eval code="const v=app.workspace.activeLeaf.view; console.log(v.file?.path+'\n'+v.editor.getSelection())"
```

## Common patterns

```bash
obsidian tags sort=count counts
obsidian property:set name="status" value="done" file="My Note"
obsidian create name="New Note" content="# Hello" template="Template" silent
obsidian backlinks file="My Note" # prefer `vault neighbors`
obsidian search query="search term" limit=10 # prefer `vault search`
obsidian read file="My Note" # prefer `vault read` or local file read
obsidian append file="My Note" content="New line" # prefer local file write
```

Use `--copy` to copy output to clipboard, `silent` to prevent files from opening, `total` on list commands for a count.

Run `obsidian help` for all available commands.

## Syntax

Parameters use `=`, flags are bare. Quote values with spaces. Use `\n`/`\t` for multiline content:

```bash
obsidian create name="My Note" content="Hello\nWorld" silent overwrite
```

## File targeting

- `file=<name>` — wikilink-style (name only, no path or extension)
- `path=<path>` — exact path from vault root
- Omit both to target the active file

Use `vault=<name>` as the first parameter to target a specific vault.
