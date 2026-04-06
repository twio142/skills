---
name: obsidian
description: |
  Interact with the user's Obsidian vault using the obsidian CLI. Use this skill whenever the user references their notes, vault, Obsidian, wants to read/search/create/update notes, manage tasks in their vault, find backlinks, search by tag, or do anything with their personal knowledge base. Trigger this even if the user doesn't say "Obsidian" explicitly — phrases like "my notes", "add a note", "search my vault", "check my tasks", "find notes about X", or "add to my knowledge base" all warrant this skill.
allowed-tools:
  - Bash(obsidian *)
---

# Obsidian

Interact with the user's active Obsidian vault via the `obsidian` CLI. The CLI talks directly to the running Obsidian app and is preferred over filesystem manipulation for anything that involves Obsidian's features (link resolution, tags, metadata, properties, plugins).

---

**When to use the CLI vs direct file actions:**

If the session is started from inside the vault directory, the notes are local files — always prefer `Read`, `Edit`, `Write`, `Glob`, and `Grep` for reading and writing content. They're faster, don't require Obsidian to be running, and keep the context clean.

Only reach for the `obsidian` CLI when the task specifically requires Obsidian's own machinery:
- Resolving wikilinks or backlinks
- Finding notes by tag
- Listing tasks across the vault or a subtree (when you don't have exact locations)
- Triggering plugin commands
- Targeting a vault by name when not working from its directory

If you already know the file path — for tasks, properties, or anything else — just edit the file directly.

When in doubt: if you can do it with a file tool, do it with a file tool.
