# Watson Live

**▶️ Live app: [https://mirrash7.github.io/Jeopardy/](https://mirrash7.github.io/Jeopardy/)**

A phone-friendly web app that reads a Jeopardy! clue from your camera or a photo and instantly gives you the correct response — phrased, of course, in the form of a question.

Point your phone at a clue, tap **Capture Frame**, and Watson Live reads the text on-device and returns the answer with a category, a confidence rating, and the exact text it read so you can tell whether it saw the clue correctly.

---

## Features

- **Two ways to scan** — a live camera mode with a capture button, or upload a photo.
- **Fast, on-device reading** — text is extracted in your browser (OCR), so only a few words of text are sent for an answer, not a whole image.
- **Answers in Jeopardy! format** — "What is…?" / "Who is…?", with category, confidence, and a one-line explanation.
- **Shows what it read** — every answer includes the captured clue text, so a misread is easy to spot.
- **Built for rapid play** — the camera stays live between captures, and your last answer stays on screen until the next one is ready.
- **Bring your own key** — you use your own Anthropic API key, so you pay only for your own usage.

---

## Getting started

1. Open the app: **[https://mirrash7.github.io/Jeopardy/](https://mirrash7.github.io/Jeopardy/)**
2. Tap the **🔑** button in the top-right corner.
3. Paste your Anthropic API key and tap **Save Key**. Tick **Remember on this device** if you want it to stick around between visits.
4. Choose **Live Camera** or **Upload Photo**, frame a clue, and capture it.

> The first scan in a session is a little slower while the text-reading engine loads. After that it's quick.

---

## Getting an API key

Watson Live calls the Anthropic API, which is billed separately from any Claude.ai subscription — a Claude Pro/Max plan does **not** include API credits.

1. Go to [console.anthropic.com](https://console.anthropic.com).
2. Add a payment method and a small amount of credit under **Plans & Billing**.
3. Create an API key — ideally a **dedicated key just for this app**, with a **monthly spending limit** set.

Cost is tiny for personal use: answers run on a lightweight model and each clue is only a handful of tokens, so a few dollars of credit covers a lot of play.

---

## Privacy & security

- Your key is kept **in your own browser** and sent **directly to Anthropic** — it never passes through this site, because there is no server.
- With **Remember** off, the key is used only for that session and forgotten when you close the tab.
- Use an API key with a **spending limit**, and don't enter your key on a shared or public computer.
- If a key is ever exposed, revoke it in the Anthropic console and generate a new one — it takes seconds.

To move your key to your phone safely, use a password manager (1Password, Bitwarden, iCloud Keychain, etc.) rather than emailing or texting it to yourself.

---

## How it works

```
Capture (camera frame or uploaded photo)
        │
        ▼
On-device OCR (Tesseract.js)  ──fails?──▶  Vision model reads the image
        │                                          │
        ▼                                          │
Send the clue text to the Anthropic API ◀──────────┘
        │
        ▼
Answer card: response + category + confidence + the clue it read
```

If the browser can't run the on-device reader (some locked-down environments block it), the app automatically falls back to having the model read the image directly, so it still works.

---

## Run or deploy your own

It's a single static file (`index.html`) — no build step, no backend.

**Locally:** open `index.html` in any modern browser. The camera needs `https://` or `localhost`, so a local file works for upload mode; for camera mode, serve it locally or deploy it.

**On GitHub Pages:**

1. Create a public repository and add `index.html` to the root.
2. In **Settings → Pages**, set the source to your `main` branch, root folder, and save.
3. Visit `https://<your-username>.github.io/<your-repo>/`. HTTPS is automatic, so the camera works.

---

## Configuration

A couple of settings live near the top of the `<script>` block in `index.html`:

- **`ANSWER_MODEL`** — the model used to answer. Defaults to a fast, lightweight model; swap in a larger one for tougher clues.
- **`PROXY_URL`** — leave empty for a pure bring-your-own-key setup. If you later host a small proxy that holds a key, paste its URL here and visitors without their own key will use it.

---

## Sharing it publicly (note)

By default, every scan is billed to whoever's key is entered. If you share the link widely **without** a proxy, anyone who enters their own key pays their own way — but if you set a `PROXY_URL` pointing at *your* key, every scan spends *your* balance, so add rate-limiting and a spending cap first.

---

## Limitations

- Reading accuracy depends on the photo: straight-on, well-lit, with the clue filling the frame works best. Glare and steep angles hurt.
- "Instant" is ~1–3 seconds end-to-end — great for studying or playing along, tight for racing a live buzzer.
- On obscure clues, the lightweight model can be wrong; the confidence rating and the shown clue text help you judge.

---

*Watson Live is an unofficial personal project. It is not affiliated with, endorsed by, or sponsored by Jeopardy!, Sony Pictures, or Anthropic. "Jeopardy!" is a trademark of its respective owner.*
