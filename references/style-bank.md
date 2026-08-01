# 风格 → 流派 + 乐器 映射词库（Style Bank）

本文件是"具象化风格识别"的核心数据源。当用户用大白话描述一个具体风格（如"武侠打斗""科幻太空""赛博朋克城市"）时，
技能必须查本表把风格**可靠地**解析为英文 genre 标签 + 核心乐器组合，而非临场发挥导致风格漂移。
所有进入最终提示词的字段一律为**英文**（见 SKILL.md §4 输出语言规则）。

## 一、模式判定线索（Mode）
- **song**：歌 / 演唱 / 有词 / vocal / 流行歌。
- **score**：配乐 / BGM / 纯音乐 / 影视 / 游戏 / 无人声 / instrumental / soundtrack / 背景音乐 → 纯器乐、cue 结构、**默认零人声**。
- **bgm-loop**：循环背景 / 无缝循环 / 氛围铺底 → 弱化段落、强调织体一致。

## 二、风格映射表

### 武侠 / 古风 / 国风 / 仙侠（Wuxia / Guofeng / Chinese Traditional）
- 触发词（中/英）：武侠、古风、国风、仙侠、江湖、刀光剑影、竹林、古装 / wuxia, gufeng, chinese traditional, ancient, martial arts
- Genre：`Chinese Wuxia Cinematic Score` 或 `Traditional Chinese Folk Instrumental`
- 核心乐器（英文，进 Instrumentation）：`guzheng (zither)`, `dizi (bamboo flute)`, `erhu (fiddle)`, `pipa (lute)`, `Chinese percussion (tanggu drums / luo gong / bangzi)`, `sheng (reed pipe)`
- 调式：`pentatonic scale`, `minor pentatonic (羽调式)`
- 战斗激烈（武侠打斗）：加 `Chinese war drums`, `pipa tremolo (轮指)`, `full orchestral tutti`, `metallic weapon Foley (刀剑金属泛音)`
- 人声：score 模式默认 **none**；song 模式可用 `Chinese operatic vocal (戏腔)` / `ethnic belting`

### 科幻 / 太空（Sci-Fi / Space）
- 触发词：科幻、太空、未来、宇宙、星际 / sci-fi, space, futuristic, cosmic
- Genre：`Sci-Fi Cinematic Score` 或 `Space Ambient`
- 核心乐器：`analog synth pads`, `modular arpeggiators`, `granular textures`, `sub-bass drones`, `metallic percussion`, `wordless choir`
- 音色：`cold`, `sterile`, `wide reverb`, `detuned`, `pristine`
- 人声：通常 none；可用 `wordless choir` / `vocoder`

### 赛博朋克（Cyberpunk）
- 触发词：赛博朋克、霓虹、未来都市、黑客、机械 / cyberpunk, neon, futuristic city, hacker
- Genre：`Cyberpunk Synthwave` 或 `Dark Electronic`
- 核心乐器：`distorted synth bass`, `gated reverb drums`, `arpeggiated synths`, `glitch percussion`, `vocoder`
- 音色：`neon`, `gritty`, `compressed`, `mid-heavy`

### 蒸汽波 / 复古（Vaporwave / Retro）
- 触发词：蒸汽波、复古、80年代、慵懒 / vaporwave, retro, 80s, chill
- Genre：`Vaporwave` 或 `Lo-Fi Chill`
- 核心乐器：`slowed-down jazz samples`, `FM bells`, `tape saturation`, `smooth sax`
- 音色：`dreamy`, `detuned`, `lo-fi`

### 史诗 / 战斗 / 预告片（Epic / Battle / Trailer）
- 触发词：史诗、战斗、宏大、预告片 / epic, battle, massive, trailer
- Genre：`Epic Orchestral` 或 `Trailer Music`
- 核心乐器：`full orchestra`, `taiko drums`, `brass stabs`, `choir`, `percussion ensemble`
- 人声：可选 `wordless choir`

### 恐怖 / 悬疑（Horror / Tension）
- 触发词：恐怖、悬疑、惊悚、诡异 / horror, tension, thriller, eerie
- Genre：`Horror Score` 或 `Dark Ambient`
- 核心乐器：`dissonant strings`, `prepared piano`, `low drones`, `found sounds`
- 音色：`unsettling`, `claustrophobic`

### 温馨 / 治愈（Warm / Healing）
- 触发词：温馨、治愈、陪伴、柔软 / warm, healing, cozy, soft
- Genre：`Acoustic Folk` 或 `Warm Ambient`
- 核心乐器：`acoustic guitar`, `soft piano`, `strings`, `light percussion`

## 三、混音 / 动态通用词（按模式复用，均用英文）
- 声场：`narrow mono-ish` → `ultra-wide 3D immersive soundstage`, `plate reverb tail`, `dry room`
- 动态：`uncompressed breathing peaks` → `wall-of-sound compressed master`
- 织体：`sparse minimalist` → `high spectral density interlocking layers`
- 生理真实：`audible breaths`, `vocal fry`, `mechanical hammer noises`, `tape saturation`
