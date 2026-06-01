---
name: browser
description: Inspect the user's browser tabs. Use when the user wants to discuss what's currently open in their browser. Triggers on phrases like "what's on this page", "check my browser", or "read the current tab" — NOT when they provide a URL to fetch.
---

# Browser Tab Inspector

Omit `--browser` to use the system default.

## Commands

```
browser-cli list [--browser chrome|safari|arc]
    → JSON array of tabs: [{id (win:tab), title, url, active}]

browser-cli html [--browser chrome|safari|arc] [--tab <win>:<tab>]
    → raw HTML of the active (or specified) tab

browser-cli selection [--browser chrome|safari|arc]
    → JSON {title, url, selection} — selection is "" if nothing is selected

browser-cli screenshot [--browser arc] [--tab <win>:<tab>] [--output <path.png>]
    → Arc only — saves PNG to path (or clipboard if --output omitted)
```

## Workflow

| User wants | What to do |
|---|---|
| page content / summary | `html` → extract text (skip `<script>`, `<style>`, nav, footer) |
| selected text | `selection` → use `selection` field; fall back to `html` if empty |
| visual look | `screenshot --output /tmp/browser-tab-$(date +%s).png` → read PNG; fall back to `html` if unsupported |
| which tab | `list` → report active tab title + URL |

## Error handling

- **Browser not running**: exits non-zero — tell the user to open it.
- **No active tab**: `list` returns empty — ask the user to open a tab.
