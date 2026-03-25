# Integration Evidence

Use this reference when the user asks why MandateOS is more than prompt glue, or when preparing a submission against an integration-heavy scorecard.

## Why This Is Not Simple Concatenation

MandateOS does not just forward user text to a single OKX action. It performs four coordinated steps:
1. resolve the active mandate
2. classify the prompt intent
3. choose the minimum lower-level skill chain
4. enforce discipline before any write

That means the same short prompt can lead to different downstream chains depending on:
- active mandate
- allowed modules
- current regime
- execution authority
- account or profile readiness

## Prompt-To-Skill Proof Matrix

| User prompt | Intent | Required reasoning step | Lower-level chain |
|---|---|---|---|
| `扫描现在的市场并结合我的 mandate 给我今日判断` | `scan` | infer regime from market data and compare it to the mandate | `okx-cex-market -> okx-cex-portfolio` |
| `基于我的 mandate 生成今日交易提案` | `propose` | convert regime plus risk budget into conservative / standard / aggressive paths | `okx-cex-market -> okx-cex-portfolio -> proposed trade/earn/bot path` |
| `执行提案中的保守版` | `execute` | verify chosen variant, execution authority, allowed modules, and safety gates | `okx-cex-market -> okx-cex-portfolio -> okx-cex-trade/okx-cex-earn/okx-cex-bot -> verification` |
| `把闲置资金按我的 mandate 处理` | `idle` | separate liquidity buffer from deployable surplus | `okx-cex-portfolio -> okx-cex-earn` |
| `如果市场进入震荡，切换到更合适的策略` | `switch` | detect regime change and replace the previous expression with a mandate-safe fallback | `okx-cex-market -> okx-cex-portfolio -> okx-cex-bot/okx-cex-earn/okx-cex-trade` |
| `给我生成今日复盘和明日建议` | `review` | reconcile realized actions with the original mandate and identify drift | `okx-cex-portfolio -> okx-cex-trade and/or okx-cex-bot and/or okx-cex-earn` |

## Regime-To-Module Proof Matrix

| Regime | Preferred expression | Why it is mandate-native |
|---|---|---|
| Trend-up | `spot` or `swap`, optional `option` overlay | keeps directional exposure inside risk limits |
| Range / chop | `bot`, optional `earn` for spare capital | avoids forcing trend expression in non-trending conditions |
| Risk-off / high volatility | lighter exposure, `option` protection, higher cash, optional `earn` | protects drawdown budget first |
| Idle / no clear edge | `earn` or hold cash | prioritizes capital efficiency without inventing a trade |

## Safety Proof

MandateOS raises the integration bar because it blocks invalid transitions:
- no execution without `explicit-execution`
- no module use outside `allowed_modules`
- no silent override of `forbidden_actions`
- no fake execution if credentials or profile are missing

This means it is not a conversational skin over OKX. It is a control layer over OKX.

## Judge-Friendly One-Liners

Use these exact lines when the user needs concise proof:
- `一个短提示词先经过 mandate 解析，再经过路由，再进入 OKX 技能链，而不是直接下单`
- `同一条 prompt 在不同 mandate 下会走不同 skill chain，这就是原生集成，不是拼接`
- `真实执行前还有 discipline gate，所以它既能跑，又不会失控`
