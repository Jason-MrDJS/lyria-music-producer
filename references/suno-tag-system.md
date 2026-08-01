# Suno 元标签控制系统（Meta-tag Control System）

本文件提炼自《AI 音乐提示词终极指南》的规范部分，是 **Suno 括号标签（bracket notation）** 的权威控制语法。
Lyria 也兼容其中的段落标签（[Intro]/[Verse] 等），但 Lyria 更偏爱 `[时间][段落]` 标签流（见 `examples.md`）。
**编译器按目标模型选择格式**：目标为 Suno 或用户明确要求 bracket 标签时，使用本系统；目标为 Lyria 时使用 `examples.md` 的 `[Global Specs] + [时间][段落]` 格式。

---

## 1. [track] 轨道容器（全局默认）

放在整首提示**最顶部**，在任何段落标签之前，定义全局控制参数：

```
[track: genre: phonk drift, style: lo-fi hip-hop, mood: gritty night drive, length: 180, instruments: 808 sub-bass, vinyl crackle, chopped lo-fi beat]
```

- 可接受参数：`genre:`, `style:`, `mood:`, `length: <秒>`, `instruments: <乐器列表>`, `persona: female`, `loop-friendly` 等。
- **规则**：各段落可用自身标签/管道符覆盖全局默认值；**不要在每个段落重复所有全局参数**。

## 2. 管道符号（局部覆盖 Pipe Notation）

紧跟结构标签后，用竖线 `|` 为**单个段落**本地覆盖参数：

```
[chorus | style: phonk hook, vocals: autotune-light, melodic]
[bridge | style: intense, dynamic, build]
[bridge-drop | instruments: synth riser, percussion build, bass drop]
```

- 格式：`[SectionName | paramA: valueA, paramB: valueB, …]`
- 仅当某段落**偏离**全局默认时才用；保持简洁，避免每段重复全部内容。

## 3. 段落 / 结构标签（Song-form & Arrangement）

按功能归类（避免形容词堆砌，用这些结构/动作标签描述"音乐如何发展"）：

**曲式（Song Form）**：`[intro]` `[verse]` `[pre-chorus]` `[chorus]` `[bridge]` `[outro]` `[coda]` `[refrain]` `[interlude]` `[prelude]` `[episode]` `[intermezzo]` `[exposition]` `[development]` `[recapitulation]` `[finale]` `[start]` `[end]`
**编曲 / 过渡 / 动态（Arrangement / Transition / Dynamics）**：`[buildup]` `[build]` `[breakdown]` `[break]` `[drop]` `[climax]` `[big finish]` `[crescendo]` `[diminuendo]` `[rise]` `[swell]` `[tension-release]` `[modulation]` `[mutation]` `[transition]` `[drum-fill]` `[beat-switch]` `[fade]` `[power-off drop]` `[no-repeat]` `[loop-friendly]` `[quiet arrangement]`
**人声 / 演唱（Vocals）**：`[vocals]` `[vocalist]` `[vocal-style]` `[background-vocals]` `[male vocal]` `[female vocal]` `[whisper]` `[shout]` `[rapped verse]` `[spoken word]` `[narrator]` `[duet]` `[chant]` `[scat break]` `[distorted vocals]` `[vulnerable vocals]` `[announcer]` `[ad-lib]` `[aria-rise]`
**乐器 / 音色（Instruments / Timbre）**：`[instrument]` `[instruments]` `[instrumental]` `[bass]` `[pad]` `[orchestra]` `[orchestration]` `[harmonies]` `[arpeggio]` `[riff]` `[solo]` `[glissando]` `[pizzicato]` `[legato]` `[staccato]` `[tremolo]` `[sforzando]` `[tenuto]` `[timbre]` `[tone]` `[texture]` `[sonority]` `[voicing]` `[harmonics]` `[subharmonic]` `[distortion]` `[gain]` `[compression]` `[reverb]` `[echo]` `[eq]` `[sfx]` `[glitch]` `[bleep]` `[siren]` `[field-recording]`
**风格 / 情绪 / 体裁（Style / Mood / Genre）**：`[genre]` `[style]` `[mood]` `[vibe]` `[era]` `[happy]` `[sad]` `[emotional]` `[epic]` `[ambient]` `[lament]` `[intensity]` `[dynamics]` `[scale]` `[chromatic]` `[register]` `[tessitura]`
**控制 / 结构（Control / Structure）**：`[structure]` `[sequence]` `[control]` `[content]` `[personae]` `[language]` `[pronunciation]` `[technique]` `[signal-processing]` `[articulation]` `[inflection]` `[layering]` `[counterpoint]` `[fugue]` `[polyphony]` `[syncopation]` `[rhythm]` `[rhythmic-motif]` `[pulse]` `[theme]` `[subject]` `[hook]` `[variation]` `[cadence]` `[resolution]` `[inversion]` `[improvisation]`

> 注意：标签名（如 `timbre`、`texture`）与"形容词堆砌"不同——它们是**结构化控制轴**，应配合冒号写法描述发展（如 `[texture: sparse then layered]`），而非孤立形容词。

## 4. 器乐 vs 人声（Instrumental vs Vocal）

- **纯器乐**：用 `[instrumental]` 明确告诉模型**不产生人声**（v4.5 比 v4.0 更严格）。可带风格参数：`[instrumental: orchestral cinematic composition]`。
- 结构标签（Intro/Verse/Chorus）**同样适用于器乐**，只是表示纯器乐段落。
- 器乐中，`[solo]` / `[Instrument: Piano]` / `[Guitar Solo]` / `[instruments: acoustic guitar, cajón, handclaps]` 等乐器标签充当"主唱"角色，承载旋律。

## 5. Suno v4.0+ 解析规则（必守）

- **未识别的标签**（如 `[emotion build]`）会被当作通用 `[section X: ...]` 处理——可用冒号语法安全嵌入指令：`[tense development: Style and mood of the theme gets more tense until a climactic counterpoint is reached]`。
- **指令不要放在标签外部**：标签外的自由文本会被**演唱/念白**出来。
- **不要把歌词塞进 `[verse:]` 冒号**：歌词应放在 `[verse]` 标签**下方**；引擎只在找不到明显渲染指令时才演唱冒号内文本。
- **防循环**：不要逐字重复相同歌词/标签段落；用 `[verse A]` / `[verse B]` 等变体增加变化。
- **长作品**：始终包含 `[sequence: ...]` 控制段落顺序。

## 6. 已知无效 / 已废弃标签（不要用）

以下标签被 Suno 拒绝或误读，视为废弃：`[autotune: ...]`、`[filter: ...]`、`[loop: ...]`、`[mix]` / `[mixing: ...]`、`[master: ...]`、`[pan]` / `[panning: ...]`、`[style: none]`、`[section: ...]`（冗余，改用 `[intro:]`/`[verse:]` 等）。
**避免别名/模糊标签**：如 `bpm`、`key`、`language` 不作为标签使用（速度/调性用 `[track]` 的全局说明或自然语言描述）。

## 7. 风格 / 歌词限制（商标护栏）

Suno 在 "Style of Music" 中发现受保护商标会**拒绝渲染**。规避方式：用描述性词汇替代具体艺人/商标名。
- `kraftwerk` → 用 `krautrock` / `old school EDM` 等描述风格/技巧。
- `skank`（吉他演奏风格）→ 用 `ska stroke`。
- `Orbis Mundi` 等特定商标名避免使用。
（与 SKILL.md §5 推理纪律第 4 条"参考音乐人≠复制风格"一致：一律改写为具体声学描述。）

## 8. 使用元标签的提示

1. 仅使用已知或已确认的标签。
2. 避免别名或模糊标签（如 `bpm`、`key`、`language`）。
3. 首先在 Standalone 模式下测试——许多在 Extend/Cover 中失效的标签单独使用时正常。
4. 如有疑问，用 `[style: experimental]` 或 `[control: hallucinatory]` 鼓励灵活输出，而非强制。

## 9. 完整范例（Suno 格式 · 器乐电影感）

```
[track: genre: cinematic orchestral, style: dark wuxia score, mood: tense, length: 95, instruments: erhu, guzheng, Chinese war drums, orchestral strings]

[intro: sparse erhu over faint wind, static tension]
[verse | style: building] Main melodic phrase on guzheng, string pizzicato ascending.
[build: tanggu drums densify, tension rises]
[climax: full ensemble eruption, pipa tremolo, orchestral tutti, metallic weapon foley]
[outro: drums cut, lingering erhu long note and rain]
```

> 对比 `examples.md` 的 Lyria 格式：Lyria 偏好显式 `[时间][段落]` 坐标；Suno 偏好 `[track]` 容器 + 段落标签 + 歌词/描述置于标签下。两者都遵循本技能的时间轴编排与动态演进纪律。
