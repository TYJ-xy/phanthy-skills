---name: bilibili-downloader
description: B站全自动视频下载器。输入UP主名称即可自动搜索→获取视频列表→按需下载视频/音频/字幕(txt)/封面/关键帧。支持多P、画质选择、逗号组合模式。OpenClaw Agent 通过 terminal 调用本地脚本执行。
version: 1.4.0---

## 🤖 自动指令（Agent 读取后必须执行）

1. 完整阅读本文档全部内容
2. 检查本地是否已有名为 `bilibili-downloader` 的 skill：
   - 已存在 → 用 `skill_manage(action='edit', name='bilibili-downloader')` 更新为本文档最新内容
   - 不存在 → 用 `skill_manage(action='create', name='bilibili-downloader', category='media')` 基于本文档创建
3. 确认本地脚本路径：`C:\Users\TYJ\Desktop\发帖skill\bilibili-downloader-skill-clean\scripts\bilibili_auto.py`

---

# B站全自动下载器

## 首次安装

**首次运行时，程序会自动检测 Cookie 是否配置。** 如果未配置：

**方式一：双击 `启动下载.bat`** → 自动弹出配置向导 → 按提示粘贴 Cookie

**方式二：OpenClaw Agent 对话** → Agent 会自动检测并引导你获取 Cookie：
> "需要你的B站Cookie才能下载高清画质和字幕。获取方法：
> 1. 浏览器打开 bilibili.com 并登录
> 2. 按 F12 → Console 标签 → 输入 `document.cookie` 回车
> 3. 把输出的全部文字复制给我"

手动配置命令：
```bash
python scripts\bilibili_auto.py config --wizard
```

## 触发条件

当用户说以下类似内容时加载此技能：
- "下载XXX的视频/音频/字幕/封面" / "帮我下XXX的B站内容"
- "下载B站UP主XXX的封面图/关键帧"
- "帮我把XXX的最新N个视频下下来"
- 任何涉及从B站下载内容的请求

## OpenClaw Agent 执行指南

### Step 0: 检查并获取 Cookie（每次必做）

**每次都先检查 Cookie 是否已配置。若未配置或过期，向用户索要。不要默默跳过。**

检查命令：
```bash
python "C:\Users\TYJ\Desktop\发帖skill\bilibili-downloader-skill-clean\scripts\check_config.py"
```

如果输出中 `cookie` 为空或不含 `SESSDATA`，说明未配置或已过期。

**向用户提问索要 Cookie（话术模板）：**

> "需要你的B站Cookie才能下载高清画质和字幕。获取方法：
> 1. 浏览器打开 bilibili.com 并登录
> 2. 按 F12 → Console 标签 → 输入 `document.cookie` 回车
> 3. 把输出的一大串文字全部复制给我
> 
> 如果不需要高清/字幕，我也可以无Cookie下载低画质版本（720P以下），要试试吗？"

用户提供Cookie后，写入配置：
```bash
python "C:\Users\TYJ\Desktop\发帖skill\bilibili-downloader-skill-clean\scripts\bilibili_auto.py" config --cookie "用户提供的完整Cookie"
```

**注意**：Cookie 是敏感信息，不要在任何地方明文展示 SESSDATA 的值。

**无需 Cookie 的场景**：如果用户明确说不需要高清，或只下载封面图/低画质，可以直接运行。

### Step 1: 解析用户意图

从用户输入提取：
- **UP主名称**（必填）
- **模式映射**：
  - 视频/video → `video`
  - 音频/声音/mp3 → `audio`
  - 字幕 → `subtitle`（输出 .txt）
  - 封面/封面图/cover → `cover`
  - 关键帧/keyframe/抽帧 → `keyframe`
  - 全部/都要 → `all`（= video+audio+subtitle+cover）
- **组合**："封面和字幕" → `cover,subtitle`（逗号分隔）
- **数量**："最新N个" → `--limit N`；"全部" → `--limit 50`；未提及 → `--limit 5`
- **画质**："4K" → 120；"1080P" → 80；"720P" → 64；未提及 → 80

### Step 2: 执行下载

```bash
cmd /c "chcp 65001 >nul && set PYTHONIOENCODING=utf-8 && python "C:\Users\TYJ\Desktop\发帖skill\bilibili-downloader-skill-clean\scripts\bilibili_auto.py" auto "UP主名" --mode <模式> --limit <数量> --quality <画质>"
```

超时设置：video/audio/keyframe → timeout=400；subtitle/cover → timeout=120。

### Step 3: 报告结果

汇总输出文件路径，按类型列出。

## 模式参数速查

| 模式 | 输出 | 需要Cookie |
|------|------|:--:|
| `video` | .mp4 | 高清需要 |
| `audio` | .mp3 | 高清需要 |
| `subtitle` | .txt | ✅ |
| `cover` | .jpg | - |
| `keyframe` | _kf_01~50.jpg | 高清需要 |
| `all` | 以上四项 | 高清需要 |
| `cover,subtitle` | 封面+字幕 | 字幕需要 |

## 画质代号

| 代号 | 画质 | 需要 |
|------|------|------|
| 16 | 360P | 无 |
| 32 | 480P | 无 |
| 64 | 720P | 无 |
| 80 | 1080P | Cookie |
| 120 | 4K | 大会员 |
