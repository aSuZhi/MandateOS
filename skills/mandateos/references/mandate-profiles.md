# Mandate Profiles

Use this reference when the user wants to create, edit, compare, or select a mandate.

## Profile Schema

Every mandate uses the same fixed fields:

| Field | Meaning | Notes |
|---|---|---|
| `objective` | The user-facing goal | Keep it in plain language |
| `horizon` | Decision horizon | Examples: `intraday`, `7d`, `30d`, `90d` |
| `risk_budget` | Risk limits | Include max drawdown, max single-trade risk, and max leverage |
| `allowed_modules` | Allowed execution modules | Subset of `spot`, `swap`, `futures`, `option`, `earn`, `bot` |
| `forbidden_actions` | Hard bans | Keep as clear behavioral rules |
| `liquidity_buffer` | Capital that must remain liquid | Percent or amount |
| `style` | Primary operating style | `trend`, `range`, `protected`, `carry`, or `balanced` |
| `approval_mode` | Execution authority | `proposal-only` or `explicit-execution` |

## Drafting Rules

Use these defaults when the user gives an incomplete profile:
- If the user names a template, start from the matching asset file in `{baseDir}/assets/`
- If the user does not name a template, choose the closest one from the selection table below
- If `approval_mode` is omitted while drafting from free text, default to `proposal-only`
- If `liquidity_buffer` is omitted, default to the template value
- If `forbidden_actions` is omitted, keep the template guardrails
- If the user asks for execution but the active mandate is still incomplete, stop and ask only for the missing field that blocks discipline

## Template Selection

| Template | Best for | Default routing bias |
|---|---|---|
| `trend_pro` | Directional traders who want controlled trend exposure | `market -> portfolio -> trade`, optional `option` overlay |
| `range_harvest` | Range traders who want grid-style harvesting and spare cash management | `market -> portfolio -> bot`, optional `earn` |
| `protected_long` | Users who are bullish but want risk protection and lower drawdown | `market -> portfolio -> trade + option`, optional `earn` |
| `carry_plus` | Users who prefer low-turnover capital efficiency over frequent trading | `portfolio -> earn`, optional spot accumulation |

Map free-form requests to templates like this:
- "看多 / 做趋势 / 抓主升" -> `trend_pro`
- "震荡 / 网格 / 高抛低吸" -> `range_harvest`
- "看多但怕回撤 / 想对冲 / 保护本金" -> `protected_long`
- "闲置资金 / 稳健增值 / 少交易" -> `carry_plus`

## Template Summaries

### trend_pro

Use when the user wants controlled directional exposure.

Default shape:
- medium horizon
- moderate drawdown tolerance
- `spot` and `swap` allowed
- `option` allowed as protection, not as pure speculation
- execution can be enabled

Asset file:
- `{baseDir}/assets/trend_pro.yaml`

### range_harvest

Use when the user expects a choppy market and wants harvesting behavior.

Default shape:
- medium horizon
- low leverage
- `bot` and `earn` favored
- no breakout chasing
- proposal-first unless the user opts into execution

Asset file:
- `{baseDir}/assets/range_harvest.yaml`

### protected_long

Use when the user wants bullish participation with a tighter risk budget.

Default shape:
- medium horizon
- low to moderate drawdown tolerance
- `spot`, `option`, and `earn` allowed
- no naked high-leverage directional bets
- execution can be enabled

Asset file:
- `{baseDir}/assets/protected_long.yaml`

### carry_plus

Use when the user values capital efficiency and low turnover.

Default shape:
- medium horizon
- low drawdown tolerance
- `earn` favored first
- optional spot accumulation only when the risk budget permits
- proposal-first unless the user opts into execution

Asset file:
- `{baseDir}/assets/carry_plus.yaml`

## Response Pattern For Profile Creation

When creating or editing a profile:
1. Echo the resolved template
2. Show the full field set in a compact block
3. Call out inferred defaults explicitly
4. Ask for confirmation only if the missing information would change execution authority or risk boundaries
