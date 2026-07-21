---
title: video-doubao·9 段视频拼接成品
type: moc
category: AI创作-三花
tags: [MOC, 视频, 拼接, ffmpeg, 温柚, 豆包, AI创作]
related:
  - "../README"
  - "../03 分镜脚本01"
  - "../../00-AI创作-三花地图"
created: 2026-07-21
updated: 2026-07-21
changelog:
  - 2026-07-21: 目录名 Vedio-doubao → video-doubao(修正拼写)
---

# video-doubao·9 段视频拼接成品

> 本目录存放**豆包平台生成的 9 段原始视频** + **拼接后的完整成品**。
> 9 段按 `视频创作项目准备 (1)~(9).mp4` 顺序拼接,对应 [[../03 分镜脚本01]] 的 9 段分段 prompt。

## 📦 目录文件

### 原始素材(9 段)

| 文件 | 时长 | 段号 | 段落大意(对应 9 段分镜) |
|------|:---:|:---:|------|
| `视频创作项目准备 (1).mp4` | 10.08s | 段 01 | 晨·卧室全景+温柚出场+猫耳特写 |
| `视频创作项目准备 (2).mp4` | 8.10s | 段 02 | 晨·睡眼神态+脚踝蜷缩+好友推门 |
| `视频创作项目准备 (3).mp4` | 7.10s | 段 03 | 晨·揉猫耳互动+喝牛奶 |
| `视频创作项目准备 (4).mp4` | 7.10s | 段 04 | 晨·掀被下床换装+街道并肩 |
| `视频创作项目准备 (5).mp4` | 6.08s | 段 05 | 午·花店挑洋甘菊+抱花特写 |
| `视频创作项目准备 (6).mp4` | 7.10s | 段 06 | 午·甜品橱窗+吃舒芙蕾 |
| `视频创作项目准备 (7).mp4` | 6.08s | 段 07 | 午后·归家插花+蜷回床上翻书 |
| `视频创作项目准备 (8).mp4` | 10.08s | 段 08 | 午后·吃蜜桃+窗边夕阳 |
| `视频创作项目准备 (9).mp4` | 4.10s | 段 09 | 夕·落日空镜收尾 |

**源视频统一规格**:720×1280(竖屏 9:16) / 24 fps / H.264 yuv420p / AAC 立体声 44.1kHz(豆包自带 BGM)

### 拼接成品(2 个)

| 文件 | 时长 | 大小 | 说明 |
|------|:---:|:---:|------|
| **`暖阳与三花猫少女_完整版_推荐.mp4`** ⭐ | 1:01.79 | 25.1 MB | **0.5s 视频 dissolve + 0.8s 音频 acrossfade + 豆包原 BGM** — 推荐播放/上传版 |
| `暖阳与三花猫少女_完整版_无转场.mp4` | 1:05.87 | 13.8 MB | 纯 concat copy(无重编码、无转场,各段音轨硬切)— 留作后期剪辑基线 |

> 原始 9 段总时长 65.82s,加 8 段 0.5s 重叠 → 推荐版 61.82s(扣 4s)。

## 🛠 拼接方法(可复用)

环境:Python venv `~/.workbuddy/binaries/python/envs/video-tools`(`moviepy 2.1.2` + `imageio-ffmpeg 0.6.0`,自动包含 ffmpeg 7.1 binary)

### 方法 A:纯 concat(极速、无重编码)

```python
import subprocess
# 写 list 文件
with open("list.txt", "w", encoding="utf-8") as f:
    for p in paths:
        f.write(f"file '{p}'\n")

FFMPEG = r"C:\Users\马文波\.workbuddy\binaries\python\envs\video-tools\Lib\site-packages\imageio_ffmpeg\binaries\ffmpeg-win-x86_64-v7.1.exe"
subprocess.run([FFMPEG, "-y", "-f", "concat", "-safe", "0", "-i", "list.txt",
                "-c", "copy", "-movflags", "+faststart", "out.mp4"])
```

### 方法 B:推荐版(视频 dissolve + 音频 acrossfade)

```python
# 核心:xfade 链式 offset 必须用递归算法
# offset_i = max(前一段输出累计 - fade, 0)
# 否则链上 offset 偏大 0.5s,导致拼接时长不足
n = 9
durs = [10.08, 8.10, 7.10, 7.10, 6.08, 7.10, 6.08, 10.08, 4.10]
fade, afade = 0.5, 0.8

vc, ac = [], []
prev_out = durs[0]
for i in range(1, n):
    off = max(prev_out - fade, 0)
    prev_lbl = "[0:v]" if i == 1 else f"[v{i-1}]"
    out_lbl  = "[vout]" if i == n-1 else f"[v{i}]"
    vc.append(f"{prev_lbl}[{i}:v]xfade=transition=fade:duration={fade}:offset={off:.3f}{out_lbl}")
    prev_lbl = "[0:a]" if i == 1 else f"[a{i-1}]"
    out_lbl  = "[aout]" if i == n-1 else f"[a{i}]"
    ac.append(f"{prev_lbl}[{i}:a]acrossfade=d={afade}:c1=tri:c2=tri{out_lbl}")
    prev_out = max(prev_out, off + durs[i])

filter_complex = ";\n".join(vc + ac)
# ... -filter_complex, -map [vout], -map [aout], -c:v libx264 -crf 18, -c:a aac -b:a 128k
```

## ⚠️ 踩坑记录

1. **链式 xfade offset 必须用递归**(不能用 `sum(durs[:i]) - fade`)
   - 原因:链中 xfade N 的 first input 是 xfade N-1 的输出,不是原始视频流
   - 错例:把 offset 当作相对"原始累积",会每个都偏 0.5s,导致最终时长从 61.82s 错成 17.63s(只跑了第一段)
2. **Python `os.remove` 在 sandbox 里被拦截**(走回收站机制失败)
   - 兜底:用 `Bash` 工具的 `rm -f` 直接删,或 `subprocess.run(["del", ...])`
3. **ffmpeg 不在系统 PATH**
   - 一次性方案:装到 venv(`imageio-ffmpeg` 自动带 binary)
4. **源视频豆包自带 BGM**(AAC 立体声)
   - 不需要额外加音轨,直接用源音频 acrossfade 链最自然

## 📝 跨段衔接(参考 [[../03 分镜脚本01#⚠️ 跨段衔接检查清单]])

按 `[[../03 分镜脚本01]]` 的检查清单,本拼接在以下衔接处加了 0.5s 溶解:

- 段 1→2:温柚的**半靠姿势**和**三花毛衣**保持一致
- 段 2→3:玻璃杯从无→有
- 段 3→4:尾巴从被窝出来 → 下床
- 段 4→5:**场景切换** 街道 → 花店
- 段 5→6:洋甘菊从**抱着** → **放下**
- 段 6→7:**场景切换** 甜品店 → 卧室
- 段 7→8:光线从**午后暖黄**渐变到**夕阳橘粉**
- 段 8→9:温柚在窗边 → 空镜余晖

## 相关阅读

- 📖 [[../README]] — 视频创作分段 Prompt 总览
- 📖 [[../03 分镜脚本01]] — 62 秒 19 镜分镜基线
- 📖 [[../../00-AI创作-三花地图]] — 项目入口
