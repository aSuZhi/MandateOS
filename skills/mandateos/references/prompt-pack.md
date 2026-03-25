# Prompt Pack

Use this reference when the user relies on short prompts instead of describing a full workflow.

## Core Entry Points

Treat the following prompts and close paraphrases as first-class MandateOS intents.

| Prompt | Intent | Expected result |
|---|---|---|
| `mandateos` / `$mandateos` / `mandateos help` | `home` | Show the MandateOS capability menu, supported workflows, template choices, and recommended next prompts |
| `扫描现在的市场并结合我的 mandate 给我今日判断` | `scan` | Read the active mandate, inspect market context, and return a daily judgment |
| `基于我的 mandate 生成今日交易提案` | `propose` | Return a mandate-aligned proposal with conservative, standard, and aggressive variants |
| `执行提案中的保守版` | `execute` | Re-state the chosen variant and only then enter lower-level write flows if execution is allowed |
| `把闲置资金按我的 mandate 处理` | `idle` | Route to portfolio and earn logic while preserving the liquidity buffer |
| `如果市场进入震荡，切换到更合适的策略` | `switch` | Re-evaluate regime and move from trend expression to range expression or a safer fallback |
| `给我生成今日复盘和明日建议` | `review` | Summarize results, discipline adherence, and next actions |

## Normalization Rules

- If the user omits "我的 mandate", keep using the currently active mandate
- If the user only types `mandateos` or `$mandateos`, do not ask follow-up questions first; show the capability menu and a recommended first step
- If no active mandate exists, draft one from the closest template before answering
- If the user names a template inline, bind that template for the current turn
- If the user asks for a single variant, skip the other variants and say so explicitly
- If the user says "执行" but the active mandate is `proposal-only`, stop at the execution gate and explain the mismatch in `纪律校验`

## Proposal Variant Rules

Unless the user asks for only one path, `propose` should return three versions:
- `保守版`: smallest size, strongest discipline, highest cash buffer
- `标准版`: baseline expression of the mandate
- `进攻版`: only if the mandate and current conditions still permit it

Each version should name:
- the modules it expects to call
- the capital posture
- the reason it fits the current regime

## Good Short-Form Examples

```text
mandateos
```

```text
$mandateos
```

```text
mandateos help
```

```text
帮我创建一个 trend_pro mandate：14天周期，最大回撤8%，允许 spot/swap/option
```

```text
扫描现在的市场并结合我的 protected_long mandate 给我今日判断
```

```text
基于我的 mandate 生成今日交易提案，标的聚焦 BTC 和 ETH
```

```text
执行提案中的保守版，使用 demo
```

```text
把闲置资金按我的 mandate 处理，优先保留 30% 流动性
```

```text
给我生成今日复盘和明日建议，只看 BTC 相关动作
```

## Prompt Expansion Discipline

Do not force the user to restate a long prompt after the first turn.

Instead:
1. recover the active mandate
2. recover the last proposal when needed
3. infer the intent from the short prompt
4. keep the response inside the fixed output contract
