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

## The Prompt Format for ACE

**stylePrompt** = 2-3 sentences describing the sound:
```
"2000s R&B slow jam, yearning and intimate, velvety midrange male tenor with warm vibrato, 65 BPM in A♭ major, lush Rhodes piano chords, sub-bass pulse, finger-snap beat, gospel choir swell in chorus, arrangement: piano intro → verses → gospel chorus lift → guitar solo bridge → fade"
```

**lyrics** = Full song with structure markers:
```
[Verse 1]
He moved in next door last July
Ford truck parked in the driveway
...
[Chorus]
Every night I hear your music
...
```

---

## The Ghostwriter Method (For Vocal Songs)

When a user asks for music with vocals:

1. **Pick the song** — genre, mood, core emotion
2. **Choose BPM and key** — write them into the stylePrompt
3. **Write lyrics** section by section with structure markers — 4-8 lines per section
4. **Write the stylePrompt** as 2-3 sentences (genre, era, mood, vocal type, tempo, key, instruments, arrangement arc)
5. **Call `generate_ace_music(stylePrompt, lyrics, title)`**

---

## Key Rules

- **`thinking: true`** is already set by the tool — you don't configure this
- **`bpm`** comes from the stylePrompt text (e.g. "65 BPM") — ACE reads it from there
- **`duration`** — default 90 seconds. User can request shorter or longer
- **Write lyrics yourself** — don't call external lyrics APIs. Write them yourself.
- **NO production notes in lyrics** — no "(soft piano)", no "(drums enter)"
- **Only call when user explicitly requests music** — never generate unprompted

---

## Style Prompt Formula

"[Era] [Genre + Subgenre], [Mood/Emotion], [Vocal Type + Quality + Accent], [Tempo BPM in Key], [Key Instruments with Emotional Quality], [Production Style/Atmosphere/Room Sound], [Arrangement Arc]"

Examples:
- "1990s R&B slow jam, yearning and intimate, velvety midrange male tenor with warm vibrato, 65 BPM in A♭ major, warm Rhodes piano chords, sub-bass pulse, finger-snap beat, gospel choir swell in chorus, arrangement: piano intro → verses → gospel chorus lift → guitar solo bridge → fade"
- "Early 2000s club banger, euphoric and confident, female vocal powerhouse with light auto-tune, 128 BPM in E major, punchy four-on-the-floor beat, layered synth leads, wide stereo image, arrangement: intro → verse → pre-chorus → massive chorus → verse → pre-chorus → chorus → outro"
- "Tropical house summer anthem, carefree and warm, breezy female vocal, 112 BPM in G major, marimba chords, soft kick, plucks, ocean wave ambience, arrangement: intro → verse → pre-chorus → chorus drop → verse → chorus → outro"

---

## Song Structure (Always Use This)

```
[Intro]          (8-16 seconds, hook tease or atmospheric)
[Verse 1]        (setup the story — who, what, where)
[Pre-Chorus]     (build tension, rising emotion)
[Chorus]         (THE BIG PAYOFF — repetitive, sing-along, biggest energy)
[Verse 2]        (story continuation, new beat)
[Pre-Chorus]     (second build)
[Chorus]         (even bigger than first)
[Bridge]         (contrast — key change, stripped production, emotional truth)
[Final Chorus]   (ultimate release, layered vocals, anthemic)
[Outro]          (hook repeat or fade with energy)
```

---

## Lyrics Rules

- **Write like you're texting your best friend at 3 AM** — conversational, not poetic
- **Simple language** — "I can't sleep" not "My restless mind won't rest"
- **Strong end rhymes** — AABB for pop, ABAB for R&B/ballads
- **Chorus = title phrase 3-4 times** + the quotable hook
- **Add "oh-oh-oh" wordless hooks** — incredibly sticky
- **6-10 syllables per line** for singability
- **Verse = story, Pre-Chorus = rising, Chorus = release**
- **NO production notes in lyrics** — no "(soft piano)", no "(drums enter)"
- **NO parentheticals** — lyrics should only have section markers + words to sing

---

## Genre BPM Guide

| Genre | BPM |
|-------|-----|
| Pop Ballad | 65-85 |
| R&B Slow | 65-90 |
| Hip-Hop/Rap | 80-100 |
| Pop/Dance | 118-128 |
| EDM | 128-140 |
| House | 120-128 |
| Tropical House | 100-115 |
| Corridos/Norteño | 85-110 |

---

## Vocal Style Reference

| Style | Description |
|-------|-------------|
| Female Pop Powerhouse | Confident, belting, light auto-tune, layered ad-libs — like Katy Perry / Ariana |
| Male R&B-Pop | Smooth, warm, falsetto on hooks, gentle verses — like Justin Bieber / Ed Sheeran |
| Ethereal Female | Soft, breathy, dreamlike — like Sia / Adele |
| Aggressive Rap-Pop | Fast cadence, rhythmic, attitude — like Doja Cat / Bebe Rexha |
| Soulful Male | Deep chest voice, gospel influences — like John Legend |
| Youthful Female | Bright, airy, playful — like Olivia Rodrigo / Ariana early |
| Deep Male Baritone | Rich, low register — like Bruno Mars / Harry Styles |

---

## Token Limits

| Provider | Limit |
|----------|-------|
| **ACE-Step** | UNLIMITED — use freely |
| **MiniMax music-2.6** | 100 songs per 5 hours — backup only |

MiniMax music-2.6 kicks in as a fallback if ACE fails completely.

---

## What NOT To Do

❌ Do NOT run `./scripts/acestep.sh` or any shell script — this is not a Claude Code project
❌ Do NOT cd to skill directories
❌ Do NOT call `https://api.acemusic.ai` directly — use the `generate_ace_music` tool
❌ Do NOT reference `acestep-songwriting`, `acestep-simplemv`, or `acestep-lyrics-transcription`
❌ Do NOT write "(soft piano)" or any production notes in lyrics
❌ Do NOT call music generation unprompted from greetings
❌ Do NOT use `music-2.5+` or `music-01` — those are MiniMax paid models
❌ Do NOT use the `/v1/text/chatcompletion_v2` endpoint — that path doesn't exist

---

## Reference

Full ACE documentation: `sparkie-studio/docs/MUSIC-GUIDE.md`
