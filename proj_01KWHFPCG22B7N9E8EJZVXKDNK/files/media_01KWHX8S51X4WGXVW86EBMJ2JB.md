---
name: biographics-biography-video
description: >-
  User provides only a celebrity/subject name → agent auto-generates an identical
  Biographics-style ~66s HyperFrames biography video (same layout, motion, and
  AV rules as examples/benedict-cumberbatch-biography). Triggers on: 做传记片,
  Biographics video, 人物名字, celebrity biography montage, Alan Rickman-style bio,
  「和 benedict 一样」, or any request to generate a named person's doc short.
---

# Biographics Biography Video — Name In, Video Out

**Gold template (pixel-identical structure):** `examples/benedict-cumberbatch-biography/`  
**Style reference:** [Alan Rickman — Biographics](https://www.youtube.com/watch?v=PKB8vBa2t5I)

Also load: `/hyperframes-read-first` → `/hyperframes-core` → `/hyperframes-cli` → `/hyperframes-media` → `/biographics-chapter-transition`

---

## Trigger

User gives **one name** (required). Everything else is automated.

| User says | Agent does |
|-----------|------------|
| `做一集 Tom Hanks 的 Biographics` | Full pipeline below |
| `Alan Rickman biography video` | Full pipeline below |
| `人物：Keanu Reeves，和 benedict 那集一样` | Full pipeline below |
| `Benedict Cumberbatch` (name only) | Full pipeline below |

**Do not** ask for layout, chapter count, or style — always match the gold template.

### Optional overrides (only if user states them)

| Override | Default |
|----------|---------|
| Duration | ~66s (same beat structure as gold) |
| Narration language | English Biographics voice |
| TTS voice | `en-GB-RyanNeural` |
| Skip clip review | **No** — always show `clips-preview.html` unless user says「直接渲染」 |

---

## One-line start (agent runs this first)

```bash
python .agents/skills/biographics-biography-video/scripts/scaffold_biography.py "FULL NAME"
cd examples/{slug}-biography
```

Creates `examples/{name-slug}-biography/` by copying gold HTML/CSS/JS structure (no Benedict media/clips). Writes `SUBJECT_PROFILE.md` + patches poster name in `index.html`.

---

## Auto pipeline (run end-to-end)

```
[USER] name only
  ↓
1. scaffold_biography.py          → new project folder
2. Research subject               → fill SUBJECT_PROFILE.md (web/Wikipedia)
3. SCRIPT.md + narration.txt      → 8 chapters, Biographics tone, ~66s when spoken
4. design.md                      → copy brand tokens from gold (unchanged)
5. download_assets.py             → Wikimedia portraits + location stills (subject-specific)
6. download_clips.py              → yt-dlp + Commons, trim all bg/insert/dialogue clips
7. clips-preview.html             → gallery for user review  ← ONLY PAUSE POINT
8. [USER] 「可以」or pick filenames
9. Wire index.html + bio-scene.js → video bgs, inserts, QUOTE + FAMOUS_LINE cards
10. TTS + build_captions.py       → narration.mp3 + bio-captions-data.js
11. Sync SCENES / FILM_INSERTS    → align to caption word timings
12. hyperframes preview + render  → renders/{slug}-final.mp4
13. SOURCES.md                    → credits
```

Agent executes steps 1–7 and 9–13 **without re-asking** style questions. Step 8 waits for clip approval only.

---

## C2V clip orchestration (分段编排 — 暄雯主路径)

Monolithic gold (`index.html` + `bio-scene.js`) remains the visual reference. **Per-clip C2V** is the production path for parallel generation and RAG templating.

**Task guide:** `CLIP_ORCHESTRATION.md` in this skill directory.

```bash
SKILL=.agents/skills/biographics-biography-video
cd examples/{slug}-biography

# A. Shot list → clip manifest
node $SKILL/scripts/shotlist_to_clip_manifest.mjs \
  --shotlist ../../skills-export/BIOGRAPHICS-SHOTLIST.csv \
  --out ./clip_manifest.json

# B. Manifest → multi-track group_spec
node $SKILL/scripts/prep_biography_group_spec.mjs \
  --manifest ./clip_manifest.json \
  --clip-types $SKILL/clip-types.json \
  --hyperframes . --out ./group_spec.json

# C. Dispatch one worker per visual_clips[] → compositions/clip_*.html
#    Prompt: agents/hyperframes-clip.md
#    If layout components cannot be split, rebuild clip from RECIPE + gold time window.

# D. Assemble + render
node $SKILL/scripts/assemble-biography-index.mjs --group-spec ./group_spec.json --hyperframes .
npx hyperframes lint .
npx hyperframes render --output ./renders/{slug}-c2v.mp4
```

| Artifact | Role |
|----------|------|
| `clip-types.json` | clip_type → track / slots / RECIPE § |
| `clip_manifest.json` | 32 timed clips from SHOTLIST (Benedict gold) |
| `group_spec.json` | Engine contract for assemble |
| `compositions/clip_*.html` | One worker output per segment |

---

## Must match gold template (一模一样)

### Fixed structure (do not change)

| Item | Gold spec |
|------|-----------|
| Scenes | 8 (`scene1` cover … `scene8` legacy) |
| Canvas | 1920×1080 |
| Cover | Still duotone poster — **no video** |
| Chapters 2–8 | Full-screen **video bg** + left/right panel |
| Chapter transitions | 7 cards, track 8, ~1s each |
| Film inserts | Maroon PiP, one at a time, track 6 |
| Quote card 引用卡 | Full-screen 2.2–2.6s, track 12 |
| 名句卡 | Full-screen → **must** cut to film clip |
| Captions | Word-level, track 9, hide on overlay |
| Hook insert | `#film-hook` ~5.55s, SDCC-style accent clip |
| Intro timing | `INTRO_HOOK_END=7.7`, `INTRO_CHAPTER_END=8.7` |
| Scene 2 contrast | `#s2contrast` RED CARPET ↔ THE TUBE flash @ ~10.6s |

### Fixed layout map

| Scene | Panel side | Modifiers |
|-------|------------|-----------|
| 1 | none | `.hook-cover-poster` |
| 2 | left | contrast flash |
| 3 | right | optional broll A→B |
| 4 | right | |
| 5 | left | `.scene-newspaper` |
| 6–7 | right compact | `.scene-cinema` + letterbox |
| 8 | left compact | outro lines |

### UI that must NOT be added (gold removed these)

- Bottom `#global-hud` / `.chapter-bar`
- Side panel `.chapter-tag` (Chapter 01…)
- Cover `#hookTicker`
- Sherlock `221B` badge
- `#brand-top-rule` top red line
- Career timeline, watermarks, `.headline-card`, `.newspaper-banner`

Keep: `#atmosphere-mist`, `#transition-flash`, `#s2contrast`.

Full motion + video spec: [reference.md](reference.md)

---

## Step 2 — Research (agent, no user input)

Fill `SUBJECT_PROFILE.md` from reliable sources:

1. Birth date + city/country → `poster-meta`
2. One hook **paradox** (dry, British, ≤12 words)
3. Eight chapter titles + 1–2 sentence panel copy each
4. Breakout role + franchise role → clip search terms
5. ≥1 **引用卡** (real interview quote) + ≥1 **名句卡** (verified in-film line + clip)
6. ≥3 Wikimedia portrait filenames (verify face in description)

Paradox lines go on **chapter transition cards only** (`chap-trans-paradox` for breakout/franchise chapters).

---

## Step 3 — Narration

Write `narration.txt` — third-person British documentary, wry tone, ~250–320 words total (~66s at RyanNeural pace).

Chapter arc (fixed):

1. Introduction + name hook  
2. Early life / parents / origin  
3. Formative years / education  
4. Long climb / theatre  
5. Breakout role  
6. Hollywood / awards  
7. Signature franchise role  
8. Legacy + dry outro  

Then `SCRIPT.md` with beat table: caption group → scene → suggested insert/quote time.

---

## Step 5–6 — Assets & clips (agent)

### Stills

Edit `scripts/download_assets.py` `FILES` list for subject portraits + locations. Run:

```bash
python scripts/download_assets.py
python scripts/make_cover.py   # if cover plate needed
```

### Video

Edit `scripts/download_clips.py`:

```python
YOUTUBE_SOURCES = [("yt-city-raw.mp4", "https://youtube.com/watch?v=ID")]
COMMONS_FILES = [("filming-raw.ogv", "Commons_File_Name.ogv")]
VIDEO_TRIMS = [
  ("london-bg.mp4", "yt-city-raw.mp4", 4, 8),      # chapter bg 6–10s
  ("breakout-clip.mp4", "filming-raw.ogv", 48, 4.5), # insert 2.5–4.5s
]
```

**Rules:** Commons CC first · verify subject with `ffmpeg -ss N -frames:v 1` · never wrong-person B-roll · trim to 1280×720 H.264.

Run:

```bash
python scripts/download_clips.py
```

Rebuild `clips-preview.html` grouped by scene (gold file as template).

---

## Step 9 — Wire timeline

Replace Benedict-specific copy in `index.html` (panels, quotes, `<video src>`). Update `bio-scene.js`:

- `SCENES[]` — add `videoBg` for scenes 2–8  
- `CHAPTER_TRANSITIONS` — paradox on chapters 5 & 7 if applicable  
- `FILM_INSERTS[]` — hook + 3–4 role clips, `hide: "#sceneN"`  
- `QUOTE_CARDS[]` + `FAMOUS_LINE_CARDS[]` — full-screen frames only  
- `contrastFlashMotion()` — keep for scene 2 (adjust `at` if narration beat shifts)

Register timeline: `window.__timelines["root"] = tl`

HyperFrames black-screen fixes: `<main id="root">` first in body · SVG defs in `<head>` · every layer has `class="clip"` + `data-start` + `data-duration` + `data-track-index`.

---

## Step 10–12 — TTS, sync, render

```bash
npx hyperframes tts --text-file narration.txt --output assets/narration.mp3 --voice en-GB-RyanNeural
python scripts/build_captions.py
npx hyperframes preview --port 3013
# http://127.0.0.1:3013/clips-preview.html
# http://127.0.0.1:3013/#project/{slug}-biography
npx hyperframes render --output "./renders/{slug}-final.mp4"
```

After TTS: retime `SCENES[].start/dur`, `FILM_INSERTS[].start`, quote cards to `bio-captions-data.js` word boundaries.

---

## Deliverables

| File | Purpose |
|------|---------|
| `renders/{slug}-final.mp4` | Final ~66s video |
| `clips-preview.html` | Clip review gallery |
| `SOURCES.md` | Licenses |
| `downloaded-clips.zip` | Optional: zip trimmed clips for user |

Tell user: preview URL + MP4 path + zip path if created.

---

## Quality gate (before delivery)

- [ ] Same 8-scene structure as gold; cover still-only  
- [ ] Chapters 2–8 use video backgrounds (not Ken Burns stills)  
- [ ] No forbidden HUD/tags/watermarks  
- [ ] Scene 2 contrast flash plays ~10.6s  
- [ ] 名句卡 full-screen ≥2.2s → film clip within 0.1s  
- [ ] Captions hidden during transitions/inserts/quotes  
- [ ] Preview visible at t=0  
- [ ] `SOURCES.md` complete  

---

## Common pitfalls

| Problem | Fix |
|---------|-----|
| Black preview | SVG before `#root`; missing `__timelines["root"]` |
| Wrong clip subject | ffmpeg frame grab before adding to preview |
| Tourist BTS ≠ iconic scene | Use Commons filming BTS or verified film clip |
| Clip review skipped | Never wire new clips without user「可以」unless they said 直接渲染 |
| yt-dlp timeout exit | Verify output files exist on disk |
| Commons 429 | Download locally, backoff, trim offline |

---

## Related skills

- `/biographics-chapter-transition` — chapter hard-cut animation  
- `/hyperframes-media` — TTS, transcription  
- `/hyperframes` — composition patterns  

Templates & subject profile: [reference.md](reference.md)
