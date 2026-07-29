# Getting Started — the no-experience-needed guide

This guide is for you if you've never touched an API key, a terminal, or an
"MCP server" in your life. Follow the steps in order. Don't skip any. By the
end, you'll be able to type *"make music for this video"* into Claude and get
a finished soundtrack back.

**What you're setting up, in plain words:**

- **Sonilo** is the service that turns your video into music and sound
  effects.
- An **API key** is like a house key for your Sonilo account. It's a long
  string of letters and numbers. Anyone who has it can spend your credits —
  so treat it like a password.
- **MCP** is the cable that connects Claude to Sonilo. Once it's plugged in,
  Claude can send your video to Sonilo and bring the music back, all by
  itself.
- The **skills** in this repo are instruction cards that teach Claude how to
  ask Sonilo for *good* music instead of random music. (Skills work in Claude
  Code; if you only use the Claude desktop app, the MCP part still works and
  you can skip the skills section.)

---

## Part 1 — Get your Sonilo API key (5 minutes)

1. Open your web browser and go to:
   **https://platform.sonilo.com/dashboard/api-keys**
2. Log in to your Sonilo account. No account? Sign up first at
   **https://sonilo.com** — the free tier includes trial credits.
3. Look for a button that says **"Create API key"** (or similar). Click it.
4. A key appears. It starts with `sks_` followed by a long jumble of
   characters. That whole thing — including the `sks_` part — is your key.
5. Click **copy**, then paste it somewhere safe for the next 10 minutes
   (a private note is fine — NOT a group chat, NOT a shared doc).

⚠️ **Never share this key.** If it ever leaks, go back to the same dashboard
page and delete it — then make a new one.

---

## Part 2 — Install `uv` (one command, 2 minutes)

The Sonilo connector runs on a small helper tool called `uv`. You install it
once and never think about it again.

**On a Mac:**

1. Open the **Terminal** app. (Press `Cmd + Space`, type `Terminal`, press
   Enter. A window with text appears — that's it, that's the terminal.)
2. Copy this entire line, paste it into the terminal, and press Enter:

   ```
   curl -LsSf https://astral.sh/uv/install.sh | sh
   ```

3. Wait for it to finish (it prints some text and stops). Then **close the
   terminal window and open a new one** — this makes the computer notice the
   new tool.

**On Windows:**

1. Press the **Windows key**, type `PowerShell`, press Enter.
2. Copy this entire line, paste it in, and press Enter:

   ```
   powershell -ExecutionPolicy ByPass -c "irm https://astral.sh/uv/install.ps1 | iex"
   ```

3. Close PowerShell and open a new one when it's done.

**Check it worked:** in the new terminal window, type `uvx --version` and
press Enter. If you see a version number (like `uv 0.7.x`), you're good. If
you see "command not found", close the window, open a new one, and try again.

---

## Part 3 — Plug Sonilo into Claude

Pick the one that matches how you use Claude. (Both is fine too.)

### Option A — Claude Code (the terminal version)

One command. In your terminal, paste this — but **replace
`sks_PASTE_YOUR_KEY_HERE` with your real key from Part 1**:

```
claude mcp add sonilo --env SONILO_API_KEY=sks_PASTE_YOUR_KEY_HERE -- uvx sonilo-mcp
```

Press Enter. Done. Skip to Part 4.

### Option B — Claude Desktop (the app)

1. Open the Claude desktop app.
2. Open **Settings** (Claude menu → Settings on Mac, gear icon on Windows).
3. Click **Developer** in the left sidebar.
4. Click **Edit Config**. This opens a file called
   `claude_desktop_config.json` in a text editor.
5. Replace the file's contents with this — or, if there's already text
   between the outermost `{ }`, add just the `"mcpServers"` block inside it.
   **Replace `sks_PASTE_YOUR_KEY_HERE` with your real key:**

   ```json
   {
     "mcpServers": {
       "sonilo": {
         "command": "uvx",
         "args": ["sonilo-mcp"],
         "env": {
           "SONILO_API_KEY": "sks_PASTE_YOUR_KEY_HERE"
         }
       }
     }
   }
   ```

6. Save the file (`Cmd + S` / `Ctrl + S`).
7. **Quit Claude completely and reopen it.** (On Mac: `Cmd + Q`, not just
   closing the window.) MCP connectors load when the app starts.
8. Look for a small tools icon (🔨 or a plug) near the message box. Click it
   — if you see Sonilo tools like `video_to_music` in the list, it worked.

---

## Part 4 — Install the skills (Claude Code only)

Two commands, typed inside a Claude Code session:

```
/plugin marketplace add sonilo-ai/sonilo-prompt-assist
```

then:

```
/plugin install sonilo@sonilo-prompt-assist
```

When it asks for a scope, pick **user** (that means "for me, in every
project").

Now Claude knows the technique, not just the tool: it will check your video's
length, write a proper audio brief, and warn you before spending credits.

---

## Part 5 — Try it

Put a short video (under 6 minutes; under 3 for sound effects) somewhere easy,
like your Desktop. Then ask Claude:

> Make background music for the video at ~/Desktop/my-video.mp4. It's a
> product demo — upbeat, no vocals.

Claude will look at the video, write the brief, ask you to confirm (music
generation costs credits), call Sonilo, and save the finished audio file to
your folder.

---

## When something goes wrong

| What you see | What it means | Fix |
|---|---|---|
| `Invalid SONILO_API_KEY` | The key is wrong or was pasted with a typo | Re-copy the key from https://platform.sonilo.com/dashboard/api-keys — make sure you got the whole thing, starting with `sks_` |
| `command not found: uvx` | The computer can't find the helper tool | Redo Part 2, then open a **new** terminal window |
| `command not found: claude` | Claude Code isn't installed | Install it first: https://claude.com/claude-code |
| No Sonilo tools in Claude Desktop | The app didn't reload the config | Quit Claude fully (`Cmd + Q`) and reopen; re-check the config file for a missing comma or quote |
| Video rejected (422) | Video is over the limit | Music: max 360 seconds. Sound effects: max 180 seconds. Trim the video and retry |
| It generated, but the music is wrong | The brief, not the plumbing | That's what the skills fix — see [README](README.md) |

Still stuck? Open an issue on this repo and paste the exact error message
(never paste your API key).
