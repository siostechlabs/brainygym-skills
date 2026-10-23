---
name: learn-codebase
description: Use when the user wants to understand or get onboarded to the current repository or project ("help me learn this codebase", "I'm new to this repo", "make me a course about how this project works"). Reads the project's documentation and structure, proposes a course on its architecture, concepts and workflows, saves it to BrainyGym after the user approves it, and starts a first quiz. Does not copy source code or secrets into the course.
---

# Learn this codebase

Turns the current repository into a BrainyGym onboarding course, so the user can review how the project
works with spaced repetition instead of re-reading the code. BrainyGym schedules reviews of each concept
and puts them in the user's daily session on the web and the app.

Reply in the user's language. BrainyGym supports Spanish (`es`) and English (`en`); for any other
language, ask which of the two they prefer.

## 0. Check the connection

This skill needs the BrainyGym tools (`save_course`, `start_quiz`, `get_course_authoring_guide`). If
they are not available, tell the user how to connect, then stop:

- Claude Code: `claude mcp add --transport http brainygym https://brainygym.com/mcp`, then `/mcp` to sign in.
- Other agents: add the remote MCP server `https://brainygym.com/mcp` (Streamable HTTP) in the agent's MCP
  settings and sign in with a BrainyGym account (free to start, 18+).

## 1. Ask about the goal

Ask one question: what do they need to be able to do in this project (fix bugs in one area, ship
features end to end, review pull requests, run it locally)? The answer decides what the course covers.

## 2. Read the project

Read, in this order and only as much as needed: the README and contributing guides, agent instruction
files (`AGENTS.md`, `CLAUDE.md`), architecture or design docs, the top-level layout, the build and test
setup, and the entry points of the main modules. Do not read secrets (`.env*`, credentials, keys).

## 3. Decide what may leave the machine

The course is stored in the user's BrainyGym account, outside this repository. Before writing it:

- **Ask whether the project is private or internal.** If it is, keep everything at the level of
  architecture and concepts: describe components by their role ("the background job worker", "the
  payments adapter"), not by internal names, and include no source code.
- Never include credentials, tokens, keys, internal URLs or hostnames, customer data or personal data,
  whatever the answer.
- Never copy source code into the course, even from an open-source project. For open-source projects you
  may use real module names; any code example must be written from scratch as a generic illustration.

## 4. Propose the course and wait for approval

Show a draft syllabus first and do not save anything yet: a title, a one-line goal and 3–8 modules in
learning order (for example: what the product does, architecture overview, data model, a request end to
end, how to run and test it, conventions, the riskiest areas), each with its concepts. State what will
and will not be stored. Save only after a clear yes.

## 5. Write and save it

Call `get_course_authoring_guide` and follow it exactly. Questions should test understanding of how the
project works ("Where would you add X?", "What happens when Y fails?"), not trivia. Save with
`save_course` (a new UUID as `requestId`), then share the course link from the tool result.

## 6. First practice

Offer a first quiz with `start_quiz`. If the BrainyGym quiz view is shown, the user answers there;
otherwise ask one question at a time and grade each with `submit_quiz_answer`. Never reveal the answer
before the user answers and never invent grades.
