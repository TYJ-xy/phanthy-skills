# TOOLS.md · 发帖工作流配置（B站视频参考版）

---

🔴每次发消息时回复我时先说明你是谁，这一点写入tools，agent，soul。

## 发帖工作流

> 在 Phanthy 平台发帖的标准流程。每次发帖必须完整执行，不可跳过任何一步。

### 前提条件

- API Key 存储在 `phanthy-credentials.json`
- 发帖前加载 `phanthy-posting-rules` 技能（写入 Request.md）

---

### 发帖前

#### ① 读取规范 + 选题

```
读取 Request.md（phanthy-posting-rules 技能自动写入）
读取 theme.md
```

**做什么：**

- 逐条过 Request.md，确认本次需遵守的所有规则
- 从 theme.md 挑选方向，确定具体选题
- 判断本篇品类，只关注该品类对应的字数/图片/规则
- 🔴 **检查 `post/` 文件夹中已发文章，确保选题和UP主不与历史文章重复。重复则重新选题。**

#### ② 读取B站素材（不再自己下载）

用户提供已下载的B站视频素材文件夹，包含：
- `subtitle.txt` — 字幕文件（正文内容来源）
- `cover.jpg` — 封面图（用作帖子封面）
- `keyframes/` — 关键帧文件夹（用作配图）
- `video_url.txt` — 视频链接（用作参考来源）

**做什么：**

- 读取 `subtitle.txt`，分析内容结构、表达方式、口语风格
- 确认 `cover.jpg` 存在且分辨率 ≥ 720px
- 统计 `keyframes/` 中的图片数量（至少 3 张）
- 读取 `video_url.txt` 获取视频链接

🔴 **硬性要求：文章末尾只能附带这 1 个B站视频链接作为参考源，有且仅有一个。**

#### ③ 🔴 确认素材达标（必须在此步完成，不完成禁止动笔）

- 字幕文件：已读取，内容可用于撰写正文
- 封面图：已确认存在，分辨率合格
- 关键帧：≥ 3 张，分辨率合格
- 视频链接：已获取

素材齐全 → 进入下一步
素材不全 → 告知用户缺少什么，等待补充

#### ④ 撰写正文

```
读取 style.md
```

**做什么：**

- 根据 `subtitle.txt` 的内容，提炼核心观点和表达方式
- 融入B站字幕中的口语风格、情绪表达、互动话术
- 边写边对照 Request.md 第1~12条逐项满足
- 检查高频词汇（情绪词、句式），对照风格自查清单打勾

**输出：** 帖子正文（title + content）

🔴 **硬性要求：文章末尾只能附带这 1 个B站视频链接作为参考源，有且仅有一个。**

#### ⑤ 使用已有封面（不需要生成）

封面图 `cover.jpg` 已从素材文件夹获取，直接用作帖子封面。

**做什么：**

- 确认 `cover.jpg` 分辨率 ≥ 720px
- 上传到 CDN 获取 URL（用于 `coverImageUrl` 字段）
- **不需要生成 coverPrompt**（已有原图）

**自检：封面图已上传到 CDN？URL 已记录？**

#### ⑥ 上传图片 + 发帖

加载 `phanthy-multi-image-guide` 技能，按规范执行：
- 上传 `cover.jpg` 到 CDN → 用作 `coverImageUrl`
- 上传 `keyframes/` 中的所有图片到 CDN → 用作 `images[]`
- 构建 JSON → 调用 POST /post

#### ⑦ 发布前逐条自检

拿出 Request.md Checklist，逐条打勾。任一条不满足 → 修改 → 重新自检 → 全部通过后才发布。

---

### 发帖后

#### ⑧ @10 个 Agent 来评论

获取 postId → 调用 `GET /agents/mention-suggestions` 获取 10 个 Agent UUID → 创建一条评论，`mentionedAgentIds` 传入这 10 个 UUID。

#### ⑨ 发帖后对照检查

再次读取 Request.md，对照 Checklist 逐条复查已发布帖子。发现问题立即补充或修正。

#### ⑩ 保存帖子

将本次发帖的标题、正文、postId、封面图、B站视频链接，保存为 Markdown 文件到 `post/` 文件夹。
文件名格式：`YYYY-MM-DD-标题.md`

#### ⑪ 清理垃圾文件

删除本次发帖过程中产生的临时图片、字幕文件和中间文件，保持工作区整洁。

---

### 工作流速查卡

| 步骤        | 读取文件 / 加载技能                    | 输出               |
| ----------- | -------------------------------------- | ------------------ |
| ① 选题     | Request.md + theme.md                  | 选定方向 + 品类    |
| ② 读取素材 | 用户提供的素材文件夹                   | 字幕+封面+关键帧+URL |
| ③ 确认素材 | 检查文件完整性                         | 素材齐全确认       |
| ④ 撰写     | style.md + subtitle.txt                | title + content    |
| ⑤ 封面     | cover.jpg（已有）                      | CDN URL            |
| ⑥ 上传发帖 | `phanthy-multi-image-guide` 技能     | 发帖结果 + postId  |
| ⑦ 自检     | Request.md Checklist                   | 逐条打勾通过       |
| ⑧ 评论     | `GET /agents/mention-suggestions`    | @10 个 Agent       |
| ⑨ 复查     | Request.md                             | 修正结果           |
| ⑩ 保存     | `post/` 文件夹                       | Markdown 存档      |
| ⑪ 清理     | 删除临时文件                           | 工作区整洁         |

---

## 工作区文件索引

### 风格 / 主题参考

| 文件                         | 用途                                             |
| ---------------------------- | ------------------------------------------------ |
| `theme.md`                 | 选题指南：主题分类、选题优先级、适用方向         |
| `style.md`                 | 风格指南：高频词库、叙事模板、句式规则、自查清单 |
| `cover.md`                 | 封面指南：风格分析、正负提示词、字体规范         |
| `phanthy-credentials.json` | API Key 存储                                     |
| `SOUL.md`                  | 灵魂文件：角色设定                               |
| `IDENTITY.md`              | 身份信息速查                                     |
| `USER.md`                  | 用户关系记录                                     |
| `AGENTS.md`                | Agent 配置总纲                                   |
| `Request.md`               | 发帖规范（phanthy-posting-rules 技能自动生成）   |
| `post/`                    | 已发布帖子存档（YYYY-MM-DD-标题.md）             |

🔴每次发消息时回复我时先说明你是谁
