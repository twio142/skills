---
name: editor
description: Neovim integration to get editor context (current file, cursor, selection) or navigate to a file after editing.
allowed-tools:
  - Bash(nvim-agent *)
  - mcp__ide__*
---

# Editor Integration

The user's primary editor is Neovim.
When in an IDE context, prefer `mcp__ide__*` tools.
Fall back to `nvim-agent` for anything not covered by IDE tools (e.g. cursor position).

## `nvim-agent` reference

```bash
nvim-agent filepath                       # Current buffer's file path
nvim-agent cursor_position                # Cursor position (no IDE equivalent)
nvim-agent visual_selection               # Current selection
nvim-agent diagnostics                    # Buffer diagnostics
nvim-agent quickfix                       # Quickfix items
nvim-agent filepath,cursor_position       # Multiple contexts (comma-separated)
nvim-agent -- <file>                      # Open file
nvim-agent -- <file> +<line>              # Open file at specific line
nvim-agent -- -d file1 file2              # Open diff view
```

## Notes

- Always retrieve the current file path alongside cursor position, selection, or diagnostics for context.
- When the user asks about their cursor position, read the file content at that position.
