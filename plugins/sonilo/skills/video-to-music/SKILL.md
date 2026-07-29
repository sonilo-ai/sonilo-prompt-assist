---
name: video-to-music
description: Write the style prompt for Sonilo's video_to_music (API or MCP). Use when generating music for a video with Sonilo — covers the audio brief, genre/mood/energy wording, preserve_speech and ducking, exclusions, and segmented music. Run the sonilo-prompting pre-flight first (duration cap 360 s, credits, existing audio).
---

# Sonilo video_to_music prompting

The model reads the cut itself — pacing, motion, edit points — and generation
targets the video's length. What it cannot know is your intent: genre, the
moment that matters, the sounds that must not appear. The prompt supplies
intent; keep timing expectations honest (see below).

## The music brief

Four lines before writing the prompt:

| Field | What to write | Example (10 s MTB clip) |
|---|---|---|
| **Style & register** | Genre + what the piece is commercially (if it has a commercial register — skip for personal/editorial footage) | Kinetic action-sports score; product launch film |
| **Energy arc** | Shape over time, in relative terms | builds through the first half, peaks at the landing |
| **Instrumentation** | Texture and what carries it | live drums, distorted bass, no synths |
| **Exclusions** | What must NOT appear | no vocals, no EDM drop |

## Writing the prompt

Spend the words on style, not timestamps:

- genre + mood + energy arc ("builds from sparse to driving")
- instrumentation and texture
- commercial register when real ("30-second product-film score", "cozy vlog bed")
- exclusions, stated plainly ("no vocals, no lo-fi crackle")

```
prompt: "Kinetic action-sports score for a 10-second product film. Gritty
live drums and distorted bass, builds tension over the first half and peaks
hard near the end. Modern, confident, cinematic. No vocals, no EDM drop,
no orchestral swell."
```

A second register, so the first example doesn't get copied everywhere —
talking-head/interview footage:

```
prompt: "Warm, unobtrusive bed for a founder interview. Soft keys and muted
guitar, slow pulse, stays out of the way of the voice. No drums entering
mid-phrase, no melody hooks, no vocals."
```

**Timing honesty.** Alignment of transitions and drops to cut points is
best-effort model behavior, not a guarantee — the backend has no alignment
logic. Use relative structural language ("peak near the final landing"),
never promise frame-exact sync, and don't write timestamp syntax expecting
enforcement. Output length targets the video's duration but is not
contract-trimmed to it.

## Segmented music (different sections)

Two routes when the piece needs distinct sections (calm intro → driving second half):

1. **REST**: an explicit `segments` parameter exists — segment starts are
   rounded to whole seconds. Check the current API reference for the schema.
2. **MCP** (and REST without `segments`, single variant): describe the
   sections *in the prompt text*, in order, with rough spans. A backend
   prompt-analysis pass parses clearly section-shaped text into a segments
   plan; text it can't parse falls back to a whole-prompt style hint. This is
   the only segmented-music route on MCP.

## Speech and ducking

- Footage with dialogue/VO: set `preserve_speech=true` (**default is false**).
  On REST this requires `mode=async`; MCP is always async. You get a vocals
  stem plus a speech+music mux at no extra charge.
- **Ducking is ON by default in async mode**, independent of
  `preserve_speech`; disable with `ducking=false`. Best-effort — a video with
  no audio track silently gets no ducking.
- Mix levels are computed server-side from loudness measurement; nothing in
  the prompt changes levels. Don't write "quiet" or "louder drums" —
  restate the *role* instead ("background bed", "drums carry the track").

## Exclusions are best-effort

"No vocals" has zero backend enforcement. State exclusions plainly, then
verify by listening (or stem-split as a diagnostic). If an excluded element
keeps returning, strengthen the positive description of what you want instead
of stacking more negatives.

## Iterating

One targeted change per regeneration, in this order: genre words wrong →
fix genre first; genre right but energy wrong → adjust arc words ("builds",
"sustained", "sparse"); style right but a section misses → move to the
segmented route. Each regeneration is a new charge — diagnose before rerolling.
