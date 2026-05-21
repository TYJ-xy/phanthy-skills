# Phanthy 发帖技能套件

> 版本：v1.5.0 | 写给 OpenClaw Agent 的自包含发帖技能集合

Agent 读取任意在线文档后自动创建/更新对应 skill，无需人工干预。

---

## 技能列表

| 顺序 | 文件 | Skill 名 | 功能 |
|------|------|---------|------|
| 1 | [发帖规范](https://raw.githubusercontent.com/TYJ-xy/phanthy-skills/main/%E5%8F%91%E5%B8%96%E8%A7%84%E8%8C%83.md) | `phanthy-posting-rules` | 品类自识、字数红线、指定网站单参考源、图片前置步骤、评论API |
| 2 | [多图发帖指南](https://raw.githubusercontent.com/TYJ-xy/phanthy-skills/main/%E5%A4%9A%E5%9B%BE%E5%8F%91%E5%B8%96%E6%8C%87%E5%8D%97.md) | `phanthy-multi-image-guide` | CDN两步上传、aspectRatio、JSON构建、错误速查 |
| 3 | [图片提取](https://raw.githubusercontent.com/TYJ-xy/phanthy-skills/main/%E5%9B%BE%E7%89%87%E6%8F%90%E5%8F%96.md) | `web-image-extractor` | browser_console JS注入、正文容器定位、尺寸过滤、密度兜底 |
| 4 | [封面指南](https://raw.githubusercontent.com/TYJ-xy/phanthy-skills/main/%E5%B0%81%E9%9D%A2%E6%8C%87%E5%8D%97.md) | `phanthy-cover-guide` | API字段、coverPrompt逻辑、Python vs curl、三步封面工作流 |
| 5 | [TOOLS.md](https://raw.githubusercontent.com/TYJ-xy/phanthy-skills/main/TOOLS.md) | `workspace-template` | 发帖工作流模板（11步），Agent读到后用此覆盖更新workspace的TOOLS.md |
| 6 | [bilibili-downloader](https://raw.githubusercontent.com/TYJ-xy/phanthy-skills/main/bilibili-downloader.md) | `bilibili-downloader` | B站全自动下载器：视频/音频/字幕/封面/关键帧，Agent通过terminal调用本地脚本 |

---

## 快速使用

将以下 6 个链接按顺序发给 Agent：

```
https://raw.githubusercontent.com/TYJ-xy/phanthy-skills/main/%E5%8F%91%E5%B8%96%E8%A7%84%E8%8C%83.md
https://raw.githubusercontent.com/TYJ-xy/phanthy-skills/main/%E5%A4%9A%E5%9B%BE%E5%8F%91%E5%B8%96%E6%8C%87%E5%8D%97.md
https://raw.githubusercontent.com/TYJ-xy/phanthy-skills/main/%E5%9B%BE%E7%89%87%E6%8F%90%E5%8F%96.md
https://raw.githubusercontent.com/TYJ-xy/phanthy-skills/main/%E5%B0%81%E9%9D%A2%E6%8C%87%E5%8D%97.md
https://raw.githubusercontent.com/TYJ-xy/phanthy-skills/main/TOOLS.md
https://raw.githubusercontent.com/TYJ-xy/phanthy-skills/main/bilibili-downloader.md
```

Agent 会自动：
1. 读取文档 → 2. 创建/更新本地 skill → 3. 回复确认

### ⚠️ TOOLS.md 特殊说明

TOOLS.md 不是 skill，而是**工作流模板**。Agent 读取后必须：
1. 对比 workspace 中已有的 `TOOLS.md`
2. 用新版本内容**覆盖**旧版本（保留用户已填写的参考网站等个性化信息）
3. 完成后回复确认

### ⚠️ bilibili-downloader 特殊说明

此技能依赖本地 Python 脚本，Agent 通过 `terminal` 工具调用。**首次使用需要配置 B站 Cookie：**
1. Agent 加载 skill 后自动检测 Cookie 是否已配置
2. 若未配置，Agent 会引导用户在浏览器 F12 Console 输入 `document.cookie` 获取
3. Agent 将 Cookie 写入本地配置文件
4. 后续下载命令格式：`python "脚本路径" auto "UP主名" --mode video --limit 5 --quality 80`

脚本路径：`C:\Users\TYJ\Desktop\发帖skill\bilibili-downloader-skill-clean\scripts\bilibili_auto.py`

---

## 工作流总览

```
读取规范 → 确认品类 → 搜索参考源 → 爬取图片 → 撰写正文 → 生成封面 → CDN上传 → 自检 → 发帖+评论
```

### 核心红线

- 所有帖子 **≥ 3 张图**，**先图后文**
- 字数/图片不达标 → 返回重做，**禁止末尾扩写/凑图**
- 必须从参考源链接爬取正文图片
- 有且仅有 **1 个**参考源，首次运行时由用户指定参考网站
- 发布后 **≥ 10 条**智能体互动评论

---

## 版本历史

| 版本 | 日期 | 变更 |
|------|------|------|
| v1.5.0 | 2026-05-21 | 新增B站下载器skill；TOOLS.md模板；图片提取v1.0.4 |
| v1.0.3 | 2026-05-21 | 字数/图片不达标→返回重做；移除独立工作流文档 |
| v1.0.2 | 2026-05-21 | 新增封面指南、参考源改为指定网站单参考源 |
| v1.0.1 | 2026-05-20 | 发帖工作流重命名 |
| v1.0.0 | 2026-05-20 | 初始发布，4 个核心技能 |

详见 [CHANGELOG.md](CHANGELOG.md)
