# 暖阳与三花猫少女 · AI 创作项目

> **Warm Sun and the Calico Cat Girl** — A healing-genre AI original IP project.
> A complete, reproducible workflow from story → character design → image prompts → video storyboard → video prompts.

[English](#english) · [中文](#中文)

---

## 中文

### 项目简介

**「暖阳与三花猫少女」** 是一个治愈系 AI 原创 IP 创作项目,记录从故事构思、角色定调、图像提示词,到视频分镜与生成的全流程工作流。

主角是 **温柚**,一只半人半猫的 **三花猫耳少女**:18 岁,黑/白/橘三色长发,琥珀色眼眸,身披蓬松三花尾巴,性格慵懒温顺。项目调性是「慢、柔、轻、慵懒、治愈」,围绕 **晨光暖金 → 午后暖黄 → 夕阳橘粉** 的光影变化展开一天的日常。

### 核心设定(影响所有创作)

| 维度 | 设定 |
|------|------|
| 主角 | 温柚,18 岁,三花猫耳少女 |
| 发色分布 | 黑 / 白 / 橘三色,长直发为主 |
| 眼眸 | 琥珀色 |
| 服饰 | 米杏色宽松露肩针织毛衣(肌肤映暖光) |
| 主线道具 | 洋甘菊(chamomile)花束 |
| 光影时段 | 晨光暖金 → 午后暖黄 → 夕阳橘粉(**不可逆序**) |
| 调性 | 慢、柔、轻、慵懒、治愈 |

### 目录结构

```
ai-creative-sanhua/
├── 00-AI创作-三花地图.md           # 项目 MOC 索引(从这里开始读)
├── 01第一幕  暖阳与三花猫少女.md    # 故事原文:清晨起床
├── 02第二幕  暖阳与三花猫少女.md    # 故事原文:出门/花店/甜品/归家
├── 第一步:txt2img ,img2img.md      # 图像生成流程与提示词模板
├── 03 分镜脚本01.md                 # 62 秒版视频分镜
├── 04 视频分镜脚本_90秒版.md        # 90 秒版分镜(留作参考)
├── 05 各分镜镜头视频提示词.md        # 19 个单镜头英文 prompt
├── 01-1.png / 02-1.png             # 主角定妆参考图
├── 视频创作/                        # v1:9 段分段 prompt(10s/段上限)
│   ├── README.md
│   ├── 01_段01_…md ~ 09_段09_…md
│   └── video-doubao/               # 早期豆包平台试验素材
└── 视频创作v2/                      # ★ 当前生产版:5 段分段(15s/段)
    ├── README.md                   #   v2 总览(段间冗余 + @ 引用体系)
    ├── 01_段01_…md
    ├── 02_段02_…md
    ├── 03a_段03_…md
    ├── 03b_段03_…md
    ├── 04_段04_…md
    └── 05_段05_…md
```

### 创作工作流

```
Step 1  故事构思
   ↓  01第一幕 / 02第二幕
Step 2  角色形象定调
   ↓  第一步:txt2img,img2img
Step 3  图像生成(txt2img → img2img)
   ↓
Step 4  视频分镜设计
   ↓  03 分镜脚本01 → 04 视频分镜脚本_90秒版
Step 5  视频 prompt 分段(v1 / v2 双版本)
   ↓  视频创作/ 9 段(10s 上限,留作对比)
   ↓  视频创作v2/ 5 段 + 段 03 拆 a/b(15s 上限,@ 引用,段间冗余)★ 当前
   ↓  (备选:05 各分镜镜头视频提示词 单镜打磨)
Step 6  第三方视频生成(可灵 / Sora / Runway / 通义万相 / 智谱清影)
   ↓
Step 7  后期拼接(剪映 / CapCut / PR)+ BGM + 调色
```

### 当前进度

- [x] 故事:第一幕 + 第二幕(完成)
- [x] 图像:提示词模板已固化(完成)
- [x] 视频分镜:62s + 90s 双版本(完成)
- [x] 视频 prompt 单镜版:19 镜头英文模板(完成)
- [x] **视频 prompt 分段版 v1**:9 段分段(10s/段,完成,留作对比)
- [x] **视频 prompt 分段版 v2**:5 段分镜(15s/段 + @ 引用 + 段间冗余,完成)★ 当前生产
- [ ] 视频生成:需用第三方工具(见下方推荐)
- [ ] 后期:BGM + 拼接 + 调色(待视频片段产出后)

### 视频生成工具推荐

> 本仓库不包含视频生成能力,需接入第三方 AI 视频平台。

| 平台 | 单次上限 | 是否支持 i2v | 适配版本 |
|------|----------|--------------|----------|
| **可灵 1.6(Kling)** | **15s** | ✅(可上传锚帧) | **★ v2(当前)** |
| Sora | ~20s | ✅ | v2(截断 15s) |
| Runway Gen-3 | 10s | ✅ | v1(9 段) |
| 通义万相 | 5s | 部分 | v1(重新切片) |
| 智谱清影 | 6s | 部分 | v1(重新切片) |

**v2 关键设计**:
- 段间冗余(sliding window):每段头 2-3s 与上一段末帧重叠,便于 i2v 锚点对接
- 段 03 拆 03a / 03b,作为整套的 i2v 锚点
- `@<图片名>` 引用上一段输出截图,保证角色/场景一致性

### 许可证

本仓库的 **代码与脚本部分** 默认仅作示例参考;
**故事原文、角色设定、提示词** 采用 [CC BY-NC 4.0](LICENSE) 许可 ——
允许非商业转载与改编,需署名作者,不得商业使用。

### 致谢

- 角色灵感:经典三花猫 + 治愈系日常 ACG
- 平台工具:可灵 / Sora / Runway / 豆包
- 知识管理:Obsidian

---

## English

### Project Overview

**"Warm Sun and the Calico Cat Girl"** is a healing-genre AI original IP project documenting the full creative workflow: from story conception and character design, through image prompting, to video storyboarding and generation.

The protagonist is **Wen You (温柚)**, a half-human half-cat **calico-eared girl**: 18 years old, with long tri-color (black / white / orange) hair, amber eyes, and a fluffy calico tail. Her personality is languid and gentle. The story follows one slow, sun-soaked day, illuminated by three distinct lighting periods — **morning gold → afternoon warm yellow → sunset orange-pink** — in chronological order.

### Core Design (applies to all content)

| Aspect | Design |
|--------|--------|
| Protagonist | Wen You, 18, calico-eared cat girl |
| Hair | Tri-color black / white / orange, long |
| Eyes | Amber |
| Outfit | Loose cream / apricot off-shoulder knit sweater |
| Main prop | Chamomile bouquet |
| Lighting periods | Morning gold → afternoon yellow → sunset orange-pink (**strictly sequential**) |
| Tone | Slow, soft, light, languid, healing |

### Directory Layout

```
ai-creative-sanhua/
├── 00-AI创作-三花地图.md           # MOC index (start here)
├── 01第一幕  暖阳与三花猫少女.md    # Act I: waking up
├── 02第二幕  暖阳与三花猫少女.md    # Act II: outing / flowers / dessert / home
├── 第一步:txt2img ,img2img.md      # Image generation flow & prompt template
├── 03 分镜脚本01.md                 # 62-second storyboard
├── 04 视频分镜脚本_90秒版.md        # 90-second storyboard (reference)
├── 05 各分镜镜头视频提示词.md        # 19 single-shot English prompts
├── 01-1.png / 02-1.png             # Character design references
├── 视频创作/                        # v1: 9 segments (10s/segment cap)
│   ├── README.md
│   ├── 01_段01_…md ~ 09_段09_…md
│   └── video-doubao/               # Early Doubao platform experiments
└── 视频创作v2/                      # ★ Current production: 5 segments (15s/segment)
    ├── README.md
    ├── 01_段01_…md
    ├── 02_段02_…md
    ├── 03a_段03_…md
    ├── 03b_段03_…md
    ├── 04_段04_…md
    └── 05_段05_…md
```

### Workflow

```
Step 1  Story drafting
   ↓  01 / 02 (Act I / II)
Step 2  Character design
   ↓  第一步:txt2img,img2img
Step 3  Image generation (txt2img → img2img)
   ↓
Step 4  Video storyboarding
   ↓  03 → 04 (62s / 90s versions)
Step 5  Video prompt segmentation (v1 / v2 dual version)
   ↓  视频创作/  9 segments (10s cap, kept for reference)
   ↓  视频创作v2/  5 segments + 03a/03b split (15s cap, @ references, segment overlap) ★ Current
   ↓  (alternative: 05 single-shot English prompts for fine-tuning)
Step 6  Third-party video generation (Kling / Sora / Runway / Tongyi / Zhipu)
   ↓
Step 7  Post-production (CapCut / Premiere) — BGM, color grade, assembly
```

### Status

- [x] Story: Acts I + II
- [x] Image: prompt template finalized
- [x] Storyboard: 62s + 90s dual versions
- [x] Single-shot video prompts: 19 English templates
- [x] **Segmented video prompts v1**: 9 segments (10s/seg)
- [x] **Segmented video prompts v2**: 5 segments (15s/seg + @-refs + overlap) ★ Current
- [ ] Video generation: pending third-party tools
- [ ] Post-production: pending clip output

### Recommended Video Tools

| Platform | Single-shot cap | i2v support | Best for |
|----------|-----------------|-------------|----------|
| **Kling 1.6** | **15s** | ✅(anchor frames) | **★ v2 (current)** |
| Sora | ~20s | ✅ | v2 (truncated to 15s) |
| Runway Gen-3 | 10s | ✅ | v1 (9 segments) |
| Tongyi Wanxiang | 5s | partial | v1 (re-split) |
| Zhipu Qingying | 6s | partial | v1 (re-split) |

**v2 key design**:
- **Sliding-window overlap**: every segment's first 2-3s mirrors the previous segment's last frame, enabling clean i2v anchor handoff
- **Segment 03 split (03a / 03b)** acts as the i2v anchor pivot for the whole set
- **`@<image_name>` references** in prompts point to the prior segment's output frame, locking character & scene consistency

### License

**Code and scripts** in this repo are provided as reference examples.
**Story text, character design, and prompts** are released under [CC BY-NC 4.0](LICENSE) ——
free to share and adapt non-commercially with attribution; commercial use is not permitted.

### Credits

- Character inspiration: classic calico cat + healing-genre daily-life ACG
- Platform tools: Kling / Sora / Runway / Doubao
- Knowledge management: Obsidian

---

**Author**: Mik-Rain · **Repository**: `ai-creative-sanhua` · **Created**: 2026-07-20
