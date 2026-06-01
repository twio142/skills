---
name: obsidian
description: Interact with the user's Obsidian vault using the obsidian CLI. Use this skill whenever the user references their notes or vault, wants to read/search/create/update notes, manage tasks, find backlinks, search by tag, or save a web article to the vault.
allowed-tools:
  - Bash(obsidian *)
---

# Obsidian

To save a web article to the vault, read [rules/save-article.md](rules/save-article.md) and follow those instructions.

## Useful CLI examples

Get the currently active file:
```
obsidian file active
```

Get the current editor selection (always includes the active file path for context):
```
obsidian eval code="const v=app.workspace.activeLeaf.view; console.log(v.file?.path+'\n'+v.editor.getSelection())"
```

## Command reference

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

## Common patterns

```bash
obsidian read file="My Note"
obsidian create name="New Note" content="# Hello" template="Template" silent
obsidian append file="My Note" content="New line"
obsidian search query="search term" limit=10
obsidian daily:read
obsidian daily:append content="- [ ] New task"
obsidian property:set name="status" value="done" file="My Note"
obsidian tasks daily todo
obsidian tags sort=count counts
obsidian backlinks file="My Note"
```

Use `--copy` to copy output to clipboard, `silent` to prevent files from opening, `total` on list commands for a count.

---

## When to use the CLI vs direct file actions

If the session is started from **inside the vault directory**, prefer `Read`, `Edit`, `Write`, `Glob`, and `Grep` for reading and writing content. Only reach for the `obsidian` CLI when the task specifically requires Obsidian's own machinery.
