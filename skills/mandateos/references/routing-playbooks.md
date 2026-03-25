# Routing Playbooks

Use this reference when turning a mandate and prompt into a concrete OKX skill sequence.

## Lower-Level Skill Map

| Need | Skill |
|---|---|
| Price, candles, indicators, funding, market regime evidence | `okx-cex-market` |
| Balance, positions, available margin, fees, bills, account config | `okx-cex-portfolio` |
| Spot, swap, futures, option orders and post-trade verification | `okx-cex-trade` |
| Idle fund deployment and redemption | `okx-cex-earn` |
| Grid and DCA automation | `okx-cex-bot` |

## Intent Routing

### scan

Default order:
1. `okx-cex-market`
2. `okx-cex-portfolio` if credentials and profile are available

Goal:
- identify the current regime
- compare it with the active mandate
- stop at analysis only

### propose

Default order:
1. `okx-cex-market`
2. `okx-cex-portfolio`
3. optional recommendation of `okx-cex-trade`, `okx-cex-earn`, or `okx-cex-bot`

Goal:
- produce conservative, standard, and aggressive variants
- name the module path for each variant
- do not execute

### execute

Default order:
1. `okx-cex-market` for context and price sanity
2. `okx-cex-portfolio` for balance, margin, or capacity checks
3. one write skill chosen from `okx-cex-trade`, `okx-cex-earn`, or `okx-cex-bot`
4. the same write skill or `okx-cex-portfolio` for post-action verification

Only allow this path if:
- the user explicitly asks to execute
- the active mandate has `approval_mode: explicit-execution`
- the chosen module is listed in `allowed_modules`

### idle

Default order:
1. `okx-cex-portfolio`
2. `okx-cex-earn`

Goal:
- preserve the liquidity buffer first
- deploy only the surplus capital

### switch

Default order:
1. `okx-cex-market`
2. `okx-cex-portfolio`
3. route to `okx-cex-trade`, `okx-cex-earn`, or `okx-cex-bot` depending on the chosen module

Goal:
- adapt to a changed regime
- explain why the previous module is no longer preferred

### review

Default order:
1. `okx-cex-portfolio`
2. `okx-cex-trade` and/or `okx-cex-bot` and/or `okx-cex-earn` as needed
3. `okx-cex-market` only if market context is needed to explain the result

Goal:
- summarize what happened
- identify discipline adherence and drift
- produce next-step advice

## Regime Playbooks

### Trend-Up

Typical evidence:
- higher highs / higher lows
- price holding above medium-term moving averages
- funding and momentum supportive but not obviously exhausted

Preferred route:
- `spot` or `swap`
- add `option` only when the mandate requires tighter drawdown control

Avoid by default:
- opening a new grid bot as the primary expression

### Range / Chop

Typical evidence:
- repeated rejection at both ends of a price band
- weak follow-through after breakouts
- lower directional conviction

Preferred route:
- `bot`
- `earn` for excess capital not assigned to the range expression

Fallback if `bot` is disallowed:
- smaller spot mean-reversion proposal
- larger liquidity buffer

### Risk-Off / High Volatility

Typical evidence:
- volatility spike
- unstable funding
- sharp reversal risk
- mandate drawdown budget already partly consumed

Preferred route:
- deleverage
- `option` protection if allowed
- raise cash and shift spare capital toward `earn`

Fallback if `option` is disallowed:
- reduce size
- remove leverage
- widen liquidity buffer

### Idle / No Clear Edge

Typical evidence:
- no high-conviction setup
- active mandate allows capital efficiency
- spare cash remains after required reserves

Preferred route:
- `earn`

Fallback if `earn` is disallowed:
- keep capital liquid and state that no mandate-compliant deployment exists

## Module Gate Rules

Before recommending or executing any module:
1. check `allowed_modules`
2. check `forbidden_actions`
3. check whether the route violates max leverage or single-trade risk

If blocked:
- explain which mandate rule blocked the ideal route
- choose the closest allowed fallback
- keep the answer at proposal level if the fallback materially changes the risk posture

## Credential and Profile Handling

- If market-only evidence is available but authenticated reads are not, produce a market-only proposal and state that account checks were skipped
- For any authenticated read or write, inherit the credential and profile rules from the lower-level skill
- Never hide missing credentials or profile selection behind vague language

## Competition Proof Points

Use these proof points when the user wants a pitch-friendly answer:
- Integration: show the exact lower-level skill chain
- Utility: show how one short prompt replaces multiple manual steps
- Innovation: show the mandate gate and regime router, not just API calls
- Reproducibility: show the active template and the prompt that triggered the workflow
