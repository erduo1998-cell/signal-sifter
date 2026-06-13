# 📡 Signal Sifter

> **Signal from noise. Pain points from complaints. Gems from gravel.**
>
> A Claude Code skill that finds hidden open-source projects by sifting real pain points from internet chatter — then matching them to overlooked repos.

---

## What it does

Signal Sifter runs two independent discovery pipelines, then cross-matches the results:

| Pipeline | What it finds | Where it looks |
|----------|--------------|----------------|
| **Pain Point Discovery** | Real problems people complain about repeatedly | Reddit, Hacker News, GitHub Issues, V2EX, Zhihu |
| **Project Discovery** | Low-star, high-quality repos nobody has found yet | GitHub (via Fossick MCP), Gitee, Product Hunt |

The output is a single report: **pain ↔ project pairs**, with match scores, source links, and a "why this matters" for each finding.

It's built for content creators who need a steady stream of **specific, actionable** open-source topics — not industry analyst reports.

---

## Why "Signal Sifter"

Most open-source discovery tools sort by stars. That gives you the same 50 projects everyone else is covering. Signal Sifter looks in the other direction:

- **Complaints** instead of stars — real frustration leaves better trails than appreciation
- **Low-star repos** instead of trending — `stars:10..50` is the sweet spot
- **Individual pain points** instead of industry trends — "I can't sync bookmarks across browsers" beats "the SaaS industry faces data sovereignty challenges"

The name says it: this is a signal processing pipeline disguised as a prompt template.

---

## Quick Start

### Prerequisites

- [Claude Code](https://claude.ai/code)
- [Fossick MCP](https://pypi.org/project/fossick-mcp/) for GitHub search (recommended)

```bash
# Install Fossick MCP
claude mcp add fossick --scope user uvx fossick-mcp
```

### Install the skill

```bash
# Clone to your skills directory
git clone https://github.com/erduo1998-cell/signal-sifter.git ~/.claude/skills/signal-sifter
```

The skill auto-registers. Restart Claude Code and you're ready.

### Usage

Just say what you're looking for:

```
帮我挖选题                                    # both pipelines
最近有什么痛点                                # pain points only
找个能解决 session 管理的小众项目              # projects only
搜一下 AI agent 操作电脑有没有新方案            # both, with domain keyword
随便看看                                      # broad scan, no domain limit
```

Output is saved as `signal-sift-YYYY-MM-DD.md` in your working directory.

---

## Example Output

A `signal-sift` report has four sections:

```markdown
## 一、发现的痛点
"每次换电脑都要手动同步浏览器书签和扩展设置"
  → 来源: Reddit r/selfhosted (3 posts), V2EX (2 posts) — 交叉验证
  → 为什么是痛点: 浏览器同步服务绑定厂商账号，跨浏览器更是不可能

## 二、发现的项目
BrowserForge (GitHub, ⭐ 47)
  → 用 WebDriver 脚本化浏览器配置，一键还原完整工作环境
  → 活跃度: 最近 2 周有 commit

## 三、交叉匹配
| 浏览器配置同步困难 | BrowserForge | ⭐⭐⭐ | 直接命中 |

## 四、下一步
- [ ] 体验 BrowserForge：验证是否真解决了跨浏览器配置同步
```

---

## How it works (the prompt)

This is **just a prompt template**. The magic is in the filtering:

1. **Granularity gate** — if a pain point needs a16z analysis to explain, it's rejected. Good pain points fit in one sentence.
2. **Star ceiling** — hard cap at 500 stars. Above that, it's not "hidden."
3. **No big-tech repos** — Microsoft/Google/Meta projects have their own distribution channels.
4. **Evidence required** — every finding links to its source. No vibes-based curation.

Read [`SKILL.md`](SKILL.md) for the full prompt. The `references/` directory contains the search strategies and evaluation criteria the prompt loads at runtime.

---

## File Structure

```
signal-sifter/
├── SKILL.md                         # Main skill: persona + workflow
├── references/
│   ├── pain-point-sources.md        # Pain discovery: sources + search + judgment
│   ├── project-sources.md           # Project discovery: sources + Fossick MCP + evaluation
│   └── matching-logic.md            # Cross-matching logic
└── templates/
    └── hunt-report.md               # Output report template
```

---

## Philosophy

- **It's a curation problem, not a data problem.** The internet has all the information. The hard part is knowing what to ignore.
- **Stop when the report is done.** This skill doesn't auto-generate video scripts or blog posts. You experience the project first, then decide if it's worth covering.
- **Agent-first.** A project doesn't need a GUI, Chinese docs, or one-click install. If an agent can run it, it's in.

---

## Connect

<p align="center">
  <img src="wechat-qrcode.jpg" width="200" alt="WeChat QR Code"><br>
  <sub>扫码加微信 · 交个朋友</sub>
</p>

---

## License

MIT — take it, fork it, build on it. If you find a gem with it, tell more people about the project than about this tool.

---

<p align="center">
  <sub>Signal from noise. ⚡</sub>
</p>
