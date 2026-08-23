# Biographics 两分钟人物片 — AI Prompt 包（主角凹槽）

> **风格锚点：** [Alan Rickman — Biographics @ 10:46 / t=646s](https://www.youtube.com/watch?v=PKB8vBa2t5I&t=646s)  
> **该时段语法：** 中后段职业章节 — 全屏 B-roll 慢推 + 侧栏长读 + 酒红 PiP + 引用卡 + 章节转场（每章 **12–22 s**，比一分钟版从容）  
> **时长：** **120 s（2:00）** · 1920×1080 · 30fps  
> **唯一必填：** `{SUBJECT}` 人物主角 — 其余 Agent 推导填入凹槽

---

## 0. 你怎么用（30 秒）

1. 只填 **§1 主角凹槽**（或只给 `{SUBJECT_FULL_NAME}`，用 §1.1 研究 Prompt 自动填槽）  
2. 贴 **§2 Master Prompt** 到 AI 视频项目描述  
3. 按 **§4 六组合并组** 生成画面（不要拆成 40+ 碎镜）  
4. 用 **§5 旁白** 做 TTS → 对齐 §3 时间轴  
5. **§6 Negative** 全局排除 · **§7 QC** 验收  

---

## 1. 主角凹槽表（唯一必填区）

```yaml
# ─── 主角（唯一必填）───
SUBJECT_FULL_NAME: ""          # e.g. Alan Rickman
SUBJECT_SLUG: ""               # e.g. alan-rickman
SUBJECT_SHORT: ""              # 旁白简称 e.g. Rickman
SUBJECT_PRONOUN: "he"          # he / she / they

# ─── 封面 ───
POSTER_LINE_1: ""              # 名 · 全大写 e.g. ALAN
POSTER_LINE_2: ""              # 姓 e.g. RICKMAN
BORN_META: ""                  # Born 21 Feb 1946 · London, England
COVER_HOOK: ""                 # 一句记忆点 e.g. the voice that could freeze a room

# ─── 公众矛盾（全片灵魂句）───
PUBLIC_PARADOX: ""             # e.g. Beloved villain — beloved everywhere else.

# ─── 5 章侧栏（2 分钟版压缩 8 章为 5 幕）───
CH01_TITLE: "Origins"
CH01_BODY: ""                  # 早年 + 家庭矛盾 1–2 句
CH02_TITLE: ""                 # e.g. The Long Apprenticeship
CH02_BODY: ""
CH03_TITLE: ""                 # e.g. The Breakout
CH03_BODY: ""
CH04_TITLE: ""                 # e.g. Hollywood & Icon Roles  ← 对齐 ref @10:46 职业高峰段
CH04_BODY: ""
CH05_TITLE: "Legacy"
CH05_OUTRO_1: ""
CH05_OUTRO_2: "History, one life at a time."

# ─── 转场 paradox（第 3、4 章建议填）───
PARADOX_CH03: ""               # e.g. Famous face — still took the Tube.
PARADOX_CH04: ""               # e.g. Screen villain — stage gentleman.

# ─── 对比闪（早年章）───
CONTRAST_LABEL_FAME: "RED CARPET"
CONTRAST_LABEL_NORMAL: "THE TUBE"    # 按人物改 e.g. THE STAGE / THE SUBWAY

# ─── 姓名卡（可选 · 有家庭故事时）───
PARENT_GIVEN_A: ""
PARENT_SURNAME_A: ""
PARENT_GIVEN_B: ""
PARENT_SURNAME_B: ""

# ─── 3 张引用卡 ───
QUOTE_1_TEXT: ""               # 训练期反思 ~38s
QUOTE_1_CITE: ""
QUOTE_2_TEXT: ""               # 代表作/剧内名台词 ~72s  ← ref 段常在此
QUOTE_2_CITE: ""
QUOTE_3_TEXT: ""               # Legacy 前 ~108s
QUOTE_3_CITE: ""

# ─── PiP 描述（供搜 clip / 生成）───
PIP_HOOK_DESC: ""              # 采访/口音高光
PIP_BREAKOUT_DESC: ""          # 转折作名场面
PIP_PEAK_DESC: ""              # 职业高峰镜（对齐 t=646s 段气质）
PIP_LEGACY_DESC: ""            # 舞台/近年

# ─── 各章 B-roll 关键词 ───
BG_CH01: ""                    # 出生地天际线
BG_CH02: ""                    # 训练/舞台
BG_CH03: ""                    # 爆红作外景/片场
BG_CH04: ""                    # 红毯/大片/招牌 IP 视觉
BG_CH05: ""                    # 近年活动/混剪

# ─── 字幕标红词 ───
CAPTION_KEYWORDS: []           # 每章 1–2 个代表作/奖项名
```

### 1.1 主角研究 Prompt（只输入名字）

```
Research {SUBJECT_FULL_NAME} for a 120-second Biographics-style documentary short.
Reference pacing: YouTube Biographics Alan Rickman episode around 10:46 (t=646s) —
full-screen moving b-roll, side panel with 12–20s read time, maroon PiP inserts, quote cards.

Output YAML filling every field in §1 template. British dry wit narration tone.
Pick: one formative chapter, one breakout role, one peak/Hollywood chapter (like ref @646s), one legacy line.
QUOTE_2 must be a famous line from their iconic role (verbatim if possible).
PUBLIC_PARADOX must be fame vs private-life contrast for this person.
Contrast labels must be a metaphor for this subject (not generic).

[paste §1 YAML empty template]
```

---

## 2. Master Prompt（整片 — 贴 AI 工具「项目描述」）

```
You are producing a 120.000-second Biographics YouTube biography documentary.
Resolution 1920×1080, 30fps, 16:9. Structure FIXED; only protagonist content changes.

PROTAGONIST: {SUBJECT_FULL_NAME}
Style anchor: Biographics Alan Rickman at 10:46 (https://www.youtube.com/watch?v=PKB8vBa2t5I&t=646s)
— mid-career chapter grammar: slow full-screen video backgrounds, readable side panels 12–20s,
maroon PiP film inserts, black chapter cards with red left bar, full-screen quote cards 2.5–3.5s.

═══════════════════════════════════════
FIXED BRAND (DO NOT CHANGE)
═══════════════════════════════════════
Background: #0b0b0f
Accent red: #d42426
Maroon PiP / quote: #50000a – #4a0010
Panel: 520px, 4px red border, Bebas 64px title, Source Sans 22px body, bottom-aligned
Cinema chapters: 44px letterbox top+bottom
Grain + scanlines + top mist: entire 120s
Captions: bottom 72px, word-level, keywords #d42426
Chapter crossfade: 0.45s

FORBIDDEN: HUD, chapter badges on panels, watermarks, center huge titles,
PPT slides, static-only chapters 2–5, multiple full-screen overlays at once,
American news lower third, bright influencer look.

═══════════════════════════════════════
FIXED TIMELINE — 120 seconds (5 acts)
═══════════════════════════════════════
0.00–8.00     Cover duotone still poster (NO video bg)
8.00–12.00    Hook PiP TV 4:3 on maroon + waveform
12.00–13.20   Chapter 01 transition
13.20–32.00   Act 1 Origins (optional name card 14–20s; contrast flash ~26s)
32.00–33.20   Chapter 02 transition
33.20–52.00   Act 2 Apprenticeship (quote card ~38–41s)
52.00–53.20   Chapter 03 transition + paradox "{PARADOX_CH03}"
53.20–78.00   Act 3 Breakout (PiP ~58–64s; newspaper portrait overlay optional)
78.00–79.20   Chapter 04 transition + paradox "{PARADOX_CH04}"
79.20–108.00  Act 4 Peak / Hollywood (PiP peak ~85–92s; quote-2 ~72s if overlapping edit; cinema letterbox)
108.00–109.20 Chapter 05 transition
109.20–120.00 Act 5 Legacy (quote-3 ~108–111s; outro hold + BGM)

═══════════════════════════════════════
PROTAGONIST CONTENT (VARIABLE)
═══════════════════════════════════════
Poster: {POSTER_LINE_1} / {POSTER_LINE_2} | {BORN_META}
Paradox: {PUBLIC_PARADOX}
Chapters: {CH01_TITLE}…{CH05_TITLE} with bodies as provided
Quotes: {QUOTE_1_TEXT}, {QUOTE_2_TEXT}, {QUOTE_3_TEXT}
PiP: hook, breakout, peak, legacy — as described
Video backgrounds: MUST be moving video for all acts after cover

Output: cinematic British documentary — NOT a slideshow.
```

---

## 3. Negative Prompt（全局）

```
slideshow, powerpoint, static image only chapters, white background, corporate template,
TikTok influencer, anime, cartoon, 3D CGI, shaky vlog, stock collage,
bottom progress bar, chapter HUD, watermark, ticker, Scene 01 label,
multiple overlays, random fast cuts, CNN lower third, instagram gradient,
emoji, comic sans, global black crush filter, Ken Burns only on stills for main chapters
```

---

## 4. 六组合并组 Prompt（AI 生成用 — 替换 `{变量}`）

> **铁律：** 按 **6 组** 出片，组内用 NLE/HyperFrames 叠转场、PiP、引用。  
> 每组导出 **≥ 组时长 + 2 s** 余量。

### G01 · 封面 `0:00–0:08`

```
Cinematic Biographics documentary poster, 1920x1080, 30fps, 8 seconds feel.
Duotone portrait of {SUBJECT_FULL_NAME}, shadows #1a3868 highlights #e08c60 on #1a0a12.
Halftone 22%, heavy vignette, bottom typography:
  "{POSTER_LINE_1}" / "{POSTER_LINE_2}" Bebas 152px uppercase stacked
  "{BORN_META}" Oswald 13px muted below
Slow Ken Burns scale 1.0→1.06. Light sweep 0.6–2.8s. Film grain.
STILL IMAGE only — NO video background. Subject: {SUBJECT_FULL_NAME}.
```

### G02 · Hook PiP `0:08–0:12`

```
Full-screen maroon #50000a. Centered retro TV frame 4:3 cream bezel 52% width.
Inside: {PIP_HOOK_DESC} — {SUBJECT_FULL_NAME} speaking, interview energy.
White flash on enter. Frame scale 0.92→1.0. Audio waveform bars below.
Biographics hook. ~4 seconds.
```

### G03 · 起源幕 `0:12–0:32`（含转场 T01 + 可选姓名卡 + 对比闪）

```
Biographics documentary act ONE "Origins", 20 seconds, ref Rickman t=646s chapter pacing.
FULL-SCREEN MOVING VIDEO: {BG_CH01} — city/childhood atmosphere, cool desaturated grade.
LEFT panel 520px: title "{CH01_TITLE}", body "{CH01_BODY}", red left border, slides from left.
Mid-act optional: blurred vintage family video feel behind legal name cards (if parents provided).
Quick contrast flash ~26s: red carpet portrait "{CONTRAST_LABEL_FAME}" vs daily life "{CONTRAST_LABEL_NORMAL}" 0.8s total.
Film grain scanlines. Subject: {SUBJECT_FULL_NAME}.
```

**姓名卡 overlay 词（若用）：**

```
Blurred vintage TV footage full screen. Center scrim oval. Stacked legal names:
  {PARENT_GIVEN_A} small Oswald / {PARENT_SURNAME_A} large Impact
  thin rule
  {PARENT_GIVEN_B} / {PARENT_SURNAME_B}
No channel logo. Documentary Biographics @2:30 grammar. ~6 seconds within G03.
```

### G04 · 修行幕 `0:32–0:52`（含 T02 + 引用卡）

```
Biographics act TWO, 20 seconds. Moving video background: {BG_CH02} — stage/training/school.
RIGHT panel 520px: "{CH02_TITLE}" + "{CH02_BODY}", slides from right.
Black chapter transition card 1.2s at start: left red bar 8px, "Chapter 02", title white Bebas 96px, red wipe.
Insert full-screen maroon quote card 2.5s around 38s:
  "{QUOTE_1_TEXT}" Georgia italic + cite "{QUOTE_1_CITE}".
Slow camera drift. Subject: {SUBJECT_FULL_NAME}.
```

### G05 · 转折 + 高峰幕 `0:52–1:48`（含 T03–T04 + 2×PiP + 名句卡 + 院线黑边）

```
Biographics acts THREE and FOUR combined, 56 seconds, MATCH pacing of Alan Rickman Biographics @10:46 (t=646s):
unhurried side panel reads, iconic role footage, maroon PiP on narration beats.

[Act 3 — Breakout 53–78s]
Chapter transition + paradox: "{PARADOX_CH03}"
Moving video {BG_CH03}. Panel + body "{CH03_TITLE}" / "{CH03_BODY}".
Optional newspaper halftone duotone portrait overlay on video (edges still show motion).
PiP ~58–64s News/TV frame on maroon: {PIP_BREAKOUT_DESC}.
Full-screen quote card 2.5s: "{QUOTE_2_TEXT}" — "{QUOTE_2_CITE}" (iconic film line).

[Act 4 — Peak 79–108s]
Chapter transition + paradox: "{PARADOX_CH04}"
Moving video {BG_CH04} — red carpet / blockbuster / franchise visuals.
RIGHT compact panel + cinema letterbox 44px top bottom black bars.
PiP ~85–92s Scope 2.39:1 hero frame: {PIP_PEAK_DESC} — peak career moment like ref @646s.
Subject: {SUBJECT_FULL_NAME}. Film grain. NO slideshow.
```

### G06 · 遗产幕 `1:48–2:00`（含 T05 + 引用 + outro hold）

```
Biographics act FIVE Legacy, 12 seconds (+ 3s BGM hold optional to 120s).
Chapter transition "Chapter 05" / "{CH05_TITLE}".
Moving video {BG_CH05} — recent appearances / stage bow / tribute montage.
LEFT compact panel:
  "{CH05_OUTRO_1}"
  "{CH05_OUTRO_2}"
Quote card 2.5s: "{QUOTE_3_TEXT}" — "{QUOTE_3_CITE}".
Slow fade. Subject: {SUBJECT_FULL_NAME}. History one life at a time tone.
```

---

## 5. 旁白脚本凹槽（~120 s · 270–300 词）

**拦截：** 词数 **270–300** · 句数 **22–28** · 实测 VO **112–118 s** · 留 **2–8 s** BGM outro → 总 **120 s**

```
Today — {COVER_HOOK}: {SUBJECT_FULL_NAME}.

Born {BORN_PLACE}, {BORN_YEAR} — {EARLY_LIFE_HOOK_ONE_SENTENCE}. {EARLY_IRONY_LINE}.

{FORMATIVE_PARAGRAPH_TWO_SENTENCES — school, training, years of respectable work.}

Then {BREAKOUT_YEAR} changed everything — {BREAKOUT_ROLE}. {BREAKOUT_DESCRIPTION_TWO_SENTENCES}.

{PEAK_PARAGRAPH_THREE_SENTENCES — awards, franchise, iconic roles; this block carries the @646s energy: unhurried, authoritative, slightly dry.}

{LEGACY_PARAGRAPH_TWO_SENTENCES — contradiction, cultural footprint.}

{PUBLIC_PARADOX} — and for millions of fans, more than enough. History, one life at a time.
```

**TTS：** `en-GB-RyanNeural` · 英式纪录片 · 130–140 WPM  
**禁止：** 美式 George/Guy（Biographics 频道语感）

---

## 6. 时间轴一览（120 s）

| 段 | 入点 | 出点 | 类型 | 旁白节拍 |
|----|------|------|------|----------|
| 封面 | 0.0 | 8.0 | cover_poster | Hook 起句 |
| Hook PiP | 8.0 | 12.0 | film_insert TV | 人名后证明 |
| T01 | 12.0 | 13.2 | chapter_transition | — |
| 起源 | 13.2 | 32.0 | chapter_main | 早年 |
| T02 | 32.0 | 33.2 | chapter_transition | — |
| 修行 | 33.2 | 52.0 | chapter_main | 爬坡期 |
| Quote 1 | 38.0 | 41.0 | quote_card | 训练反思 |
| T03 | 52.0 | 53.2 | chapter_transition + paradox | — |
| 转折 | 53.2 | 78.0 | chapter_main + PiP | 爆红作 |
| Quote 2 | 72.0 | 75.0 | quote_card | 名台词 |
| T04 | 78.0 | 79.2 | chapter_transition + paradox | — |
| 高峰 | 79.2 | 108.0 | cinema + PiP scope | **≈ ref @646s 气质** |
| T05 | 108.0 | 109.2 | chapter_transition | — |
| 遗产 | 109.2 | 115.0 | legacy panel | 收束句 |
| Quote 3 | 108.0 | 111.0 | quote_card | 可叠在遗产前 |
| BGM hold | 115.0 | 120.0 | atmosphere | 无旁白 |

---

## 7. 与一分钟版差异（为何这是「两分钟」）

| 项 | 66–76 s 版 | **本 120 s 版** |
|----|------------|----------------|
| 章节 | 8 章压缩 | **5 幕**，每幕 **18–28 s** |
| 侧栏可读 | 常 2–6 s | **12–20 s**（对齐 [t=646s](https://www.youtube.com/watch?v=PKB8vBa2t5I&t=646s)） |
| 转场 | 1.0 s | **1.2 s** hold |
| 引用卡 | 2.3 s | **2.5–3.5 s** |
| PiP | 5 段短插 | **4 段**，高峰 PiP **6–7 s** |
| 旁白 | 155–175 词 | **270–300 词** |
| 封面 | 5.3 s | **8 s** |

---

## 8. 质检清单

- [ ] 用户只提供了 `{SUBJECT_FULL_NAME}`  
- [ ] 总时长 **118–120 s**（含 BGM hold）  
- [ ] 封面 **静图** duotone，第 2 幕起 **全是视频底**  
- [ ] 5 次转场 + 5 幕主镜，高峰段有 **院线黑边**  
- [ ] **≥3 种** PiP 框型（TV / News / Scope）  
- [ ] 3 张引用卡各 **≥2.5 s**  
- [ ] 高峰幕气质对齐 **Rickman @10:46** — 从容、侧栏能读完  
- [ ] 无 HUD / 水印 / 居中大字标题  
- [ ] TTS 英式 RyanNeural  

---

## 9. Agent 启动指令（复制即用）

```text
按「Biographics 两分钟人物片 Prompt 包」制作「{{SUBJECT_FULL_NAME}}」。

风格锚点：https://www.youtube.com/watch?v=PKB8vBa2t5I&t=646s
时长 120s · 唯一凹槽：主角人物。

执行：研究填 §1 YAML → §2 Master Prompt → §4 六组生成
→ §5 旁白 270–300 词 TTS → 时间轴 §6 对齐 → §8 QC
```

---

## 10. 填表示例：Alan Rickman（片段）

```yaml
SUBJECT_FULL_NAME: Alan Rickman
POSTER_LINE_1: ALAN
POSTER_LINE_2: RICKMAN
BORN_META: Born 21 Feb 1946 · London, England
COVER_HOOK: the voice that could freeze a room
PUBLIC_PARADOX: Beloved villain — beloved everywhere else.
PARADOX_CH03: Famous face — still took the Tube.
PARADOX_CH04: Severus Snape — Shakespearean underneath.
CH04_TITLE: Hollywood & Hogwarts
CH04_BODY: Die Hard's urbane terrorist. Harry Potter's double agent. Same velvet menace.
PIP_PEAK_DESC: Rickman as Severus Snape or Hans Gruber iconic scene
QUOTE_2_TEXT: After all this time? Always.
QUOTE_2_CITE: Severus Snape · Harry Potter and the Deathly Hallows
BG_CH04: Harry Potter premiere red carpet dark cinematic b-roll
```

---

*Prompt pack v1 · 120s · Anchor [Biographics @10:46](https://www.youtube.com/watch?v=PKB8vBa2t5I&t=646s) · Slot: protagonist only · 2026-07*
