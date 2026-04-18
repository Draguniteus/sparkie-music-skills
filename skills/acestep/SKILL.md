---
name: acestep
description: Use ACE-Step 1.5 to generate music in Sparkie Studio. Use this skill when users want to create songs, generate music, or any music production. This skill is for Sparkie Studio's AI companion agent.
allowed-tools: Read, Write, Bash, Skill
---

# ACE-Step Music — Sparkie Studio Guide

**For Sparkie Studio agent only** — This is NOT a Claude Code skill. Do NOT use shell scripts.
Sparkie Studio handles ACE-Step via the `generate_ace_music` tool built into the chat.

---

## How Music Generation Works in Sparkie Studio

Sparkie Studio has a built-in `generate_ace_music` tool. You do NOT call APIs directly.
You do NOT run shell scripts. You do NOT cd to directories.

**Your job:**
1. Write a rich style prompt (2-3 sentences describing the sound)
2. Write full original lyrics with [Verse]/[Chorus]/[Bridge] markers
3. Call `generate_ace_music(stylePrompt, lyrics, title, duration, language)`
4. Return the `AUDIO_URL:...` result to the user

---

## Key Rules

- **`thinking: true`** is already set by the tool — you don't configure this
- **`bpm`** comes from the stylePrompt text (e.g. "65 BPM") — ACE reads it from there
- **`duration`** — default 90 seconds. User can request shorter or longer
- **Write lyrics yourself** — don't call external lyrics APIs. Write them yourself.
- **NO production notes in lyrics** — no "(soft piano)", no "(drums enter)"
- **Only call when user explicitly requests music** — never generate unprompted

---

## Token Limits

| Provider | Limit |
|----------|-------|
| **ACE-Step** | UNLIMITED — use freely |
| **MiniMax music-2.6** | 100 songs per 5 hours — backup only |

---

## What NOT To Do

❌ Do NOT run `./scripts/acestep.sh` or any shell script — this is not a Claude Code project
❌ Do NOT cd to skill directories
❌ Do NOT call `https://api.acemusic.ai` directly — use the `generate_ace_music` tool
❌ Do NOT reference `acestep-songwriting`, `acestep-simplemv`, or `acestep-lyrics-transcription`
❌ Do NOT write "(soft piano)" or any production notes in lyrics
❌ Do NOT call music generation unprompted from greetings

---

## Reference

Full ACE documentation: `sparkie-studio/docs/MUSIC-GUIDE.md`
