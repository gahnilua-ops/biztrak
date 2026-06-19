---
name: telegram-brain
description: Core “brain” for a Telegram bot. Receives a message payload, runs the chosen TutPug mode (or a chat response), and returns the reply text.
category: integration
required services: none
---

## Input payload (sent by a Telegram‑Bot wrapper)

- **chat_id** (integer) – Telegram chat identifier to reply to.
- **user_id** (integer) – Telegram sender identifier (optional, for personalization).
- **text** (string) – The message the user typed.
- **mode** (string, optional) – `quiz`, `explain`, `review`, `quest`, `brain`, `mindset`, `logic`. If omitted, defaults to a friendly chat reply.
- **extra** (object, optional) – Any extra data the wrapper wants to forward.

## Processing logic

1. Detect language (Tagalog vs English) by scanning for common Tagalog words.
2. Build a system prompt: “You are TutPug … reply in English unless Tagalog”. If mode supplied: “Use the ‘<mode>’ study mode.”.
3. Call the active model (Groq/OpenRouter) with system + user text.
4. Return JSON: { "reply_text": "<answer>", "reply_markup": null }

