---
name: obsidian-save-article
description: Use this skill when the user gives a URL and asks to save or clip the article to their Obsidian vault.
---

# Save Article to Obsidian

Save a web article as a clean Obsidian note using Defuddle to extract content.

## Steps

1. **Fetch** the article:

    ```bash
    defuddle parse <url> --md
    ```

    If defuddle fails or returns no meaningful content, stop and tell the user.

2. **Derive a filename** from the article title:
    - Replace colons (`:`) with ` -`
    - Remove slashes (`/`, `\`) entirely
    - Keep concise (trim subtitle if very long)
    - If no clear title, derive from the URL slug

3. **Format the note**:

    ```markdown
    ---
    tags:
      - <one or two relevant topic tags>
    url: <original article URL>
    ---
    # <Article Title>

    > <Author, if known> · <Publication date, if known>

    <article body verbatim — do not rewrite or paraphrase>
    ```

    Tags: lowercase single words (e.g. `ai`, `coding`, `design`). Use `-` for list items, 4-space indentation.

4. **Save**:
    - If the cwd is the vault root (contains a `CLAUDE.md` describing an Obsidian vault), use the Write tool directly.
    - Otherwise, use the `obsidian` CLI to save to the vault.

5. **After saving**, tell the user the filename, then ask:
    - Question: "What would you like to do next?"
    - Header: "Open"
    - Options:
        - "Open in Obsidian" — `obsidian://open?vault=Markdown&file=<encoded filename>`
        - "Open in editor" — `nvim-agent -- "<filepath>"`
        - "Nothing"