# Lyria Agent 提示词架构 v1.0：八层 JSON Schema + 编译器逻辑

本文件提供可供 Agent 自动驱动 Lyria 的**分层（routing）结构化执行架构**（JSON Schema），以及将其编译为 Lyria / Suno 理解效果最好的自然语言提示词的"制作人语言编译器"逻辑。

> v1.0 核心变更：从"单 Prompt"改为**多层控制路由**。JSON Schema 的顶层不再是平铺字段，而是八个独立控制层（Control Layers），每层对应一个控制通道，避免模型压缩信息、混淆结构/风格/制作。

## 一、LyriaProducerRoutingSchema（八层 JSON Schema）

```json
{
  "$schema": "http://json-schema.org/draft-07/schema#",
  "title": "LyriaProducerRoutingSchema",
  "description": "v1.0 AI 音乐制作人决策系统：将创作意图路由为八个独立控制层，供 Agent 驱动 Lyria / Suno 生成。",
  "type": "object",
  "properties": {

    "mode": {
      "type": "string",
      "description": "生成模式：song(流行歌曲) / score(影视游戏配乐) / bgm-loop(循环背景)。决定结构词汇与人声开关。",
      "enum": ["song", "score", "bgm-loop"],
      "example": "song"
    },

    "music_identity": {
      "type": "string",
      "description": "LAYER 1 音乐身份：一句话定位这首音乐是谁（身份，不涉及时机）。",
      "example": "A late-night intimate R&B ballad about an unsaid goodbye."
    },

    "style_conditioning": {
      "type": "object",
      "description": "LAYER 2 风格控制层：控制'音乐是什么身份'，含优先级系统，禁止平铺。",
      "properties": {
        "genre": { "type": "string", "example": "R&B / Neo-Soul" },
        "sub_genre": { "type": "string", "example": "Late-night ballad" },
        "era": { "type": "string", "example": "Contemporary" },
        "mood": { "type": "string", "example": "Melancholic, restrained" },
        "tempo": { "type": "string", "example": "70 BPM, lazy swung pocket" },
        "core_instruments": {
          "type": "array",
          "items": { "type": "string" },
          "example": ["warm piano", "Rhodes", "upright bass", "brushed drums"]
        },
        "vocal_character": { "type": "string", "example": "Breathy, close-miced, conversational" },
        "production_aesthetic": { "type": "string", "example": "Bedroom R&B, vinyl warmth" },
        "priority_system": {
          "type": "object",
          "description": "优先级系统：明确主次，避免标签权重冲突。",
          "properties": {
            "primary_identity": { "type": "string", "example": "Modern 2026 Japanese Pop Production" },
            "secondary_influence": { "type": "string", "example": "1990s J-Pop melodic language" },
            "supporting_texture": { "type": "string", "example": "Warm Rhodes" },
            "production_standard": { "type": "string", "example": "Modern streaming-quality mix" }
          }
        }
      },
      "required": ["genre", "tempo", "core_instruments", "priority_system"]
    },

    "era_translation": {
      "type": "object",
      "description": "时代转换层：复古请求必填。判定复刻还是现代重构，并分层 KEEP/UPGRADE。",
      "properties": {
        "direction": { "type": "string", "enum": ["historical_recreation", "modern_interpretation"], "example": "modern_interpretation" },
        "keep": {
          "type": "array",
          "items": { "type": "string" },
          "example": ["melodic language", "harmonic characteristics", "emotional quality"]
        },
        "upgrade": {
          "type": "array",
          "items": { "type": "string" },
          "example": ["drum design", "low-end control", "high-frequency extension", "stereo width", "vocal production", "dynamic control"]
        }
      }
    },

    "song_structure": {
      "type": "array",
      "description": "LAYER 3 功能型歌曲结构：禁止固定时间模板，按 Function/Purpose/Energy/Relative Duration 描述。纯音乐改用 Theme/Development/Variation/Climax/Resolution。",
      "items": {
        "type": "object",
        "properties": {
          "section": { "type": "string", "example": "Intro" },
          "function": { "type": "string", "example": "Establish emotional identity." },
          "emotional_purpose": { "type": "string", "example": "Create atmosphere before the main theme." },
          "energy_level": { "type": "string", "example": "Low (20%)" },
          "relative_duration": { "type": "string", "example": "Short opening section." },
          "time_window": { "type": "string", "description": "用户硬给时间戳时保留，否则省略。", "example": "0:00 - 0:20" }
        },
        "required": ["section", "function", "emotional_purpose", "energy_level", "relative_duration"]
      }
    },

    "emotional_arc": {
      "type": "object",
      "description": "LAYER 4 情绪能量曲线：各段能量等级 + 为什么变化（因果）。",
      "properties": {
        "energy_curve": {
          "type": "array",
          "items": {
            "type": "object",
            "properties": {
              "section": { "type": "string", "example": "Chorus" },
              "energy": { "type": "string", "example": "85%" },
              "why": { "type": "string", "example": "Emotional peak; the unsaid goodnight lands as a held breath." }
            }
          }
        }
      }
    },

    "arrangement_blueprint": {
      "type": "array",
      "description": "LAYER 5 编曲蓝图：织体随段落的加减演变（减法编曲、能量梯度）。",
      "items": {
        "type": "object",
        "properties": {
          "section": { "type": "string", "example": "Verse" },
          "arrangement_shift": {
            "type": "string",
            "example": "Strip away all drums. Keep only dry close-miced vocal and felt piano."
          },
          "variation_dimension": {
            "type": "string",
            "description": "段落变化来自哪个维度（非必加乐器）：Arrangement/Harmony/Melody/Performance/Dynamics/Texture/Space。",
            "example": "Space (wider reverb) + Performance (whispered delivery)"
          }
        }
      }
    },

    "instrument_roles": {
      "type": "array",
      "description": "LAYER 6 乐器职责：每件乐器按 Role/Rhythmic Function/Interaction 定义。新增乐器套 Rhythm Integration 五维。",
      "items": {
        "type": "object",
        "properties": {
          "instrument": { "type": "string", "example": "Piano" },
          "role": { "type": "string", "example": "Main emotional narrator." },
          "rhythmic_function": { "type": "string", "example": "Carries the main groove." },
          "interaction": { "type": "string", "example": "Guitar follows piano; remains supportive." },
          "rhythm_integration": {
            "type": "object",
            "description": "新增乐器必填五维。",
            "properties": {
              "tempo_relationship": { "type": "string", "example": "Locks 70 BPM pocket." },
              "beat_alignment": { "type": "string", "example": "Accents on soft 2 & 4." },
              "accent_placement": { "type": "string", "example": "Aligns with main downbeats." },
              "groove_interaction": { "type": "string", "example": "Shares piano groove, no competing pattern." },
              "density_control": { "type": "string", "example": "Low; supportive bed." }
            }
          }
        },
        "required": ["instrument", "role", "interaction"]
      }
    },

    "production_direction": {
      "type": "object",
      "description": "LAYER 7 声音制作方向：拆 Recording / Mixing / Mastering 三层。",
      "properties": {
        "recording": {
          "type": "string",
          "example": "Close microphone character. Natural room ambience. High quality recording environment."
        },
        "mixing": {
          "type": "string",
          "example": "Clear vocal placement. Controlled low-end. Balanced frequency spectrum. Defined stereo image."
        },
        "mastering": {
          "type": "string",
          "example": "Commercial release standard. Natural loudness. Preserved dynamics. NOT overcompressed."
        }
      },
      "required": ["recording", "mixing", "mastering"]
    },

    "negative_constraints": {
      "type": "array",
      "description": "LAYER 8 负向约束：明确禁止的失效模式。",
      "items": { "type": "string" },
      "example": [
        "unnecessary cinematic climax",
        "excessive orchestration",
        "competing rhythmic patterns",
        "muddy low-end",
        "narrow stereo image",
        "outdated production style",
        "excessive compression"
      ]
    }

  },
  "required": [
    "mode",
    "music_identity",
    "style_conditioning",
    "song_structure",
    "emotional_arc",
    "arrangement_blueprint",
    "instrument_roles",
    "production_direction",
    "negative_constraints"
  ]
}
```

## 二、制作人语言编译器（Compiler Logic）

**关键规则：绝不能把整段 JSON 原封不动发给 Lyria。** Lyria / Suno 接收的是自然语言 Prompt。
编译器把八个控制层线性化为**分层路由文本**，遵循以下映射：

- **输出语言：英文**（除非用户明确要求中文）。
- `mode` → 决定结构词汇与是否出人声：`score` / `bgm-loop` / `instrumental` 时**整体跳过**人声，结构改用 Theme/Development/Variation/Climax/Resolution。
- `music_identity` → 顶层 `[Music Identity]` 块。
- `style_conditioning` → `[Style Conditioning]` 块，**必须展开 `priority_system`**（Primary / Secondary / Supporting / Production Standard），不得平铺。
- `era_translation` → 若复古请求存在，展开为 `[Era Translation]` 块，写明 direction 与 KEEP/UPGRADE 清单。
- `song_structure[]` → `[Song Structure]` 块，每段写 Function / Purpose / Energy / Duration（**不写固定秒数套餐**，除非用户硬给时间戳）。
- `emotional_arc` → `[Emotional Arc]` 块，能量曲线 + 每段 why。
- `arrangement_blueprint[]` → `[Arrangement Blueprint]` 块，织体加减 + 变化维度。
- `instrument_roles[]` → `[Instrument Roles]` 块，每件乐器 Role / Rhythmic Function / Interaction；新增乐器展开 `rhythm_integration` 五维。
- `production_direction` → `[Production Direction]` 块，拆 Recording / Mixing / Mastering 三子块。
- `negative_constraints[]` → `[Negative Constraints]` 块，逐条列出。

### 编译输出示例（八层路由 · 英文）

```text
[Music Identity]
A late-night intimate R&B ballad about an unsaid "goodnight".

[Style Conditioning]
Genre: R&B / Neo-Soul. Sub-genre: Late-night ballad. Tempo: 70 BPM lazy swung pocket.
Priority System:
  Primary Identity:       Modern intimate R&B production
  Secondary Influence:    Neo-soul harmonic warmth
  Supporting Texture:     Warm Rhodes
  Production Standard:    Bedroom-grade clean mix, not stadium

[Song Structure]
Intro:   Function: establish loneliness. Energy: Low. Duration: Short.
Verse:   Function: intimate storytelling. Energy: 40%. Duration: Medium.
Chorus:  Function: emotional peak, restrained ache. Energy: 85%. Duration: Medium.
Bridge:  Function: open up then accept. Energy: 50%. Duration: Short.
Outro:   Function: return to empty room. Energy: 20%. Duration: Short.

[Emotional Arc]
Verse 40% -> Pre 60% -> Chorus 85% (the unsaid goodnight lands as a held breath)
-> Bridge 50% (most open, still restrained) -> Final Chorus 100% then peel away.

[Arrangement Blueprint]
Intro: ONLY piano + upright bass, huge space.
Verse: add Rhodes + brushed drums (supportive, no competing groove).
Chorus: fuller (soft kick + snare, layered harmonies) but transparent.
Bridge: strip to solo piano + breathy vocal (variation via Space + Performance).

[Instrument Roles]
Piano:   Role: main emotional narrator. Interaction: guitar follows it.
Bass:    Role: low-end anchor. Interaction: locks with kick, no mud.
Drums:   Role: lazy pocket. Rhythm Integration: locks 70 BPM; accents soft 2&4;
         shares groove; density low; supportive not feature.

[Production Direction]
Recording: Close-mic breathy vocal, natural room air.
Mixing:    Clear vocal center, controlled low-end, wide-but-intimate reverb.
Mastering: Commercial standard, natural loudness, preserved dynamics, NOT brickwall.

[Negative Constraints]
Avoid: excessive orchestration, competing rhythmic patterns, muddy low-end,
narrow stereo image, outdated production style, excessive compression.
```

## 三、简化版（最小可行架构）

若用户只需最小可行架构，可只输出 `music_identity` + `style_conditioning`（含 priority_system）+ `song_structure`（功能型）三层，但仍须经编译器转为分层自然语言后再提交 Lyria，不得直接发 JSON。
