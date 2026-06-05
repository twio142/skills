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

## MCP Servers

| Server          | Description                                                |
| --------------- | ---------------------------------------------------------- |
| jina-mcp-server | Web access and content retrieval (requires `JINA_API_KEY`) |
| deepwiki        | Deep wiki search via `mcp-deepwiki`                        |

## Agents

| Agent | Description                                                                               |
| ----- | ----------------------------------------------------------------------------------------- |
| vault | Dedicated agent for searching, reading, creating, and editing notes in the Obsidian vault |

## Skills

| Skill    | Description                                                                                      |
| -------- | ------------------------------------------------------------------------------------------------ |
| editor   | Interaction with my editor                                                                       |
| obsidian | Interaction with my Obsidian vault; includes saving web articles                                 |
| media    | Discuss or process online content: YouTube summaries, media transcripts, and browser tab reading |
| defuddle | Extract clean markdown from a URL                                                                |
