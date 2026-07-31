# lyria-music-producer

**制作人级 AI 音乐提示词工程技能 · 面向 Lyria / Suno**

一个给 AI 音乐模型（Google DeepMind Lyria、Suno）写提示词的技能。它**不是关键词填表器**，而是模拟专业音乐制作人的推理过程，把你的创作想法（情绪、场景、风格）编译成模型能精准响应的结构化提示词。

---

## 能做什么

- 把大白话（"武侠打斗配乐""深夜城市孤独感"）变成可控的**英文**音乐提示词
- 自动识别具象化风格（武侠 / 科幻 / 赛博朋克 / 史诗 / 恐怖 / 治愈…）并映射成对应乐器与流派
- 区分三种模式：
  - **song** 流行歌曲（Verse / Chorus / Bridge）
  - **score** 影视 / 游戏配乐（cue 结构，**默认零人声**）
  - **bgm-loop** 循环背景（弱化段落、强调织体一致）
- 用**时间轴编排 + 动态演进 + 留白**，杜绝"无限循环 Loop 感"
- 支持 Lyria 的 `[时间][段落]` 标签流 与 Suno 的 `[track]` 元标签两种格式
- 内置 JSON Schema + 编译器，可让 Agent 自动驱动 Lyria

---

## 核心纪律（避免"AI 感"）

- **情绪 ≠ 形容词**：要写成"发展弧"（动机如何生长、张力如何释放）
- **时序优先**：每个元素都要绑定时间 / 段落
- **参考艺人 ≠ 风格**：改写成具体声学描述，别写"像某天王"
- **电影感 ≠ 大**：留白与叙事张力，而不是推满乐器

---

## 怎么用

大白话直接说：

> "用 lyria 技能帮我写一段武侠打斗配乐的提示词"

或描述你的音乐想法即可触发。

---

## 输出示例（英文交付 · score 模式 / 零人声）

```text
[Global Specs — FILM SCORE MODE]
Genre: Chinese Wuxia Cinematic Action Score, 120 BPM, D minor pentatonic.
Aesthetic: Ancient Chinese tonal base fused with modern orchestral impact.

[00:00 - 00:15] [Establishing]
Low sheng and sparse guzheng plucks, faint night-breeze ambience. Static tension.

[00:45 - 01:30] [Combat / Climax]
Full ensemble eruption: pipa tremolo, massive tanggu drums, orchestral tutti,
metallic weapon Foley. High intensity, no vocal.
