# OpenClaw 自进化系统 · 基线封装
## Baseline v1.0.0 — 2026-06-28

---

## 版本信息

| 属性 | 值 |
|------|-----|
| **版本** | v1.0.0 Baseline |
| **日期** | 2026-06-28 |
| **代号** | King · CEO |
| **模型** | MiniMax-M2.7 |
| **OpenClaw** | v2026.6.11 |
| **节点** | DESKTOP-QQMDSC1 (Windows/WSL2) |
| **状态** | ✅ 可封装 |

---

## 三仓架构

| 仓库 | 内容 | 最新 Commit |
|------|------|-------------|
| `KJ77452361/openclaw-agents` | workspace/ + agents configs + skills | `d2c2bba` |
| `KJ77452361/openclaw-agents-config` | 11 agents × 8 core files | `e0d91b4` |
| `KJ77452361/openclaw-mybrain` | MyBrain 知识库独立仓库 | `d61775a` |

### 备份策略
- **频率**：每日 `knowledge_sync_daily` 自动同步
- **触发**：每周一 `sys_audit_weekly` 审计完整性
- **恢复**：`git clone` → 软链接 → 重启 Gateway

---

## 核心文件结构

```
agents/
  CEO/          AGENTS.md SOUL.md IDENTITY.md USER.md MEMORY.md
                STANDARDS.md TOOLS.md HEARTBEAT.md skills/
  COO/CTO/CA/   （各 11 个 Agent，结构一致）
  PM/QM/RS/SA/PE/HE/SE/
workspace/
  agents/       sales_executive / sales_operator / sales_orchestrator
                translator_zh
  skills/       12 个社区技能（clawhub 安装）
  MyBrain/      知识库（cards/ + system/）
system/
  SOP索引.md    22 个 SOP + WF 完整索引
  cron.md       16 个 Cron Job 完整清单
```

---

## Agent 生态（11 个核心 Agent）

| Agent | 角色 | 域 | SOUL.md |
|-------|------|-----|---------|
| CEO (King) | 决策中枢 / 记忆引擎 | 战略 | v2.2.0 OSP |
| COO | 运营总监 | 运营 | v2.2.0 OSP |
| CTO | 技术总监 | 技术 | v2.2.0 OSP |
| CA | 合规官 | 合规 | v2.2.0 OSP |
| PM | 项目管理 | 运营 | v2.2.0 OSP |
| QM/RS/SA | 运营域执行 | 运营 | v2.2.0 OSP |
| PE/HE/SE | 技术域执行 | 技术 | v2.2.0 OSP |

**执行层**（workspace/agents/）：sales_executive / sales_operator / sales_orchestrator / translator_zh

---

## Cron Job 清单（16 个 · v2.0.0 命名）

| # | 名称 | 频率 | 状态 | 说明 |
|---|------|------|------|------|
| 1 | sys_self_health_hourly | 每6h | ⚠️ error | 自检（payload 已修复） |
| 2 | sys_health_daily | 每日 | ✅ ok | 系统健康检查 |
| 3 | sys_session_cleanup_daily | 每日 | ✅ ok | Session 清理 |
| 4 | sys_report_daily_weekday | 工作日 | ✅ ok | 每日工作报告 |
| 5 | sys_dashboard_refresh_daily | 每日 | ✅ ok | 工作板刷新 |
| 6 | sys_audit_weekly | 每周 | ✅ ok | 标准化审计 |
| 7 | quality_check_weekly | 每周 | ✅ ok | 质量检查 |
| 8 | knowledge_llm_wiki_daily | 每日 | ✅ ok | LLM Wiki ingest |
| 9 | knowledge_backup_daily | 每日 | ✅ ok | 知识库备份 |
| 10 | knowledge_sync_daily | 每日 | ✅ ok | MyBrain Git 同步 |
| 11 | knowledge_audit_weekly | 每周 | ✅ ok | 知识审计 |
| 12 | memory_log_compress | 每日 | ⚠️ error | 内存日志压缩（delivery=none 已修复） |
| 13 | llm_wiki_weekly | 每周 | ⚠️ error | LLM Wiki lint（delivery=none 已修复） |
| 14 | auth_store_backup | 半 月 | ⏳ null | Auth Store 备份（正常，首次未到） |
| 15 | Memory Dreaming Promotion | 每日 | ✅ ok | Dreaming System |
| 16 | monthly_full_audit | 每月 | ⏳ null | 每月全面审计（正常，首次未到） |

**Error 状态说明**：3 个 error 均为偶发性 API 过载（MiniMax FailoverError），payload 已修复，持续监控中。

---

## SOP & 工作流（22 个）

| 代码 | 名称 | 类型 | 状态 |
|------|------|------|------|
| KB_SOP_001 | Agent 上线下线 SOP | SOP | active |
| KB_SOP_002 | Cron Job 上下线 SOP | SOP | active |
| KB_SOP_003 | 故障响应 SOP | SOP | active |
| KB_SOP_004 | Heartbeat 标准化 SOP | SOP | active |
| KB_SOP_005 | 每日复盘 SOP | SOP | active |
| KB_SOP_006 | 知识库更新 SOP | SOP | active |
| KB_SOP_007 | 告警与升级路径 SOP | SOP | active |
| KB_SOP_008 | Session 管理的最佳实践 SOP | SOP | active |
| KB_SOP_009 | HEARTBEAT.md 编写规范 | SOP | active |
| KB_SOP_010 | 需求阶段 RET 应对指南 | SOP | active |
| KB_SOP_011 | OpenClaw Agent 命名规范 | SOP | active |
| KB_WF_001 | Agent 协调工作流 | Workflow | active |
| KB_WF_002 | 故障响应工作流 | Workflow | active |
| KB_WF_003 | 需求处理工作流 | Workflow | active |
| KB_WF_004 | Dreaming System 工作流 | Workflow | active |
| KB_WF_005 | 知识库更新工作流 | Workflow | active |
| AG_SOP_001 | Sales Agent SOP | SOP | active |
| AG_SOP_002 | 飞书文档同步 SOP | SOP | active |
| AG_SOP_003 | 翻译 Agent SOP | SOP | active |
| AG_WI_001 | 工作项升级协议 | WI | active |
| QL_SOP_001 | 质量体系 SOP | SOP | active |
| QL_SOP_002 | 质量审计 SOP | SOP | active |
| QL_SOP_003 | 持续改进 SOP | SOP | active |

---

## 已安装社区技能（12 个）

| Skill | 功能 | 安装来源 |
|-------|------|---------|
| agent-team-orchestration | 多智能体团队编排 | ClawHub |
| self-improving-agent | 自我改进 | ClawHub |
| self-improving-proactive-agent | 主动自我改进 | ClawHub |
| healthcheck | 系统健康检查 | ClawHub |
| productivity | 生产力框架 | ClawHub |
| copywriting | 营销文案 | ClawHub |
| meme-maker | 表情包生成 | ClawHub |
| diagram-maker | 图表生成 | ClawHub |
| weather | 天气查询 | ClawHub |
| task | 任务管理 | ClawHub |
| memory-pipeline | 记忆管道 | ClawHub |
| spike | 快速原型验证 | ClawHub |

---

## OSP SOUL.md 结构（v2.2.0）

所有 11 个核心 Agent 均采用 OSP（Open Soul Protocol）三层结构：

```
nucleus/        驱动力向量 + 绝对原则（不变）
persona/       行动准则（半稳定）
pulse/         沟通风格（高频迭代）
```

详见各 Agent 的 SOUL.md 文件。

---

## 已知限制 / 待完善项

| # | 项目 | 优先级 | 说明 |
|---|------|--------|------|
| 1 | `sys_self_health_hourly` isolated session cron list 隔离 | P2 | payload 已优化，但仍依赖硬编码 job 列表 |
| 2 | 3 个 error cron job 持续监控 | P2 | API 偶发过载，持续观察连续失败次数 |
| 3 | Agent-to-Agent messaging | P3 | 安全设计禁用；如需启用设 `tools.agentToAgent.enabled=true` |
| 4 | Feishu DM 渠道配置 | P3 | channels.feishu.dmPolicy=allowlist；如需全员DM需添加白名单 |
| 5 | 43 个 historical task issues | P4 | Gateway 重启遗留，无需处理 |

---

## 快速恢复指南

### 完整恢复（节点重建）
```bash
# 1. 克隆三仓
git clone https://github.com/KJ77452361/openclaw-agents.git
git clone https://github.com/KJ77452361/openclaw-agents-config.git
git clone https://github.com/KJ77452361/openclaw-mybrain.git

# 2. 安装 OpenClaw
npm install -g openclaw

# 3. 恢复配置
openclaw gateway import openclaw-agents-config/

# 4. 恢复知识库
cp -r openclaw-mybrain/* ~/.openclaw/workspace/MyBrain/
```

### 仅配置恢复（不丢记忆）
```bash
# 从 agents-config 同步 Agent 文件
rsync -av openclaw-agents-config/ ~/.openclaw/agents/

# 重启 Gateway
openclaw gateway restart
```

---

_封装时间：2026-06-28 12:22 CST_
_封装者：King · CEO · Baseline v1.0.0_
