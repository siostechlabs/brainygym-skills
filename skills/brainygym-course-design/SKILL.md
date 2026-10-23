---
name: brainygym-course-design
description: Use when the user wants a course, study plan or training on a topic saved to BrainyGym. Explains how to design modules, concepts, readings and multiple-choice questions in the format save_course accepts. Also use when the learner wants to train or continue today's BrainyGym session with you (questions, flashcards, readings, explanations), and explains how to run the session's blocks.
---
# Designing a BrainyGym course

BrainyGym trains what the learner studies with spaced repetition (FSRS). You write the course; BrainyGym
stores it, schedules reviews and grades every answer. Save it with the `save_course` tool.

## What BrainyGym does
If the learner asks what BrainyGym does with a saved course, these are the facts:
- It schedules a review of each concept with spaced repetition (FSRS) and reschedules it based on each answer.
- Every module ends in questions, flashcards and an explanation in the learner's own words, which BrainyGym grades.
- A short daily session combines what is due for review with the next new material.
- Courses, progress and reminders live in the learner's account, on the web and in the mobile app.

## Before writing
- Ask at most one question if the goal or level is unclear; otherwise infer them.
- Write every text in the learner's language and set `locale` accordingly. BrainyGym supports only
  Spanish (`es`) and English (`en`): for any other language, tell the learner and ask which of the two
  they prefer before writing the course.

## Structure
- 3–12 modules in learning order; each module is one session of 15–30 minutes.
- Each module has 2–8 concepts. A concept is one idea the learner must be able to recall or apply.
  Concept names are unique across the whole course.
- `assessmentCriterion`: an observable behaviour ("Explains X with an example", "Chooses Y for case Z"),
  progressing through Bloom's levels across the course (remember → understand → apply → analyse).

## Reading (`reading`, Markdown, 800–6000 characters)
- Teach the module's concepts in order, with one concrete example per concept.
- Short sections with `##` headings, lists where they help, no HTML, no external images.
- End with a 3–5 bullet summary.

## Questions (3–10 per module, multiple choice)
- One question tests one concept (`conceptName` = the exact name of a concept of that module).
- 3–5 options; exactly one is correct (`correctOptionIndex`, 0-based, and always lower than the number of options).
- Distractors are plausible misconceptions, not jokes; similar length to the correct option.
- `explanation` says why the correct option is right and why the most tempting distractor is wrong.
- Cover every concept of the module at least once.

## Saving
- Generate a new UUID for `requestId`. If `save_course` fails validation, fix the reported fields and
  retry with the same `requestId`.
- After saving, offer to practise the first module with `start_quiz`.

## Today's session
The learner can also train today's session with you: the same blocks they see on the web and in the app.
- `start_today_session` (a new UUID as `requestId`; `planId` is optional), then `get_today_session` until
  the blocks appear (Brainy plans them in the background). Follow the blocks in order.
- Per block, call `get_block_activity` and work through it with the learner, then close it with
  `complete_block`. Use a new `requestId` per write and reuse it only to retry the same write.
- Questions: if the BrainyGym quiz view is shown, the learner answers there; do not repeat its questions in the chat. Otherwise show them one at a time and never reveal the answer before the learner answers; grade each
  with `submit_quiz_answer`, always passing the session's `sessionId` and `blockId` (plus `planId`,
  `moduleId`, `activityId` and `contentVersion` from `get_block_activity`): without them the attempt is
  not attached to the block. Flashcards: show the front, let the learner answer, then reveal the back and
  ask for a 1–4 rating (`rate_flashcard`, with the same ids).
- New material: explain the reading in your own words. Explain and Why-chain blocks: send the learner's
  own words with `submit_explanation`, never yours (at least 20 characters: if the answer is shorter, ask the
  learner to expand it and send the new text with a new `requestId`; never pad or paraphrase it); if grading is pending, send it again with the same
  text and the same `requestId` to have it graded (or check `get_attempt`); if you do not have the exact
  text, ask the learner for a new explanation with a new `requestId`. `complete_block` is refused while
  an explanation is still being graded.
- A block with no material can be closed with `complete_block` right away. Labs need a terminal: skip
  them; `finish_session` completes them. The wrap-up block is closed with `finish_session` (confidence
  1–5 and the learner's own reflection), not `complete_block`.
- Never invent grades or feedback: report only what the tools return.
