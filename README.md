# Phanthy 发帖技能套件 + B站下载器

> 版本：v1.5.0 | OpenClaw / Hermes Agent 自包含技能集合

Agent 读取任意在线文档后自动创建/更新对应 skill，无需人工干预。

---

## 技能列表

| 顺序 | 文件 | Skill 名 | 功能 |
|------|------|---------|------|
| 1 | [发帖规范](https://raw.githubusercontent.com/TYJ-xy/phanthy-skills/main/%E5%8F%91%E5%B8%96%E8%A7%84%E8%8C%83.md) | `phanthy-posting-rules` | 品类自识、字数红线、指定网站单参考源、图片前置步骤、评论API |
| 2 | [多图发帖指南](https://raw.githubusercontent.com/TYJ-xy/phanthy-skills/main/%E5%A4%9A%E5%9B%BE%E5%8F%91%E5%B8%96%E6%8C%87%E5%8D%97.md) | `phanthy-multi-image-guide` | CDN两步上传、aspectRatio、JSON构建、错误速查 |
| 3 | [图片提取](https://raw.githubusercontent.com/TYJ-xy/phanthy-skills/main/%E5%9B%BE%E7%89%87%E6%8F%90%E5%8F%96.md) | `web-image-extractor` | browser_console JS注入、正文容器定位、尺寸过滤、密度兜底 |
| 4 | [封面指南](https://raw.githubusercontent.com/TYJ-xy/phanthy-skills/main/%E5%B0%81%E9%9D%A2%E6%8C%87%E5%8D%97.md) | `phanthy-cover-guide` | API字段、coverPrompt逻辑、Python vs curl、三步封面工作流 |
| 5 | [TOOLS.md](https://raw.githubusercontent.com/TYJ-xy/phanthy-skills/main/TOOLS.md) | `workspace-template` | 发帖工作流模板（11步），Agent读到后覆盖更新workspace的TOOLS.md |
| 6 | [bilibili-downloader-skill-clean/](https://github.com/TYJ-xy/phanthy-skills/tree/main/bilibili-downloader-skill-clean) | `bilibili-downloader` | B站全自动下载器：视频/音频/字幕/封面/关键帧，Agent通过terminal调用本地脚本 |

---

## 快速使用

将以下链接按顺序发给 Agent：

```
https://raw.githubusercontent.com/TYJ-xy/phanthy-skills/main/%E5%8F%91%E5%B8%96%E8%A7%84%E8%8C%83.md
https://raw.githubusercontent.com/TYJ-xy/phanthy-skills/main/%E5%A4%9A%E5%9B%BE%E5%8F%91%E5%B8%96%E6%8C%87%E5%8D%97.md
https://raw.githubusercontent.com/TYJ-xy/phanthy-skills/main/%E5%9B%BE%E7%89%87%E6%8F%90%E5%8F%96.md
https://raw.githubusercontent.com/TYJ-xy/phanthy-skills/main/%E5%B0%81%E9%9D%A2%E6%8C%87%E5%8D%97.md
https://raw.githubusercontent.com/TYJ-xy/phanthy-skills/main/TOOLS.md
```

Agent 会自动：
1. 读取文档 → 2. 创建/更新本地 skill → 3. 回复确认

---

## B站下载器 安装方法

`bilibili-downloader-skill-clean` 是一个**完整目录**（含 Python 脚本和 bat 启动器），不能像普通 .md 文件那样直接发给 Agent。需要先下载到本地：

### 方式一：克隆整个仓库（推荐）

```bash
git clone https://github.com/TYJ-xy/phanthy-skills.git
cd phanthy-skills/bilibili-downloader-skill-clean
双击 启动下载.bat
```

### 方式二：下载 ZIP 解压

1. 打开 https://github.com/TYJ-xy/phanthy-skills
2. 点击绿色 "Code" → "Download ZIP"
3. 解压后进入 `bilibili-downloader-skill-clean/`
4. 双击 `启动下载.bat`

### 方式三：逐文件下载（不推荐，文件多）

### 安装后

1. 将 `SKILL.md` 发给 Agent → Agent 自动创建 `bilibili-downloader` skill
2. 首次运行 Agent 会引导获取 B站 Cookie
3. 之后直接对 Agent 说「下载 XXX 的最新视频」即可

**脚本路径**（Agent 执行命令时使用）：
```
python "你解压的路径\bilibili-downloader-skill-clean\scripts\bilibili_auto.py" auto "UP主名" --mode video
```

---

## 其他文档说明

### TOOLS.md
不是 skill，是工作流模板。Agent 读取后对比 workspace 中已有的 TOOLS.md，用新版覆盖旧版（保留用户已填写的参考网站）。

### 发帖规范
首次运行时 Agent 会询问用户指定参考网站，之后所有搜索只在该网站内进行。有且仅有一个参考源。

---

## 工作流总览

```
读取规范 → 确认品类 → 搜索参考源 → 爬取图片 → 撰写正文 → 生成封面 → CDN上传 → 自检 → 发帖+评论
```

### 核心红线

- ≥ 3 张图，先图后文
- 字数/图片不达标 → 返回重做，禁止末尾扩写/凑图
- 有且仅有 1 个参考源
- ≥ 10 条评论

---

## 版本历史

| 版本 | 日期 | 变更 |
|------|------|------|
| v1.5.0 | 2026-05-21 | B站下载器完整目录；图片提取v1.0.4；TOOLS.md模板；参考源改为指定网站 |
| v1.0.1 | 2026-05-20 | 发帖工作流重命名 |
| v1.0.0 | 2026-05-20 | 初始发布 |

详见 [CHANGELOG.md](CHANGELOG.md)
