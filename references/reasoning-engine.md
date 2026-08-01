# 制作人推理引擎 · 11 条反模式与正解（Reasoning Engine）

本文件是 SKILL.md "Producer Reasoning Engine" 的详细展开。核心原则：**模拟专业制作人的推理，而非关键词拼贴**。
每条给出：误读、为何错、正确推理、中英对照范例。

---

## 1. 情绪 ≠ 形容词（Emotion is not an adjective）
- **误读**：`"sad song"` → 直接写 `Sad, melancholic, emotional`。
- **为何错**：形容词不驱动任何音乐参数，模型自由发挥导致平庸。
- **正确推理（Emotion → Musical Development）**：把"悲伤"翻译成发展弧——动机从悬而未决的犹豫，经压抑的下行旋律，到副歌的释放式爆发，最后桥段精疲力竭的接受。
- **正解**：
  > *"The emotional arc moves from suspended hesitation in the intro, through a restrained descending melody in the verse, into a cathartic爆发 in the chorus, then exhausted acceptance in the bridge."*

## 2. 歌曲结构 ≠ 声音结构（Song Form ≠ Sound Structure）
- **误读**：把"结构"同时用来指主副歌和频段分层。
- **为何错**：二者维度不同，混用会让模型既乱了曲式又乱了混音。
- **正确推理**：Song Structure = 叙事形式（Verse/Chorus/Bridge 或 Establishing/Action/Resolution）；Sound Structure = 频段/频谱/空间分层（低频占用、立体声宽度、混响尾音），归 Production Aesthetic。
- **正解**：结构段用 `[Chorus]` 标注；声音分层写进 `Mixing:` 行（如 `ultra-wide stereo, sub-bass tight, plate reverb tail`）。

## 3. 只说"有什么"不说"何时发生"（What without When）
- **误读**：`"has drums, piano, strings"` 无时间信息。
- **为何错**：缺时间轴 → 模型全程平均用力 → 循环 Loop 感。
- **正确推理（时序优先）**：每个元素必须绑定时间窗口或段落。
- **正解**：
  > *"[00:20-00:50] Verse: strip away all drums, keep only felt piano. [00:50-01:20] Chorus: full drums enter."*

## 4. 参考音乐人 ≠ 复制风格（Reference Artist ≠ Style Copy）
- **误读**：`"a song like [某天王]"`。
- **为何错**：受版权/安全护栏压制，权重最低，且无法精确控制。
- **正确推理**：把艺人风格拆成可执行的声学描述。
- **正解**：
  > *"1980s retro synth-pop with gated reverb drum machine, bright analog polysynth, stadium reverb vocals."* （替代"像某艺人"）

## 5. 音色 ≠ 乐器名称（Timbre ≠ Instrument Name）
- **误读**：把 `felt piano` 当全部音色描述。
- **为何错**：乐器是声源，音色是微观物理质感；只写乐器名，模型套用标准预设，失去控制。
- **正确推理（Instrumentation → Production Style 的微观层）**：乐器名 + 音色质感分层写。
- **正解**：
  > *"felt piano, muffled and dark with prominent mechanical hammer noises, intimate close-miced."* （乐器 = felt piano；音色 = muffled/dark/mechanical hammer/close-miced）

## 6. 忽略动态曲线（Ignoring Dynamic Curve）
- **误读**：全程同音量、同密度。
- **为何错**：无能量包络 → 平淡。
- **正确推理**：显式规划 静→涌→爆→落 的宏观动态。
- **正解**：
  > *"High dynamic contrast: uncompressed breathing peaks in verse → heavily saturated wall-of-sound compression in chorus → gradual decay in outro."*

## 7. 电影感 ≠ 大（Cinematic ≠ Loud/Big）
- **误读**：`"cinematic"` → 加满乐器、推大声。
- **为何错**：电影感来自叙事张力与留白，不是响度。
- **正确推理（叙事优先于"大"）**：用镜头式推进与休止制造张力。
- **正解**：
  > *"Cinematic not by size but by narrative: a single distant piano establishes isolation, then a two-second total dropout builds suspense before the score resolves."*

## 8. 不懂留白（Misunderstanding Negative Space）
- **误读**：每秒塞满乐器。
- **为何错**：无留白 = 无呼吸 = 疲劳。
- **正确推理**：留白（rests / sparse / 减法）是编曲核心手段。
- **正解**：
  > *"Arrangement evolves via subtractive engineering: verse strips all low-end, leaving naked midrange and air; the drop-out before chorus is intentional silence."*

## 9. 混音描述 ≠ 创作描述（Mixing ≠ Composition）
- **误读**：把 `wide stereo reverb` 当成"写歌"。
- **为何错**：混音（production）描述空间/动态/质感；创作（composition）描述音符/结构/主题。混用让模型不知该写什么。
- **正确推理**：创作描述进 Concept/Structure；混音描述进 Production Aesthetic 的 `Mixing:` 行。
- **正解**：`Theme: walking alone at midnight`（创作） + `Mixing: narrow mono-ish verse expanding to ultra-wide chorus with plate reverb tail`（混音）。

## 10. 过度相信形容词（Over-trusting Adjectives）
- **误读**：`"dark, sad, beautiful, epic, dreamy, emotional, powerful"` 一长串。
- **为何错**：形容词互相抵消，且都不驱动参数；堆砌稀释控制力。
- **正确推理（避免形容词堆砌）**：用 1–2 个精准动作描述替代一长串形容词。
- **正解**：用 `"intimate verse that strips to a single vocal and felt piano, then expands into a towering orchestral climax"` 替代形容词清单。

## 11. 忽略音乐逻辑冲突（Musical Logic Conflicts）
- **误读**：同一段落既要 `whisper` 又要 `wall of sound`，或 `acoustic` 与 `heavy distortion` 并存，无过渡、无演变。
- **为何错**：逻辑冲突 → 模型硬拼 → 生硬断裂或自相矛盾。
- **正确推理（输出前逻辑自检）**：检查调性/速度一致、人声距离与能量等级是否随段落演进、冲突元素是否有演变路径。
- **正解**：
  > *"Verse is an in-your-ear whisper; by the final chorus it opens into a stadium-sized wall of sound — the distance shift is driven by a deliberate pre-chorus riser, not an abrupt jump."*

---

## 12. v1.0 新增陷阱（多层路由专属）

### 12a. 平铺优先级（Flat Priority List）
- **误读**：把互相冲突的风格标签平铺一行（`90s J-Pop, Modern, Warm, Vintage, Cinematic`）。
- **为何错**：模型等比混合冲突标签，权重互相抵消，身份模糊。
- **正确**：套 Style Conditioning 的 **Priority System**（Primary / Secondary / Supporting / Production Standard），明确主次。
- **正解**：
  > *"Primary Identity: Modern 2026 Japanese Pop Production / Secondary Influence: 1990s J-Pop melodic language / Supporting Texture: Warm Rhodes / Production Standard: Modern streaming-quality mix"*

### 12b. 固定时间模板（Fixed-Time Template）
- **误读**：`Intro 0:00-0:15 / Verse 0:15-0:50` 套餐式结构。
- **为何错**：所有歌结构雷同、缺乏自然发展，AI 像套模板。
- **正确**：Functional Song Structure —— 每段写 Function / Emotional Purpose / Energy / Relative Duration，**不写固定秒数套餐**（用户硬给时间戳则补写功能）。
- **正解**：
  > *"Intro: Function: establish emotional identity. Energy: Low. Purpose: atmosphere before theme. Duration: Short."*

### 12c. 结构当风格（Structure-as-Style）
- **误读**：把 Verse / Chorus 当成音色 / 风格标签写进 Style 层。
- **为何错**：结构（叙事形式）与风格（声音身份）是不同控制通道，混写让模型既乱曲式又乱混音。
- **正确**：结构归 Song Structure 层；风格身份归 Style Conditioning 层；二者在八层路由中分属不同通道。
- **正解**：`[Style Conditioning] Genre: R&B` + `[Song Structure] Chorus: Function: emotional peak`。

### 12d. 关键词堆叠（Keyword Stacking）
- **误读**：一长串形容词 `dark, sad, beautiful, epic, dreamy, cinematic, professional`。
- **为何错**：形容词不驱动任何参数，且互相抵消，控制力稀释；这是"单 Prompt"模式的典型失效。
- **正确**：拆为八层控制通道，每层用动作词与具体声学描述，而非形容词清单。
- **正解**：用八层路由替代形容词堆砌（见 `examples.md`）。

---

## 推理自检清单（输出前必过）
- [ ] 情绪是否转成了"发展弧"而非形容词？
- [ ] 是否区分了歌曲结构（叙事）与声音结构（频段/空间）？
- [ ] 每个乐器/织体是否绑定了时间或段落？
- [ ] 是否避免了"像某艺人"，改用声学描述？
- [ ] 是否区分了乐器名与音色质感？
- [ ] 是否画出了动态曲线（静→涌→爆→落）？
- [ ] "电影感"是否落在叙事/留白而非单纯的"大"？
- [ ] 是否用了留白/减法？
- [ ] 是否区分了混音描述与创作描述？
- [ ] 形容词是否精简（≤2 个精准词）？
- [ ] 是否存在音乐逻辑冲突（调性/速度/人声距离/能量跳跃）？
- [ ] Style Conditioning 是否用了优先级系统（非平铺标签）？
- [ ] Song Structure 是否功能型（非固定时间套餐）？
- [ ] 是否把结构（Verse/Chorus）误写进风格层？
- [ ] 是否仍残留关键词堆叠（形容词清单）而非八层路由？
- [ ] **v1.1** Style Prompt 是否为**一行高密度标签矩阵**（无剧情 / 时间线 / 制作过程）？
- [ ] **v1.1** Structure Blueprint 是否只写发展（无音色标签矩阵混入）？两段边界是否严格分开？
