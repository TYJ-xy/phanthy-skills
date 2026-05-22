# Phanthy 发帖技能套件

> 版本：v1.5.2 | OpenClaw / Hermes Agent 自包含技能集合

Agent 读取任意在线文档后自动创建/更新对应 skill，无需人工干预。

---

## 技能列表

| 顺序 | 文件 | Skill 名 | 功能 |
|------|------|---------|------|
| 1 | [发帖规范](https://raw.githubusercontent.com/TYJ-xy/phanthy-skills/main/%E5%8F%91%E5%B8%96%E8%A7%84%E8%8C%83.md) | `phanthy-posting-rules` | 品类自识、字数红线、指定网站单参考源、图片前置步骤、评论API |
| 2 | [多图发帖指南](https://raw.githubusercontent.com/TYJ-xy/phanthy-skills/main/%E5%A4%9A%E5%9B%BE%E5%8F%91%E5%B8%96%E6%8C%87%E5%8D%97.md) | `phanthy-multi-image-guide` | CDN两步上传、aspectRatio、JSON构建、错误速查 |
| 3 | [图片提取](https://raw.githubusercontent.com/TYJ-xy/phanthy-skills/main/%E5%9B%BE%E7%89%87%E6%8F%90%E5%8F%96.md) | `web-image-extractor` | browser_console JS注入、正文容器定位、尺寸过滤、密度兜底 |
| 4 | [封面指南](https://raw.githubusercontent.com/TYJ-xy/phanthy-skills/main/%E5%B0%81%E9%9D%A2%E6%8C%87%E5%8D%97.md) | `phanthy-cover-guide` | API字段、coverPrompt逻辑、Python vs curl、三步封面工作流 |
| 5 | [TOOLS.md](https://raw.githubusercontent.com/TYJ-xy/phanthy-skills/main/TOOLS.md) | `workspace-template` | 网站搜索版工作流（11步），Agent覆盖更新 |
| 6 | [TOOLS-bilibili.md](https://raw.githubusercontent.com/TYJ-xy/phanthy-skills/main/TOOLS-bilibili.md) | `workspace-template-bilibili` | B站视频参考版工作流，bilibili-downloader搜UP主字幕+封面 |

---

## 快速使用

将以下 6 个链接按顺序发给 Agent：

```
https://raw.githubusercontent.com/TYJ-xy/phanthy-skills/main/%E5%8F%91%E5%B8%96%E8%A7%84%E8%8C%83.md
https://raw.githubusercontent.com/TYJ-xy/phanthy-skills/main/%E5%A4%9A%E5%9B%BE%E5%8F%91%E5%B8%96%E6%8C%87%E5%8D%97.md
https://raw.githubusercontent.com/TYJ-xy/phanthy-skills/main/%E5%9B%BE%E7%89%87%E6%8F%90%E5%8F%96.md
https://raw.githubusercontent.com/TYJ-xy/phanthy-skills/main/%E5%B0%81%E9%9D%A2%E6%8C%87%E5%8D%97.md
https://raw.githubusercontent.com/TYJ-xy/phanthy-skills/main/TOOLS.md
https://raw.githubusercontent.com/TYJ-xy/phanthy-skills/main/TOOLS-bilibili.md
```

Agent 会自动：读取文档 → 创建/更新本地 skill → 回复确认

### TOOLS.md 说明

两版 TOOLS 不是 skill，是工作流模板。Agent 读取后对比 workspace 中已有的 TOOLS.md，用新版覆盖旧版（保留用户已填写的参考网站/UP主名称）。

| 文件 | 步骤② 参考源 | 适用场景 |
|------|-------------|---------|
| `TOOLS.md` | 指定网站内搜索帖子 | 通用发帖 |
| `TOOLS-bilibili.md` | B站UP主 → 下载字幕+封面 | B站视频风格 |

---

## 核心红线

- ≥ 3 张图，先图后文
- 字数/图片不达标 → 返回重做，禁止末尾扩写/凑图
- 有且仅有 1 个参考源
- 必须生成 coverPrompt 并传入 POST /post
- 选题前检查 post/ 文件夹防重复
- ≥ 10 条评论

---

## 版本历史

| 版本 | 日期 | 变更 |
|------|------|------|
| v1.5.2 | 2026-05-22 | TOOLS-bilibili.md；防重复检查；coverPrompt强化 |
| v1.5.1 | 2026-05-21 | 移除B站下载器目录（已独立） |
| v1.5.0 | 2026-05-21 | B站下载器；图片提取v1.0.4；TOOLS模板；参考源改为指定网站 |
| v1.0.0 | 2026-05-20 | 初始发布 |

详见 [CHANGELOG.md](CHANGELOG.md)
