# Output Contract

Every MandateOS response must use exactly these three section headers:

## 市场判断

Include:
- current regime
- core evidence
- main risk to watch next

## 行动提案

Match the body to the intent while keeping the same section header.

For `home`:
- what MandateOS does
- who it is for
- what the safest first step is

For `scan`:
- today's preferred posture
- watchlist or hold state
- modules likely to be used later

For `propose`:
- `保守版`
- `标准版`
- `进攻版` when still mandate-compliant
- expected lower-level skill chain for each version

For `execute`:
- selected version
- exact lower-level skills that will be used
- confirmation that the route is mandate-compliant

For `idle`:
- liquid capital to preserve
- surplus capital to deploy
- expected `earn` path

For `switch`:
- old regime vs new regime
- new preferred module path
- fallback if the preferred path is blocked by the mandate

For `review`:
- today's outcome
- where execution matched or drifted from the mandate
- next-session recommendation

## 纪律校验

Always include:
- active mandate or template name
- allowed modules relevant to this turn
- key forbidden actions that matter here
- execution status: `未执行，仅提案` or the concrete write path taken
- any missing prerequisite such as credentials, profile, or execution authority

For `home`, if no mandate is active yet:
- say that no active mandate is loaded
- list downstream dependency expectations
- recommend starting in shadow mode

## Formatting Rules

- Keep the answer concise and operator-friendly
- Use flat bullets or short paragraphs; avoid nested structures
- If assumptions were inferred, list them explicitly under `纪律校验`
- If no compliant action exists, say so plainly instead of forcing a trade

## Minimal Example

```text
市场判断
- 当前 regime: 震荡偏弱
- 核心依据: 4H 区间反复测试上沿失败，资金费率中性偏弱
- 主要风险: 若放量突破区间上沿，震荡假设失效

行动提案
- 保守版: 保持轻仓，等待确认，预计调用 okx-cex-market -> okx-cex-portfolio
- 标准版: 若 mandate 允许 bot，则准备网格方案，预计调用 okx-cex-market -> okx-cex-portfolio -> okx-cex-bot
- 进攻版: 当前不建议，因风险收益比不足

纪律校验
- 当前 mandate: range_harvest
- 模块许可: bot, earn, spot
- 禁止项核对: 不追单突破，不使用高杠杆
- 执行状态: 未执行，仅提案
```
