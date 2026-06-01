---
name: media
description: Use when the user wants to discuss or process online content such as videos, podcasts, or web pages.
---

# Media Skill

Read the appropriate file based on the user's request, then follow its instructions.

| Request | File |
|---|---|
| YouTube URL + wants a summary, notes, or key takeaways | [rules/youtube-summary.md](rules/youtube-summary.md) |
| BiliBili or podcast URL (any intent); or YouTube URL without available subtitles; or any media URL where the user wants to load the full transcript into context for discussion or Q&A | [rules/transcript.md](rules/transcript.md) |
| No URL provided — user wants to discuss what's open in their browser ("what's on this page", "check my browser", "read the current tab") | [rules/browser.md](rules/browser.md) |
