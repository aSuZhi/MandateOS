# Reproduction Runbook

Use this reference when the user wants to install, verify, demo, or troubleshoot MandateOS in OpenClaw.

## Quick Reproduction Paths

### Path A - No credentials, proposal-only demo

Use this when you want the fastest safe reproduction.

Requirements:
- OpenClaw can load `skills/mandateos`
- downstream `okx-cex-market` is available

Steps:
1. open the repository as an OpenClaw workspace
2. ensure `skills/mandateos` is discoverable
3. start with:

```text
帮我创建一个 protected_long mandate：30天周期，最大回撤5%，允许 spot/option/earn，禁止高杠杆裸空
```

4. run:

```text
扫描现在的市场并结合我的 mandate 给我今日判断
基于我的 mandate 生成今日交易提案
```

Expected result:
- both answers use `市场判断 / 行动提案 / 纪律校验`
- execution status remains `未执行，仅提案`
- proposal variants mention lower-level skill chains

### Path B - Demo execution path

Use this when downstream OKX skills and demo profile are already configured.

Requirements:
- `okx-cex-market`
- `okx-cex-portfolio`
- `okx-cex-trade`
- optional `okx-cex-earn`
- optional `okx-cex-bot`
- OKX CLI available in the OpenClaw runtime
- demo profile already configured

Steps:
1. create a mandate with `approval_mode: explicit-execution`
2. run:

```text
扫描现在的市场并结合我的 mandate 给我今日判断
基于我的 mandate 生成今日交易提案
执行提案中的保守版，使用 demo
给我生成今日复盘和明日建议
```

Expected result:
- execution is routed through the lower-level demo path, not live funds
- the execution answer re-states the chosen version and module path
- the review step checks outcomes against the original mandate

## OpenClaw Checklist

Before the first demo, confirm:
- MandateOS is present under `skills/mandateos`
- required OKX skills are already installed in OpenClaw
- the environment can see the downstream CLI or MCP tools those skills require
- if using execution, the downstream demo profile is configured before the session starts

## Demo-Day Checklist

Use this exact order:
1. create mandate
2. daily judgment
3. proposal
4. execute a bounded demo path
5. review

Keep one fallback ready:
- if account access fails, stop at proposal-only and point out that the skill correctly refused unsafe execution

## Expected Proof For Judges

After each step, make sure the answer visibly shows:
- active mandate name
- lower-level skill chain or planned chain
- whether execution happened
- why the route was chosen

## Troubleshooting

### MandateOS does not trigger

Check:
- the request includes `mandate`, `投委会`, `今日判断`, `交易提案`, `闲置资金处理`, `震荡切换`, or `复盘`
- OpenClaw is loading the workspace `skills/` directory

### The flow stops before execution

This is expected if:
- the active mandate is `proposal-only`
- required modules are not allowed
- downstream credentials or profile are missing

### The demo must continue even without account access

Use proposal-only mode and emphasize:
- the market scan still works
- the proposal still names exact downstream skills
- the discipline gate prevented a dangerous fake execution
