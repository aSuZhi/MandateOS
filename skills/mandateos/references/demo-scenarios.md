# Demo Scenarios

Use this reference when the user wants to present MandateOS as a competition project.

## Five-Minute Demo Flow

### Scene 1 - Create the mandate

User line:

```text
帮我创建一个 protected_long mandate：30天周期，最大回撤5%，允许 spot/option/earn，禁止高杠杆裸空
```

What MandateOS should prove:
- it can turn a plain-language request into a structured profile
- it can surface assumptions instead of hiding them

Judge signal:
- reproducibility
- usability from the very first turn

Presenter line:
- "我先定义纪律，再让系统围绕纪律工作，而不是先让 AI 给我冲动交易建议。"

### Scene 2 - Morning scan

User line:

```text
扫描现在的市场并结合我的 mandate 给我今日判断
```

What MandateOS should prove:
- it reads market context through `okx-cex-market`
- it reads account context through `okx-cex-portfolio` when available
- it explains the regime instead of dumping raw data

Judge signal:
- integration
- utility

Presenter line:
- "这里不是把 K 线原样返回给我，而是把市场状态翻译成今天该不该动、该怎么动。"

### Scene 3 - Generate the proposal

User line:

```text
基于我的 mandate 生成今日交易提案
```

What MandateOS should prove:
- it outputs conservative, standard, and aggressive variants
- each variant names the lower-level skills it would call
- the proposal is clearly tied back to the mandate

Judge signal:
- innovation through routing and discipline

Presenter line:
- "同一句短 prompt，不同 mandate 会走不同的 OKX skill chain，这就是原生路由，不是简单拼接。"

### Scene 4 - Execute a bounded version

User line:

```text
执行提案中的保守版
```

What MandateOS should prove:
- it does not execute unless the mandate allows it
- it re-states the selected path before calling lower-level write skills
- it keeps the execution path narrow and auditable

Judge signal:
- real-world usability
- safety

Presenter line:
- "只有我明确说执行，而且 mandate 允许执行，它才会向下进入交易链路。"

### Scene 5 - Regime switch and review

User lines:

```text
如果市场进入震荡，切换到更合适的策略
给我生成今日复盘和明日建议
```

What MandateOS should prove:
- it can move from trend expression to range expression
- it can use `earn` or `bot` as alternative operating modes
- it can summarize discipline adherence and next steps

Judge signal:
- full-lifecycle workflow
- reproducibility

Presenter line:
- "这一步证明它不是一次性信号器，而是完整的日内工作流。"

## Four Must-Show Capability Cards

### 1. Trend expression

Target proof:
- `market -> portfolio -> trade`
- directional exposure is proposed inside mandate limits

### 2. Risk protection

Target proof:
- `trade + option`
- protection is framed as a discipline rule, not a bolt-on feature

### 3. Idle fund utilization

Target proof:
- `portfolio -> earn`
- liquidity buffer is preserved before any deployment

### 4. Range adaptation

Target proof:
- `market -> portfolio -> bot`
- the router knows when not to keep forcing a trend trade

## Fallback Demo Path

Use this if credentials, profile, or the downstream runtime is unavailable.

Safe fallback sequence:
1. create mandate
2. run market scan
3. generate proposal
4. attempt execution
5. let MandateOS stop at the discipline gate and explain why

What this still proves:
- mandate parsing works
- routing works
- output contract stays stable
- the system refuses unsafe fake execution

Judge framing:
- "一个真实可用的交易系统，不能因为环境没准备好就假装自己执行成功。这个拒绝动作本身就是产品成熟度。"

## Submission Story

Use this narrative when the user needs a stronger high-score framing:
- before: traders manually jump between market, account, execution, earn, and bot tools
- after: one short prompt enters MandateOS, which resolves a mandate, chooses a regime, routes to the right OKX skills, and keeps the whole flow auditable
- key leap: the product is not a trading chatbot; it is a discipline and capital-operations layer above OKX

## Scoring Translation

Use this wording when the user wants to align the demo to judging criteria:
- Integration: "一个短提示词触发多 skill 编排，而不是单 API 调用"
- Utility: "解决交易者每天都要重复做的扫描、提案、风控和资金利用"
- Innovation: "把 mandate 和投委会逻辑放在 agent-tradekit 之上"
- Reproducibility: "skill 可下载，模板可复制，提示词固定，输出结构固定"
