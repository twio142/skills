---
name: obsidian-cli
description: Reference for the obsidian CLI — syntax, file targeting, and common commands. Requires an open Obsidian window.
---

# Obsidian CLI

## Common patterns

```bash
# Get the currently active file
obsidian file active
# Get the current editor selection (always includes the active file path)
obsidian eval code="const v=app.workspace.activeLeaf.view; console.log(v.file?.path+'\n'+v.editor.getSelection())"
# List all tags, sorted by frequency
obsidian tags sort=count counts
# Set a frontmatter property on a note
obsidian property:set name="status" value="done" file="My Note"
# Create a note from a template without opening it
obsidian create name="New Note" content="# Hello" template="Template" silent
# List backlinks to a note
obsidian backlinks file="My Note"
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