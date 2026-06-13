# 痛点发现：信息源与搜索策略

## 信息源

### Reddit

| 子版块 | 搜索方式 | 备注 |
|--------|----------|------|
| r/programming | WebSearch "site:reddit.com/r/programming [关键词]" | 热门讨论区，吐槽集中地 |
| r/selfhosted | WebSearch "site:reddit.com/r/selfhosted frustration OR annoying OR sucks" | 自托管场景，容易发现工具缺口 |
| r/coolgithubprojects | WebSearch "site:reddit.com/r/coolgithubprojects comment" | 看评论区，项目推荐帖下面经常暴露痛点 |
| r/InternetIsBeautiful | WebSearch "site:reddit.com/r/InternetIsBeautiful" | 面向普通用户的 Web 工具，偶尔有痛点讨论 |

### Hacker News

| 板块 | 搜索方式 | 备注 |
|------|----------|------|
| Show HN | WebSearch "site:news.ycombinator.com Show HN [领域]" | 看评论区的批评——批评就是痛点信号 |
| Ask HN | WebSearch "site:news.ycombinator.com Ask HN tool OR alternative OR looking for" | 直接的需求表达 |

### GitHub Issues / Discussions

| 搜索方式 | 备注 |
|----------|------|
| WebSearch "site:github.com [工具名]/issues feature request" | 功能请求 = 现有方案不够好 |
| WebSearch "site:github.com [工具名]/issues frustration OR pain OR slow" | 直接搜索抱怨关键词 |
| WebSearch "site:github.com [工具名]/discussions alternative" | 用户在讨论替代方案 = 对现有方案不满 |

### 中文社区

| 平台 | 搜索方式 | 备注 |
|------|----------|------|
| V2EX | WebSearch "site:v2ex.com [关键词] 求推荐 OR 有没有 OR 替代" | 中文社区最直接的需求信号 |
| 知乎 | WebSearch "site:zhihu.com [领域] 痛点 OR 体验 OR 吐槽" | 长文分析多，适合深度痛点 |

## 搜索策略

### 通用方法

1. **定向搜索**：用户指定领域时，在关键词中加入领域限定
2. **宽泛扫描**：用户说"随便看看"时，用通用痛点关键词扫多个源
3. **交叉验证**：同一个痛点出现在 2+ 个源 = 信号增强

### 通用痛点关键词

搜索时组合以下关键词来提高信噪比：

- 抱怨类：`frustration`, `pain point`, `sucks`, `hate`, `terrible`, `nightmare`
- 寻找替代：`alternative to`, `looking for`, `any tool that`, `is there a`, `推荐`, `求推荐`, `有没有`
- 缺口表达：`wish there was`, `why is there no`, `someone should build`, `missing`, `lack of`

### 时效性控制

- 默认搜索最近 6 个月（痛点有时效性，太旧的可能已解决）
- 如果结果太少，放宽到 12 个月
- 搜索结果太少时，去掉时间限制

## 判断标准：什么是真痛点

### 最重要的闸门：粒度（必须先过这一关）

**一个好痛点的粒度：一个人 + 一个具体场景 + 一个工具能解决。**

- ✅ "每次在多台电脑间同步浏览器书签都要手动导出导入" → 具体、单人、有工具能解决
- ✅ "想给视频加字幕但不想上传到任何云服务" → 具体、单人、有工具能解决
- ❌ "AI Agent 上下文管理是行业瓶颈" → 这是行业趋势，不是选题
- ❌ "企业数据主权焦虑" → 这是市场报告标题，不是选题
- ❌ "开源社区平台老化" → 这是社会学议题，不是选题

**判定方法**：如果一个痛点需要引用 a16z 分析报告或行业论文来解释，它太大了。如果一个痛点你在 Reddit 评论区看到有人吐槽、立刻觉得"对啊我也遇到过"——这才是你要的粒度。

### 信息源优先级

**优先搜索个人求助和吐槽**（信号干净）：
- Reddit 评论区、V2EX "求推荐"帖、GitHub Issues 里的 feature request
- 这些地方的人在描述自己遇到的具体问题

**避免行业分析媒体**（信号太宏观）：
- VentureBeat、a16z、Business Insider、行业白皮书 → 这些产出的是"行业趋势"，不是"个人痛点"
- 学术论文（NeurIPS 等）→ 同上
- 如果搜索结果主要是这类内容，换关键词重新搜

### ✅ 真痛点的特征

- **反复出现**：不同人、不同时间、不同平台提到类似问题
- **有情绪**：抱怨带有具体细节（"每次都要手动 XXX，浪费我半小时"），不是泛泛的"不太好用"
- **可解决的**：不是一个行业的结构性问题，而是有具体工具/方案能解决的
- **有尝试**：有人已经尝试过现有方案但不满，说明需求真实存在
- **能一句话说清楚**：不需要背景知识、不需要行业术语，一个普通人看完就懂

### ❌ 假痛点的特征（过滤掉）

- **行业级趋势**："AI 行业缺少 XXX 基础设施"、"企业数字化转型的 XXX 困境" → 这是分析报告，不是选题
- **一次性配置问题**："装不上"、"环境配不好" → 这是文档问题，不是选题
- **纯个人偏好**："我不喜欢 Electron" → 不是痛点
- **已充分解决的**：搜索发现有 5+ 个成熟方案 → 这匹马已经跑了
- **太窄的**：只有 1 个人在 1 个帖子里提过，且没有共鸣 → 信号太弱
- **需要 200 字背景介绍才能理解的**：如果痛点需要解释"XX 行业的结构性问题"才能让人明白，淘汰

### 输出要求

每个痛点至少附带：
- 1 个以上的来源链接
- "为什么是痛点"的判断依据
- 如果来自多个源，标注"交叉验证"
