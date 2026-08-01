---

name: lyria-music-producer
description: "AI 音乐制作人决策系统（v1.2）· 面向 Lyria / Suno 的「强制锚点 + 原生双槽」路由技能。v1.2 把 v1.1 的 8 层冗长流水压缩为 3 个强制锚点桶（声学身份 / 发展逻辑 / 频谱禁区）——LLM 先死记 3 锚点再推导交付物，杜绝长推理跑偏；最终强制输出 Suno v4 原生双槽：Style Prompt（前置 Energy Start、Final Compressor 三删法压到 ≤25 词）进 Prompt Box，Structure Blueprint（[段落] 标签 + 括号实时编曲指令）进 Lyrics Box。内置 Retro Fidelity Dial (1-10) 年代滑杆、20 条内联硬规则、Final Compressor。核心仍是模拟专业制作人的判断（哪个元素占主导、什么信息进哪个通道、何时加什么、如何避免音乐逻辑冲突），而非关键词拼贴。默认英文输出，仅用户明确要求中文才用中文。关键词：Lyria、Suno、AI 音乐生成、音乐提示词工程、制作人级决策、结构化音乐生成、编曲、混音、歌曲结构、影视配乐。"
version: "1.2"
---

# Lyria Music Producer v1.2 — AI Music Producer Decision System

## Overview

本技能**不是 AI 音乐 Prompt 生成器**，而是 **AI 音乐制作人决策系统（decision system）**。

它要回答的不是"如何写一个 AI 音乐 Prompt"，而是：

> **如果我是一个专业音乐制作人，我如何指导 AI 完成这首音乐？**

核心区别：生成器把"想要什么声音"堆成一个大段文本；决策系统模拟制作人的判断——**哪个元素应该占主导？哪些信息应该进入哪个控制通道？什么时间加入什么元素？为什么加入？如何避免音乐逻辑冲突？**

### 制作决策管线（Producer Decision Pipeline）

```
用户创意 (Creative Input)
  → 音乐理解 (Music Understanding)
  → 制作决策 (Production Decision)   ← 强制锚点阶段（3 锚点）
  → 内部推导 (Internal Derivation)    ← 8 控制层压缩映射
  → 编译双槽 (Compile Dual-Slot)      ← Suno v4 原生语法
  → 终稿压缩 (Final Compress)         ← ≤25 词
```

### 底层心法（贯穿所有层）

> **Emotion → Structure → Arrangement → Performance → Sound → Production → Final Prompt**
> （情绪 → 结构 → 编曲 → 表演 → 声音 → 制作 → 最终 Prompt）

### 为什么从"单 Prompt"改为"多层 Routing + 双槽"

把所有信息混在一个 Prompt 里，AI 音乐模型容易：
1. **压缩信息** —— 把关键约束淹没在长文本里；
2. **忽略时间逻辑** —— 看不到"何时发生变化"；
3. **把结构理解为风格描述** —— 把 Verse/Chorus 当成音色标签；
4. **产生关键词堆叠** —— 形容词互相抵消，控制力稀释。

因此本技能**先锚定、再推导、最后双槽输出**：Style Prompt 进 Prompt Box（"这首音乐是什么"），Structure Blueprint 进 Lyrics Box（"这首音乐如何发展"，用 Suno 原生 `[段落]` + 括号指令，让解析器按时间戳执行编曲变化）。

## When to Use

- 用户要求"写一段 Lyria / Suno 提示词""帮我生成一首 AI 音乐的描述"。
- 用户希望 AI 生成的音乐有清晰的结构演进、情绪递进，而非无限循环 Loop。
- 用户想从笼统创意（"悲伤的电影感钢琴曲""90 年代日流但要 2026 标准"）升级为制作人级可控指令。
- 用户需要结构化、可路由、且适配 Suno / Lyria 原生语法的音乐生成架构。

**不适用**：本技能不执行真实音频渲染（由 Lyria / Suno 服务端完成），它产出的是"最优提示词与分层执行架构"，而非音频文件。

## Core Mental Model（核心心智模型）

### §1. 底层生成逻辑（一句话版）

Lyria / Suno 采用 **"分层条件潜空间 + 宏观长程规划器"**：前端把 Prompt 解构为带时间戳的段落标记（Time-stamped Section Tokens），通过交叉注意力把流派 / BPM / 调性 / 乐器条件注入生成网络，底层潜空间扩散渲染高保真音频。

> 推论：**Prompt 是跨模态映射到控制轴的语义-音频对齐**。显式 `[Verse]`/`[Chorus]` 标签 + 时间戳会触发长程规划器在时间轴划定段落窗口、切换解码策略——所以"结构+时间轴"是控制力最强的通道。

### §2. 八维信息影响力排序（决定笔墨花在哪）

1. **Song Structure（曲式 / 时间戳）** —— 最高，直接干预长程注意力。
2. **Instrumentation（乐器 / 织体进出）** —— 高，决定频谱能量占用。
3. **Emotional Arc（能量曲线 / 动态）** —— 较高，决定张力释放点。
4. **Scene / Narrative（场景 / 叙事）** —— 中偏上，转化为具体乐器 / 速度副词。
5. **Theme（主题 / 歌词概念）** —— 中，主要影响人声语气。
6. **Production Aesthetic（混音审美）** —— 中偏下，建立在乐器与结构之上。
7. **Timbre（音色细节）** —— 较低，依附于乐器词汇。
8. **Reference Artist（参考艺术家）** —— 最低，**永不写"像某天王"，改写具体声学描述**。

> **实操推论**：把最多笔墨放在「明确的曲式结构 + 时间戳」和「具体的乐器增减剧本」上；参考艺术家权重最低。

### §3. 三个反"AI 感"核心心法

- **减法编曲（Subtractive Arrangement）**：段落切换写明乐器"进出动态"，用织体厚度变化打破循环惯性。
- **能量梯度规划（Energy Gradient）**：后续副歌必须比前一个加码，绝不允许两遍副歌相同。
- **生理与物理真实感（Human Artifacts）**：注入微观不完美（audible breaths, vocal fry, 钢琴机械击弦声），破坏机器冰冷感。

### §4. 输出语言规则（默认英文）

最终交付给 Lyria / Suno 的提示词 **默认使用英文**。仅当用户明确要求中文时才输出中文。

- 理由：Suno / Lyria 对英文提示词识别与风格控制最佳。
- 范围：技能内部说明可用中文，但**编译产物（双槽提示词）必须是英文**；若需中文，仅作面向用户的解释性注释，不得混入交付给模型的提示词正文。

### §5. 模式识别（Mode）—— 结构词汇与人声开关

在 Step 1 必须判定生成模式（词库与线索见 `references/style-bank.md`）：

- **song（歌曲）**：Verse / Chorus / Bridge 流行曲式；人声按需。
- **score（影视 / 游戏配乐）**：纯器乐、cue 驱动；结构用 `Establishing / Tension / Action / Climax / Resolution`；**默认零人声**。
- **bgm-loop（循环背景）**：短时长、可无缝循环；结构弱化、强调织体与氛围；默认零人声。
- **判定线索**：用户说"配乐 / BGM / 纯音乐 / 影视 / 游戏 / 无人声 / instrumental / soundtrack / 背景音乐" → `score` 或 `bgm-loop`，**绝不带人声**；说"歌 / 演唱 / 有词 / vocal" → `song`。

### §6. Retro Fidelity Dial（年代滑杆 1-10 · 替代"默认现代重构"）

v1.2 废除"Era Translation 默认走 Modern Interpretation"的二元判定，改为**连续滑杆**，让用户或 LLM 在 Step 1 内部量化复古保真度：

| 档位 | 含义 | 制作处理 |
|---|---|---|
| **1–3** | 现代高清重构 | 保留和声 / 旋律语言 / 情绪，升级声场、动态、低频、高频延伸、人声制作 |
| **4–7** | 混合复古 | 保留模拟设备饱和度与染色，但扩展立体声、控制低频、保留动态 |
| **8–10** | 历史复刻 | 保留窄频响、高底噪、机械瑕疵、原年代混响，**不做任何现代处理** |

- 判定后写入 🎯 声学身份锚 的 Era 字段，并直接驱动 🚧 频谱禁区锚 与 Production Direction 层。
- **禁止默认现代重构**：只要用户提到年代词（80s / 90s j-pop / lo-fi / 黑胶），必须先给 Dial 值再推导，不得 silently 升级全部制作。

## Mandatory Anchor Phase（强制锚点 · 推理第一步）

> **LLM 必须先死记这 3 个锚点，再推导任何交付物。** 锚点用「3 个具体声学名词」回答，禁止虚词。锚点是内部推理锚，不直接输出给用户，但最终双槽必须能从锚点追溯。

### 🎯 锚点 1 · 声学身份锚（Acoustic Identity）

**合并自**：Music Identity + Style Conditioning + Era Translation（含 Retro Dial）

**LLM 必须回答**：
> 如果用 **3 个具体的声学名词**（如「毛毡钢琴 / 80 年代门混响 / 指弹木吉他」）定义这首歌，它们是什么？再补一句：年代保真度 Dial = ?（1–10）

- 输出约束：3 个名词必须是**可被模型识别的声音特征**，不是形容词清单。
- 例：`felt piano · 80s gated reverb · fingerpicked acoustic guitar`，Dial = 9（历史复刻）。

### 📐 锚点 2 · 发展逻辑锚（Development Logic）

**合并自**：Song Structure + Emotional Arc + Arrangement Blueprint

**LLM 必须回答**：
> 从第 1 秒到最后 1 秒，**唯一不变的核心律动**是什么？**唯一变化的关键事件**（如某个乐器进出 / 调性突变 / 能量跳变）是什么？能量起点 / 峰值 / 终点各多少？

- 输出约束：必须给出"不变项"与"变化项"各一，能量用数字（如 20% → 85% → 100% → peel to 20%）。
- 例：不变项 = `lazy swung pocket`；变化项 = `brush drums enter at Verse, full kit at Chorus, strip to solo piano at Bridge`；能量 20→85→100→20。

### 🚧 锚点 3 · 频谱禁区锚（Spectrum No-Go）

**合并自**：Negative Constraints + Production Direction

**LLM 必须回答**：
> 必须避开哪 **3 个具体的频段 / 编曲灾难**？（禁止"过度电影化"这种虚词，要写可执行的具体灾难）

- 输出约束：每条必须**具体到频段或声部冲突**，如「禁止 808 鼓组盖过钢琴中频」「禁止副歌低频与贝斯互殴导致 mud」「禁止人声混响盖过咬字」。
- 例：`808 sub overpowering piano mids · bass/kick mud at 200Hz · reverb washing out vocal consonants`。

## Internal Derivation（8 层压缩映射 · 推导交付物）

锚点确定后，内部按下表把 8 个控制层**压缩推导**为双槽。底层逻辑（职责边界 / 优先级 / 功能结构 / 律动融合）全部保留，只是不再逐层长篇展开，硬规则见下方 Decision Rules。

| 旧层（逻辑保留） | 归属锚点 | 推导时必须遵守的纪律（对应硬规则） |
|---|---|---|
| 1. Music Identity | 🎯 身份 | 一句话定位"是谁"，不涉及时机（R14） |
| 2. Style Conditioning | 🎯 身份 | 优先级系统 Primary/Secondary/Supporting/Production，禁平铺（R12） |
| 3. Song Structure | 📐 发展 | 功能型（Function/Purpose/Energy/相对时长），禁固定时间套餐（R13） |
| 4. Emotional Arc | 📐 发展 | 能量曲线 + 每段"为什么变化"（R6, R11） |
| 5. Arrangement Blueprint | 📐 发展 | 织体随段落加减；减法编曲、能量梯度（R8, R18） |
| 6. Instrument Roles + Rhythm Integration | 📐 发展 | 每件乐器 Role/Interaction；新增乐器锁现有 groove 五维（R3, R17） |
| 7. Production Direction | 🚧 禁区 + 🎯 身份(Era) | Recording/Mixing/Mastering 三层；套用 Retro Dial 值（§6） |
| 8. Negative Constraints | 🚧 禁区 | 3 条具体频段/声部灾难，不用虚词（R10） |

## Decision Rules（20 条硬规则 · 必执行 · 由 references 内联）

> 下列规则原散落于 6 个外部参考文件，v1.2 内联为此处唯一权威来源；外部文件降为可选扩展阅读（见 References）。

**R1 · 情绪 ≠ 形容词**：把情绪翻译成发展弧（悬而未决 → 压抑下行 → 释放式爆发 → 接受），不写 "sad/emotional"。
**R2 · 歌曲结构 ≠ 声音结构**：Verse/Chorus 是叙事形式；频段/空间/立体声归 Production。两者不混写。
**R3 · 要素必绑时间**：每个乐器/织体必须绑定段落或时间窗，否则模型平均用力 → Loop 感。
**R4 · 参考艺人 ≠ 复制风格**：永不写"like [artist]"，拆成具体声学描述（受版权护栏压制且权重最低）。
**R5 · 音色 ≠ 乐器名**：乐器名 + 微观物理质感（felt piano, muffled/dark, mechanical hammer, close-mic）。
**R6 · 动态曲线必画**：显式规划 静→涌→爆→落 的宏观动态，禁止全程同音量同密度。
**R7 · 电影感 ≠ 大**：电影感来自叙事张力与留白，不是响度；用镜头推进与休止制造张力。
**R8 · 留白是手段**：每秒塞满乐器 = 无呼吸 = 疲劳；减法/稀疏是核心编曲工具。
**R9 · 混音 ≠ 创作**：创作描述进结构/主题；混音描述（空间/动态/质感）进 Production，不混通道。
**R10 · 形容词 ≤2 个**：用 1–2 个精准动作词替代一长串形容词（形容词互相抵消、不驱动参数）。
**R11 · 无音乐逻辑冲突**：检查调性/速度/人声距离/能量是否随段落演进；冲突元素须有演变路径，不突变。
**R12 · 优先级系统**：风格用 Primary/Secondary/Supporting/Production Standard 分层，禁平铺冲突标签（如 `90s J-Pop, Modern, Vintage` 平铺会权重抵消）。
**R13 · 功能型结构**：每段写 Function/Purpose/Energy/相对时长，禁"0:00-0:15"固定时间套餐（用户硬给时间戳则补写功能）。
**R14 · 结构 ≠ 风格**：Verse/Chorus 是结构通道，不是音色/风格标签，不写进 Style Prompt。
**R15 · 禁关键词堆叠**：用动作词 + 具体声学描述，不用形容词清单。
**R16 · 笔墨权重**：最多笔墨给「曲式结构+时间戳」与「乐器增减剧本」；参考艺术家权重最低。
**R17 · 结构标签触发规划器**：用 `[Verse]`/`[Chorus]` + 时间戳触发长程规划；用 Pre-Chorus/Bridge + riser/drop-out 自然承接，避断崖。
**R18 · 乐器演进**：同一乐器跨段落须质变（主歌干声阴暗吉他 → 副歌过载推宽），不静态。
**R19 · 人声生理真实**：注入 audible breaths / vocal fry / half-spoken cadence，避免机械满人声。
**R20 · 模式控制**：score / bgm-loop / instrumental → 整体跳过人声，结构用 Theme/Development/Variation/Climax/Resolution。

**Suno v4 解析纪律（R21–R23）**
- **R21 · 指令在括号内**：标签外的自由文本会被**演唱/念白**出来；歌词放 `[verse]` **下方**，不塞进 `[verse: 冒号]`。
- **R22 · 防循环 / 废弃标签**：不逐字重复相同歌词/标签块（用 `[verse A]`/`[verse B]`）；禁用 `[autotune:]` `[filter:]` `[loop:]` `[mix]`/`[mixing:]` `[master:]` `[pan]`/`[panning:]` `[style: none]` `[section:]`。
- **R23 · 商标护栏**：受保护艺人/商标名会被拒渲染，改用描述性风格词（如 `kraftwerk` → `krautrock`）。

**输出纪律（R24–R25）**
- **R24 · 绝不直发 JSON**：八层必须编译为自然语言分层路由文本再提交（见 Workflow Step 4）。
- **R25 · 双槽不可混合**：Style Prompt 不写剧情/时间线/制作过程；Structure Blueprint 不写音色标签矩阵（已在 Style Prompt）。

## Workflow（标准工作流）

### Step 1 — 采集创意 + 判定模式 + Retro Dial

- 判定 **Mode**（§5）：song / score / bgm-loop（R20）。
- 若含年代/复古词，先定 **Retro Fidelity Dial (1–10)**（§6），**禁止默认现代重构**。
- 若用户只给笼统描述，查 `references/style-bank.md` 把风格解析为 流派 + 乐器组合。
- 确认全局元数据：Genre、Tempo、Key、Narrative Context。

### Step 2 — 强制锚点（Mandatory Anchor Phase）

依次回答 🎯 声学身份锚 / 📐 发展逻辑锚 / 🚧 频谱禁区锚 三问（见上文）。**未完成锚点不得进入 Step 3**。

### Step 3 — 内部推导（Internal Derivation）

按"8 层压缩映射"表把锚点展开为 8 控制层纪律，套用 R1–R20。此步内部完成，不向用户输出长文。

### Step 4 — 编译双槽（Suno v4 原生语法 · 强制）

最终**只输出两个互不混合的交付物**（R24/R25）：

**Output 1 · Style Prompt（给 Prompt Box）**
- 一行高密度标签矩阵，**前置 `Energy Start: %`**。
- 固定顺序：`Energy Start: % | [Genre] → [Sub Genre/Emotional Identity] → [BPM] → [Groove] → [Key] → [Vocal Character] → [Performance] → [Core Instruments] → [Arrangement] → [Production] → [Mix]`。
- 不写剧情、不写时间线、不写制作过程（R14/R25）。

**Output 2 · Structure Blueprint（给 Lyrics Box）**
- 直接用 Suno 原生 `[段落]` 标签 + **括号实时编曲指令**，不再写散文结构描述。
- song 模式：歌词（中文/任意）放 `[verse]` 等标签**下方**；括号里写编曲动作（R21）。
- score / bgm-loop 模式：无歌词，括号里写器乐描述（R20）。
- 每段可用管道符 `|` 局部覆盖（如 `[chorus | style: wall-of-sound]`），见 `references/suno-tag-system.md`。

### Step 5 — 终稿压缩器（Final Compressor · 三删法 · 强制）

生成 Style Prompt 后立即执行**三删法**，直至**纯英文单词数 ≤ 25**（Suno/Lyria Prompt Box 隐性截断保护）：

1. **删介词**：删 `with a` / `that has a` / `featuring`，改逗号连接 → `warm Rhodes, brushed drums`。
2. **删程度副词**：删 `very` / `deeply` / `extremely`，只留形容词 → `emotional` 优于 `deeply emotional`。
3. **合并同类项**：guitar+piano+bass → `organic trio`；rhodes+piano → `twin keys`。

> 执行后自检：`Energy Start: 20% | Intimate Neo-Soul, 72bpm, F#min, breathy vocal, felt piano, brushed drums, upright bass, warm analog, airy mix` —— 计单词（不含标点与数字标签）应 ≤ 25。

### Step 6 — 输出前自检（必过）

- [ ] 🎯 3 个声学名词具体、非虚词？Retro Dial 已给？
- [ ] 📐 不变项 + 变化项各一？能量有数字曲线？
- [ ] 🚧 3 条禁区具体到频段/声部冲突？
- [ ] R1–R20 是否违反（尤其 R10≤2形容词 / R12优先级 / R13功能结构 / R14结构≠风格）？
- [ ] R21–R23 Suno v4 纪律（指令在括号内 / 无废弃标签 / 无商标）？
- [ ] Style Prompt ≤ 25 词（三删法已执行）？
- [ ] 双槽边界严格分开（R25）？

### Step 7 —（可选）结构化 JSON Schema

若用户需供 Agent 自动驱动 Lyria，按 `references/prompt-framework.md` 的八层 JSON Schema 输出（但仍须经 Step 4 编译为自然语言，R24）。

## Output Format（Suno v4 原生双槽 · 范本）

**Output 1 · Style Prompt（Prompt Box）**
```
Energy Start: 20% | Intimate Neo-Soul, 72bpm, F#min, breathy Mandarin vocal, felt piano, brushed drums, upright bass, warm analog, airy mix
```

**Output 2 · Structure Blueprint（Lyrics Box · song 模式）**
```
[track: genre: neo-soul, style: intimate late-night, mood: melancholic, length: 180, instruments: felt piano, brushed drums, upright bass]

[Intro] (solo felt piano, close-mic, rubato, 8 bars)
[Verse 1] (brush drums enter, lock with piano left hand, bass plays root notes only)
[Pre-Chorus] (snare rolls, synth pad swells, vocal enters breathy)
[Chorus] (full kit, layered backing vox, wall-of-sound, energy peaks at 85%)
[Bridge] (strip to solo piano + breathy vocal, variation via space + performance)
[Final Chorus] (double rhythmic density, open to stadium-wide, then peel away to 20%)
```

**Output 2 · Structure Blueprint（Lyrics Box · score 模式 / 零人声）**
```
[track: genre: chinese wuxia score, style: dark action, mood: tense, length: 95, instruments: erhu, guzheng, tanggu drums, orchestral strings]

[Intro] (sparse erhu over faint wind, static tension)
[Build] (tanggu drums densify, pipa tremolo enters, tension rises)
[Climax] (full ensemble eruption, orchestral tutti, metallic weapon foley)
[Outro] (drums cut, lingering erhu long note and rain)
```

> Lyria 兼容上述 `[段落]` 标签；若目标明确为 Lyria，可在标签前加时间戳（如 `[0:20 Verse 1]`），括号指令同样生效。

## Producer Lexicon（高频有效"动作词"，直接写入 Prompt）

- **结构 / 动态**：minimal, sparse, strip away, layer, expand, drop out, build up, counter-melody, wall of sound, crescendo, fade out。
- **混音空间**：narrow mono-ish, ultra-wide 3D immersive, plate reverb tail, dry room, close-miced, in-your-ear, stadium-sized distance。
- **生理真实**：audible breaths, vocal fry, mechanical hammer noises, tape saturation, raw emotion crack。
- **能量对比**：intimate vs cinematic climax, low spectral density vs high sonic density, quiet whispered vs towering peak。

## References

**必查表（查字典性质，生成时必须参考）：**
- `references/style-bank.md` —— 场景/风格 → 流派 + 乐器 映射词库（武侠/科幻/赛博朋克/史诗/恐怖/治愈等）+ 模式判定线索。
- `references/suno-tag-system.md` —— Suno 括号标签控制系统：`[track]` 容器、管道符局部覆盖、按功能分类元标签、v4.0+ 解析规则、废弃标签、商标护栏。

**可选扩展阅读（推理逻辑已内联至本文件 Decision Rules，仅作深入参考）：**
- `references/reasoning-engine.md` —— 11 条反模式 + 自检清单的详尽展开。
- `references/architecture.md` —— Lyria 底层层级生成逻辑、八维权重、结构控制难点。
- `references/prompt-framework.md` —— 八层 JSON Schema + 编译器逻辑（供 Agent 驱动用）。
- `references/producer-playbook.md` —— 制作人六大维度 + 混音师六参数（Production Direction 展开依据）。
- `references/examples.md` —— 多案例两段式范本（电影感流行 / 武侠配乐 / 90 年代日流 / 纯音乐写法）。

