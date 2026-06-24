# signal-sifter

> 小众开源选题发现器 / 需求真伪验证器（Claude Code skill）。
> 说明：本仓库对外名 signal-sifter，在 Claude Code 内的 skill name 为 `gem-hunter`，二者是同一个东西。

## 核心能力

- **两条独立流水线**：痛点发现（X/Twitter + Reddit + HN + GitHub + 微信公众号 + 小红书 + V2EX）和方案发现（Fossick MCP + 社区验证）
- **交叉匹配**：把痛点和对策连起来，标注匹配度，输出"痛点→方案"配对报告
- **非开发者过滤器**：四道闸（粒度、语言、门槛、场景）过滤掉开发者工具噪音

## 目录结构

```
SKILL.md              # Skill 主文件（Claude Code 加载入口）
references/
  pain-point-sources.md   # 痛点发现：信息源与搜索策略（API 调用格式 + 判断标准）
  project-sources.md      # 方案发现：信息源与搜索策略（Fossick MCP + 社区验证）
  matching-logic.md       # 交叉匹配逻辑
templates/
  hunt-report.md          # 选题报告输出模板
docs/
  specs/                  # 设计文档
```

## 数据采集原则

**不走搜索引擎中转论坛内容。直接调平台 API，拿到带互动数据的一手讨论。**

具体每个源需要什么、缺了会怎样，见下方「[数据源依赖](#数据源依赖)」。

## 数据源依赖

这是外部用户最需要先看懂的一节：**哪些源开箱即用，哪些要另装东西**。

| 信息源 | 依赖什么 | 开箱即用？ | 缺了会怎样 |
|--------|---------|-----------|-----------|
| Hacker News | Algolia API（curl） | ✅ 是 | — |
| GitHub Issues | Fossick MCP 或 gh CLI | ✅ 是 | — |
| 微信公众号 | WebSearch `site:mp.weixin.qq.com` | ✅ 是 | — |
| V2EX | WebSearch `site:v2ex.com` | ✅ 是 | — |
| X / Twitter | last30days 项目的 Bird 引擎 + AUTH_TOKEN/CT0 + 代理 | ❌ 否 | 自动降级为 WebSearch `site:x.com`，信号偏弱 |
| Reddit | last30days 引擎（scripts/last30days.py） | ❌ 否 | 自动降级为 WebSearch `site:reddit.com` |
| 小红书 | xiaohongshu-mcp 本地服务（REST API） | ❌ 否 | 跳过（WebSearch 对小红书无效，不降级） |

> ⚠️ **X / Reddit / 小红书** 三个源依赖另外两个独立项目——`last30days` 调研引擎和 `xiaohongshu-mcp`——**本仓库不分发它们**。未安装时，这三个源会自动降级或跳过，其余四个源（HN / GitHub / 微信公众号 / V2EX）可正常工作。如果你需要完整七源，需自行安装上述两个项目；只用公开源也足以跑通核心流程。

## 安装

```bash
mkdir -p ~/.claude/skills
git clone https://github.com/erduo1998-cell/signal-sifter.git ~/.claude/skills/gem-hunter
```

> 目录名保持 `gem-hunter` 以匹配 Claude Code 内的 skill name。

### 依赖

各信息源的具体依赖见上方「[数据源依赖](#数据源依赖)」表格——**装了 Node + curl 不等于能跑全部源**，X / Reddit / 小红书 还需要各自的项目。

基础运行环境：

- **Node.js** — X/Twitter Bird 引擎（仅在你启用 X 源时需要）
- **curl + python3** — Hacker News / Reddit / 小红书 API 解析
- **可选：代理**（如 ClashX `127.0.0.1:7890`）— 访问 Reddit / X 等被墙源时需要
- **可选：AUTH_TOKEN / CT0** — X/Twitter 认证，从 `last30days` 项目的 `.env` 读取（仅启用 X 源时需要）

## 使用

在 Claude Code 中直接说：
- "帮我挖选题" → 两条流水线都跑
- "最近有什么痛点" → 只跑痛点发现
- "找个能解决 session 管理的小众项目" → 只跑方案发现
- "搜一下 AI agent 操作电脑有没有新方案" → 关键词限定

输出报告保存到：`gem-hunt-YYYY-MM-DD.md`
