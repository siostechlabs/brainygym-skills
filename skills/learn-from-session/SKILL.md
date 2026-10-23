---
name: learn-from-session
description: Use when the user wants to learn or remember what was done in this coding session ("teach me what we just did", "turn this session into a course", "I don't want to forget how this works", "quiz me on what you changed"). Extracts the general concepts the session relied on (technologies, patterns, decisions, bugs and their fixes), proposes a short course, saves it to BrainyGym after the user approves it, and starts a first quiz. Never sends code, secrets or project-internal names.
---

# Learn from this coding session

Coding agents ship code faster than people absorb it. This skill turns what happened in the current
session into a short BrainyGym course, so the user keeps the knowledge: BrainyGym schedules reviews of
each concept with spaced repetition and puts them in the user's daily session on the web and the app.

Reply in the user's language. BrainyGym supports Spanish (`es`) and English (`en`); for any other
language, ask which of the two they prefer.

## 0. Check the connection

This skill needs the BrainyGym tools (`save_course`, `start_quiz`, `get_course_authoring_guide`). If
they are not available, tell the user how to connect, then stop:

- Claude Code: `claude mcp add --transport http brainygym https://brainygym.com/mcp`, then `/mcp` to sign in.
- Other agents: add the remote MCP server `https://brainygym.com/mcp` (Streamable HTTP) in the agent's MCP
  settings and sign in with a BrainyGym account (free to start, 18+).

## 1. Extract concepts, not code

Look back over the session and list what the user would need to understand to do this work again
without you. Good concepts are general and reusable:

- the technologies, libraries and APIs that were used, and why;
- patterns and decisions (and the alternative that was rejected);
- the bugs that came up: the symptom, the cause and how it was diagnosed;
- commands and tools that mattered (in general form).

**Privacy rules (mandatory):**

- Never include source code, file contents, diffs, logs, stack traces, credentials, tokens, keys,
  URLs of internal systems, customer data or personal data.
- Never include project-internal names: repository, company, service, class, function, table or file
  names. Rewrite every concept in general terms ("a Next.js route handler that streams a response", not
  "`app/api/foo/route.ts` in Acme's billing service").
- Short illustrative snippets are fine only if you write them from scratch as generic examples.
- If the session was about something sensitive (security vulnerabilities, unreleased products), say so
  and ask before going on.

## 2. Propose the course and wait for approval

Show the user a draft syllabus first and do not save anything yet:

- a course title and a one-line goal ("Understand X so I can do Y on my own");
- 2–5 modules in learning order, each with 2–5 concepts and one sentence on what it covers;
- a note that nothing from the code itself will be stored.

Ask: "Shall I save this to BrainyGym?" Adjust it with their feedback. Save only after a clear yes.

## 3. Write and save it

Call `get_course_authoring_guide` and follow it exactly (module size, reading, questions, the format
`save_course` accepts). Keep the readings general, apply the privacy rules again to every reading,
question and explanation, and save with `save_course` (a new UUID as `requestId`).

Then tell the user what was saved, share the course link from the tool result, and offer to start right
away.

## 4. First practice

If the user agrees, call `start_quiz` with the `planId`. If the BrainyGym quiz view is shown, the user
answers there and you do not repeat the questions; otherwise ask them one at a time and grade each with
`submit_quiz_answer`. Never reveal the answer before the user answers and never invent grades.

Finish by mentioning that the reviews will come back in their BrainyGym daily session.
