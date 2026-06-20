---
name: telegram-brain
description: Core "brain" for a Telegram bot. Accepts a message payload, runs the chosen TutPug mode (or a chat response), and returns the reply text.
category: integration
required_services: none
---

## Input payload (sent by a Telegram‑Bot wrapper)

- **chat_id** (integer) – Telegram chat identifier to reply to.
- **user_id** (integer) – Telegram sender identifier (optional, for personalization).
- **text** (string) – The message the user typed.
- **mode** (string, optional) – `quiz`, `explain`, `review`, `quest`, `brain`, `mindset`, `logic`. If omitted, defaults to a friendly chat reply.
- **extra** (object, optional) – Any extra data the wrapper wants to forward.

## Processing logic

1. Detect language (Tagalog vs English) by scanning for common Tagalog words.
2. Build a system prompt:
   ```
   You are TutPug, a warm, playful pug‑dog tutor. Reply in English unless the user wrote Tagalog.
   ```
   If `mode` is supplied, append "Use the ‘<mode>’ study mode." to the prompt.
3. Call the active model (Groq / OpenRouter) with the system prompt and the user message.
4. Return a JSON payload:

```json
{
  "reply_text": "<generated reply>",
  "reply_markup": null   // optional InlineKeyboard JSON
}
```

## Example pseudo‑implementation (for reference)
```python
def telegram_brain(request):
    data = request.json
    text = data.get("text", "")
    mode = data.get("mode")
    lang = "tl" if any(w in text.lower() for w in ["po", "salamat", "kaibigan"]) else "en"

    system = "You are TutPug, a warm, playful pug‑dog tutor. "
    system += f"Reply in {'Tagalog' if lang == 'tl' else 'English'}."
    if mode:
        system += f" Use the '{mode}' study mode."

    prompt = [{"role": "system", "content": system},
              {"role": "user",   "content": text}]

    reply = run_prompt(prompt)  # Kai built‑in function
    return {"reply_text": reply, "reply_markup": None}
```

## Hooking up a Telegram wrapper
1. Deploy this skill in Kai (Skill → Import).
2. Write a tiny Python wrapper (see previous message) that forwards `{chat_id, user_id, text, mode}` to `https://<kai-host>/skill/telegram-brain` and sends `reply_text` back via `sendMessage`.

