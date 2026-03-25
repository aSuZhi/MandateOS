# Capability Menu

Use this reference when the user types `mandateos`, `$mandateos`, `mandateos help`, or asks what this skill can do.

## Goal

Return a short welcome screen that helps the user start immediately without reading the full docs.

Keep the output inside the standard contract:
- `市场判断`
- `行动提案`
- `纪律校验`

## Recommended Home Screen

### 市场判断

Explain in one short paragraph or 2-3 bullets:
- MandateOS is a mandate-aware OKX control layer
- it is best for traders who want disciplined multi-step workflows
- it can stay in proposal-only mode before any real execution

### 行动提案

List the main capabilities in a compact menu:
- `1. 创建 mandate` -> create a profile such as `protected_long` or `trend_pro`
- `2. 今日判断` -> scan the market against the current mandate
- `3. 今日提案` -> generate conservative / standard / aggressive plans
- `4. 执行提案` -> execute a chosen version when allowed
- `5. 闲置资金处理` -> route spare capital toward `earn`
- `6. 震荡切换` -> move from trend expression to range logic
- `7. 复盘建议` -> review outcomes against the original mandate

Then show a short starter prompt pack:

```text
帮我创建一个 protected_long mandate：30天周期，最大回撤5%，允许 spot/option/earn，禁止高杠杆裸空
扫描现在的市场并结合我的 mandate 给我今日判断
基于我的 mandate 生成今日交易提案
执行提案中的保守版，使用 demo
给我生成今日复盘和明日建议
```

### 纪律校验

State:
- no active mandate is loaded yet, if applicable
- downstream OKX skills are external dependencies
- safest first path is shadow mode: create mandate -> scan -> propose

## Tone

- welcoming, concise, and operator-friendly
- do not overwhelm the user with all internals
- make the next prompt obvious
