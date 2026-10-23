# BrainyGym: setup for AI agents

You are an AI agent and the user wants to use BrainyGym from this session. Follow these steps in order.
Pause only where the user has to act (typing a command you cannot run, or signing in in the browser).
Finish by doing something useful with BrainyGym and sharing the link it returns. Reply in the user's
language.

BrainyGym is a learning app: it stores courses, schedules reviews with spaced repetition and runs a short
daily session on the web and the mobile app. Its server is `https://brainygym.com/mcp` (remote MCP,
Streamable HTTP, OAuth). A BrainyGym account is free to start; users must be 18 or older.

## Step 1: Already connected?

If the BrainyGym tools are already available to you (for example `list_my_courses` or `save_course`),
skip to Step 4.

## Step 2: Install the skills and the server

**Claude Code.** `/plugin` is a command only the user can type. Ask the user to run:

```
/plugin marketplace add siostechlabs/brainygym-skills
/plugin install brainygym@brainygym
```

The plugin includes the server. Do not also run `npx skills add` (it would install a second copy).

**Any other agent.** Run, using your own agent's slug (for example `codex`, `cursor`, `gemini-cli`,
`github-copilot`, `windsurf`, `cline`):

```
npx skills add siostechlabs/brainygym-skills -g -a <your-agent-slug> -y
```

Then add the remote MCP server `https://brainygym.com/mcp` to your MCP configuration, in the format your
agent documents. If you cannot edit your own configuration, show the user the exact entry to add.

**Claude or ChatGPT apps (no terminal).** You cannot install anything there. Send the user to
https://brainygym.com/en/connect-your-ai and stop.

## Step 3: Sign in

The first call to a BrainyGym tool opens a BrainyGym sign-in page in the browser (in Claude Code, the user
can also run `/mcp`). Ask the user to sign in or create a free account, and wait until they confirm.

## Step 4: First result

Ask the user one short question and do what they choose:

- "Shall I turn what we just did in this session into a BrainyGym course?" (skill `learn-from-session`)
- "Shall I make you a course to learn this codebase?" (skill `learn-codebase`)
- "What topic do you want a course on?" (skill `brainygym-course-design`)

Follow the chosen skill. It always shows the syllabus and asks before saving. When the course is saved,
share the course link from the tool result and offer a first quiz.

Never send source code, secrets or project-internal names to BrainyGym.
