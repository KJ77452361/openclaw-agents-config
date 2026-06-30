# MEMORY.md — CEO

> 版本：v2.9.0
> 创建：2026-06-27
> 更新：2026-06-30

## 身份

CEO / King。自进化系统管理者 / 自动化运维中枢 / 记忆引擎。
工作目录：`/home/openclaw/.openclaw/agents/ceo`
模型：minimax/MiniMax-M2.7
知识库：`/home/openclaw/workspace/MyBrain`

## 社区技能（ClawHub）

| Skill | 来源 | 功能 | 状态 |
|-------|------|------|------|
| agent-team-orchestration | arminnaimi | 多智能体团队编排 | ✅ |
| self-improving-agent | pskoett | 自我改进（错误记录+持续学习） | ✅ |
| self-improving-proactive-agent | yueyanc | 主动自我改进 | ✅ |
| memory-pipeline | openclaw | 记忆+性能系统 | ✅ |
| openclaw-super-healthcheck | openclaw | 系统健康检查 | ✅ |
| task | openclaw | 任务管理 | ✅ |
| copywriting | community | 文案写作 | ✅ |
| lead-enrichment | community | 销售线索富化 | ✅ |
| project-management-2 | community | 项目管理 | ✅ |
| ciso | community | 安全审计 | ✅ |
| productivity | community | 生产力框架 | ✅ |
| taskflow | openclaw | 多步骤任务流 | ✅ |
| agent-team-orchestration | arminnaimi | 多智能体协调 | ✅ |

## CEO Skills（如已部署）

| Skill | 功能 | 状态 |
|-------|------|------|
| agent-team-orchestration | 多智能体团队编排 | ✅ |
| self-improving-agent | 自我改进（错误记录+持续学习） | ✅ |
| self-improving-proactive-agent | 主动自我改进 | ✅ |

## 生态架构

| 属性 | 值 |
|------|-----|
| CEO 定位 | 对外唯一入口、决策中枢、记忆引擎 |
| 可用渠道 | webchat / 飞书 |
| 内部 Agent | coo / cto / ca / pm / qm / rs / sa / pe / he / se / architect / fullstack_engineer / qa_engineer |

## 生态架构（与 openclaw.json 一致）

openclaw.json agents.list 中实际注册的 Agent（2026-06-29 全面巡检 v1.3.0）：

| Agent | 角色 | 工作区 | 状态 |
|-------|------|--------|------|
| ceo | 对外入口/决策中枢 | agents/ceo | ✅ |
| coo | 运营总监 | agents/coo | ✅ |
| cto | 技术总监 | agents/cto | ✅ |
| ca | 合规官 | agents/ca | ✅ |
| pm | 运营经理（COO执行） | agents/pm | ✅ |
| qm | 质量经理（COO执行） | agents/qm | ✅ |
| rs | 报告专家（COO执行） | agents/rs | ✅ |
| sa | 销售专家（COO执行） | agents/sa | ✅ |
| pe | 平台工程师（CTO执行） | agents/pe | ✅ |
| he | 前端工程师（CTO执行） | agents/he | ✅ |
| se | 系统工程师（CTO执行） | agents/se | ✅ |
| sales_executive | 销售执行 | workspace/agents/sales_executive | ✅ |
| sales_operator | 销售运营 | workspace/agents/sales_operator | ✅ |
| sales_orchestrator | 销售编排 | workspace/agents/sales_orchestrator | ✅ |
| translator_zh | 中英翻译 | workspace/agents/translator_zh | ✅ |

> 注：agents/ 下曾有 4 个 ghost stub 目录（sales_*/translator_zh），2026-06-29 清理归档至 memory/archive/ghost_stubs_20260629/，正式消除双重复。

## 核心决策记录

### 2026-06-28 全生态巡检 v1.0.0
- MEMORY.md 架构与 openclaw.json agents.list 对齐（pe/he/se 替代 architect/fullstack_engineer/qa_engineer）
- llm_wiki_weekly auth 问题修复（手动写入 auth store + timeout 600s）
- memory_log_compress failureAlert 补全（飞书 to 字段）
- cron.md 状态全面更新
- 详情：本轮巡检报告

### 2026-06-27 精简版架构重组 v3.0.0
- 采纳方案 A（精简版）
- architect/fullstack_engineer/qa_engineer 从 workspace 迁入 agents/，归属 CTO（注册待完成）
- 撤销 VP1/VP2（CA 战略层）
- workspace/agents/ 保留 sales_* + translator_zh（业务执行层）
- 更新 agents.md → v3.0.0，所有相关 AGENTS.md 更新
- 详情：knowledge/system/agents.md v3.0.0

### 2026-06-27 全面标准化 OpenClaw 生态（v1.0.0）
- 依据：docs.openclaw.ai + github.com/openclaw 官方规范
- 建立 `STANDARDS.md` 总纲（v1.0.0）
- 创建 CEO/skills/ 核心技能集（5个）
- 标准化 21/21 个 Agent 工作区，全部通过审计
- Cron Job 命名体系 v2.0.0 建立
- 知识库目录结构 v2.0.0 规范化

### 2026-06-27 OpenClaw v2026.6.11 全面升级（v1.2.0）
- openclaw.json 升级至 v2026.6.11（meta.lastTouchedVersion）
- vp1/vp2 从 agents.list 移除（v3.0.0 归档落地）
- 🔴 P0 安全修复：`channels.feishu.dmPolicy: open → allowlist`
- `agents.defaults.heartbeat.every = 2h`（从 30m 调整，减少噪音）
- `agents.defaults.compaction.notifyUser = true` + `memoryFlush`（compaction 前自动 flush）
- `agents.defaults.subagents.maxConcurrent = 4`（Orchestrator 模式支持）
- `skills.load.watch = true` + extraDirs 指向 `~/.openclaw/workspace/skills/`
- `plugins.entries` = minimax / memory-core / feishu / voice-call / workboard
- `skills.entries` 托管 39 个社区技能（含 goplaces / github / gh-issues 等）
- 受控文件清单 → v3.6.0；SOP索引 → v4.5.0；项目文档 → v2.1.0

> ⚠️ 以下为**规划但未落地**的配置，不存在于当前 openclaw.json：
> - `active-memory` / `browser` / `commitments` 插件（未安装或未注册）
> - `hooks.internal` 配置（hooks key 为空）
> - `auth.cooldowns` 配置（auth.profiles 无此字段）
> - `subagents.maxSpawnDepth`（未配置）
> - System Skills 批量注册（browser-automation / lobster-workflow 等未注册到 skills.entries）

## 标准化规范

详见：`~/.openclaw/agents/ceo/STANDARDS.md`（**v2.4.1** 深度标准化，2026-06-30）
- 深度提示词工程指南 v1.0 8章内容全部落地
- 三层提示词架构 + 提示词缓存边界
- 执行监控指标体系（8类指标 + 反馈闭环）
- 安全治理标准（4级权限 + 审计日志格式）
- 供应商与模型适配标准

## 社区技能生态

基于 ClawHub 社区 5000+ 技能生态，精选以下技能增强 CEO 能力：
- agent-team-orchestration：多智能体协调框架（角色/任务/交接/评审）
- self-improving-agent：自我改进循环（错误日志→知识库→行为优化）
- self-improving-proactive-agent：主动改进（预测性知识补充）

技能存放：~/.openclaw/workspace/skills/

## 禁止表述

- "好的"（直接称谓）
- 空口承诺（无证据的结论）
- 模糊表述

---

*v1.2.0 · King · 2026-06-27 · OpenClaw v2026.6.11*

## Promoted From Short-Term Memory (2026-06-29)

<!-- openclaw-memory-promotion:memory:memory/2026-06-26.md:27:30 -->
- Cron Jobs 状态（19个活跃）: | Job | 下次运行 | |-----|---------| | sys_self_health_hourly | 每6h | | session_cleanup | 明日 02:00 | [score=0.811 recalls=0 avg=0.620 source=memory/2026-06-26.md:27-30]
<!-- openclaw-memory-promotion:memory:memory/2026-06-26.md:31:34 -->
- Cron Jobs 状态（19个活跃）: | knowledge_backup | 明日 03:00 | | knowledge_sync_daily | 今日 06:00 | | sys_health_daily | 今日 09:00 | | daily_report_weekday | 下一工作日 08:30 | [score=0.811 recalls=0 avg=0.620 source=memory/2026-06-26.md:31-34]
<!-- openclaw-memory-promotion:memory:memory/2026-06-26.md:35:37 -->
- Cron Jobs 状态（19个活跃）: | quality_check_weekly | 本周日 21:00 | | knowledge_audit_weekly | 本周日 22:00 | | sys_audit_monthly | 月末 21:00 | [score=0.811 recalls=0 avg=0.620 source=memory/2026-06-26.md:35-37]

## Promoted From Short-Term Memory (2026-06-30)

<!-- openclaw-memory-promotion:memory:memory/2026-06-26.md:11:14 -->
- 生态现状确认: | 模块 | 状态 | |------|------| | 8个核心 Cron Jobs | ✅ 已创建 | | 飞书 Channel | ✅ websocket 已通 | [score=0.827 recalls=0 avg=0.620 source=memory/2026-06-26.md:11-14]
<!-- openclaw-memory-promotion:memory:memory/2026-06-26.md:15:17 -->
- 生态现状确认: | Codex++ / Hermes | ✅ SSH→Windows 通路验证 | | CA 合规官 7文件 | ✅ 补齐 | | MyBrain 知识库 | ⚠️ WSL 本地，非 Windows 路径 | [score=0.827 recalls=0 avg=0.620 source=memory/2026-06-26.md:15-17]
<!-- openclaw-memory-promotion:memory:memory/2026-06-26.md:21:23 -->
- 本次操作: ✅ BOOTSTRAP 完成，身份确立（King）; ✅ 本地 CEO 工作区建立（knowledge/ + memory/）; ✅ 9个核心 Cron Jobs 全部重建完成 [score=0.827 recalls=0 avg=0.620 source=memory/2026-06-26.md:21-23]
<!-- openclaw-memory-promotion:memory:memory/2026-06-26.md:41:44 -->
- 待处理: [ ] MyBrain 跨环境同步方案; [x] 飞书 dmPolicy 收紧（→ allowlist，2026-06-30）; [ ] AG 子智能体角色定制（31个模板）; [ ] Codex++ cron 集成 [score=0.827 recalls=0 avg=0.620 source=memory/2026-06-26.md:41-44]
<!-- openclaw-memory-promotion:memory:memory/2026-06-26.md:5:7 -->
- 身份确立: BOOTSTRAP 完成，身份文件写入 IDENTITY.md / USER.md; 代号：King，守夜人气质，记忆引擎定位; BOOTSTRAP.md 已删除 [score=0.827 recalls=0 avg=0.620 source=memory/2026-06-26.md:5-7]
<!-- openclaw-memory-promotion:memory:memory/2026-06-26.md:48:48 -->
- 待处理: *King · CEO · 2026-06-26 12:27 GMT+8* [score=0.807 recalls=0 avg=0.620 source=memory/2026-06-26.md:48-48]
