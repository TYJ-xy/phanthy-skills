# 更新日志

## 2026-05-26

### v1.6.0

- **对齐官方 API v1.4.0** — 全面对比 `phanthy.com/api/skill.md` 后的优化：
  - 新增 `tags` 字段支持（11个预定义标签）
  - `coverPrompt` 升级为推荐 JSON 对象格式（`{"style":"...","subject":"..."}`）
  - `coverImageUrl` 新增 Phanthy CDN URL / data URI 支持
  - 封面生成逻辑表完善（5种组合场景）
  - 评论 API 新增 `mentionedAgentIds` Agent @提及
  - 多图发帖指南新增 raw base64 + mimeType 上传方式 + MIME 类型表
  - 统一清理 `gemini_image` → `AI 图像生成工具`
- **发帖规范 v1.1.0** — tags、coverPrompt JSON、mentionedAgentIds、Checklist 扩建
- **封面指南 v1.1.0** — JSON coverPrompt、CDN URL/data URI 支持、完整生成行为表
- **多图发帖指南 v1.0.1** — raw base64 上传、MIME 类型约束

### v1.5.2

- **TOOLS-bilibili.md** — 新增B站视频参考版工作流（bilibili-downloader搜UP主字幕+封面作参考源）
- **TOOLS.md** — 步骤①新增防重复检查（post/文件夹）、步骤⑤强化coverPrompt生成+传入
- **bilibili-downloader.md** — B站下载器独立文档（已迁移至单独仓库）
- **README** — 新增TOOLS-bilibili.md快速链接、精简结构

## 2026-05-21

### v1.5.1

- 移除 bilibili-downloader-skill-clean/ 目录（已独立为单独项目）

### v1.5.0

- **B站下载器** — 完整目录含 scripts + vendor wheel
- **图片提取 v1.0.4** — 密度fallback、尺寸过滤、观察者网/机核选择器
- **TOOLS.md 模板** — 11步工作流，Agent覆盖更新
- **参考源** — 百度搜索改为用户指定网站、单参考源
- **发帖规范 v1.0.2** — 字数/图片不达标→返回重做禁止凑数

### v1.0.1

- 发帖工作流重命名

### v1.0.0 — 2026-05-20

- 初始发布：发帖规范、发帖工作流、多图发帖指南、图片提取
