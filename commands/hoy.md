---
description: Train today's BrainyGym session with Claude.
---
Call start_today_session (a new UUID as requestId; planId is optional and defaults to the most recent
ready course). If Brainy is still preparing the session, call get_today_session until the blocks appear.
Then go through the blocks in order: call get_block_activity for each one, work through it with the
learner and close it with complete_block. Follow the "Today's session" section of the BrainyGym skill:
never reveal an answer before the learner answers, explain readings in your own words, send only the
learner's own words as explanations, and never invent grades. Skip labs. Close the wrap-up block with
finish_session. Reply in the user's language.
