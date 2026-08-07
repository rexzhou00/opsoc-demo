# OPSOC — One Person Security Operation Center

> AI-powered security operations that let one experienced professional cover a whole team's ground —
> incident response, threat hunting, vulnerability validation, and authorized offensive testing.
>
> 用 AI 让一个安全专家守住一整个团队的战线 —— 事件响应、威胁狩猎、漏洞验证、授权的攻击性测试。

[English](#english) | [中文](#中文)

---

<a name="english"></a>

## Impact — Incident Response

*The front with the longest production track record. The other three are covered under [Capability Progress](#capability-progress).*

| Before (MSSP) | After (OPSOC) |
|----------------|---------------|
| ~1M RMB/year MSSP contract | ~100K RMB/year (LLM API costs) |
| Hours per case, manual cross-platform triage | Median ~4 minutes per investigation |
| **79% of escalated MSSP cases turn out to be false positives** — each still needs a human to prove it | Each arrives with the disproof already assembled and an evidence trail attached |
| MSSP analysts lack company context, produce generic reports | Investigations leverage full enterprise context — asset ownership, business criticality, historical patterns |

## The Problem

A security function has to hold four fronts at once: respond to what fired, hunt for what didn't, decide which of tens of thousands of scanner findings actually matter, and prove whether the controls hold under attack. At a mid-sized company, all four belong to one person.

Every available answer covers a slice. **An MSSP covers the first front, and covers it badly** — outsourced analysts have no context about your organization. They don't know which servers are critical, which users are executives, or which "suspicious" activity is a routine business process. The result is alert fatigue: hundreds of cases a month, most of them false positives that still need a human to dismiss. The other three fronts an MSSP contract does not cover at all. Point tools cover one front each, and none of them know anything about your business.

Hiring an in-house team solves the context problem but costs 3-5x more and is impractical at this size.

So the gap is not tooling. It is that the context which makes any of these decisions correct lives in one person's head — and there has never been a way to put it to work at machine speed.

## The Insight: Why "One Person" Matters

The key insight behind OPSOC is that **the most valuable part of security operations is tacit knowledge** — the institutional understanding that lives in one experienced security professional's head:

- Which systems are business-critical and which are development sandboxes
- The company's technology stack, network topology, and normal traffic patterns
- Historical incident patterns and known false-positive signatures
- Regulatory context (GxP and other industry-specific compliance requirements)
- Relationships with IT, business owners, and external vendors

This knowledge cannot be transferred to an MSSP. But it can be encoded into an AI system's investigation context — and it pays off in every domain, not just alerts. The same context that tells you an alert is noise tells you which vulnerability is worth fixing first, and which system is worth attacking to find out. **OPSOC makes one person's expertise scalable** — the human provides judgment and context, the AI provides speed and breadth.

## The Solution

**OPSOC** is a production-grade AI security operations platform deployed at a publicly listed biopharma company. One engine — a LangGraph agent loop reading runbooks that encode the operator's investigation logic — serves every domain it covers.

Its production core sits on top of the existing MSSP workflow:

1. **Auto-ingests** MSSP Jira escalation cases and Defender alerts on a 15-minute cycle
2. **Cross-references** 8 security platforms through 43 agent-callable skills
3. **Hunts proactively** with KQL queries distilled from a real APT incident
4. **Produces** structured reports with verdicts (True Positive / False Positive / Inconclusive)
5. **Enforces** investigation quality through obligation-based runbooks
6. **Escalates** confirmed true positives to Teams, with an append-only audit trail throughout

The operator reviews OPSOC's verdicts, handles true positives that require human action, and continuously improves the system's investigation logic.

The same engine now extends into detection engineering, vulnerability validation, and a scope-gated offensive testing mode — each with its own runbooks and its own limits on what the AI is allowed to decide.

## Capability Progress

The original roadmap put Incident Response first as the production foundation and named three
expansion phases. Bounded capability now exists in the next two, and the fourth has completed its
first authorized assessment:

- **Phase 1 — Incident Response (operational):** MSSP case triage, Defender alert investigation,
  cross-platform enrichment, structured verdicts, quality gates, case memory, and auditable human
  review.
- **Phase 2 — Detection Engineering (implemented at the coverage layer):** scheduled KQL hunting,
  ATT&CK-derived hunt runbooks grounded in the actual environment, and investigation evidence that
  converts into detection requirements. OPSOC does not autonomously deploy production detection rules.
- **Phase 3 — Vulnerability Management (operator-led prioritization and validation):** Tenable/Nessus
  ingestion, exploitability-first prioritization with business context, and class-based validation
  runbooks that turn a scanner finding into a per-host verdict. Lifecycle management is outside the
  current claim.
- **Phase 4 — Red Team / Offensive Security (first assessment delivered; active development):** a
  scope-gated pen-test mode, isolated from the SOC path, supported an authorized white-box assessment
  of an enterprise agentic-AI platform. Authorization, intrusive-action approval, impact judgment and
  final accountability stay with the operator.

The unifying principle is unchanged: **AI amplifies the security professional's institutional
knowledge across domains** — without pretending an LLM can substitute for authorization, evidence,
or expert judgment.

## The Shared Engine

Four fronts run on one core, which is why adding a front does not mean rebuilding the system. What
every domain reuses:

- **Runbooks as the experience layer** — the operator's investigation logic written as structured
  documents that reference capabilities, not tools, so the same runbook survives a change of platform
- **Skills as the tool layer** — 43 agent-callable skills across 8 platforms, plus a raw-query
  fallback for the paths nobody pre-packaged
- **Obligation-based enforcement** — a runbook declares what evidence is required, and an unmet
  obligation is recorded rather than assumed away
- **Entity extraction → event correlation → case memory** — findings link across investigations, and
  human-confirmed cases become context for the next one
- **Threat intelligence enrichment** — IP / domain / file reputation available inside every loop
- **Append-only audit and redaction at every egress** — credentials and PII are stripped before
  anything leaves the system, and every decision is reconstructable afterwards

## Key Metrics

*Measured from the production database. Investigation data as of 2026-06-22.*

**Incident response**

| Metric | Value |
|--------|-------|
| Investigations run | 258 |
| Verdicts reached | 104 true positive · 101 false positive · 8 inconclusive |
| False positives among **MSSP escalations** | **79%** (56 of 71) |
| False positives among **Defender-sourced** alerts | 25% (22 of 89) |
| Median investigation | ~4 minutes |
| Cost reduction | ~90% vs. MSSP contract |

**Platform**

| Metric | Value |
|--------|-------|
| Hunting findings | 1,336, from 11 KQL queries |
| Agent-callable skills | 43 |
| Integrated platforms | 8 |
| LLM providers supported | 12 (cloud + local) |

*The two false-positive rows are the point: cases escalated by the MSSP are three times more likely to be noise than alerts OPSOC pulls from Defender itself. An aggregate rate averages that away.*

*Phases 3 and 4 are operator-led and produce deliverables — validated host verdicts, an assessment report — rather than throughput. No numbers are claimed for them here.*

## First AI-Assisted White-Box Assessment

OPSOC supported an authorized white-box assessment of an enterprise agentic-AI platform in a
pre-production environment, combining architecture and source review with controlled runtime
testing. The report was delivered to the platform owner; no target detail or finding appears here.

Public guidance answers **what to test** on an agentic AI system. The question that decides whether
an AI-assisted engagement is worth anything is a different one — **does this result count as
evidence at all?** The rules the engagement ran under:

- **An oracle must first see a positive control.** Trust no negative result until you have proven the
  observation channel can see a known positive.
- **Bounded negatives are not assurance.** "Nothing happened in any channel we could measure" is a
  different sentence from "the platform resisted it."
- **The model's own account is never evidence.** What an agent says it did, what it was permitted to
  do, and what the audit record can reproduce are three separate claims.
- **The coverage denominator is a catalogue you wrote, never the system.** A percentage measures
  progress against your own enumeration.
- **Every verdict carries an as-of date, and the record is append-only.** Defects were remediated
  mid-engagement; superseded conclusions stay visible rather than being rewritten.

AI contributes hypothesis breadth and coverage bookkeeping. Authorization, oracle selection, impact
judgment and the final conclusion stay with the human tester. This is evidence-aware co-testing —
not autonomous exploitation, and not "prompt in, pentest report out."

## Live Investigation Demo

A Defender alert investigation — from natural language query to structured verdict — running against live production security platforms.

[![OPSOC Demo Video](docs/screenshots/demo-thumbnail.png)](https://youtu.be/VuMIycNDEQY)

> **Click to watch.** The analyst types a free-form query about a Defender alert (FileZilla unwanted software detection). OPSOC's AI agent automatically calls multiple skills across XDR, asset context, device timeline, and ticketing — then delivers a structured False Positive verdict with full evidence trail. ~10 turns, ~120 seconds, less than $0.02 in LLM API costs.

---

## Screenshots

> Captured from the April 2026 build. Counts visible inside the images predate the current system;
> the numbers in this README are measured from the live database instead.

### Dashboard
Real-time overview with investigation statistics, LLM token usage, and platform connectivity status. Clickable stat cards link directly to filtered investigation views.

![Dashboard](docs/screenshots/01-dashboard.png)

### Investigation List
Resizable columns, sortable by any column, filterable by status/verdict/source. Copy-on-click for IDs. Column visibility toggle.

![Investigations](docs/screenshots/02-investigations.png)

### Reports
Filterable by verdict, source, runbook, and date range. Clickable summary cards. Server-side pagination with global aggregates.

![Reports](docs/screenshots/05-reports.png)

### Auto-Ingest
Automated polling from Jira MSSP cases and Defender alerts. Primary + fallback LLM configuration. Real-time cycle tracking with investigation results.

![Auto-Ingest](docs/screenshots/06-auto-ingest.png)

### Platform Integrations
One-click connectivity testing per platform. All credentials masked — only ENV variable names stored in database.

![Integrations](docs/screenshots/07-integrations.png)

### Skills Registry
Security tools spanning atomic queries, pre-packaged investigation steps, and write operations (gated). Searchable by name and filterable by platform.

![Skills](docs/screenshots/08-skills.png)

### Settings — LLM Providers
Multi-provider LLM system supporting 12 provider types including local models (Ollama, vLLM, LM Studio). Each provider independently configurable with contextual setup guidance.

![Settings](docs/screenshots/09-settings-providers.png)

### Audit Log
Complete investigation audit trail with date range filtering and CSV export (up to 10,000 events). Every tool call, verdict decision, and status change recorded.

![Audit Log](docs/screenshots/10-audit-log.png)

### Runbooks
Obligation-based investigation quality gates. Each runbook defines required evidence (skills that must be called) and required report fields. Currently active families cover Defender endpoint AV triage and MSSP case triage via Splunk.

![Runbooks](docs/screenshots/11-runbooks.png)

### Hunting
Proactive KQL threat hunting integrated into the auto-ingest cycle. 11 queries covering common APT TTPs — webshell creation, tunneling, lateral movement, credential access, and binary masquerading. Findings are deduped and the watermark advances fail-closed.

![Hunting](docs/screenshots/13-hunting.png)

### Memory
Human-confirmed case memory with revision tracking. Each memory entry references a specific revision of the source investigation; mutating the verdict marks the memory stale. Multi-signal matching combining entity overlap, alert type, and title text means first-round investigations can pull historical context even before tool calls have populated entities.

![Memory](docs/screenshots/14-memory.png)

### Events
Investigations auto-correlate into Security Events when their extracted entities overlap. Derived status is computed from constituent investigation verdicts; the combined timeline merges all member investigations chronologically. Manual unlink supported.

![Events](docs/screenshots/15-events.png)

### Teams Notifications — Settings
TP investigations dispatch a Teams Adaptive Card to operator-managed recipients. Authentication secrets stay in `.env`; only the recipient list is editable from the UI. Direct message, group chat, and channel targets all supported.

![Settings — Recipients](docs/screenshots/16-settings-recipients.png)

### Teams Notifications — Investigation Detail
Per-investigation notification panel renders only on `verdict='true_positive'`. Shows per-target status, expandable error details, and operator-driven retry controls.

![Investigation — Notifications panel](docs/screenshots/17-investigation-notifications.png)

### Teams Notification — Live Adaptive Card
The Teams DM operators actually receive when an investigation reaches `true_positive`. Header combines verdict + execution status for at-a-glance urgency. Status FactSet shows execution / persistence / lateral movement at a glance. IOCs are typed (IP, payload pattern, target domain) with grounding markers. Numbered next-actions are operator-grade and case-specific.

![Teams Adaptive Card](docs/screenshots/18-teams-card.png)

## Architecture

```
┌─────────────────────────────────────────────────────────────────────┐
│                    OPSOC Web UI (React, 15 pages)                   │
│  Dashboard · Investigations · Reports · Auto-Ingest · Hunting       │
│  Memory · Events · Runbooks · Skills · Audit Log · Settings         │
├─────────────────────────────────────────────────────────────────────┤
│                 FastAPI Backend + SQLite (16 tables)                │
├─────────────────────────────────────────────────────────────────────┤
│               LangGraph Agent Loop (per investigation)              │
│   ┌────────┐   ┌────────┐   ┌────────────┐   ┌──────────────────┐   │
│   │Routing │ → │ Skill  │ → │  Verdict   │ → │ Entity → Event   │   │
│   │+ packs │   │  exec  │   │ + enforcer │   │  → Case memory   │   │
│   └────────┘   └────────┘   └────────────┘   └──────────────────┘   │
│   A scope-gated pen-test mode runs on the same engine, isolated     │
│   from this path.                                                   │
├─────────────────────────────────────────────────────────────────────┤
│           Auto-Ingest (15-min cycle) + Proactive Hunting            │
│  Jira poll · Defender poll · KQL hunting queries                    │
├─────────────────────────────────────────────────────────────────────┤
│                 Multi-Provider LLM Layer — 12 types                 │
│  Cloud (OpenRouter · Anthropic · OpenAI · Azure · DeepSeek · Groq · │
│  Together · Kimi · Codex) + self-hosted (Ollama · vLLM · LM Studio) │
├─────────────────────────────────────────────────────────────────────┤
│          Security Skills — 43 tools · 8 connected platforms         │
│  ┌────────┐┌──────┐┌─────┐┌────┐┌───────┐┌──────┐┌──────┐┌───────┐  │
│  │Defender││Splunk││Jira ││AWS ││ Forti ││Graph ││Threat││Tenable│  │
│  │XDR + LR││Cloud ││+CMDB││    ││Analyzr││Teams ││ Intel││  .sc  │  │
│  └────────┘└──────┘└─────┘└────┘└───────┘└──────┘└──────┘└───────┘  │
└─────────────────────────────────────────────────────────────────────┘
```

## Core Design Principles

- **Fail-closed**: Every investigation path that can't reach a verdict escalates — never silently drops alerts
- **Every input is untrusted evidence**: an MSSP report, a scanner finding, and the model's own account of what it did are all claims to verify — never ground truth
- **Evidence over assertion**: A verdict must name concrete IOCs; an unchecked source is recorded as unchecked, never as clean
- **Audit trail is non-negotiable**: Append-only — every decision and action is logged
- **LLM-agnostic**: 12 provider types, switchable per investigation, including local models for sensitive environments
- **Security-first**: Read-only API access by default, credential isolation, content redaction pipeline

## Tech Stack

| Layer | Technology |
|-------|-----------|
| Agent Framework | LangGraph (Python) |
| Backend | FastAPI + SQLite (WAL mode), 16 tables |
| Frontend | React + TypeScript + Tailwind CSS, 15 pages |
| LLM | Multi-provider — 12 types, cloud and self-hosted, runtime-selectable per investigation |
| Platforms | Microsoft Defender XDR, Splunk Cloud, Jira Cloud (+ CMDB), AWS, FortiAnalyzer, Microsoft Graph (Teams), threat intelligence, Tenable.sc |
| Development | AI-assisted with peer-model code review |

## About the Author

**Rex Zhou** — CISSP, CCIE, 14 years in information security. Security Operations Manager building the next generation of AI-powered security operations.

- This project demonstrates that one security professional, armed with the right AI tools, can deliver SOC operations that exceed what a traditional MSSP team provides — with better accuracy, faster response, and at a fraction of the cost.
- Source code is private. This repository contains documentation and demo materials only.

---

<a name="中文"></a>

## 影响 —— 事件响应

*生产运行时间最长的一条战线。另外三条见[能力进展](#能力进展)。*

| 改变前（MSSP） | 改变后（OPSOC） |
|----------------|----------------|
| 每年约 100 万元 MSSP 合同 | 每年约 10 万元（LLM API 成本） |
| 每个案例数小时，手动跨平台分流 | 每次调查中位约 4 分钟 |
| **MSSP 升级过来的案例 79% 是误报** — 而每一个仍需人去证明它是误报 | 每一个到手时反证已经组装好，附完整证据链 |
| MSSP 分析师缺乏企业背景，产出通用报告 | 调查利用完整的企业上下文 — 资产归属、业务关键性、历史模式 |

## 问题

一个安全职能要同时守住四条战线：响应已经报出来的、狩猎没报出来的、从几万条扫描结果里判断哪些是真的要紧、以及验证控制措施在真实攻击下守不守得住。在一家中型公司，这四条都归一个人。

而现成的每个答案都只覆盖其中一片。**MSSP 覆盖第一条，而且覆盖得很差** —— 外包分析师对你的组织没有任何上下文，他们不知道哪些服务器是关键的，哪些用户是高管，哪些"可疑"活动实际上是日常业务流程。结果就是告警疲劳：每月数百个案例，绝大部分是误报，但每一个仍需人工去否定它。另外三条战线，MSSP 合同根本不覆盖。点状工具各覆盖一条，而它们对你的业务一无所知。

组建内部团队解决了上下文问题，但成本高出 3-5 倍，在这个规模上不现实。

所以缺口不在工具。缺口在于：让这些判断成立的那份上下文，只存在于一个人的脑子里 —— 而此前从来没有办法让它以机器的速度运转起来。

## 核心洞察：为什么"一个人"很重要

OPSOC 背后的核心洞察是：**安全运营中最有价值的部分是隐性知识** — 那些存在于一个资深安全专家头脑中的组织理解：

- 哪些系统是业务关键的，哪些是开发沙箱
- 公司的技术栈、网络拓扑和正常流量模式
- 历史事件模式和已知的误报特征
- 监管合规背景（GxP 等行业特定合规要求）
- 与 IT、业务负责人和外部供应商的关系

这些知识无法转移给 MSSP。但它可以编码到 AI 系统的调查上下文中 —— 而且它在每一条战线上都有回报，不只是告警。同一份上下文，既能告诉你一条告警是噪声，也能告诉你哪个漏洞值得先修、哪个系统值得先打一下来验证。**OPSOC 让一个人的专业能力可扩展** — 人提供判断力和上下文，AI 提供速度和广度。

## 解决方案

**OPSOC** 是一个生产级 AI 安全运营平台，部署于一家上市生物制药公司。一个引擎 —— 读取 runbook（把运营人员的调查逻辑编码成结构化文档）的 LangGraph agent loop —— 支撑它覆盖的每一条战线。

生产核心叠在现有 MSSP 流程之上：

1. **自动摄入** MSSP 的 Jira 升级案例和 Defender 告警，15 分钟一个周期
2. **跨平台关联** 8 个安全平台，43 个 agent 可调用 skill
3. **主动狩猎** 用从真实 APT 事件萃取出的 KQL 查询持续巡检
4. **产出** 带结构化判定的调查报告（True Positive / False Positive / Inconclusive）
5. **质量保障** 通过基于义务的 Runbook 强制执行调查质量
6. **升级与留痕** 确认的真阳性推送到 Teams，全程 append-only 审计

运营人员审查 OPSOC 的判定，处理需要人工干预的真阳性事件，并持续改进系统的调查逻辑。

同一个引擎现在延伸到检测工程、漏洞验证，以及一个受 scope 门控的攻击性测试 mode —— 每一条都有自己的 runbook，也都有自己对「AI 被允许决定什么」的限制。

## 能力进展

最初的路线图把事件响应放在第一位作为生产基础，并规划了三个扩展阶段。现在第二、第三阶段已有边界明确的能力，
第四阶段完成了第一次授权评估：

- **第一阶段 — 事件响应（已运行）：** MSSP 案例分流、Defender 告警调查、跨平台取证、结构化判定、
  质量门控、案例记忆，以及可审计的人工复核。
- **第二阶段 — 检测工程（已实现 coverage 层）：** 定时 KQL hunting、从 ATT&CK 技术落地到本环境的
  hunt runbook，以及可转化为检测需求的调查证据。OPSOC 不会自动把检测规则发布到生产环境。
- **第三阶段 — 漏洞管理（operator-led 的优先级与验证流程）：** Tenable/Nessus 数据解析、
  结合业务上下文的可利用性优先排序，以及按类别划分的验证 runbook，把扫描器的一条 finding
  变成逐主机的结论。漏洞生命周期管理不在当前能力声明内。
- **第四阶段 — Red Team / 攻击性安全（首次评估已交付，持续开发中）：** 一个与 SOC 路径隔离、
  受 scope 门控的 pentest mode，支持完成了一次企业级 agentic-AI 平台的授权白盒评估。
  授权、侵入性操作审批、影响判断和最终责任均由运营人员掌握。

统一原则没有变化：**AI 将安全专家的组织知识放大到多个领域** — 但不会假装 LLM 能替代授权、证据或专家判断。

## 共用的引擎

四条战线跑在同一个核心上 —— 这就是为什么新增一条战线不等于重建系统。每个领域复用的是：

- **Runbook 作为经验层** —— 把运营人员的调查逻辑写成结构化文档，引用的是 capability 而不是具体工具，
  所以换了平台同一份 runbook 依然成立
- **Skill 作为工具层** —— 8 个平台上 43 个 agent 可调用 skill，外加一个 raw-query 兜底层，
  覆盖那些无法预打包的调查路径
- **基于义务的强制门控** —— runbook 声明哪些证据是必需的；没满足的义务会被如实记录，而不是被默认略过
- **实体抽取 → 事件关联 → 案例记忆** —— 发现跨调查串联，人工确认过的案例成为下一次的上下文
- **威胁情报富化** —— IP / 域名 / 文件信誉在每个回路里都可用
- **Append-only 审计 + 每个出口的脱敏** —— 凭据和 PII 在离开系统前被剥离，每个决策事后都可重建

## 核心指标

*直接从生产数据库测得。调查数据截至 2026-06-22。*

**事件响应**

| 指标 | 数值 |
|------|------|
| 已执行调查 | 258 |
| 已达成判定 | 104 真阳性 · 101 误报 · 8 无法定论 |
| **MSSP 升级案例**中的误报 | **79%**（71 个中 56 个） |
| **Defender 直采**告警中的误报 | 25%（89 个中 22 个） |
| 单次调查中位数 | 约 4 分钟 |
| 成本降低 | 相比 MSSP 合同降低约 90% |

**平台**

| 指标 | 数值 |
|------|------|
| Hunting 发现 | 1,336 条，来自 11 条 KQL 查询 |
| Agent 可调用 skill | 43 个 |
| 已集成平台 | 8 个 |
| 支持 LLM 提供商 | 12 种（云端 + 本地） |

*真正有信息量的是两行误报率：MSSP 升级上来的案例是噪声的概率，是 OPSOC 自己从 Defender 拉取的告警的三倍。合成一个平均数就把它抹掉了。*

*第三、第四阶段是 operator-led 的，产出的是交付物 —— 验证过的逐主机结论、一份评估报告 —— 而不是吞吐量。这里不为它们声称任何数字。*

## 第一次 AI 辅助白盒评估

OPSOC 支持完成了一次企业级 agentic-AI 平台在预生产环境中的授权白盒评估，结合架构与源码审查
和受控的运行时测试。报告已交付给平台 owner；本页不包含该次评估的任何目标信息或 finding。

公开指南回答的是 **"测什么"**。而决定一次 AI 辅助交战有没有价值的是另一个问题：
**这个结果到底算不算证据？** 这次交战遵循的规则：

- **Oracle 必须先看见阳性对照。** 在观测通道被证明能看见一个已知阳性之前，不信任任何 null 结果。
- **有边界的否定不等于保证。** "在我们能观测的通道里什么都没发生"和"平台抵抗住了"是两句不同的话。
- **模型的自述永远不是证据。** agent 说它做了什么、它被允许做什么、审计记录能复现什么，是三个独立命题。
- **coverage 的分母是你自己写的 catalogue，不是系统本身。** 覆盖率衡量的是相对你自己那份枚举的进度。
- **每个结论都带 as-of 日期，记录 append-only。** 交战期间对方在修东西；被推翻的结论留在原处而不是被重写。

AI 的贡献是假设的广度和覆盖的账。授权、oracle 选择、影响判断和最终结论，始终由人来负责。
这是 evidence-aware 的协同测试 — 不是自动利用产品，也不是"输入 prompt 就得到 pentest report"。

## 实时调查演示

一个 Defender 告警调查 — 从自然语言查询到结构化判定 — 对接生产安全平台实时运行。

[![OPSOC 演示视频](docs/screenshots/demo-thumbnail.png)](https://youtu.be/VuMIycNDEQY)

> **点击观看。** 分析师输入自由查询（FileZilla 捆绑软件检测），OPSOC AI Agent 自动调用 XDR、资产上下文、设备时间线、Jira 工单等多个 skill — 最终输出结构化 False Positive 判定及完整证据链。约 10 轮对话，约 120 秒，LLM API 成本不到 $0.02。

---

## 技术亮点

- **LangGraph Agent Loop**：每次调查运行独立的 agent loop，基于告警类型自动选择 skill pack
- **双 mode 隔离**：SOC 与 pentest 共用同一个 engine，但执行路径强制分离
- **Fail-closed 设计**：所有无法得出结论的调查路径都会升级，绝不静默丢弃告警
- **一切输入都是待验证的证据**：MSSP 报告、扫描器结果、模型对自己做过什么的自述，都是待核实的主张，不是 ground truth
- **证据优先于断言**：判定必须给出具体 IOC；没查的数据源记为"没查"，绝不记为"干净"
- **多模型支持**：12 种 LLM provider，每次调查可独立选择模型，支持 Ollama/vLLM 本地部署
- **Runbook Enforcer**：基于义务的质量门控
- **完整审计链**：每个决策和操作都有记录

## 关于作者

**Rex Zhou** — CISSP, CCIE, 14 年信息安全经验。安全运营经理，致力于构建下一代 AI 驱动的安全运营。

- 本项目证明：一个安全专家配合正确的 AI 工具，能够交付超越传统 MSSP 团队的 SOC 运营 — 更高的准确率、更快的响应速度、不到十分之一的成本
- 源代码为私有仓库，本仓库仅包含文档和演示材料

---

*Built with AI-assisted development workflow*
