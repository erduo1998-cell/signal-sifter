# 方案发现：信息源与搜索策略

## 信息源

### GitHub 搜索（通过 Fossick MCP）

核心策略：搜小众项目（Star 不高、发布时间不短但还在活跃）。**Gem Hunter 的核心目标是低星高质项目，Star 上限硬约束：500。超过 500 星的只在作为对比或特别说明时提及。**

**使用 Fossick MCP 的 `search_repos` 工具**（替代 WebSearch）：

| 搜索场景 | Fossick 参数 | 用途 |
|----------|-------------|------|
| 被埋没的项目 | `search_repos(stars_min=10, stars_max=50, created_after="30days", topics=["领域"])` | 30 天前发布，还在更新，但没火 |
| 冷启动项目 | `search_repos(stars_min=1, stars_max=20, created_after="60days")` | 发布时间够长但没人发现 |
| 特定领域小众 | `search_repos(stars_min=5, stars_max=100, topics=["领域"], pushed_after="YYYY-MM-DD")` | 在特定 topic 下扫描 |
| 近期活跃的低星项目 | `search_repos(stars_min=5, stars_max=50, pushed_after="YYYY-MM-DD")` | 最近还在更新但 Star 不多 |

**Fossick 深度评估工具链**（搜到候选后用这些深入了解）：

| 工具 | 用途 | 调用时机 |
|------|------|----------|
| `search_repos` | 多维度搜索 GitHub 仓库 | 第一步：发现候选 |
| `repo_tree` | 浏览仓库目录结构（无需 clone） | 看项目规模和组织 |
| `get_file` | 读取 README 或任意文件 | 判断项目是否说清楚了要解决的问题 |
| `list_tags` | 查看版本发布历史 | 判断维护活跃度 |
| `search_code` | 全 GitHub 公开文件搜索 | 找类似项目做对比 |

**如何使用这些查询：**
1. 用户说了领域 → 用 `search_repos` 加 `topics` 过滤
2. 用户没说领域 → 不限定 topics，靠后续人工判断
3. 搜索结果太少 → 放宽 `stars_max` 到 100，或放宽时间范围
4. 搜索结果太多 → 收窄到 `stars_max=30`，或限定具体 topics
5. 发现候选后 → 逐个用 `get_file` 读 README，用 `list_tags` 看活跃度

### Gitee

中文项目天然壁垒，大部分没有被 GitHub Trending 覆盖。

| 搜索方式 | 备注 |
|----------|------|
| WebSearch "site:gitee.com [领域] 推荐" | 中文社区的项目推荐帖 |
| WebSearch "gitee 宝藏项目 [领域]" | 博客/论坛整理的 Gitee 项目列表 |

### Product Hunt

面向"想用工具解决问题的人"，不是开发者。能在这里上榜说明有人觉得有用。

| 搜索方式 | 备注 |
|----------|------|
| WebSearch "site:producthunt.com [领域]" | PH 的标题和描述会说明解决了什么问题 |
| WebSearch "site:producthunt.com open source [领域]" | PH 上也有开源项目，打标签的 |

### Reddit

| 子版块 | 搜索方式 | 备注 |
|--------|----------|------|
| r/coolgithubprojects | WebSearch "site:reddit.com/r/coolgithubprojects [关键词]" | 用户自发推荐的项目 |
| r/selfhosted | WebSearch "site:reddit.com/r/selfhosted [领域] tool" | 自托管社区常推荐小众工具 |

## 项目评估标准

### ✅ 好项目的特征

- **解决真痛点**：项目 README 说清楚了"为什么要做这个"（不是又一个 Todo App）
- **聚焦单一问题**：README 第一句话就能让普通人理解它解决什么。如果一个项目需要先解释行业背景才能说明白它做什么——太大了
- **能跑起来**：有可用的安装方式，agent 可以执行
- **有活人维护**：最近 3 个月有 commit，issue 有人回
- **有验证**：有其他人在用、在讨论（Reddit/HN/V2EX 有人提过）
- **不是重复造轮子**：不是因为"不想学现有工具"而重写的

### ❌ 不够好的特征（降权或淘汰）

- **README 只有技术细节没有动机**：说明作者没想清楚要解决什么
- **最后一次 commit 超过 6 个月**：可能已弃坑
- **Star 太高反而可疑**：Star > 10K 的不是"小众宝藏"，是"已经很火了"——偶尔可用但不是 gem-hunter 的核心目标
- **大厂开源项目**（微软/Google/Meta/阿里等）：天然有传播渠道，不是"被埋没的宝藏"
- **又一个框架/CLI 工具/构建工具**：开发者内卷产物，普通人/agent 用不上
- **基础设施/平台级项目**：如"重新定义 XX 工作流"、"统一 XX 基础设施"——太大，不是一个人用一个工具能解决的
- **0 issue 0 discussion**：完全没人用

### 特殊加分项

- **解决了中文特有场景**：如中文 NLP、国内平台 API 封装等
- **README 有 GIF/截图**：说明作者认真做了
- **有 Docker 一键部署**：agent 上手成本低

## 输出要求

每个项目至少附带：
- 仓库链接
- 当前 Star 数
- "为什么值得关注"的判断依据（不能只说"Star 少"）
- 与该领域的现有方案对比（如果有）
