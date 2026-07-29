# Sonilo Skills

Prompting skills that make [Sonilo](https://sonilo.com) generations better.
Sonilo turns video into licensed music and sound effects (`video_to_music`,
`video_to_sfx` — REST API or MCP). Uploading a video bare gives the model
only what it can infer; a structured brief closes the gap. These skills teach
your agent to write that brief.

## Skills

| Skill | What it does | Use it when |
|---|---|---|
| `/sonilo:sonilo-prompting` | Pre-flight: endpoint choice, duration caps, existing-audio check, credit sign-off | Any Sonilo generation, before prompting |
| `/sonilo:video-to-music` | Style-prompt craft: genre, energy arc, speech preservation, ducking, segmented music | Generating music for a clip |
| `/sonilo:video-to-sfx` | Action map → segments array: sound bundles, materials vocabulary, enforced rules | Generating sound effects for a clip |

## Install

**New to API keys, terminals, or MCP?** Follow the step-by-step
[Getting Started guide](GETTING-STARTED.md) — it assumes zero experience and
walks through everything: getting a Sonilo API key, connecting Claude to
Sonilo, and installing these skills.

**Claude Code**

```
/plugin marketplace add sonilo-ai/sonilo-skills
/plugin install sonilo@sonilo-skills
```

**Cursor / Codex / anything that reads instruction files**

Copy the `SKILL.md` files from `plugins/sonilo/skills/` into your rules or
agent-instructions directory.

**Pairs with** [`sonilo-mcp`](https://github.com/sonilo-ai/sonilo-mcp) — the
MCP server provides the tools; these skills provide the technique.

## Why it works

Same clip, same model — the difference is the brief. Before/after audio pairs
and full recipes live in the
[video-to-music cookbook](https://github.com/cindyxu1030/sonilo-video-to-music-cookbook).

## API facts

All numeric limits and behavior claims in these skills were verified against
the Sonilo backend on 2026-07-28 — see `references/api-claims.md`. If you're
reading this long after that date, trust the live API reference over any
number here.
