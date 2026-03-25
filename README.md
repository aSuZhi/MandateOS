# MandateOS for OpenClaw

## 中文简介

MandateOS 是一个面向 OpenClaw 的 Prompt-first AI 投资委员会 skill。它位于 OKX 执行 skills 之上，把一句短提示词转换成一条有纪律的工作流：

- 创建或更新 `Mandate Profile`
- 基于 mandate 做市场扫描
- 生成保守版 / 标准版 / 进攻版提案
- 只有 mandate 允许时才进入执行
- 处理闲置资金
- 市场 regime 改变时自动切策略
- 输出复盘与下一步建议

一句话定位：

`MandateOS 不是另一个交易聊天机器人，而是一层带 mandate、路由和 discipline gate 的 OKX 多 skill 控制层。`

## English Overview

MandateOS is a prompt-first AI investment committee skill for OpenClaw. It sits above the OKX execution skills and turns one short prompt into a disciplined workflow:

- create or update a `Mandate Profile`
- scan the market against that mandate
- generate conservative / standard / aggressive proposals
- execute only when the mandate allows it
- handle idle funds
- switch strategy when regime changes
- produce a review and next-step recommendation

One-line positioning:

`MandateOS is not another trading chatbot. It is a mandate-aware control layer that turns one short prompt into a disciplined OKX multi-skill workflow.`

## 为什么这个作品容易拿高分 / Why This Scores Well

中文：
- `Integration`：一条短 prompt 先解析 mandate，再进入路由，再决定下游 OKX skill chain
- `Utility`：解决交易者在市场、账户、执行、赚币、机器人之间来回切换的真实痛点
- `Innovation`：在执行层之上增加 mandate gate、strategy router、proposal-to-execution control
- `Reproducibility`：以 skill 形式交付，提示词、模板、输出结构、Demo 路径都固定

English:
- `Integration`: one short prompt resolves a mandate and routes into a lower-level OKX skill chain instead of calling a single endpoint
- `Utility`: it turns fragmented OKX capabilities into one disciplined daily workflow
- `Innovation`: it adds a mandate gate, strategy router, and proposal-to-execution control layer above the execution kit
- `Reproducibility`: it ships as a downloadable skill with fixed prompts, fixed templates, fixed output sections, and a demo runbook

Proof artifacts:
- `skills/mandateos/references/architecture-diagram.md`
- `skills/mandateos/references/integration-evidence.md`
- `skills/mandateos/references/repro-runbook.md`
- `skills/mandateos/references/demo-scenarios.md`

## 仓库包含什么 / What Is Included

这份仓库的主交付物只有一个：

- `skills/mandateos`

This repository is intended to ship **only one primary skill**:

- `skills/mandateos`

`MandateOS` 依赖的 5 个底层 OKX skills：
- `okx-cex-market`
- `okx-cex-portfolio`
- `okx-cex-trade`
- `okx-cex-earn`
- `okx-cex-bot`

**不需要跟这个作品一起上传。**

They are treated as **external prerequisites** and do **not** need to be uploaded with this repository.

## 深度依赖说明 / Deep Dependency Model

中文：

MandateOS 不是脱离 OKX skills 独立运行的“空壳 skill”。它的核心价值正是建立在这 5 个底层 OKX skills 之上：

- `okx-cex-market` 提供市场状态、指标和 regime 依据
- `okx-cex-portfolio` 提供余额、仓位、PnL、可用保证金与账户上下文
- `okx-cex-trade` 提供现货、永续、交割、期权执行能力
- `okx-cex-earn` 提供闲置资金利用路径
- `okx-cex-bot` 提供震荡市场下的 grid / DCA 自动化

MandateOS 的创新不在于重复实现这些底层能力，而在于：
- 先解析 `mandate`
- 再判断 `intent`
- 再做 `regime routing`
- 最后通过 `discipline gate` 决定是否允许进入执行

也就是说，**它是深度依赖这 5 个 OKX skills 的控制层，而不是简单调用某一个接口的包装层。**

English:

MandateOS is not a standalone shell that happens to mention OKX skills. Its core value is built directly on top of five lower-level OKX skills:

- `okx-cex-market` for market regime and indicator context
- `okx-cex-portfolio` for balances, positions, PnL, and account context
- `okx-cex-trade` for spot, swap, futures, and options execution
- `okx-cex-earn` for idle capital deployment
- `okx-cex-bot` for grid and DCA automation

The innovation of MandateOS is not re-implementing those capabilities. The innovation is the control layer that:
- resolves a `mandate`
- classifies `intent`
- performs `regime routing`
- applies a `discipline gate` before execution

In other words, **MandateOS is deeply dependent on these five OKX skills as a control layer, not a thin wrapper around a single endpoint.**

## 上传范围 / What Should Be Uploaded

推荐上传内容：
- `skills/mandateos`
- `README.md`
- `.gitignore`

Recommended upload scope:
- `skills/mandateos`
- `README.md`
- `.gitignore`

不推荐把底层 5 个 OKX skills 一起作为本作品提交，因为它们属于外部依赖，不是 MandateOS 自身的创新交付。

The 5 lower-level OKX skills should not be submitted as part of this project deliverable because they are dependencies, not the core innovation of MandateOS.

## OpenClaw 目录结构 / OpenClaw Layout

OpenClaw 通常从工作区 `skills/` 目录或全局 skills 目录发现 skill。

OpenClaw usually discovers skills from a workspace `skills/` directory or from a global skills directory.

本仓库主 skill 位于：

```text
skills/mandateos
```

So the easiest way to test it is to open this repository as an OpenClaw workspace.

## 前置依赖 / Prerequisites

在 OpenClaw 里单独预装这些底层 skills：
- `okx-cex-market`
- `okx-cex-portfolio`
- `okx-cex-trade`
- `okx-cex-earn`
- `okx-cex-bot`

Install these lower-level OKX skills separately in OpenClaw:
- `okx-cex-market`
- `okx-cex-portfolio`
- `okx-cex-trade`
- `okx-cex-earn`
- `okx-cex-bot`

如果你要跑真实市场读取或账户动作，还需要确保 OpenClaw 运行环境中已经配置好 OKX CLI 和凭证。

For live market reads or account actions, make sure the OpenClaw runtime already has the OKX CLI and credentials configured.

## 首次测试 / First Test

先创建一个 mandate：

```text
mandateos
```

或者直接：

```text
帮我创建一个 protected_long mandate：30天周期，最大回撤5%，允许 spot/option/earn，禁止高杠杆裸空
```

然后运行主流程：

```text
扫描现在的市场并结合我的 mandate 给我今日判断
基于我的 mandate 生成今日交易提案
执行提案中的保守版，使用 demo
给我生成今日复盘和明日建议
```

If you want the safest first run, stop after the proposal step. That is the intended shadow-mode path.

安装后也可以直接输入：

```text
mandateos
```

to show the capability menu and starter prompts.

## 核心提示词入口 / Core Prompt Entry Points

- `mandateos`
- `$mandateos`
- `mandateos help`
- `扫描现在的市场并结合我的 mandate 给我今日判断`
- `基于我的 mandate 生成今日交易提案`
- `执行提案中的保守版`
- `把闲置资金按我的 mandate 处理`
- `如果市场进入震荡，切换到更合适的策略`
- `给我生成今日复盘和明日建议`

## Skill 内容 / Skill Contents

Inside `skills/mandateos`:

- `SKILL.md` - trigger rules, routing, safety, and workflow
- `references/architecture-diagram.md` - Mermaid architecture and flow diagrams
- `references/capability-menu.md` - welcome menu and starter prompt screen
- `references/integration-evidence.md` - concrete prompt-to-skill proof chains
- `references/mandate-profiles.md` - schema and template selection
- `references/prompt-pack.md` - prompt entry points
- `references/repro-runbook.md` - install, demo, and fallback reproduction steps
- `references/routing-playbooks.md` - regime and module routing
- `references/output-contract.md` - fixed output format
- `references/demo-scenarios.md` - competition demo flow
- `assets/*.yaml` - reusable mandate templates

## 真实测试模式 / Realistic Testing Modes

### 1. Shadow Mode

适合：
- 先验证 routing 和 proposal 质量
- 尚未配置凭证
- 想快速、安全地演示

Use when:
- you only want to validate routing and proposal quality
- credentials are not configured yet
- you want the fastest safe demo

Expected behavior:
- MandateOS still resolves the mandate
- it still produces market judgment and proposals
- `纪律校验` clearly says `未执行，仅提案`

### 2. Demo Execution Mode

适合：
- 底层 OKX skills 已经装好
- 下游 demo profile 已经配置完成
- 你想展示受控的端到端执行

Use when:
- downstream OKX skills are installed
- the downstream demo profile is configured
- you want to show bounded end-to-end execution

Expected behavior:
- the selected proposal version is restated
- the downstream module path is named before any write
- the review step ties the result back to the original mandate

## GitHub 上传 / GitHub Upload

这个仓库已经准备成适合上传 MandateOS 主 skill 的结构。`.gitignore` 已经排除了本地作者环境和那 5 个底层 skill 的复制目录。

This repository is prepared so that `.gitignore` excludes the local authoring workspace and the copied lower-level OKX skill folders.

Recommended upload flow:

```bash
git init
git add .
git commit -m "Add MandateOS OpenClaw skill"
git remote add origin <your-repo-url>
git push -u origin main
```

## 备注 / Notes

- MandateOS 是编排层，不是执行连接器
- 非执行提示词应该保持在 proposal-only
- 真实账户动作仍然依赖你单独安装的 OKX skills 和凭证
