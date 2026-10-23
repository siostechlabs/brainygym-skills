# BrainyGym skills for AI assistants and coding agents

**Remember what you learn with AI.** Your AI explains things and writes code for you; a week later most of
it is gone. BrainyGym turns it into a short course and brings each concept back for review right before
you would forget it (spaced repetition), in a daily session on the web and the mobile app.

This repository has the BrainyGym skills, the plugin for Claude Code, and the settings to connect the
BrainyGym server to any agent that supports remote MCP.

## Skills

| Skill | Use it when you say… | What it does |
| --- | --- | --- |
| `learn-from-session` | "Teach me what we just did" | Turns the concepts behind your coding session (technologies, decisions, bugs and fixes) into a short course, after you approve the syllabus. No code or secrets are sent to BrainyGym. |
| `learn-codebase` | "Help me learn this repo" | Builds an onboarding course on how the current project works: architecture, data model, a request end to end, how to run and test it. |
| `brainygym-course-design` | "Make me a course on X and save it to BrainyGym" | Designs a course (modules, readings, questions) in the format BrainyGym stores, and runs today's session with you. |

Commands in Claude Code: `/brainygym:curso <topic>`, `/brainygym:repasar` (reviews due today) and
`/brainygym:hoy` (today's session).

## Install

**Fastest: let your agent do it.** Paste this into Claude Code, Codex, Cursor or any other agent:

```
Read https://brainygym.com/install.md and set me up with BrainyGym.
```

### Claude Code

```
/plugin marketplace add siostechlabs/brainygym-skills
/plugin install brainygym@brainygym
```

The plugin includes the skills, the commands and the BrainyGym server. Run `/mcp` and sign in with your
BrainyGym account (free to start).

### Other agents (Codex, Cursor, Gemini CLI, GitHub Copilot, and 70+ more)

1. Install the skills:

   ```
   npx skills add siostechlabs/brainygym-skills
   ```

2. Add the BrainyGym server to your agent's MCP settings and sign in when asked:

   - URL: `https://brainygym.com/mcp`
   - Transport: Streamable HTTP, OAuth sign-in

   For example, in Cursor (`.cursor/mcp.json`) or any agent that uses the same format:

   ```json
   { "mcpServers": { "brainygym": { "url": "https://brainygym.com/mcp" } } }
   ```

### Claude and ChatGPT apps

Add BrainyGym as a connector: see [brainygym.com/en/connect-your-ai](https://brainygym.com/en/connect-your-ai).

## Privacy

The skills never send source code, file contents, secrets or project-internal names to BrainyGym: they
write general concepts and always show you the syllabus before saving. The BrainyGym server only receives
what each action needs, never your conversation. See the
[privacy policy](https://brainygym.com/en/privacy) and the [connector docs](https://brainygym.com/en/claude).

## Support

Write to [support@brainygym.com](mailto:support@brainygym.com).

## License

MIT. See [LICENSE](LICENSE).
