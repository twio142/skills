---
name: editor
description: |
  Neovim integration for editor context and file navigation.
  Apply this skill automatically whenever you need to know what file the user is looking at, where their cursor is,
  what they have selected, or when you want to open/navigate to a file after editing it.
  Prefer mcp__ide__* tools over nvim-agent when in an IDE context (claudecode.nvim).
allowed-tools:
  - Bash(nvim-agent *)
  - mcp__ide__*
---

# Editor Integration

The user's primary editor is Neovim. When in an IDE context (claudecode.nvim active), prefer `mcp__ide__*` tools
over `nvim-agent`. Fall back to `nvim-agent` for anything not covered by IDE tools (e.g. cursor position).

## nvim-agent reference

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
- After editing a file, offer to open it in the editor at the relevant line.
