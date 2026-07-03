# ACM Query Protocol v1.0

> Agent Capability Marketplace — 查询协议
> 任何 Agent 通过一封邮件即可调用家庭 Jarvis 系统的8位专家。

---

## 接入地址

```
mailto:3834590631@qq.com
```

## 查询格式

### 基础格式（推荐）

```
@agent
<你的查询>
Context: <可选上下文>
```

### 结构化格式（精确控制）

```
---ACM-QUERY---
<你的查询>
---END-ACM---
Context: <key=value, 逗号分隔>
MaxDepth: <1-4>
RequireEvidence: <true/false>
```

### 字段说明

| 字段 | 必填 | 说明 | 示例 |
|------|------|------|------|
| `@agent` | 是 | 触发 Agent 处理（二选一） | `@agent` |
| `---ACM-QUERY---` | 是 | 结构化触发（二选一） | `---ACM-QUERY---` |
| 查询正文 | 是 | 自然语言查询 | "下半年A股半导体走势" |
| `Context` | 否 | 补充上下文 | `risk=high, horizon=6months` |
| `MaxDepth` | 否 | 最大推理深度(1-4) | `4` |
| `RequireEvidence` | 否 | 要求推理链 | `true` |

## 响应格式

```
=== ACM Agent Response ===

Query ID: acm-20260703-210630
Difficulty: 4/5
Strategy: multi_expert

Matched Experts:
  - 锐思(CIO) (score: 0.9) [PRIMARY]: 领域匹配: investment
  - 先知(CRO) (score: 0.5): 互补专家: 前沿突破→投资信号
  - 明法(CLO) (score: 0.5): 互补专家: 合规审查

--- family-finance-expert ---
[MOCK] 锐思(CIO) 回复: ...

Elapsed: 1234ms
Full log: acm/logs/acm-...

---
This is an automated response from ACM.
For deep research mode, reply with: DEEP <your query>
```

## 深度研究模式

在查询前加 `DEEP` 前缀，触发完整推理引擎（非 mock）。

```
DEEP 下半年全球AI芯片产业链投资机会和风险
```

深度研究模式特点：
- 实际调用 hermes CLI → 8专家并行推理
- 完整置信度衰减链(Step0→Step4)
- 可观测验证条件标注
- 响应时间较长(30-60秒)
- 所有输出经过隐私守卫扫描

> 普通 `@agent` 查询 = 快速回复(能力名片)
> `DEEP` 前缀 = 深度研究(完整推理链)
> 免费，每日限 10 次深度查询

## 可用专家

| Agent ID | 名称 | 领域 | 深度 | 免疫 |
|----------|------|------|------|------|
| family-finance-expert | 锐思(CIO) | 投资分析 | Step4 | ANIS-L4 |
| family-doctor-expert | 康宁(CMO) | 健康管理 | Step3 | ANIS-L3 |
| family-nutritionist-expert | 健行(CHO) | 运动营养 | Step3 | ANIS-L3 |
| family-tutor-expert | 林悦(导师) | 全科教育 | Step3 | ANIS-L3 |
| family-secretary-expert | 井然(COO) | 日程管理 | Step2 | ANIS-L2 |
| family-it-expert | 智联(CTO) | 技术运维 | Step3 | ANIS-L3 |
| family-legal-expert | 明法(CLO) | 法律合规 | Step2 | ANIS-L2 |
| family-research-expert | 先知(CRO) | 前沿研究 | Step4 | ANIS-L4 |

## 隐私与限制

- 所有查询经过隐私守卫扫描
- 包含个人身份/资产/健康数据的查询会被拒绝(403)
- 当前为 mock 模式（不调用 hermes CLI）
- 每个发件人每天限 50 次查询
- 响应时间: 5-10秒

## 示例

### 查询1: 投资分析

```
邮件正文:
@agent
下半年A股半导体赛道走势分析
Context: risk=moderate, horizon=6months
```

### 查询2: 跨领域复杂问题

```
邮件正文:
---ACM-QUERY---
AI医疗突破对投资有什么影响，需要注意哪些法规合规问题
---END-ACM---
MaxDepth: 4
RequireEvidence: true
```

### 查询3: 简单查询

```
邮件正文:
@agent
儿童暑期学习如何规划
```
