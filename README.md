# lyria-music-producer

**AI 音乐制作人决策系统（v1.2）· 面向 Lyria / Suno 的「强制锚点 + 原生双槽」路由技能**

一个给 AI 音乐模型（Google DeepMind Lyria、Suno）写提示词的技能。它**不是关键词填表器，也不是长篇制作人笔记**，而是模拟专业音乐制作人的判断过程：先把创作意图**锚定为 3 个强制锚点**，再压缩推导为 **Suno v4 原生双槽**交付：

- **Style Prompt**（给 Prompt Box）：前置 `Energy Start`、经 Final Compressor 三删法压到 ≤25 词的一行标签矩阵，告诉 AI "这首音乐是什么"。
- **Structure Blueprint**（给 Lyrics Box）：`[段落]` 标签 + 括号实时编曲指令，让解析器按时间戳执行编曲变化，告诉 AI "这首音乐如何发展"。

Skill 从"描述生成器"升级为 **音乐语义解析器 + 锚点决策器 + Prompt 压缩器 + 结构规划器**。

---

## v1.2 相对 v1.1 的关键变化

| 原版痛点 | v1.2 优化 | 收益 |
|---|---|---|
| 8 层推理太冗长，LLM 记不住 | 降维为 3 个强制锚点桶（身份 / 发展 / 禁区） | 推理偏离率降低 |
| 结构与歌词框脱节、缺编曲动作 | Structure 改为 Suno `[段落]` + 括号实时指令 | 编曲变化准确率提升 |
| 复古判定非黑即白（默认现代重构） | 引入 Retro Fidelity Dial (1-10) 滑杆 | 精确满足"瑕疵"诉求 |
| 外部参考文件过多、实际未被读取 | 内联 20+ 条硬规则，外部仅作词库/语法查表 | 指令遵循度提高 |
| Prompt 输出太长、易被截断 | 强制 Final Compressor 三删法，限制 ≤25 词 | 确保 100% 被接收 |

---

## 能做什么

- 把大白话（"武侠打斗配乐""深夜城市孤独感"）变成可控的音乐指令
- 自动识别具象化风格（武侠 / 科幻 / 赛博朋克 / 史诗 / 恐怖 / 治愈…）并映射成对应乐器与流派
- 区分三种模式：
  - **song** 流行歌曲（Verse / Chorus / Bridge）
  - **score** 影视 / 游戏配乐（cue 结构，**默认零人声**）
  - **bgm-loop** 循环背景（弱化段落、强调织体一致）
- 用**时间轴编排 + 动态演进 + 留白**，杜绝"无限循环 Loop 感"
- 复古请求用 **Retro Fidelity Dial (1-10)** 量化保真度（现代重构 / 混合复古 / 历史复刻），不再一刀切默认现代

---

## 三个强制锚点（推理第一步）

1. 🎯 **声学身份锚**：用 3 个具体声学名词定义这首歌（如 `felt piano · 80s gated reverb · fingerpicked acoustic guitar`），并给 Retro Dial 值。
2. 📐 **发展逻辑锚**：写出"唯一不变的核心律动"与"唯一变化的关键事件"，能量用数字曲线。
3. 🚧 **频谱禁区锚**：写出 3 条具体到频段/声部冲突的禁区（如"禁止 808 鼓组盖过钢琴中频"）。

---

## 原生双槽交付（v1.2 核心）

**Output 1 · Style Prompt（一行矩阵 + Energy Start，给 Prompt Box）：**

```
Energy Start: 20% | Intimate Neo-Soul, 72bpm, F#min, breathy Mandarin vocal, felt piano, brushed drums, upright bass, warm analog, airy mix
```

**Output 2 · Structure Blueprint（Suno 原生 `[段落]` + 括号指令，给 Lyrics Box）：**

```
[track: genre: neo-soul, style: intimate late-night, mood: melancholic, length: 180, instruments: felt piano, brushed drums, upright bass]

[Intro] (solo felt piano, close-mic, rubato, 8 bars)
[Verse 1] (brush drums enter, lock with piano left hand, bass plays root notes only)
[Pre-Chorus] (snare rolls, synth pad swells, vocal enters breathy)
[Chorus] (full kit, layered backing vox, wall-of-sound, energy peaks at 85%)
[Bridge] (strip to solo piano + breathy vocal, variation via space + performance)
[Final Chorus] (double rhythmic density, open to stadium-wide, then peel away to 20%)
```

---

## 怎么用

在 WorkBuddy 中直接说：

> "用 lyria 技能帮我写一段武侠打斗配乐的提示词"

或描述你的音乐想法即可触发。

---

## 目录结构

| 文件 | 内容 |
|---|---|
| `SKILL.md` | v1.2 主流程、3 强制锚点、20+ 条内联硬规则、Suno v4 双槽、Final Compressor |
| `references/style-bank.md` | **必查** 风格 → 流派 + 乐器 映射词库 |
| `references/suno-tag-system.md` | **必查** Suno 括号标签控制系统 |
| `references/reasoning-engine.md` | 可选 11 条反模式 + 自检清单详展 |
| `references/architecture.md` | 可选 Lyria 底层生成逻辑、八维权重 |
| `references/prompt-framework.md` | 可选 八层 JSON Schema + 编译器（供 Agent 驱动） |
| `references/producer-playbook.md` | 可选 制作人六维度 + 混音师六参数 |
| `references/examples.md` | 可选 多案例两段式范本 |

---

## License

MIT
