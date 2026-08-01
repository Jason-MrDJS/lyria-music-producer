# Lyria 底层架构与结构控制（深度参考）

本文件供技能内部参考，解释 Lyria 的运作机理，以便在编写提示词时做出正确决策。

## 一、Lyria 如何理解自然语言 Prompt

Lyria 不是把自然语言当"文本分类任务"，而是通过 **多模态语义-音频对齐空间（Semantic-Audio Alignment Space）** 做跨模态映射：

1. **LLM 前端赋能（Gemini 协同）**：富有画面感/情绪色彩的输入（甚至含图像/视频）先被底层语言理解能力语义解构。
2. **多粒度文本标注训练（Multi-detail Text Captions）**：训练数据被不同精细度的文本描述标注，模型学会把形容词（"忧伤的""复古的"）、名词（"808鼓机""萨克斯风"）、抽象意图（"适合深夜独自驾驶"）映射到高维风格嵌入空间（如 MusicCoCa 对比学习）。
3. **从字面量到音乐特征的桥梁**：不只匹配关键词，而是理解词语组合背后的音乐学隐喻。例如 "Lo-Fi beats for studying" 被隐式转化为：低高频滤波、磁带沙沙声、懒散摇摆节奏、温暖爵士和弦走向。

## 二、Prompt 的隐式层级拆解（非硬编码规则树）

用户只输入一段话，但 Lyria 的注意力权重会把句子"投影"到内部不同控制轴（Control Axes）/ 条件注入层（Conditioning Layers）：

| 用户维度 | Lyria 内部对应 | 机制 |
|---|---|---|
| Song Identity（歌曲身份） | 全局条件向量 | 顶层风格锚定 |
| Genre（流派） | Style Embeddings + Cross-Attention | 决定频谱能量分布 |
| Song Structure（结构） | 长程规划器（Long-range Planner） | 时间窗口内切换解码策略 |
| Emotional Arc（情感弧光） | Transformer 注意力 + 过渡插值向量 | 平滑/突变的能量跃升 |
| Arrangement（编曲） | 织体密度控制 | 频段占用与乐器进出 |
| Instrumentation（配器） | Timbre Space + Cross-Attention | 音色特征空间映射 |
| Performance Style（演奏/唱法） | Conditioning Tokens（Lyria 3/3.5） | 声带震动、气声、颤音独立控制 |
| Production Aesthetic（制作美学） | 混响/压缩/空间条件层 | 声场与母带质感 |

## 三、Lyria 生成方式定位

介于"先规划语义/结构骨架，再生成高保真音频"之间，结合 **分块自回归（Chunk-based Autoregression）** 与 **潜空间扩散（Latent Diffusion）**：

- **B（先生成结构/语义规划，再生成音频）最贴合核心逻辑**：高层 Transformer 主干或分块自回归器，先在 Audio Tokens / SpectroStream / 音乐语义空间规划时间轴上的和弦走向、节奏型、结构转变、歌词时间戳；再由底层 LDM 或高效解码器转为高保真音频潜变量，渲染 48kHz 立体声。
- **C（连续流与分块自回归）是亮点**：Lyria RealTime（Live Music Models 架构）依赖自身上一时刻输出（Self-conditioning）与实时控制信号（Key/Tempo/Density），像爵士乐手即兴，不断预测下一个 Micro-chunk，保持连贯与交互响应。

**A（直接从文字预测原始波形）过于绝对**，计算复杂度极高且易长距离崩塌（失调度性与节拍一致性），并非 Lyria 路线。

## 四、Lyria 是否真正理解歌曲结构概念

**是的，Lyria 3 Pro 及后续具备"结构性意识（Structural Awareness）"**，但基于：
- **数据分布映射**：训练数据中商业流行/摇滚/R&B/电子严格遵循段落美学，潜空间内建构了能量曲线模板（Verse 织体薄、Chorus 能量最高、Bridge 转调突变）。
- **时间对齐与标记**：Lyria 3 引入显式结构标签与时间戳控制（如 `[Verse]` `[Chorus]`）。用户输入这些标签，触发长程规划器在时间轴划定时间窗口，强制切换解码策略。

## 五、AI 音乐结构控制的三大困难（及 Lyria 的突破）

1. **长程依赖衰减（"金鱼记忆"）**：传统自回归在生成数分钟时注意力漂移，生成到 Bridge 已"忘记"Verse 的调性/主旋律 → 解决方案：Lyria 的长程规划器 + 时间戳锚点。
2. **断崖式过渡**：缺乏全局蓝图时，结构交界处出现节奏错位/和弦冲突/人声强行中断 → 解决方案：显式设计 Pre-Chorus/Bridge，用 Riser/Drop-out 自然承接。
3. **宏观结构与微观音色的矛盾**：既要 3 分钟宏观架构不乱，又要每秒钟微观音色真实，多尺度建模极耗算力 → 解决方案：分层潜空间扩散解耦。

## 六、八维信息影响力排序（Prompt 笔墨分配依据）

详见 SKILL.md 的 "八维信息影响力排序"。核心：结构 > 乐器 > 情感 > 场景 > 主题 > 混音 > 音色 > 参考艺术家。
**实操推论**：把最多笔墨放在"明确的曲式结构与时间戳"和"具体的乐器增减剧本"上；参考艺术家权重最低，用具体声学描述替代艺人姓名。
