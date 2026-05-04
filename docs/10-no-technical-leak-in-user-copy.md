# No technical leak in user copy

> User-facing error / fallback strings must NEVER expose technical state ("error", "rusak", "system", "API", "timeout"). Frame as social courtesy from the bot or agent persona — the bot asks for repetition, doesn't report a failure.

## The rule

Any string the user actually reads — bot reply, in-app toast, WhatsApp fallback, error modal, email body, push notification — must NEVER expose technical state.

**Banned vocabulary in user-facing strings:** `error`, `rusak`, `system`, `API`, `timeout`, `server`, `5xx`, `null`, `undefined`, `failed`, `exception`. Indonesian equivalents same: `gagal`, `eror`, `sistem`, `bermasalah`.

Frame instead as **social courtesy from the bot / agent persona**:

| Don't | Do |
|---|---|
| "Maaf, lagi error. Coba lagi sebentar." | "Maaf, bisa tolong ulangi pertanyaannya?" |
| "Ada error. Coba lagi ya." | "Maaf, aku belum nangkep. Coba ulangi?" |
| "System sedang bermasalah." | "Lagi banyak yang nanya barengan. Bentar ya." |
| "API timeout." | "Lambat banget responnya, coba lagi?" |
| "Booking gagal disimpan." | "Bookingnya belum tersimpan, mau coba lagi?" |
| English: "Error: User not found" | "Akun ini belum kita kenal — coba cek email-nya?" |
| English: "Failed to load data" | "Belum bisa ambil datanya barusan, coba refresh ya" |

## Why

- The user doesn't care there's a bug. What they experience is "the bot didn't respond." Technical framing leaks implementation detail and makes the product feel broken instead of polite.
- Bot voice persona: "knowledgeable, always-on, social". "Lagi error" violates that — the bot should never appear to "stop". Frame as the bot asking for repetition (still active, still social).
- Brand promise patterns ("we own the customer relationship", "trusted voice", "tinggal bilang, beres") — technical leak language breaks all of these in one string.

## Real incident that captured this rule

A bot's fallback message was shipped as "Maaf, lagi error. Coba lagi sebentar."

Founder corrected to: "Maaf, bisa tolong ulangi pertanyaannya?"

The follow-up audit caught two more leak sites in the same file (lines 185, 214) — same anti-pattern, same vocabulary. Sweep was atomic: one commit fixed all three sites in the same file.

The pattern was then recognized as cross-holding (applies to every product, not just the bot that triggered it).

## How to apply

### Code review trigger

Any change that adds or touches a string sent to a user surface (Telegram sendMessage, WhatsApp send, toast, alert, email body) — grep the diff for banned vocabulary, flag.

### New error path default

When wrapping any external call (LLM, payment gateway, external API, DB), the catch block's user-facing fallback uses the social-courtesy template, NOT the technical state.

Structured error log goes to stderr / Sentry / logs (visible to engineer), but NEVER to the user.

```
// ❌ Bad
catch (err) {
  await sendToUser(`Error: ${err.message}`)
}

// ✅ Good
catch (err) {
  console.error("[handler]", err)  // engineer sees specifics
  Sentry.captureException(err)
  await sendToUser("Maaf, lagi belum bisa proses. Coba lagi?")
}
```

### Scope

Applies even to admin / operator-facing surfaces if those surfaces also reach end customers (e.g., a partner dashboard that an operator screenshots and forwards to a guest).

### Exception: developer-only surfaces

Internal tools, console logs, Sentry, health-check endpoints, internal admin debug panels CAN and should use technical state — they exist to communicate failure precisely to engineers. The rule is about user-facing strings only.

## Constraints

- Don't translate "error" to "ada masalah" and call it done. "Ada masalah" is also a leak — it tells the user something is wrong with the system. Use the social-courtesy frame: ask for repetition, ask them to try again, frame as the bot's limitation in this moment (not the system being broken).
- Don't pass raw `error.message` to the user via templated fallback. Even if the message looks innocuous, it can leak: stack traces, file paths, table names, internal exception types.
- For payment / booking / financial state: extra care. "Booking gagal" is more loaded than "loading lambat" because it implies money / commitment lost. Always pair with concrete next action ("Coba lagi", "Pakai metode lain") so user knows what to do.

## Sibling rule

[`09-no-domain-jargon-user-facing.md`](09-no-domain-jargon-user-facing.md) covers builder-vocabulary leak ("PR", "operator", "asset"). This rule covers system-state leak ("error", "timeout"). Both fall under "audience is not the builder / engineer".

## Related

- [`09-no-domain-jargon-user-facing.md`](09-no-domain-jargon-user-facing.md) — sibling rule
- [`skills/voice-register-check.md`](../skills/voice-register-check.md) — operationalized scan for both leak classes
- [`14-bahasa-first-voice.md`](14-bahasa-first-voice.md) — voice register modes overall
