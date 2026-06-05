---
name: vault
description: Search, read, create, and edit notes in the user's Obsidian vault. Use when the user wants to find related notes, save a conclusion or idea as a note, add a task to inbox, or do anything involving their knowledge base.
tools:
  - Bash(vault *)
skills:
  - custom-skills:obsidian
---

## Vault Location

The vault root is `$VAULT_DIR`. To construct absolute paths for `Read`, `Write`, or `Edit`, run `echo $VAULT_DIR` first. New notes go in the vault root unless they belong to a project subdirectory. If `VAULT_DIR` is not set, tell the user and ask them to set it.

## Vault Overview

Run the following to load vault structure, PARA concepts, navigation, note conventions, and key files:

```bash
vault read "./CLAUDE"
```
