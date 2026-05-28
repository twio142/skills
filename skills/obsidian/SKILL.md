---
name: obsidian
description: Interact with the user's Obsidian vault using the obsidian CLI. Use this skill whenever the user references their notes or vault, wants to read/search/create/update notes, manage tasks, find backlinks, or search by tag.
allowed-tools:
  - Bash(obsidian *)
---

# Obsidian

**Useful CLI examples:**

Get the currently active file:
```
obsidian file active
```

Get the current editor selection (always includes the active file path for context):
```
obsidian eval code="const v=app.workspace.activeLeaf.view; console.log(v.file?.path+'\n'+v.editor.getSelection())"
```

---

**When to use the CLI vs direct file actions:**

If the session is started from inside the vault directory, the notes are local files — always prefer `Read`, `Edit`, `Write`, `Glob`, and `Grep` for reading and writing content. They're faster, don't require Obsidian to be running, and keep the context clean.

Only reach for the `obsidian` CLI when the task specifically requires Obsidian's own machinery:
- Resolving wikilinks or backlinks
- Finding notes by tag
- Listing tasks across the vault or a subtree (when you don't have exact locations)
- Triggering plugin commands
- Targeting a vault by name when not working from its directory
- Getting the currently active file or editor state

If you already know the file path — for tasks, properties, or anything else — just edit the file directly.

When in doubt: if you can do it with a file tool, do it with a file tool.
