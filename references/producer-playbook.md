# 制作人级 Prompt 工程学（六大维度 + 混音师视角）

> 本文件是 SKILL.md **Production Direction（声音制作方向）层**的展开依据。该层拆为 Recording / Mixing / Mastering 三层，本文件的"六大维度"与"混音师六参数控制法"分别对应其方法论底座。

本文件把"如何让 Lyria 写出有血有肉、有叙事起伏的完整歌曲"拆解为可操作的工程学策略。

## 一、制作人六大维度（以总制作人思维解构）

### 1. Song Structure（曲式结构）—— 设计"电影化转场"
- 商业歌曲结构不是机械积木，而是**心理预期的建立与打破**。
- 在结构中植入**心理重音（Psychological Accent）**，加入转折预告：
  > *"From a suffocating, claustrophobic Verse 1, instantly catapulting into a wide, liberating Chorus without a transitional buffer."*

### 2. Emotional Journey（情感弧光）—— 找"微观痛点"
- 情绪不是形容词（别写 *very sad / so happy*），情绪是**物理张力的松弛与紧绷**。
- 用行为/心理状态驱动：
  > *"The emotional arc shifts from a state of suspended hesitation in the intro, through an aggressive, driving confrontation in the chorus, down to a vulnerable, exhausted acceptance in the bridge, before a cathartic release."*

### 3. Arrangement Development（编曲演变）—— 用减法与加法制造呼吸感
- 业余制作人每秒塞满乐器；老手懂得"该闭嘴时闭嘴"。核心是**织体厚度变化率**。
  > *"Arrangement evolves via subtractive engineering: Verse 1 strips away all low-end frequencies, leaving a sparse, naked midrange. Pre-chorus introduces a rising white-noise sweep and a ticking percussion tension-builder. Chorus 2 doubles the rhythmic density by introducing a syncopated bassline."*

### 4. Instrument Evolution（乐器演进）—— 让音色"发生变异"
- 同一乐器在不同段落需质变（如主歌干声阴暗吉他 → 副歌推过载/加宽 Chorus/Delay）。
  > *"Instrument evolution: The felt piano in Verse 1 is muffled, dark, intimate with heavy mechanical hammer noises. By the final Chorus it transforms into a bright, heavily compressed, wide-stereo grand piano layered with a shimmering octave synth."*

### 5. Vocal Performance（人声表现）—— 注入人类生理特征
- AI 最容易露怯处：人声太机械太满。必须注入 **Human Physiological Artifacts**。
  > *"Vocal execution requires extreme micro-dynamics: Close-miced, unpolished, featuring audible breaths and vocal fry at the start of phrases. In Verse 1 it's a fragile, half-spoken cadence. In the Chorus it opens into a chest-belting performance with double-tracked octave harmonies that crack slightly with raw emotion."*

### 6. Mixing Style（混音审美）—— 声场三维建筑学
- 混音是**三维空间建筑**（纵深 Depth / 左右宽度 Panning / 上下频段分层）。
  > *"Mixing aesthetic: Mono-ish, claustrophobic spatial imaging in the verses with a dry, dark room reverb. Upon hitting the Chorus, the mix instantly expands into an ultra-wide, 3D immersive soundstage with a massive tail of plate reverb on the snare, clear high-mid air, and a tightly controlled, punchy low-end sub-bass."*

## 二、混音工程师视角：通过 Prompt 控制六大核心参数

AI 直接输出 48kHz 立体声母带级波形，把混音与编曲"焊死"在生成中。混音师从"拉推子"转为"用语言指挥 AI 的母带总监"。对应控制法：

| 参数 | 传统做法 | Prompt 控制写法示例 |
|---|---|---|
| **Spatial Imaging** | 调 Reverb / Panner | *"Intimate, narrow mono-ish vocal panning in center, surrounded by an ultra-wide 3D immersive stereo synth pad with a 3-second decay plate reverb."* |
| **Dynamic Range** | API 2500 / L2 压动态 | *"High dynamic range with uncompressed, breathing acoustic peaks in the verse, transitioning into a heavily saturated, wall-of-sound compressed master profile in the chorus."* |
| **Timbre & Tone** | Pultec EQ | *"Dark, warm analog tone with rolled-off harsh high frequencies, prominent low-mid body on guitars, airy bright top-end on vocals."* |
| **Vocal Processing** | De-esser / Doubler | *"Close-miced vocal delivery with audible breath sounds, gentle tape saturation, subtle slap-back delay, double-tracked unison harmonies."* |
| **Emotional Proximity** | 干湿比 / 远近 | *"Ultra-close, whispering 'in-your-ear' vocal perspective in verses shifting to a distant, stadium-sized anthemic distance in chorus."* |
| **Spectral Density** | Mute 冲突轨 | *"Sparse, minimalist frequency spectrum in verse with plenty of air; high spectral density in chorus with interlocking rhythm guitars, sub-bass, orchestral layers."* |

## 三、Lyria 是否在学习制作人审美？—— 是的

1. **多粒度专业音频标注**：训练集经高质量过滤与专业术语标注，模型理解"什么是好听的混音平衡"。
2. **对平衡与空间的内化**：Lyria 3 Pro 核心突破之一是"更自然的乐器混音平衡"，自动为人声留中频通道（Vocal Pocket），鼓与低频各司其职，不会无脑推满导致 Clipping。
3. **交互式审美共创**：Lyria RealTime 能根据用户实时指令（密度/亮度/调性）动态调整，像有经验的乐手"察言观色"——初级算法模拟的音乐审美直觉。

## 四、制作人级 Prompt 底层心法（总结）

把 Lyria 当"需要你提供总谱的虚拟乐队"：
1. **永远不要只给风格词**（如 *Sad Pop song*）。
2. **多用编曲动作词**（*Strip away, Layer, Expand, Drop out, Build up, Counter-melody*）。
3. **用时间标记锁死段落演进**，让 Lyria 3.5 的长程规划能力真正为你所用。
