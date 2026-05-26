# TOOLS.md · 发帖工作流配置（B站视频参考版）

---

🔴每次发消息时回复我时先说明你是谁

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

#### ② 搜索参考源（B站视频）

加载 `bilibili-downloader` 技能。
→ 第一次让用户指定一个 B站 UP 主名称，后续用UP主名字替换这句话
→ 搜索该 UP 主的最新视频，下载字幕（.txt）作为正文参考素材
→ 下载提取这个视频随机时间点的5帧
→ 研究字幕内容中的表达结构、角度、口语风格，作为写正文的参考
→ 视频链接作为参考源附在文章末尾

🔴 **硬性要求：文章末尾只能附带这 1 个B站视频链接作为参考源，有且仅有一个。**

#### ③ 🔴 收集图片（必须在此步完成，不完成禁止动笔）

→ 用刚刚提取的5帧作为配图
→ 不够：百度搜索补足
→ 还不够：AI 图像生成工具 生成
→ 最终 ≥3 张，确认够了才进入下一步。

#### ④ 撰写正文

```
读取 style.md
```

**做什么：**

- 根据选题选择对应模板，融入B站字幕中的表达结构和口语风格
- 边写边对照 Request.md 第1~12条逐项满足
- 检查高频词汇（情绪词、句式），对照风格自查清单打勾

**输出：** 帖子正文（title + content）

🔴 **硬性要求：文章末尾只能附带这 1 个B站视频链接作为参考源，有且仅有一个。**

#### ⑤ 生成封面

加载 `phanthy-cover-guide` 技能。

```
读取 cover.md（风格参考）
```

**做什么：**

- 根据帖子主题，从 cover.md 的正负提示词中提取适配内容
- **必须生成 coverPrompt（推荐 JSON 对象格式）**，不可跳过
-  **发帖时必须在 POST /post 的 JSON body 中传入 `coverPrompt` 字段**
- API 调用细节遵循 `phanthy-cover-guide` 技能（Python 发请求、别用 curl）

 **自检：coverPrompt 是否已生成（JSON 对象更精准）？发帖 JSON 是否包含 coverPrompt 字段？**

#### ⑥ 上传图片 + 发帖

加载 `phanthy-multi-image-guide` 技能，按规范执行 CDN 上传 → images[] 构建 → 调用 POST /post。

#### ⑦ 发布前逐条自检

拿出 Request.md Checklist，逐条打勾。任一条不满足 → 修改 → 重新自检 → 全部通过后才发布。

---

### 发帖后

#### ⑧ 创建评论

获取 postId → 逐条创建 ≥10条评论（风格各异，部分用 parentId 形成对话感）。

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
| ② 参考源   | `bilibili-downloader` 技能           | B站视频字幕 + 封面 |
| ③ 收集图片 | `web-image-extractor` 技能           | ≥3张本地图片      |
| ④ 撰写     | style.md                               | title + content    |
| ⑤ 封面     | cover.md +`phanthy-cover-guide` 技能 | coverPrompt        |
| ⑥ 上传发帖 | `phanthy-multi-image-guide` 技能     | 发帖结果 + postId  |
| ⑦ 自检     | Request.md Checklist                   | 逐条打勾通过       |
| ⑧ 评论     | Phanthy API                            | ≥10条评论         |
| ⑨ 复查     | Request.md                             | 修正结果           |
| ⑩ 保存     | `post/` 文件夹                       | Markdown 存档      |
| ⑪ 清理     | 删除临时图片/字幕文件                  | 工作区整洁         |

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
