---
name: obsidian
description: Interact with the user's Obsidian vault. Use this skill whenever the user references their notes or vault, wants to read/search/create/update notes, manage tasks, find backlinks, search by tag, or save a web article to the vault.
allowed-tools:
  - Bash(obsidian *)
  - Bash(vault *)
---

# Obsidian

## When to use which tool

The `obsidian` CLI requires an open Obsidian window. Prefer alternatives when possible:

- **Searching/navigating notes**: Use `vault` — semantic and works without Obsidian open. See [rules/vault-cli.md](rules/vault-cli.md).
- **Reading notes**: If inside the vault directory, use `Read` directly; otherwise use `vault read`.
- **Writing/editing content**: Use `Edit`, `Write`, `Glob`, `Grep` directly.
- **`obsidian` CLI**: Only when the task requires Obsidian's own machinery: tags, properties, tasks, creating from a template, or interacting with the active file/editor state. See [rules/obsidian-cli.md](rules/obsidian-cli.md).

To save a web article to the vault, follow [rules/save-article.md](rules/save-article.md).
