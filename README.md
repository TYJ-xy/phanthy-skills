# Phanthy 发帖技能套件

> 版本：v1.5.1 | OpenClaw / Hermes Agent 自包含技能集合

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
| 6 | [TOOLS-bilibili.md](https://raw.githubusercontent.com/TYJ-xy/phanthy-skills/main/TOOLS-bilibili.md) | `workspace-template-bilibili` | B站视频参考版工作流，用bilibili-downloader搜UP主字幕+封面作参考 |(https://raw.githubusercontent.com/TYJ-xy/phanthy-skills/main/TOOLS.md) | `workspace-template` | 发帖工作流模板（11步），Agent读到后覆盖更新workspace的TOOLS.md |

---

## 快速使用

将以下 5 个链接按顺序发给 Agent：

```
https://raw.githubusercontent.com/TYJ-xy/phanthy-skills/main/%E5%8F%91%E5%B8%96%E8%A7%84%E8%8C%83.md
https://raw.githubusercontent.com/TYJ-xy/phanthy-skills/main/%E5%A4%9A%E5%9B%BE%E5%8F%91%E5%B8%96%E6%8C%87%E5%8D%97.md
https://raw.githubusercontent.com/TYJ-xy/phanthy-skills/main/%E5%9B%BE%E7%89%87%E6%8F%90%E5%8F%96.md
https://raw.githubusercontent.com/TYJ-xy/phanthy-skills/main/%E5%B0%81%E9%9D%A2%E6%8C%87%E5%8D%97.md
https://raw.githubusercontent.com/TYJ-xy/phanthy-skills/main/TOOLS.md
```

Agent 会自动：
1. 读取文档 → 2. 创建/更新本地 skill → 3. 回复确认

### TOOLS.md 特殊说明

不是 skill，是工作流模板。Agent 读取后对比 workspace 中已有的 TOOLS.md，用新版覆盖旧版（保留用户已填写的参考网站）。

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
| v1.5.1 | 2026-05-21 | 移除B站下载器（已独立为单独项目） |
| v1.5.0 | 2026-05-21 | B站下载器完整目录；图片提取v1.0.4；TOOLS.md模板 |
| v1.0.1 | 2026-05-20 | 发帖工作流重命名 |
| v1.0.0 | 2026-05-20 | 初始发布 |

详见 [CHANGELOG.md](CHANGELOG.md)
