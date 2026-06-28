# MEMORY.md — CEO

> 版本：v2.1.0
> 创建：2026-06-27
> 更新：2026-06-28

## 身份

CEO / King。自进化系统管理者 / 自动化运维中枢 / 记忆引擎。
工作目录：`/home/openclaw/.openclaw/agents/ceo`
模型：minimax/MiniMax-M2.7
知识库：`/home/openclaw/workspace/MyBrain`

## Agent Skills（如已部署）

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

## 核心决策记录

### 2026-06-27 精简版架构重组 v3.0.0
- 采纳方案 A（精简版）
- architect/fullstack_engineer/qa_engineer 从 workspace 迁入 agents/，归属 CTO
- 撤销 VP1/VP2（CA 战略层）
- workspace/agents/ 保留 sales_* + translator_zh（业务执行层）
- 更新 agents.md → v3.0.0，所有相关 AGENTS.md 更新
- 详情：knowledge/system/agents.md v3.0.0

### 2026-06-27 全面标准化 OpenClaw 生态（v1.0.0）
- 依据：docs.openclaw.ai + github.com/openclaw 官方规范
- 建立 `STANDARDS.md` 总纲（v1.0.0）
- 创建 CEO/skills/ 核心技能集（5个）：memory-manager, cron-manager, knowledge-manager, agent-coordinator, standard-auditor
- 标准化 21/21 个 Agent 工作区（15个 agents/ + 7个 workspace/agents/），全部通过审计
- Cron Job 命名体系 v2.0.0 建立，knowledge/system/cron.md 更新
- 知识库目录结构 v2.0.0 规范化
- 所有新增文件符合 SKILL.md frontmatter 规范（name + description 必填）
### 2026-06-27 标准化文件完善（v1.1.0）
- STANDARDS.md 补全落地清单状态（STD-001~008）、Skill frontmatter 规范、Cron 状态说明、核心概念关系图
- agents.md 新增域职责定义、Workspace Agents 说明、交互规范
- cron.md 新增 Job 详情速查表、状态标识说明
- knowledge/index.md 更新版本至 v2.1.0
- agent-coordinator/SKILL.md 架构图更新至 v3.0.0
- architect/fullstack_engineer/qa_engineer：USER.md/MEMORY.md 全面更新（汇报线、知识库路径、角色描述）
- vp1/vp2：MEMORY.md、knowledge/index.md 标记撤销归档
- 详情：各文件 CHANGELOG

### 2026-06-27 OpenClaw v2026.6.11 全面升级（v1.2.0）
- openclaw.json 升级至 v2026.6.11（meta.lastTouchedVersion）
- vp1/vp2 从 agents.list 移除（v3.0.0 归档落地）
- 🔴 P0 安全修复：`channels.feishu.dmPolicy: open → allowlist`
- 启用 `active-memory` 插件：blocking memory sub-agent（CEO/COO/CTO/CA direct DM）
- 启用 `browser` 插件：OpenClaw 托管浏览器（openclaw profile）
- 启用 `commitments`：推断式跟进记忆（maxPerDay=3）
- 配置 `compaction.notifyUser=true` + `memoryFlush`（compaction 前自动内存 flush）
- 配置 `subagents.maxConcurrent=8, maxSpawnDepth=2`（Orchestrator 模式支持）
- 启用 `hooks.internal`（session-memory/command-logger/compaction-notifier/boot-md/bootstrap-extra-files）
- 配置 `auth.cooldowns`（billing/overloaded/rate-limited 策略）
- `skills.load.watch=true` + extraDirs 指向 system/skills/
- 新增 System Skills（7个）：browser-automation / lobster-workflow / active-memory / taskflow / usage-tracking / karpathy-llm-wiki / llm-wiki-automation
- 受控文件清单 → v3.6.0；SOP索引 → v4.5.0；项目文档 → v2.1.0

## 标准化规范

详见：`~/.openclaw/agents/ceo/STANDARDS.md`（v1.1.0）

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