# Getting Started — the no-experience-needed guide

This guide is for you if you've never touched an API key or an "MCP server"
in your life. Three steps, about ten minutes. By the end you'll type *"make
music for this video"* into your coding agent and get a finished soundtrack
back.

It works with any coding agent that speaks MCP — **Claude Code**, **Codex**,
Cursor, and others. The steps are the same everywhere.

**What you're setting up, in plain words:**

- **Sonilo** is the service that turns your video into music and sound
  effects. The video itself is the context — no prompt required; a short
  prompt just gives you more control.
- An **API key** is like a house key for your Sonilo account. It's a long
  string of letters and numbers. Anyone who has it can spend your credits —
  so treat it like a password.
- **MCP** is the cable that connects your coding agent to Sonilo. The nice
  part: you don't install it by hand. You ask your agent to do it.

---

## Part 1 — Get your Sonilo API key (5 minutes)

1. Open your browser and go to:
   **https://platform.sonilo.com/dashboard/api-keys**
2. Log in. No account? Sign up first at **https://sonilo.com** — the free
   tier includes trial credits.
3. Click **Create API key**.
4. A key appears. It starts with `sks_` followed by a long jumble of
   characters. That whole thing — including `sks_` — is your key.
5. Copy it and keep it somewhere private for the next few minutes (a private
   note is fine — NOT a group chat, NOT a shared doc).

⚠️ **Never share this key.** If it ever leaks, come back to the same page,
delete it, and make a new one.

---

## Part 2 — Ask your coding agent to install it (2 minutes)

Open your coding agent — Claude Code, Codex, whichever you use — and send it
this message, with your real key pasted in:

> Install the Sonilo MCP server from
> https://github.com/sonilo-ai/sonilo-mcp and configure it for this agent.
> My API key is sks_PASTE_YOUR_KEY_HERE

That's the whole step. The agent reads the repo, installs whatever helpers it
needs, wires up the key, and tells you when it's done. If it asks permission
to run a command or install a tool, say yes. If it hits an error, paste the
error back and say "fix it" — that's what the agent is for.

*(Prefer doing it by hand? The exact commands for every setup live in the
[sonilo-mcp README](https://github.com/sonilo-ai/sonilo-mcp).)*

---

## Part 3 — Try it

Put a short video somewhere easy, like your Desktop, then ask:

> Make background music for ~/Desktop/my-video.mp4

That's enough — the video is the context. Want more control? Add a sentence:

> Make background music for ~/Desktop/my-video.mp4 — upbeat product-demo
> energy, no vocals.

The agent sends your video to Sonilo, confirms before spending credits, and
saves the finished audio next to your video.

**Limits to know:** music handles videos up to 6 minutes; sound effects up to
3 minutes.

**Level up (optional):** Claude Code users can install the prompting skills
from this repo — they teach the agent to write a proper audio brief before
generating, which is the difference between "good" and "exactly what I
meant":

```
/plugin marketplace add sonilo-ai/sonilo-prompt-assist
/plugin install sonilo@sonilo-prompt-assist
```

On other agents, copy the `SKILL.md` files from `plugins/sonilo/skills/` into
your agent's instructions.

---

## When something goes wrong

| What you see | Fix |
|---|---|
| `Invalid SONILO_API_KEY` | Re-copy the key from https://platform.sonilo.com/dashboard/api-keys — the whole thing, starting with `sks_` |
| Video over the limit | Music: max 6 min. SFX: max 3 min. Trim the video and retry |
| Anything else | Paste the exact error to your coding agent and ask it to fix it. That usually works |

Still stuck? Open an issue on this repo and paste the exact error message
(never paste your API key).
