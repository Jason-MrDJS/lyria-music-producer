# 两段式交付范本（Style Prompt + Structure Blueprint · v1.1）

本文件是 SKILL.md Step 7 的**输出范本**。每个案例都先给 **Style Prompt（一行高密度标签矩阵，给 Prompt Box）**，再给 **Structure Blueprint（功能性结构发展，给 Lyrics Box）**。

> **红线**：Style Prompt 只回答"这首音乐是什么"（不写剧情/时间线/制作过程）；Structure Blueprint 只回答"如何发展"（不写音色标签矩阵）。两者不混合。

---

## 内部推理 → 压缩（Style Prompt Compression Layer 演示）

以"深夜城市，一个人回忆过去，治愈但带一点遗憾，现代 R&B"为例：

```
内部分析：
  Genre = R&B
  Emotion = Intimate / nostalgic
  Tempo = Slow
  Vocal = Breathy Mandarin vocal
  Instrument = Rhodes + brushed drums
  Production = Warm analog modern

↓ 压缩为（固定顺序：Genre → Sub Genre → BPM → Groove → Key → Vocal → Performance → Core Instruments → Arrangement → Production → Mix）

Style Prompt:
Intimate late-night R&B, Neo-Soul ballad, 72 bpm, lazy pocket groove, F# minor,
breathy emotional Mandarin vocals, close-mic storytelling, warm Rhodes piano,
brushed drums, upright bass, subtle gospel harmony layers, warm analog texture,
dynamic and airy modern production.
```

---

## 案例 1 — 现代 R&B（深夜 / 治愈带遗憾）

**Style Prompt（给 Prompt Box）：**
```
Intimate late-night R&B, Neo-Soul ballad, 72 bpm, lazy pocket groove, F# minor, breathy emotional Mandarin vocals, close-mic storytelling, warm Rhodes piano, brushed drums, upright bass, subtle gospel harmony layers, warm analog texture, dynamic and airy modern production.
```

**Structure Blueprint（给 Lyrics Box）：**
```
[Intro]      Minimal Rhodes piano. Create intimate bedroom atmosphere.
[Verse]      Close-mic vocal with sparse arrangement. Bass and brushed drums support the groove.
[Pre-Chorus] Gradually increase harmonic tension.
[Chorus]     Expand vocal layers and harmony. Increase emotional release.
[Bridge]     Strip back arrangement. Create vulnerability.
[Final Chorus] Maximum emotional expression while maintaining intimacy.
```

---

## 案例 2 — 武侠打斗配乐（score 模式 / 零人声）

**Style Prompt（给 Prompt Box）：**
```
Chinese Wuxia cinematic action score, orchestral-ethnic fusion, 120 bpm, driving martial groove, D minor pentatonic, instrumental only no vocal, aggressive yet disciplined performance, pipa tremolo plus tanggu war drums plus orchestral tutti plus guzheng, dense combat texture, modern hybrid trailer production, wide immersive mix.
```

**Structure Blueprint（给 Lyrics Box）：**
```
[Establishing] Sparse guzheng and low sheng. Static night tension. Wide space, no drums.
[Action]      Full ensemble enters. Combat intensity builds through layering, not volume.
[Climax]      Maximum texture. Add metallic weapon Foley. No vocal.
[Resolution]  Strip to solo instrument. Leave silence. Tension resolved, not exploded.
```

---

## 案例 3 — 90 年代日流现代重构（Modern Interpretation）

**Style Prompt（给 Prompt Box）：**
```
Modern 2026 Japanese pop production, 1990s J-Pop melodic language, 112 bpm, bright four-on-floor groove with swung hats, A major / F#m, bright clear Mandarin vocal, soaring catchy lead performance, Rhodes electric piano plus FM bell arpeggio plus analog string pad plus tight synth bass, lush pop texture, streaming-quality modern mix, wide stereo, controlled low-end, natural dynamics.
```

**Structure Blueprint（给 Lyrics Box）：**
```
[Intro]      Rhodes hook establishes identity. Lock all elements to 112 grid.
[Verse]      Vocal-forward, restrained arrangement. FM arpeggio supportive, not competing.
[Pre-Chorus] Harmonic tension ramp via string pad swell.
[Chorus]     Full ensemble, maximum hook. Clap aligns backbeat, no independent groove.
[Bridge]     Drop to single piano for breathing. Change space, not just add instruments.
[Final]      Hook returns with fuller harmonies, then natural decay to Rhodes.
```

> 注意 Era Translation：保留 90s 旋律语言/和声/情绪（KEEP），升级鼓/低频/高频/宽度/人声/动态到 2026（UPGRADE）。低频与高频不复古。

---

## 案例 4 — 纯音乐 Theme / Development / Variation（Decision Rule 1）

**Style Prompt（给 Prompt Box）：**
```
Healing cinematic instrumental, warm ambient-pop, 82 bpm, soft flowing groove, C major / G, no vocal, intimate piano-led performance, grand piano plus fingerstyle guitar plus string pad plus celesta, sparse-to-lush evolving texture, warm analog production, close and airy mix.
```

**Structure Blueprint（给 Lyrics Box · 用器乐发展逻辑，不套 Verse/Chorus）：**
```
[Theme]       Solo piano states main motif. Lowest density, maximum space.
[Development] Guitar and pad join. Texture thickens via arrangement, not louder.
[Variation]   Change harmony and spatial width. Avoid merely adding instruments.
[Climax]      Full arrangement peak. Still controlled low-end.
[Resolution]  Peel back to solo piano. Leave the motif hanging in reverb.
```

---

## 红线速查（输出前必过）

- Style Prompt 是**一行**，不是段落、不是故事、不是制作笔记。
- Style Prompt 不含 "Create a song about..." / "The chorus should..."。
- Structure Blueprint 不含音色标签矩阵（那些已在 Style Prompt）。
- 中文仅作面向用户的说明；交付给模型的 Style Prompt / Structure Blueprint 一律英文（歌词内容除外，可嵌入 Suno 段落）。
