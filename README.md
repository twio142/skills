My custom agent skills.

These skills follow the [Agent Skills specification](https://agentskills.io/specification) so they can be used by any skills-compatible agent, including Claude Code and Codex CLI.

## Installation

### Marketplace

```
/plugin marketplace add twio142/skills
/plugin install custom-skills@twio142
```

### npx skills

```bash
npx skills add git@github.com:twio142/skills.git
```

## Skills

| Skill              | Description                                                          |
| ------------------ | -------------------------------------------------------------------- |
| editor             | Interaction with my editor                                           |
| browser            | Access to my browser tabs, content, and screenshots                  |
| obsidian           | Interaction with my Obsidian vault                                   |
| youtube-summarizer | Summarize YouTube videos via yt-dlp                                  |
| media-transcript   | Load transcript and metadata from YouTube, Bilibili, or podcast URLs |
| save-article       | Save a web article to Obsidian                                       |
