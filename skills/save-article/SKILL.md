---
name: save-article
description: Use this skill when the user gives a URL and asks to save, clip, or archive the article to the vault / notes.
allowed-tools:
  - Bash(obsidian *)
  - Bash(nvim-agent *)
---

# save-article

Save a web article as a clean Obsidian markdown note.

## Steps

1. **Fetch** the article using the `mcp__jina-mcp-server__read_url` tool with the URL the user provided.

2. **If the fetch fails or returns no meaningful content**, stop immediately and tell the user you couldn't retrieve the article. Do not proceed.

3. **Clean the content**: keep the article title, author, date (if present), and body text. Remove:
    - Navigation menus and headers/footers
    - Comment sections
    - "Related articles" / "You may also like" sections
    - Newsletter sign-up prompts
    - Social sharing buttons / calls to action
    - Ads or sponsor blocks
    - Any other boilerplate unrelated to the article content

4. **Derive a filename** from the article title:
    - Replace colons (`:`) with ` -`
    - Remove slashes (`/`, `\`) entirely
    - Keep the result concise (trim subtitle if very long)
    - Do not add a date prefix

5. **Format the note**:

    ```markdown
    ---
    tags:
      - <one or two relevant topic tags>
    url: <original article URL>
    ---
    # <Article Title>

    > <Author, if known> · <Publication date, if known>

    <cleaned article body, preserving headings and structure>
    ```

6. **Determine save location**:
    - If the current working directory is the vault root (contains a `CLAUDE.md` that describes an Obsidian vault), write the file directly using the Write tool.
    - Otherwise, use the `obsidian` CLI skill (`/obsidian`) to save the note to the vault.

7. **After saving**, tell the user the filename, then use the `AskUserQuestion` tool to ask:
    - Question: "What would you like to do next?"
    - Header: "Open"
    - Options:
        - "Open in Obsidian" — open with `obsidian://open?vault=Markdown&file=<encoded filename>`
        - "Open in editor" — run `nvim-agent -- "<filepath>"`
        - "Nothing" — done

## Notes

- Preserve the article's original wording — do not summarize or paraphrase.
- Use `-` for list items, 4-space indentation.
- If the article has no clear title, derive one from the URL slug.
- Topic tags should be lowercase single words (e.g. `ai`, `coding`, `design`, `psychology`). Use 1–2 tags only.
