# OpenClaw Agents Config v2.2.0

> OpenClaw 自进化系统 · 智能体核心配置文件
> 版本：v2.2.0 · 更新：2026-06-28

---

## 架构概览

```
openclaw-agents/
├── ceo/        # CEO · 决策中枢 · 守夜人
├── coo/        # COO · 运营总监
├── cto/        # CTO · 技术总监
├── ca/         # CA · 合规总监
├── pm/         # PM · 项目经理
├── qm/         # QM · 质量工程师
├── rs/         # RS · 供应链管理
├── sa/         # SA · 市场分析师
├── pe/         # PE · 产品工程
├── he/         # HE · 硬件工程
└── se/         # SE · 软件工程
```

## 文件规范（每 Agent 8 个核心文件）

| 文件 | 用途 | 版本 |
|------|------|------|
| **AGENTS.md** | Agent 架构定义、角色定位、汇报线 | v3.0.0 |
| **SOUL.md** | 人格内核驱动力（OSP 三层结构） | v2.2.0 |
| **IDENTITY.md** | 身份定义、名称、沟通风格 | v3.0.0 |
| **USER.md** | 用户/管理员信息 | v1.0.0 |
| **MEMORY.md** | 记忆引擎、核心决策记录 | v2.1.0 |
| **STANDARDS.md** | 标准化规范 | v1.1.0 |
| **TOOLS.md** | 本地工具配置 | v1.0.0 |
| **HEARTBEAT.md** | 心跳检查项 | v1.0.0 |

## SOUL.md OSP 结构（v2.2.0）

```
nucleus (内核)
  ├── drives: 驱动力向量 0.0-1.0
  └── prime_directives: 绝对原则

persona (交互)
  ├── 核心职能
  └── 行为准则

pulse (表现)
  ├── 语气向量
  └── 沟通风格
```

### 各 Agent SOUL.md 特色

| Agent | 核心框架 |
|-------|---------|
| CEO | agent-team-orchestration, self-improving |
| COO | Eisenhower 矩阵, 周/daily ritual, 任务分层 |
| CTO | healthcheck, github-sync, ADR, tech debt |
| CA | CISO 框架, 主动合规审计, 风险量化 |
| PM | APQP 阶段门禁, 里程碑跟踪 |
| QM | SPC 控制图, IATF 16949, 8D 报告 |
| RS | 供应商分层, 库存周转, 备选方案 |
| SA | AIDA/PAS/FAB 文案, 竞争情报 |
| PE | DFM, ECN, CAD 命名规范 |
| HE | EMI/EMC, 热分析, BOM 追溯 |
| SE | Clean Code, CI/CD, OWASP Top 10 |

## 社区技能生态

已从 ClawHub 采集优质技能：

| Skill | 来源 | 用途 |
|-------|------|------|
| self-improving-agent | pskoett (464K 下载) | 错误日志自改进 |
| agent-team-orchestration | arminnaimi | 多智能体协调 |
| project-management-2 | jk-0001 | 项目管理框架 |
| ciso | ivangdavila | 合规审计框架 |
| productivity | ivangdavila | 效率提升 |
| openclaw-github-sync | bradvin | GitHub 自动同步 |

技能存放：`~/.openclaw/workspace/skills/`（软链接到各 Agent）

## 相关仓库

- **[openclaw-agents](https://github.com/KJ77452361/openclaw-agents)** — Workspace + MyBrain 知识库备份

## 版本历史

| 版本 | 日期 | 变更 |
|------|------|------|
| v2.2.0 | 2026-06-28 | SOUL.md 全面升级 OSP 三层结构（nucleus/persona/pulse） |
| v2.1.0 | 2026-06-27 | ClawHub 社区技能集成 |
| v1.0.0 | 2026-06-26 | 初始版本 |

---

*🤖 OpenClaw · 自进化系统 · King CEO*
