---
name: browser-tab
description: Read and discuss the user's browser tabs. Use this skill whenever the user wants Claude to look at, read, summarize, or discuss what's currently open in their browser. Triggers on phrases like "look at my tab", "what's on this page", "read this page", "can you see my browser", "what am I looking at", "summarize this tab", "check my browser", "what does this page say", "read the current tab", or any time the user shares context suggesting they want Claude to see their browser content.
allowed-tools:
  - Bash(browser-cli *)
---

# Browser Tab Skill

This skill reads the user's browser tabs.

## Commands

```
browser-cli list [--browser chrome|safari|arc]
    → JSON array of tabs: [{id (win:tab), title, url, active}]

browser-cli html [--browser chrome|safari|arc] [--tab <win>:<tab>]
    → raw HTML of the active tab (or specified tab)

browser-cli screenshot [--browser arc] [--tab <win>:<tab>] [--output <path.png>]
    → Arc only — saves PNG to path (or clipboard if --output omitted)
```

Omit `--browser` to use the system default browser (do this by default).

## Default workflow

When the user wants you to see their active tab:

1. **Get context** — run `browser-cli list` to find the active tab (title + URL). This is fast and always works.

2. **Read content** — run `browser-cli html` to get the page HTML. Extract the meaningful text from the HTML (ignore `<script>`, `<style>`, nav boilerplate) and use it to answer the user's question.

3. **Screenshot** — if the user asks to *see* the page visually (e.g. "look at", "show me", "what does it look like"), run:
   ```
   browser-cli screenshot --output /tmp/browser-tab-$(date +%s).png
   ```
   If it fails (unsupported browser), fall back to `html`. Otherwise read the PNG with your image tool.

## Choosing the right approach

| User says | What to do |
|-----------|-----------|
| "what's on this page" / "summarize" | `html` → extract text → summarize |
| "read this" / "look at my tab" | `html` → extract text → engage |
| "what does this page look like" | `screenshot` (Arc) or `html` fallback |
| "which tab am I on" | `list` → report active tab title + URL |

## HTML extraction

If a skill for extracting clean content from raw HTML is available, use it. Otherwise, focus on:
- `<title>`, `<h1>`–`<h3>` for structure
- `<article>`, `<main>`, `<p>` for body content
- Skip `<nav>`, `<footer>`, `<script>`, `<style>`, `<svg>`

If the HTML is very large, summarize the key content rather than dumping raw text.

## Error handling

- **Browser not running**: the command exits non-zero with a message — tell the user which browser to open.
- **Screenshot on non-Arc**: falls back gracefully — use `html` instead.
- **No active tab**: `list` returns an empty array — ask the user to open a tab.
