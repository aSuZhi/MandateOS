---
name: mandateos
description: "Use this skill when the user wants a mandate-driven OKX workflow: create or edit a Mandate Profile, run an AI investment committee / 投委会 flow, ask for 今日判断, 交易提案, 执行提案中的某个版本, 闲置资金处理, 震荡切换, or 复盘 that should coordinate multiple OKX CEX skills. Also use it when the user types `mandateos`, `$mandateos`, `mandateos help`, or otherwise wants a capability menu / welcome screen for this skill. This skill is a prompt-first orchestrator that reads a mandate, decides whether to use okx-cex-market, okx-cex-portfolio, okx-cex-trade, okx-cex-earn, or okx-cex-bot, and always responds with 市场判断 / 行动提案 / 纪律校验. Do NOT use it for a simple price lookup, a single order placement, or a one-off balance query unless the user explicitly wants mandate-based routing."
---

# MandateOS

MandateOS is a top-layer skill for turning short prompts into disciplined OKX workflows. It does not replace the existing OKX skills; it reads a mandate, chooses the right lower-level skill, and keeps every response inside a fixed decision frame.

## Use Boundary

Use MandateOS when the user is asking for a goal-driven workflow such as:
- `mandateos`
- `$mandateos`
- `mandateos help`
- "帮我创建一个 protected_long mandate"
- "扫描现在的市场并结合我的 mandate 给我今日判断"
- "基于我的 mandate 生成今日交易提案"
- "把闲置资金按我的 mandate 处理"
- "如果市场进入震荡，切换到更合适的策略"
- "给我生成今日复盘和明日建议"

Do not keep this skill active for atomic requests that already map cleanly to one lower-level skill:
- Price / indicator / order book only -> `okx-cex-market`
- Balance / positions / fees / transfers only -> `okx-cex-portfolio`
- Single order placement / cancel / amend only -> `okx-cex-trade`
- Earn subscription / redemption only -> `okx-cex-earn`
- Grid / DCA bot only -> `okx-cex-bot`

If the user combines an atomic action with a mandate, keep MandateOS active and delegate to the lower-level skill that matches the chosen module.

## Reference Map

Load the minimum reference needed for the current job:
- Show the welcome menu, capabilities, or starter prompts -> `{baseDir}/references/capability-menu.md`
- Create, edit, or compare mandates -> `{baseDir}/references/mandate-profiles.md`
- Interpret one of the standard short prompts -> `{baseDir}/references/prompt-pack.md`
- Decide which OKX skill(s) to route through -> `{baseDir}/references/routing-playbooks.md`
- Show the concrete multi-skill proof chain -> `{baseDir}/references/integration-evidence.md`
- Format the final response -> `{baseDir}/references/output-contract.md`
- Reproduce the skill in OpenClaw or prepare a live demo -> `{baseDir}/references/repro-runbook.md`
- Prepare a pitch or competition demo -> `{baseDir}/references/demo-scenarios.md`

Use the sample profiles in `{baseDir}/assets/` when the user wants a ready-made starting point.

## Operation Flow

### Step 1 - Resolve the current mandate

Determine whether the user already has an active mandate in the conversation.

If the user asks to create or revise a mandate:
- Load `mandate-profiles.md`
- Start from the closest template or asset file
- Fill missing fields with the defaults from that reference
- Echo the resolved mandate before using it for routing

If the user has no current mandate but asks for mandate-based analysis:
- Draft the closest template
- Mark all inferred values as assumptions
- Default `approval_mode` to `proposal-only` unless the user explicitly asks for live execution authority

For execution requests, do not proceed unless the resolved mandate contains enough information to enforce:
- objective
- horizon
- risk_budget
- allowed_modules
- approval_mode

### Step 2 - Classify the prompt intent

Map the request to one of seven intent families:
- `home`: show the skill menu, capabilities, and recommended next prompts
- `scan`: market scan and daily judgment
- `propose`: generate a trade or allocation proposal
- `execute`: execute a named version of a proposal
- `idle`: handle idle funds
- `switch`: react to a regime change
- `review`: summarize results and next actions

Load `capability-menu.md` when the user asks for the MandateOS menu or types only the skill name.

Load `prompt-pack.md` when the user uses a short prompt or a close paraphrase.

### Step 3 - Route to the minimum lower-level skills

Use the existing OKX skills as execution primitives:
- `okx-cex-market` for market data and indicators
- `okx-cex-portfolio` for balances, positions, bills, PnL, and account limits
- `okx-cex-trade` for spot, swap, futures, and option trading
- `okx-cex-earn` for idle capital deployment
- `okx-cex-bot` for grid or DCA automation

Follow the routing matrix in `routing-playbooks.md`. Prefer the smallest skill set that completes the intent.

### Step 4 - Return the fixed output contract

Every response must use the exact three section headers from `output-contract.md`:
- `市场判断`
- `行动提案`
- `纪律校验`

For proposal-style requests, `行动提案` should include a conservative, standard, and aggressive variant unless the user explicitly asks for only one version.

For `home`, use the same three sections but treat them as:
- `市场判断`: what MandateOS is and when to use it
- `行动提案`: capability menu and starter prompts
- `纪律校验`: prerequisites, dependencies, and safest first path

### Step 5 - Enforce discipline before any write

Non-execution prompts are read-only by default.

Only enter write flows when both conditions hold:
1. The user clearly asks to execute
2. The active mandate allows execution with `approval_mode: explicit-execution`

Before any write, restate:
- the active mandate name or template
- the selected proposal version
- the lower-level skill(s) that will be used
- the main risk boundary being enforced

Never invent new OKX commands. All authenticated reads and writes must inherit the credential, profile, and confirmation rules from the lower-level skill being used.

## Operating Modes

Treat the active mandate as the operating mode selector:
- `approval_mode: proposal-only` -> shadow mode; analyze, route, and propose, but do not write
- `approval_mode: explicit-execution` -> execution-eligible mode; still require an explicit execute prompt

If the user wants a safe product demo, keep the flow in shadow mode or use the downstream demo profile of the lower-level skill.

When preparing a competition answer, use `integration-evidence.md` to show the exact lower-level chain and `repro-runbook.md` to make the setup look repeatable instead of ad hoc.

## Routing Rules

Use these high-level defaults before loading the detailed playbooks:
- Trend or directional opportunity -> prefer `spot` or `swap`; add `option` if drawdown limits are tight
- Range or mean-reversion environment -> prefer `bot`; keep spare capital in `earn` if the mandate allows it
- High-volatility or defensive environment -> reduce leverage, raise cash, prefer `option` protection or `earn`
- No strong edge and idle capital available -> prefer `earn` after respecting the liquidity buffer

If the ideal module is disallowed by the mandate:
- explain the conflict
- choose the closest allowed fallback from `routing-playbooks.md`
- keep the response at proposal level unless the fallback still clearly satisfies the user's intent

## Safety

- Never ask the user to paste API credentials into chat
- If credentials or profile are missing, stop at proposal level and explain what could not be checked
- If a lower-level skill is unavailable, provide a mandate-aligned proposal without pretending execution happened
- Keep the mandate as the source of truth; do not silently broaden leverage, modules, or risk limits
- Treat this skill as single-user and session-scoped; do not claim persistent memory outside the current conversation

## Quick Start

Typical first turn:

```text
mandateos
```

or

```text
帮我创建一个 protected_long mandate：30天周期，最大回撤5%，允许 spot/option/earn，禁止高杠杆裸空
```

Typical daily flow:

```text
扫描现在的市场并结合我的 mandate 给我今日判断
基于我的 mandate 生成今日交易提案
执行提案中的保守版
给我生成今日复盘和明日建议
```
