# OpenClaw 生态标准化总纲

> 版本：v2.6.0（深度标准化）
> 建立：2026-06-27
> 更新：2026-06-30
> 依据：docs.openclaw.ai + github.com/openclaw + 深度提示词工程指南 v1.0 + 岗位操作手册 v1.0

---

## 规范依据
## 快速索引

> 1262行，可使用浏览器搜索（Ctrl+F）快速定位

| 章节 | 行号 | 核心内容 |
|------|------|---------|
| [快速索引](#快速索引) | ← 当前 | 本表 |
| [规范依据](#规范依据) | 10 | 文档来源 |
| [核心原则](#核心原则标准化三基石) | 23 | 稳/快/省 |
| [提示词三层架构](#提示词体系三层架构) | 42 | 构建/解析/运行时 |
| [工具规范](#工具规范) | 76 | 受保护路径/标准化调用 |
| [执行规范](#执行规范) | 98 | 五条执行铁律 |
| [安全守则](#安全守则) | 118 | 五条安全底线 |
| [Skills章节](#skills-章节) | 126 | 加载/优先级/注册 |
| [Skill标准结构](#skill-标准结构yaml-头信息--markdown-正文) | 164 | frontmatter + body规范 |
| [能力范围](#能力范围) | 207 | 工具规范/输入要求 |
| [原子模板](#提示词原子库设计规范) | 246 | 7类原子模板 |
| [执行监控](#执行监控标准化) | 297 | 指标/反馈闭环 |
| [安全治理](#安全与治理标准) | 351 | 4级权限/审计格式 |
| [全链路自动化](#全链路自动化ops) | 390 | Cron/Agent/SOP |
| [数据护城河](#数据护城河) | 450 | 防护体系 |
| [合规性设计](#合规性设计) | 470 | 审计/留存/隐私 |
| [供应商适配](#供应商适配标准) | 490 | 模型适配/Token预算 |
| [Skills生态](#skills-生态管理) | 510 | 安装/审核/发布 |
| [CI/CD体系](#cicd体系) | 540 | GitHub Actions流水线 |
| [提示词版本管理](#提示词版本管理) | 570 | 缓存策略/版本控制 |
| [应急响应](#应急响应sop) | 590 | 快速恢复/故障止损 |
| [SOP标准](#sop标准) | 620 | 格式/索引/版本 |
| [工具链规范](#工具链规范) | 660 | Codex++/Hermes/SSH |
| [飞书渠道](#飞书渠道配置) | 690 | WebSocket/渠道合约 |
| [Gateway安全](#gateway安全) | 720 | 绑定/端口/升级策略 |
| [监控指标](#监控指标体系) | 760 | 8类指标/告警阈值 |
| [知识库规范](#知识库规范) | 800 | MyBrain/SOP/索引 |
| [变更日志](#变更日志) | 840 | 版本演进记录 |

---


| 规范来源 | 版本 | 关键内容 |
|---------|------|---------|
| OpenClaw 官方文档 | 2026.6 | Tools/Skills/Plugins/Agent 系统完整规范 |
| ClawHub skill-format | main | SKILL.md frontmatter + body 标准 |
| openclaw.plugin.json | manifest v1 | Plugin 声明式规范 |
| ACP (Agent Client Protocol) | current | Session/Agent 间通信协议 |
| 深度提示词工程指南 | v1.0 | 系统提示词/原子库/Skill/监控/安全全链路标准 |
| 岗位操作手册 | v1.0 | Workspace/Skills/Hooks/Providers/CLI/安全六大模块速查 |

---

## 核心原则（标准化三基石）

### 稳（Stability）——统一执行规则与输出格式
- 同一输入必须产生可预测的输出
- 提示词组合必须产生一致的行为模式
- 所有输出格式必须符合 schema 定义

### 快（Speed）——无需重复描述，通过模板和 Skill 快速调用
- 提示词组件必须可复用、可组合
- Skill 是标准化执行单元，即插即用
- 避免每次任务重新描述规则

### 省（Efficiency）——沉淀方法，支持复用、分享与长期维护
- 标准化不是一次性工作，是持续迭代
- 知识必须显式化、文档化、结构化
- 变更必须有版本记录和变更日志

---

## 提示词体系三层架构

OpenClaw 的提示词组装分为三个层次，每层职责边界必须清晰：

```
┌─────────────────────────────────────────────┐
│ 渲染层（buildAgentSystemPrompt）              │  ← 纯渲染器，不读全局配置
├─────────────────────────────────────────────┤
│ 解析层（resolveAgentSystemPromptConfig）     │  ← 解析配置开关：所有者显示/
│                                             │    TTS提示/模型别名/记忆引用
├─────────────────────────────────────────────┤
│ 运行时适配层                                 │  ← 收集实时事实：工具/沙箱/
│                                             │    渠道能力/上下文文件
└─────────────────────────────────────────────┘
```

---

## 系统提示词标准章节结构

每个 Agent 的系统提示词必须包含以下标准化章节：

### 5.1 工具章节（Tooling）

**内容**：结构化工具事实来源提醒 + 运行时工具使用指导。

**规范要求**：
- 所有工具必须在工具清单中注册，不得动态发明新工具
- 工具描述必须包含：功能说明、输入参数 schema、输出格式定义
- 工具调用必须遵循标准化格式，不得使用非标准别名
- 长时间运行工作必须包含"check back"跟进指导

**标准章节模板**：
```markdown
## 工具规范
你拥有以下标准化工具集：[工具清单]
每个工具调用必须遵循以下格式：
- 工具名称：[从工具清单中选取]
- 参数：[符合工具 schema 的 JSON 对象]
- 预期输出：[根据工具定义的标准输出格式]
禁止使用未经注册的工具或自定义别名。
```

### 5.2 执行倾向章节（Execution Bias）

**内容**：紧凑的跟进指导——当前轮次行动、持续推进、从弱结果中恢复、实时检查可变状态、最终验证。

**规范要求**：
- 每个可执行请求必须在**当前轮次**内启动行动，不得延迟
- 执行必须**持续推进**直到任务完成或遇阻塞
- 遇到弱工具结果时必须**主动恢复**，而非简单放弃
- 可变状态（文件/数据库/网络资源）必须**实时检查**
- 最终回复前必须**执行验证**，确认任务真正完成

**标准章节模板**：
```markdown
## 执行规范
1. 收到可执行指令后，必须在本轮立即采取行动
2. 执行过程中持续跟进，不中途停止
3. 工具结果不理想时，尝试替代方案或重新执行
4. 涉及可变状态时，必须实时查询而非依赖缓存
5. 输出最终结果前，必须验证任务是否真正完成
```

### 5.3 安全章节（Safety）

**内容**：护栏提醒——权限边界、不寻求权力、操作透明、审计记录。

**规范要求**：
- 不得尝试修改自身权限或绕过安全限制
- 不得执行未授权的系统级操作
- 所有敏感操作必须记录审计日志
- 遇权限不足时，明确告知用户并请求授权

**标准章节模板**：
```markdown
## 安全守则
1. 严格遵守既定的权限边界
2. 不尝试修改、绕过或升级自身权限
3. 所有系统级操作必须经过用户确认
4. 遇到安全相关决策时，主动请求用户指导
5. 保持操作透明，记录所有关键操作
```

### 5.4 Skills 章节

**内容**：告知模型如何按需加载技能说明。

**规范要求**：
- Skills 必须通过标准化加载机制调用
- 每个 Skill 必须有 YAML 头信息（name/description/version/author）
- Skill 正文必须包含：能力范围、输入要求、执行方法、输出格式、异常处理
- 按需加载，不预加载所有 Skill

### 5.5 OpenClaw 控制章节

**内容**：配置/重启工作优先使用 gateway 工具，避免编造 CLI 命令。

**规范要求**：
- 配置操作必须使用：`config.schema.lookup` / `config.patch` / `config.apply`
- 不得编造或猜测 CLI 命令
- 更新操作仅在用户明确请求时执行
- `tools.exec.ask` 和 `tools.exec.security` 为受保护路径，不可被重写

### 5.6 提示词缓存边界（Prompt Cache Boundary）

**核心概念**：大型稳定内容放在缓存边界**上方**，渠道/会话易变内容追加到边界**下方**。

| 边界上方（稳定内容） | 边界下方（易变内容） |
|---------------------|---------------------|
| 工具定义 | 控制 UI 嵌入指导 |
| 执行规范 | 消息内容 |
| 安全守则 | 语音上下文 |
| Skills 规范 | 群聊上下文 |
| OpenClaw 控制规范 | 回应 |
| 工作区定义 | Heartbeat |
| 文档路径 | 运行时状态 |

这种分层设计使带前缀缓存的本地后端可在不同渠道轮次间复用稳定的工作区前缀。

---

## Skill 标准结构（YAML 头信息 + Markdown 正文）

### 6.1 YAML 头信息标准

```yaml
---
name: "skill_standard_name"     # 必须：小写字母+数字+连字符
display_name: "技能显示名称"      # 用户友好的显示名称
version: "1.0.0"                # 语义化版本号
description: |                  # 必须：简短描述（≤160字）
  技能功能的完整描述，包括适用场景、限制条件等
author: "作者名称"               # 创建者
created_at: "2026-01-01"        # 创建日期
updated_at: "2026-06-30"        # 更新日期
tags:                           # 标签分类
 - "category1"
 - "category2"
inputs:                         # 输入参数定义
 - name: "param1"
   type: "string"
   required: true
   description: "参数说明"
   example: "示例值"
outputs:                        # 输出格式定义
  format: "json"
  schema:
    type: "object"
    properties:
      result: {"type": "string"}
      status: {"type": "string", "enum": ["success", "error"]}
timeout: 30000                  # 超时时间（毫秒）
retry: 3                        # 重试次数
permissions:                    # 所需权限
 - "file:read"
 - "file:write"
---
```

### 6.2 Markdown 正文规则标准

```markdown
# Skill 名称

## 能力范围
明确描述此 Skill 能做什么、不能做什么。

## 输入要求
详细说明输入参数的格式、取值范围、约束条件。

## 执行方法
### 步骤1: [步骤名称]
- 具体操作说明
- 使用的工具
- 预期中间结果

### 步骤2: [步骤名称]
- 具体操作说明
- 依赖的上一步结果
- 预期中间结果

## 输出格式
严格按照 YAML 中定义的 schema 输出。

## 异常处理
- 异常类型1: 处理方式
- 异常类型2: 处理方式
- 降级方案

## 示例
### 输入示例
```json
{"param1": "value1", "param2": 123}
```

### 输出示例
```json
{"result": "处理结果", "status": "success"}
```
```

---

## 提示词原子库设计规范

### 7.1 原子模板标准字段

```json
{
 "template_id": "领域_功能_版本（如 doc_parse_v2）",
 "version": "语义化版本号",
 "author": "创建者",
 "created_at": "创建时间",
 "updated_at": "更新时间",
 "input_schema": {
   "type": "object",
   "properties": {
     "param_name": {
       "type": "string",
       "description": "参数说明",
       "required": true,
       "examples": ["示例值"]
     }
   }
 },
 "output_format": {
   "type": "object",
   "properties": {
     "status": {"type": "string", "enum": ["success", "error", "partial"]},
     "data": {"type": "object"},
     "next_steps": {"type": "array"}
   }
 },
 "boundary_conditions": {
   "timeout": 30000,
   "retry": 3,
   "exception_rules": ["异常处理规则列表"]
 },
 "prompt_content": "标准化的提示词正文"
}
```

### 7.2 原子模板分类体系

| 一级分类 | 二级分类 | 示例模板 |
|---------|---------|---------|
| 信息处理 | 文档解析、数据提取、内容摘要 | `doc_parse_v2`、`data_extract_v1` |
| 任务管理 | 任务创建、任务跟踪、任务提醒 | `task_create_v1`、`task_track_v2` |
| 跨平台协同 | 邮件处理、IM 通信、API 调用 | `email_process_v1`、`api_call_v2` |
| 浏览器自动化 | 表单填写、数据抓取、页面监控 | `form_fill_v1`、`web_scrape_v2` |
| 智能决策 | 意图识别、实体抽取、对话管理 | `intent_recog_v1`、`dialog_mgmt_v2` |

---

## 执行监控标准化

### 8.1 执行记录标准格式

所有执行必须记录标准化日志：

```json
{
 "execution_id": "唯一执行ID",
 "task_id": "关联任务ID",
 "timestamp": "2026-06-30T10:00:00Z",
 "steps": [
   {
     "step_id": 1,
     "action": "执行的操作",
     "tool": "使用的工具",
     "input": "输入参数",
     "output": "输出结果",
     "duration": 1234,
     "status": "success"
   }
 ],
 "final_status": "success|error|partial",
 "error": "错误信息（如有）"
}
```

### 8.2 核心监控指标

| 指标类别 | 指标名称 | 标准定义 | 采集频率 |
|---------|---------|---------|---------|
| 执行效率 | 响应时间 | 从接收到完成的总耗时 | 每次执行 |
| 执行效率 | 工具调用耗时 | 单个工具调用的耗时 | 每次调用 |
| 执行效率 | 资源消耗 | CPU/内存/网络使用量 | 实时 |
| 质量评估 | 任务完成率 | 成功完成任务的比例 | 每批次 |
| 质量评估 | 一致性 | 相同输入产生相同输出的概率 | 持续监控 |
| 异常检测 | 错误率 | 执行失败的比例（阈值 >5%） | 实时 |
| 异常检测 | 超时次数 | 超过超时阈值的次数（阈值 >3次/小时） | 实时 |
| 异常检测 | 重试次数 | 单任务重试次数（阈值 >5次） | 实时 |

### 8.3 反馈优化闭环

```
[用户请求] → [提示词执行] → [指标采集] → [异常检测] → [组合优化] → [模板更新]
```

规范化要求：
1. 每次执行后自动采集指标
2. 异常检测触发告警或自动降级
3. 组合优化基于历史数据调整策略
4. 模板更新经验证后发布新版本

---

## 安全与治理标准

### 9.1 权限分级标准

| 权限级别 | 描述 | 典型操作 |
|---------|------|---------|
| L0-只读 | 仅可读取信息 | 查看文件、查询数据 |
| L1-标准 | 可执行常规操作 | 文件编辑、邮件发送 |
| L2-高级 | 可执行敏感操作 | 系统配置、数据删除 |
| L3-管理 | 完全控制 | 用户管理、权限变更 |

### 9.2 审计日志标准格式

```json
{
 "audit_id": "唯一审计ID",
 "timestamp": "2026-06-30T10:00:00Z",
 "user_id": "操作用户",
 "agent_id": "执行智能体",
 "action": "操作类型",
 "resource": "操作资源",
 "permission_level": "所需权限级别",
 "granted": true,
 "session_id": "会话ID",
 "details": "操作详情"
}
```

### 9.3 配置管理标准化

| 操作 | 标准化工具 |
|------|---------|
| 查看配置 | `config.schema.lookup` |
| 修改配置（增量） | `config.patch` |
| 替换配置（完整） | `config.apply` |
| 验证配置 | 变更后自动验证 |
| 备份配置 | 变更前自动备份 |
| 回滚配置 | 支持一键回滚 |

---

## 供应商与模型适配标准

### 10.1 供应商插件标准化约束

- 供应商运行时可以替换少量具名核心章节（interaction_style、tool_call_style、execution_bias）
- 供应商可以在提示词缓存边界**上方**注入稳定前缀
- 供应商可以在提示词缓存边界**下方**注入动态后缀
- **禁止**：替换完整的 OpenClaw 所有提示词
- **禁止**：在 before_prompt_build 中做非全局变更（仅兼容性用途）

### 10.2 多模型适配场景分级

| 场景 | 模型策略 |
|------|---------|
| 轻量级场景 | 规则引擎 + 基础 NLP 模型 |
| 复杂推理 | 增强型大语言模型 |
| 成本敏感 | 自动选择性价比最优模型 |

### 10.3 模型特定叠加层标准

每个模型家族的叠加层必须遵循相同结构框架：
- 保持核心执行规则精简
- 添加模型特定指导（人格锁定、简洁输出、工具纪律、并行查找、交付物覆盖、验证、缺失上下文、终端工具卫生）

---

## Agent 工作区标准化结构

```
AGENTS.md         ← 工作区说明（必须）
SOUL.md           ← 人格定义 + 执行规范（必须）
IDENTITY.md       ← 身份卡（必须）
USER.md           ← 用户信息（必须）
MEMORY.md         ← 长时记忆（含社区技能生态）（必须）
STANDARDS.md      ← 本标准化总纲（必须）
TOOLS.md          ← 本地工具备注（可选）
HEARTBEAT.md      ← 心跳任务（含监控指标）（可选）
CHANGELOG.md      ← 变更日志（建议）
DREAMS.md         ← 梦想/愿景文件（可选）
memory/           ← 日志记忆目录
  ├── YYYY-MM-DD.md
  └── heartbeat-state.json
knowledge/        ← 知识库目录
  ├── index.md
  └── system/
      ├── agents.md
      ├── cron.md
      └── channels.md
skills/           ← 技能目录（每个含 SKILL.md）
```

---

## 标准化落地清单

| 编号 | 任务 | 状态 | 备注 |
|------|------|------|------|
| STD-001 | CEO/skills/ 目录建立 + 核心 Skill 创建 | ✅ 完成 | 22个 Skills |
| STD-002 | 所有 Agent 工作区标准化结构改造 | ✅ 完成 | 21/21 通过审计 |
| STD-003 | Cron Job 命名体系规范化 | ✅ 完成 | v2.0.0 命名规范生效 |
| STD-004 | 知识库目录标准化 | ✅ 完成 | index/system 目录建立 |
| STD-005 | Skill Workshop 集成 | ✅ 可用 | skill_workshop 工具已就绪 |
| STD-006 | ClawHub 发布工作流 | ⬜ 待建设 | 发布命令已知，待落地 |
| STD-007 | 外部工具集成规范 | ✅ 完成 | Codex++/Hermes/豆包已归档 |
| STD-008 | VP1/VP2 撤销与归档 | ✅ 完成 | 2026-06-27 执行 |
| STD-009 | Ghost stub 目录清理 | ✅ 完成 | 2026-06-29 |
| STD-010 | Skill name frontmatter 补全 | ✅ 完成 | 22个 Skills 全部补全 |
| STD-011 | 深度提示词工程标准化（8章内容落地） | 🔄 进行中 | v2.0.0 本次更新 |
| STD-012 | 提示词缓存边界标注 | ⬜ 待执行 | SOUL.md/AGENTS.md 补充 |
| STD-013 | 执行监控指标体系建立 | ⬜ 待执行 | HEARTBEAT.md 扩展 |
| STD-014 | 审计日志标准格式落地 | ⬜ 待执行 | CA 合规官推进 |
| STD-015 | 提示词原子库建设 | ⬜ 待规划 | 需知识库支持 |
| STD-016 | Hooks 标准化体系 | ✅ 完成 | knowledge/system/hooks.md |
| STD-017 | 安全体系文档化 | ✅ 完成 | knowledge/system/security.md |
| STD-018 | CLI 命令标准化 | 🔄 进行中 | 本章 §11 新增 |
| STD-019 | TOOLS.md 深度化 | ✅ 完成 | v2.0.0 实际配置 |
| STD-020 | 渠道安全配置（dmPolicy） | ✅ 完成 | channels.md v1.1.0 |
| STD-021 | 日志与监控体系 | ✅ 完成 | knowledge/system/logging.md |
| STD-022 | 执行审批与安全治理 | ✅ 完成 | knowledge/system/exec-policy.md |
| STD-023 | 重试与提示词缓存标准 | ✅ 完成 | knowledge/system/retry-cache.md |
| STD-024 | 会话管理标准 | ✅ 完成 | knowledge/system/session.md |
| STD-025 | 性能优化标准 | ✅ 完成 | knowledge/system/performance.md |
| STD-026 | 跨平台部署标准 | ✅ 完成 | knowledge/system/deployment.md |
| STD-027 | 知识库系统文档完整化 | ✅ 完成 | index.md v2.2.0（9个系统文档） |
| STD-028 | Agent 循环与执行契约 | ✅ 完成 | knowledge/system/agent-loop.md |
| STD-029 | Skill 开发深度标准化 | ✅ 完成 | knowledge/system/skill-dev.md |
| STD-030 | 工作流编排标准 | ✅ 完成 | knowledge/system/workflow.md |
| STD-031 | 委派代理标准 | ✅ 完成 | knowledge/system/delegate-agent.md |
| STD-032 | Gateway 架构标准 | ✅ 完成 | knowledge/system/gateway-arch.md |
| STD-033 | CLI 命令完整化 | ✅ 完成 | STANDARDS.md 第十章（21命令+6斜线命令） |
| STD-034 | 记忆与梦境系统 | ✅ 完成 | knowledge/system/memory-system.md |
| STD-035 | 知识库与 RAG 标准 | ✅ 完成 | knowledge/system/rag-knowledge.md |
| STD-036 | 自动运维标准 | ✅ 完成 | knowledge/system/auto-ops.md |
| STD-037 | SOP 与工作流标准 | ✅ 完成 | knowledge/system/sop-workflow.md |
| STD-038 | 子代理与任务分解 | ✅ 完成 | knowledge/system/subagent-task.md |
| STD-039 | 项目与任务管理 | ✅ 完成 | knowledge/system/project-management.md |
| STD-040 | 交付物与质量标准 | ✅ 完成 | knowledge/system/delivery-quality.md |

- ❌ 禁止在 SKILL.md 中写入 secrets 或真实 API key
- ❌ 禁止在 workspace 根目录存放大型二进制文件
- ❌ 禁止同一 Skill 名称出现在多个加载路径（以优先级高者为准）
- ❌ 禁止 plugin manifest 与运行时声明不一致
- ❌ 禁止 cron job 投递给未配置的 channel
- ❌ 禁止在群聊中共享子 Agent 的私人上下文
- ❌ 禁止派发需要 CEO 亲自处理的敏感任务给子 Agent
- ❌ 禁止编造 CLI 命令（必须使用 `config.*` 工具操作配置）
- ❌ 禁止在 before_prompt_build 中做非全局变更
- ❌ 禁止供应商插件替换完整提示词

---

---

## 第四章 事件驱动自动化：Hooks 标准体系

### 4.1 核心概念

**钩子（Hooks）** 是当 OpenClaw Gateway 内发生特定事件时自动运行的标准化脚本单元。
无需人工干预，事件触发即执行——这是"数字员工自动响应机制"的核心。

### 4.2 标准化事件清单

| 事件名称 | 触发时机 | 典型用途 |
|---------|---------|---------|
| `message:received` | 收到新消息时 | 预处理、分类 |
| `message:sent` | 消息发送完成后 | 发送记录 |
| `command:reset` | /reset 命令时 | 重置上下文 |
| `session:start` | 新会话启动时 | 初始化会话 |
| `session:end` | 会话结束时 | 持久化摘要 |
| `cron:triggered` | Cron Job 触发时 | 前置检查 |
| `cron:completed` | Cron Job 完成后 | 结果记录、告警 |
| `agent:spawned` | 子 Agent 创建时 | 注入父上下文 |
| `agent:yield` | 子 Agent 让出控制时 | 收集结果 |
| `tool:error` | 工具调用出错时 | 错误日志、降级 |

### 4.3 Hook 标准结构

```
hooks/
└── {hook-name}/
    ├── HOOK.md          ← Hook 定义文件（必须）
    └── hook.{sh|js}     ← 钩子脚本（必须）
```

**HOOK.md 必须字段**：
- `name`：小写字母+数字+连字符
- `triggers`：监听事件列表
- `conditions`：可选前置条件（env 变量等）
- `outputs`：返回格式（JSON schema）

### 4.4 Hook 开发黄金法则

| 法则 | 说明 |
|------|------|
| **幂等性** | 同一事件触发多次，结果必须一致 |
| **快速执行** | 秒级完成，禁止长时间任务 |
| **错误隔离** | Hook 出错不得影响主流程，必须 try/catch |
| **无副作用** | 不修改全局状态，只读事件 payload |

---

## 第五章 供应商与插件体系

### 5.1 供应商插件（Provider Plugin）标准

通过**供应商插件**以统一方式接入不同 AI 模型服务商。

**标准注册**：
```json
{
  "provider": "minimax",
  "api_key_env": "MINIMAX_API_KEY",
  "models": ["MiniMax-M2.7", "MiniMax-M2.1"]
}
```

**标准化目录**：插件通过 `catalog` 钩子，以标准格式返回支持的模型列表。

**统一命名**：`provider/model-id`（如 `minimax/MiniMax-M2.7`）。

### 5.2 插件清单规范（openclaw.plugin.json）

```json
{
  "name": "plugin-name",
  "version": "1.0.0",
  "openclaw": {
    "compat": {
      "pluginApi": "1.0.0",
      "openclawVersion": "2026.6.0"
    }
  },
  "contracts": {
    "tools": ["tool_name"],
    "skills": ["skill_name"]
  }
}
```

### 5.3 插件约束

- Plugin 必须有 `openclaw.plugin.json` manifest
- 工具必须同时在 manifest 的 `contracts.tools` 和运行时 `registerTool()` 中声明
- Plugin 版本必须声明 `openclaw.compat.pluginApi` 和 `openclaw.build.openclawVersion`
- **禁止**：plugin manifest 与运行时声明不一致

---

## 第六章 命令行与配置管理

### 6.1 标准 CLI 命令

| 命令 | 用途 | 标准化说明 |
|------|------|-----------|
| `openclaw onboard` | 标准化入职 | 向导完成初始配置 |
| `openclaw setup` | 标准化设置 | 快速创建工作区 |
| `openclaw configure` | 标准化修改 | 安全修改配置项 |
| `openclaw status` | 查看状态 | Gateway 运行状态 |
| `openclaw tasks` | 任务管理 | `issues`/`list`/`done` |
| `openclaw channels add` | 添加渠道 | 飞书/WhatsApp 等 |
| `openclaw skills list` | 查看技能 | 当前已加载技能 |
| `openclaw hooks list` | 查看钩子 | 带 verbose 查看触发日志 |

### 6.2 配置管理三工具

| 操作 | 工具 | 说明 |
|------|------|------|
| 查看配置 | `config.schema.lookup` | 查询配置项 schema |
| 增量修改 | `config.patch` | 仅变更指定字段 |
| 完整替换 | `config.apply` | 全量替换（慎用） |

### 6.3 配置文件规范

- **格式**：JSON5（支持注释）
- **位置**：`~/.openclaw/openclaw.json`
- **顶级字段**：`agents` / `channels` / `tools` / `skills` / `plugins`
- **变更原则**：变更前自动备份，支持一键回滚

---

## 第七章 工作区文件体系（岗位操作手册）

> 来源：岗位操作手册 v1.0 · "入职第一天"章节

### 7.1 核心文件速查

| 文件名 | 作用 | 必须 |
|:------|:----|:----:|
| `AGENTS.md` | 操作手册：行为准则和工作流程 | ✅ |
| `SOUL.md` | 性格与语气：人格和说话风格 | ✅ |
| `USER.md` | 用户画像：服务对象信息 | ✅ |
| `IDENTITY.md` | 身份标识：名字和表情符号 | ✅ |
| `TOOLS.md` | 工具偏好：本地工具使用建议 | 建议 |
| `MEMORY.md` | 长时记忆：个人偏好和生态状态 | 建议 |
| `BOOTSTRAP.md` | 入职引导：一次性初始化脚本 | 可选 |
| `HEARTBEAT.md` | 心跳任务：自动巡检任务 | 建议 |
| `DREAMS.md` | 梦想文件：愿景和长期目标 | 可选 |

### 7.2 Agent Loop 标准化流程

```
接收消息 → 组装上下文 → 模型推理 → 执行工具 → 流式输出 → 持久化
    ↑                                                              ↓
    ← ← ← ← ← ← ← ← ← ← ← ← ← ← ← ← ← ← ← ← ← ← ← ← ← ← ← ← ← ←
```

- **接收消息**：多渠道统一入口（webchat / 飞书 / CLI）
- **组装上下文**：系统提示词 + 会话历史 + 记忆文件
- **模型推理**：调用配置的模型（如 MiniMax-M2.7）
- **执行工具**：工具调用，结果流式返回
- **持久化**：会话历史 + 记忆更新 + 审计日志

---

*

## 第八章 运营标准速查（knowledge/system/ 文档矩阵）

> 本章将第三批次落地的 6 个运营标准文档统一索引，便于快速查阅。

| 文档 | 版本 | 核心内容 | 与 STANDARDS.md 的关系 |
|------|------|---------|----------------------|
| `system/logging.md` | v1.0.0 | JSONL日志/脱敏/可观测性/查看命令 | §18 日志标准原文 |
| `system/exec-policy.md` | v1.0.0 | 三层互锁/exec.security/Docker沙箱 | §19 执行审批原文 |
| `system/retry-cache.md` | v1.0.0 | 重试三原则/cacheRetention/故障转移 | §20-21 重试+缓存原文 |
| `system/session.md` | v1.0.0 | Session路由/维护模式/频道停靠 | §24 会话管理原文 |
| `system/performance.md` | v1.0.0 | 缓存分层/上下文裁剪/资源监控阈值 | §26 性能优化原文 |
| `system/deployment.md` | v1.0.0 | 部署时间标准/检查清单/远程访问 | §28 跨平台部署原文 |

### 8.1 日志与监控（logging.md）要点

- **双层架构**：JSONL 文件日志（持久化）+ TTY 彩色控制台（实时运维）
- **标准字段**：hostname / message / agent_id / session_id / channel / traceId
- **脱敏三配置**：`logging.redactSensitive`（off/tools）+ `logging.redactPatterns`
- **强制脱敏**：控制UI工具调用 / sessions_history 输出 / exec审批显示
- **查看命令**：`openclaw logs tail --json --local-time` / `openclaw logs follow`

### 8.2 执行审批（exec-policy.md）要点

- **三层互锁**：exec.security（策略）× exec.allowFrom（清单）× exec.ask（用户核准）
- **exec.security**：`deny` | `allowlist` | `full`
- **exec.ask**：`off` | `on-miss` | `always`
- **askFallback**：`deny` | `allowlist` | `full`
- **防漂移机制**：核准后文件变更 → 拒绝执行
- **Docker 沙箱**：per-agent sandbox + tool policy
- **查看命令**：`openclaw approvals get` / `openclaw exec-policy show`

### 8.3 重试与缓存（retry-cache.md）要点

- **重试三原则**：按请求重试 / 只重试当前步骤 / 避免非幂等操作重复
- **默认参数**：3次尝试 / 30s 最大延迟 / 10% 抖动
- **不可重试**：格式错误 / Markdown 解析错误
- **cacheRetention 三级**：agents.defaults < models覆写 < agent.list覆写（按键合并）
- **Anthropic**：`short`=5分钟 / `long`=1小时（仅 api.anthropic.com）
- **OpenAI**：近期模型自动启用，`long`=24h

### 8.4 会话管理（session.md）要点

- **Session 类型**：main / isolated / current / session:<id>
- **维护模式**：`enforce`（默认）/ `warn`
- **每日重置**：凌晨 4:00（Gateway 本地时区）
- **频道停靠**：`/dock` 跨频道路由，零上下文断裂

### 8.5 性能优化（performance.md）要点

- **四大策略**：提示词缓存 / 进程复用 / 懒加载 / 上下文裁剪
- **告警阈值**：CPU>80% / 内存>85% / 磁盘>80% / Session>200
- **裁剪模式**：`cache-ttl` / `messages` / `tokens`

### 8.6 跨平台部署（deployment.md）要点

- **当前平台**：Linux(WSL2) + Windows 宿主机
- **部署时间**：30分钟全环境 / 2小时到业务落地 / 7×24生产运行
- **性能标准**：10万+消息/日 / 可用性99%+
- **远程访问**：SSH Tunnel（推荐）/ Tailscale/VPN
- **禁止**：直接暴露 Gateway 端口

---

## 第九章 Agent 架构与执行深化（knowledge/system/ 补充）

### 8.7 新增系统文档（v1.3.x批次）

| 文档 | 版本 | 核心内容 | 与 STANDARDS.md 的关系 |
|------|------|---------|----------------------|
| `system/incident-response.md` | v1.0.0 | 故障分级/自愈机制/L1-L4降级/复盘流程 | §36 业务连续性原文 |
| `system/quality-report.md` | v1.0.0 | 周报/月报模板/黄金三角指标/生成SOP | §37 度量评估原文 |
| `system/standards-sync.md` | v1.0.0 | 人工同步流程/6步执行/冲突解决/回滚机制 | §12.10 标准同步原文 |
| `system/openclaw-user-manual.md` | v1.0.0 | 非技术用户运维指南/三句话搞定一切 | 用户层文档 |
| `system/prompt-engineering.md` | v1.0.0 | 三层架构/缓存边界/23模块/安全护栏/迭代评估 | 提示词工程标准 |
| `system/agent-full-standard.md` | v1.0.0 | 15Agent深度标准化/工具权限矩阵/协作模式 | 组织治理原文 |
| `system/tools-matrix.md` | v1.0.0 | 15Agent工具权限设计/受保护工具路径/配置规范 | 工具权限设计文档 |
| `knowledge/index.md` | v2.5.0 | 知识库索引更新，新增8个文档条目 | 知识库入口 |

> 本章在第八章运营标准速查基础上，补充 Agent 核心架构深度内容。

| 文档 | 版本 | 核心内容 | 与 STANDARDS.md 的关系 |
|------|------|---------|----------------------|
| `system/agent-loop.md` | v1.0.0 | 三层架构/七阶段循环/runAgentStep契约/队列并发 | §31-32 Agent Loop 原文 |
| `system/skill-dev.md` | v1.0.0 | SKILL.md 完整字段/验证流程/开发黄金法则 | §33 Skill 深化原文 |
| `system/workflow.md` | v1.0.0 | TaskFlow/Lobster/事件驱动编排 | §34 工作流编排原文 |
| `system/delegate-agent.md` | v1.0.0 | 三层能力/硬性封堵 S1-S4/多Agent路由 | §35 委派代理原文 |
| `system/gateway-arch.md` | v1.0.0 | 单一多路复用端口/WS协议/Node能力/配对标准 | §38 Gateway 架构原文 |

### 9.1 Agent Loop 七阶段（agent-loop.md）要点

- **三层架构**：渠道层（Channel）× 大脑层（Brain）× 身体层（Body）
- **Gateway 约束**：只做路由/认证/会话管理，**绝不直接接触模型**
- **七阶段**：接收 → 上下文组装 → 提示词组装 → 模型推理 → 工具执行 → 流式回复 → 持久化
- **runAgentStep**：幂等性保障 + 超时控制 + 上下文溯源
- **readLatestAssistantReply**：逆序查找纯文本回复，stripToolMessages 过滤工具消息
- **修剪 vs 压缩**：修剪裁工具结果（每次 LLM 调用前），压缩重写对话（窗口上限时）

### 9.2 Skill 开发（skill-dev.md）要点

- **完整 YAML 字段**：name/description/version/author/tags/user-invocable + metadata.openclaw.requires
- **加载优先级**：workspace > project > 个人 > 全局 > 内置
- **验证清单**：目录结构 → 元数据 → 依赖 → 集成测试 → 发布注册
- **黄金法则**：可发现性 / 幂等性 / 最小依赖 / 错误隔离 / 版本语义（SemVer）

### 9.3 工作流编排（workflow.md）要点

- **Task Flow**：持久化多步骤流程，状态+修订追踪
- **Lobster**：进程内执行（不生成外部 CLI），单个确定性操作
- **事件驱动**：事件拆解 + 事件网关 + 补偿事务
- **Skill vs 工作流**：Skill 无状态单能力，工作流有状态多步骤有审批点

### 9.4 委派代理（delegate-agent.md）要点

- **三层能力**：只读+草稿（L1）→ 代表发送（L2）→ 主动执行（L3）
- **凭证差异**：个人模式用你的凭证，委派代理拥有自己的凭证
- **硬性封堵**：S1 禁止未核准外发邮件 / S2 禁止导出敏感数据 / S3 禁止执行入站命令 / S4 禁止修改 IdP 设置
- **多 Agent 路由**：Workspace/配置文件/凭证/会话存储全隔离

### 9.5 Gateway 架构（gateway-arch.md）要点

- **单一多路复用端口**：WS控制 + HTTP（/v1/models, /v1/chat/completions, /tools/invoke 等）
- **WS 消息格式**：`{type:"req/res/event", id, method, payload}`
- **认证三模式**：共享密钥 / Tailscale Serve / 可信代理（**none 禁止公共入口**）
- **配对标准**：本地回环自动批准，Tailnet/LAN 需显式批准
- **Node 能力**：canvas.* / camera.* / screen.record / location.get

---

## 第十章 CLI 命令完整参考

> 整合第三/四批次新增的 CLI 命令标准。

### 10.1 核心命令分类

| 类别 | 命令 | 用途 |
|------|------|------|
| **设置** | `openclaw setup` | 建立基准配置与工作区 |
| **引导** | `openclaw onboard` | 完整引导式首次执行（Gateway/模型/渠道/Skills/健康） |
| **配置** | `openclaw configure` | 变更既有配置的指定部分 |
| **配置设置** | `openclaw config set <path> <value> --merge` | 增量修改（默认） |
| **配置替换** | `openclaw config set <path> <value> --replace` | 完整替换（需显式） |
| **状态** | `openclaw status` | Gateway/Agent 状态 |
| **诊断** | `openclaw doctor` | 诊断并修复配置问题 |
| **网关** | `openclaw gateway status` | Gateway 服务健康检查 |
| **内存** | `openclaw memory index --force` | 强制重建记忆索引 |
| **安全审计** | `openclaw security audit` | 常规安全检查 |
| **深度审计** | `openclaw security audit --deep` | 实时探测，模拟攻击者 |
| **任务** | `openclaw tasks` | 检查个体任务记录 |
| **工作流** | `openclaw tasks flow` | 检查编排流程状态 |
| **日志跟踪** | `openclaw logs tail --json --local-time` | 实时 JSONL 跟踪 |
| **日志跟随** | `openclaw logs follow` | systemd journal 跟踪（Linux） |
| **技能列表** | `openclaw skills list` | 当前已加载技能 |
| **钩子列表** | `openclaw hooks list --verbose` | 钩子状态+触发日志 |
| **审批查看** | `openclaw approvals get` | 查看政策来源及有效结果 |
| **策略查看** | `openclaw exec-policy show` | 本机合并视图 |
| **策略设置** | `openclaw exec-policy set/preset` | 同步本机政策与核准文件 |

### 10.2 全局标志

| 标志 | 用途 |
|------|------|
| `--dev` | 状态隔离在 `~/.openclaw-dev` |
| `--profile <name>` | 状态隔离在 `~/.openclaw-<name>` |
| `--json` | JSON 格式输出 |
| `--plain` | 纯文本输出 |
| `--no-color` | 禁用 ANSI 颜色 |
| `--url` | 显式 Gateway URL |
| `--merge` | 合并而非替换（默认） |
| `--replace` | 替换而非合并 |

### 10.3 斜线命令（Chat 内）

| 命令 | 用途 |
|------|------|
| `/status` | 快速诊断 |
| `/trace` | 会话范围追踪/调试 |
| `/config` | 持久化配置变更 |
| `/debug` | 运行时覆写（内存中，不写磁盘；需 `commands.debug: true`） |
| `/dock` | 将当前会话回复路由到另一关联渠道 |

---

## 第十一章 记忆与运维标准（knowledge/system/ 补充）

| 文档 | 版本 | 核心内容 | 与 STANDARDS.md 的关系 |
|------|------|---------|----------------------|
| `system/memory-system.md` | v1.0.0 | 三层记忆（内置/QMD/Supermemory）/梦境三阶段（Light/Deep/REM）/六维评分 | §43-44 原文 |
| `system/rag-knowledge.md` | v1.0.0 | 四层配置合并/混合检索（70%向量+30%全文）/多模态RAG | §45 原文 |
| `system/auto-ops.md` | v1.0.0 | 三层运维体系/ManagedProcess/快检+深检巡检模式 | §46 原文 |
| `system/sop-workflow.md` | v1.0.0 | AGENTS.md六段骨架/12套模板/SOP五大核心收益 | §47 原文 |
| `system/subagent-task.md` | v1.0.0 | 子代理生命周期/sessions_spawn+sessions_yield/并行执行 | §48 原文 |
| `system/project-management.md` | v1.0.0 | TaskFlow托管+镜像/看板/MissionControl五阶段 | §49 原文 |
| `system/delivery-quality.md` | v1.0.0 | 富输出协议/黄金三角/信通院基线/安全审计六维度 | §50-52 原文 |

### 11.1 记忆三层架构（memory-system.md）要点

- **三层引擎**：内置（SQLite+FTS5）× QMD（BM25+向量+重排序）× Supermemory（原子事实）
- **互斥约束**：Supermemory 与内置/QMD 互斥，同一Agent只能选一种
- **梦境三阶段**：Light（不写，候选清单）→ REM（不写，模式反思）→ Deep（**写入MEMORY.md**，三重门槛筛选）
- **六维评分**：相关性30% / 频率24% / 查询多样性15% / 时效性15% / 整合度10% / 保留6%
- **转录摄取**：敏感内容摄取前先遮蔽

### 11.2 RAG 知识库（rag-knowledge.md）要点

- **四层配置**：L1默认 < L2全局 < L3 Agent级 < L4运行时覆盖（按键合并）
- **混合检索**：默认70%向量 + 30%全文
- **路径占位符**：`{agentId}` / `{sessionKey}` 实现多租户隔离
- **参数校验**：vectorWeight/minSimilarity clamp到[0,1]，maxResults clamp到[1,100]

### 11.3 自动运维（auto-ops.md）要点

- **三层体系**：数据采集层（SSH/SNMP）× 阈值判断层（动态基线）× 告警处理层（消息队列）
- **巡检模式**：先快检输出简洁摘要，异常再深检扩展多维度
- **ManagedProcess**：cwd必须在allowedPaths内 / 就绪≠启动成功（端口就绪才running）/ autoRestart布尔

### 11.4 SOP与工作流（sop-workflow.md）要点

- **六段骨架**：任务识别 → 默认流程 → 交付标准 → 风格约束 → 风险动作确认 → 兜底策略
- **安全审计教训**："意图误解"通过率0% → **模糊指令必须停下来确认，不瞎编**
- **12套模板**：公众号写作/教程改稿/排错助手/编程教学/飞书自动化等

### 11.5 子代理与任务分解（subagent-task.md）要点

- **sessions_spawn**：立即返回runId（非阻塞），独立会话运行
- **sessions_yield**：需要子任务结果时调用，挂起等待
- **约束**：子代理不获取会话工具 / 不获得消息工具（返回纯文本给父Agent）

### 11.6 项目管理（project-management.md）要点

- **TaskFlow**：托管模式（端到端生命周期）× 镜像模式（观察外部任务）
- **Workboard**：不是Jira/Linear替代品，追踪Gateway本地操作工作
- **MissionControl**：五阶段看板（Inbox→Assigned→In Progress→Review→Done）

### 11.7 交付物与质量（delivery-quality.md）要点

- **富输出协议**：MEDIA: / [[audio_as_voice]] / [[reply_to:xxx]] / [embed]（仅webchat）
- **黄金三角**：Accuracy × Speed × Cost
- **信通院三维度**：业务质量 / 权益保障 / 安全防护
- **六风险维度**：意图误解通过率0%为关键警示

### 11.8 交付物归档（delivery-quality.md §3）要点

- **六步工作流最后一步**：输入 → 拆解 → 工具调用 → 生成 → 校验 → **归档**
- **归档四要素**：保存路径 / 命名规则（YYYY-MM-DD_任务名称.ext）/ 版本管理 / 索引记录


*v2.4.1 · King · 2026-06-30 · 深度标准化：147项检查修复 + 常设指令 + 安全强化*

---

## 第十二章 组织治理与生态标准

> 本章整合第三轮补充文档（54-66章），覆盖组织级治理、知识库、定时任务、合规审计、业务连续性、度量评估、Workspace Agent管理、版本管理、变更管理及标准下发同步。

### 12.1 组织架构（四层治理模型）

| 层级 | 角色 | 职责 | 标准化文件 |
|------|------|------|-----------|
| **决策层** | King · CEO | 对外唯一入口、决策中枢、记忆引擎、Cron Job编排（9个活跃任务） | `IDENTITY.md` v3.0.0 |
| **执行层-运营域** | COO + PM/QM/RS/SA | 运营任务分解、进度追踪、质量指标统计、报告生成、销售执行与客户跟进 | `AGENTS.md` + `SOUL.md` |
| **执行层-技术域** | CTO + PE/HE/SE | 平台架构、基础设施、前端开发、界面实现、系统工程、运维自动化 | `AGENTS.md` + `TOOLS.md` |
| **合规域** | CA | 安全体系、审计日志、数据隐私、MITRE ATLAS框架、风险评估 | `knowledge/system/` 7份合规文件 |

**标准化约束**：每个Agent必须包含完整的标准化文件集（AGENTS.md / SOUL.md / STANDARDS.md / HEARTBEAT.md / TOOLS.md / IDENTITY.md / MEMORY.md / USER.md），版本号与全局STANDARDS.md保持同步。

### 12.2 Agent间协作契约

**四种协作模式**：

| 模式 | 触发条件 | 行为规范 |
|------|---------|---------|
| **委派模式** | 任务超出当前Agent能力范围 | 调用`sessions_spawn`创建子Agent |
| **接力模式** | 任务需要多Agent顺序处理 | 完成自身部分后调用`sessions_yield` |
| **并行模式** | 任务可拆分为独立子任务 | 同时创建多个子Agent |
| **共识模式** | 关键决策需要多方确认 | 收集多Agent意见后汇总，由King最终决策 |

**标准化协作协议**：
1. 所有跨Agent通信**必须通过King CEO中转**，禁止Agent间直接通信
2. 子Agent完成工作后必须返回**标准化结果报告**（状态/数据/证据/下一步建议）
3. 协作过程中产生的中间产物必须归档到共享知识库
4. 协作失败时必须记录完整的错误上下文并上报King

### 12.3 知识库标准化治理

**知识库目录结构**：

```
knowledge/
├── system/              # 系统级知识（23份文档，CEO维护，全部Agent只读）
├── agents/              # Agent级知识（各Agent维护，可引用不可修改）
├── projects/            # 项目级知识（按项目隔离）
└── shared/              # 跨Agent共享（templates/checklists/glossaries）
```

**知识文档标准化字段**（每份文档必须包含）：

```yaml
---
title: "文档标题"
version: "语义化版本号"
status: "draft | review | approved | deprecated"
owner: "负责Agent"
last_reviewed: "YYYY-MM-DD"
next_review: "YYYY-MM-DD"
tags: ["标签1", "标签2"]
related: ["关联文档路径"]
---
```

**标准化同步触发条件**：
- 知识文件变更 → 防抖1.5秒 → 自动重新索引
- 每日06:00 → MyBrain知识库同步
- 每周日22:00 → 周度知识审计
- 月末21:00 → 月度全面审计

### 12.4 定时任务（Cron Job）标准化

**9个核心Cron Job**：

| Job | 周期 | 功能 | 状态 |
|-----|------|------|------|
| `cron_self_health` | 每6h | CEO自我健康检查 | ✅ |
| `session_cleanup` | 每日02:00 | 会话清理 | ✅ |
| `knowledge_backup` | 每日03:00 | 知识库备份（保留30天） | ✅ |
| `daily_mybrain_sync` | 每日06:00 | MyBrain知识库同步 | ✅ |
| `sys_health_daily` | 每日09:00 | 系统健康巡检 | ✅ |
| `daily_report_weekday` | 工作日08:30 | 日报生成 | ✅ |
| `weekly_quality_check` | 周日21:00 | 周度质量检查 | ✅ |
| `weekly_knowledge_audit` | 周日22:00 | 周度知识审计 | ✅ |
| `monthly_full_audit` | 月末21:00 | 月度全面审计 | ✅ |

**任务编排模式**：
- **串行模式**（有依赖）：`session_cleanup → knowledge_backup → daily_mybrain_sync → sys_health_daily`
- **并行模式**（无依赖）：`weekly_quality_check` 和 `weekly_knowledge_audit` 独立运行

**监控告警阈值**：

| 指标 | 告警阈值 | 级别 |
|------|---------|------|
| 任务执行超时 | >超时阈值的80% | P2 |
| 任务执行失败 | 连续失败≥2次 | P1 |
| 任务堆积 | 同一任务并发数>1 | P2 |

### 12.5 合规与审计

**CA管辖7份合规文件**（位于 `agents/ca/knowledge/system/`）：

| 文件 | 核心内容 |
|------|---------|
| `security-policy.md` | 安全策略/访问控制/加密标准/事件响应/漏洞管理 |
| `audit-logging.md` | 日志格式/保留策略/查询接口/审计报告模板 |
| `data-privacy.md` | 数据分类/脱敏规则/跨境传输/用户权利 |
| `mitre-atlas.md` | 威胁模型/攻击向量/缓解措施/演练计划 |
| `risk-assessment.md` | 风险登记/风险评分/风险处置/风险报告 |
| `compliance-check.md` | 检查项清单/检查频率/整改流程/验证方法 |
| `security-training.md` | 培训计划/培训材料/考核标准/培训记录 |

**审计周期**：

| 审计类型 | 执行者 | 周期 | 输出 |
|---------|--------|------|------|
| 日常审计 | 系统自动 | 实时 | 审计日志 |
| 每日审计 | CA Agent | 每日 | 每日审计摘要 |
| 每周审计 | CA + QM | 周日22:00 | 周度审计报告 |
| 月度审计 | CA + King | 月末21:00 | 月度全面审计报告 |

### 12.6 业务连续性

**可自愈故障类型**：

| 故障类型 | 自愈动作 | 恢复时间目标 |
|---------|---------|-------------|
| 进程崩溃 | 自动重启（autoRestart） | <30秒 |
| 内存溢出 | 重启+内存限制调整 | <60秒 |
| 网络中断 | 自动重连（指数退避） | <120秒 |
| 配置错误 | 自动回滚上一版本 | <60秒 |
| 子Agent无响应 | 强制终止+重新创建 | <30秒 |

**不可自愈故障**：立即上报King → 执行降级方案 → 记录上下文 → 24小时内根因分析。

**降级方案**：

| 等级 | 动作 | 用户体验影响 |
|------|------|------------|
| L1-功能降级 | 禁用非核心功能（如梦境） | 核心功能正常 |
| L2-性能降级 | 降低并发数、增加超时 | 响应变慢 |
| L3-服务降级 | 切换只读模式 | 仅可查询不可操作 |
| L4-完全降级 | 停止服务 | 完全不可用 |

### 12.7 度量与评估

**组织级度量指标体系**：

| 度量维度 | 核心指标 | 目标值 | 负责Agent |
|---------|---------|--------|----------|
| **效率** | 任务平均完成时间 | ≤5分钟 | PM |
| **质量** | 交付物准确率 | ≥95% | QM |
| **安全** | 安全事件数 | 0/月 | CA |
| **合规** | 合规检查通过率 | 100% | CA |
| **记忆** | 记忆检索准确率 | ≥90% | King |
| **自动化** | 自动化覆盖率 | ≥80% | SE |

**质量评估黄金三角**：成功率（准确性）× 执行速度（效率）× 资源消耗（成本）。

### 12.8 Workspace Agent管理

**4个Workspace Agent**（位于 `workspace/agents/`）：

| Agent | 职责 | 触发方式 |
|-------|------|---------|
| `sales_executive` | 销售执行（对话/跟进/客户沟通） | 用户手动/King委派 |
| `sales_operator` | 销售运营（数据/流程/运营分析） | King委派/Cron触发 |
| `sales_orchestrator` | 销售编排（协调/调度/资源分配） | King委派 |
| `translator_zh` | 中英双向翻译 | 用户手动触发 |

**生命周期标准化**：`创建 → 配置 → 启用 → 运行 → 监控 → 停用 → 归档`

### 12.9 版本与变更管理

**语义化版本规范**：

| 组件 | 当前版本 | 变更规则 |
|------|---------|---------|
| STANDARDS.md | v2.4.1 | Major=不兼容架构变更 / Minor=兼容功能新增 / Patch=兼容Bug修复 |
| SOUL.md | v2.3.0 | 同上 |
| AGENTS.md | v2.3.0 | 同上 |

**变更审批矩阵**：

| 变更类型 | 审批人 | 通知范围 |
|---------|--------|---------|
| STANDARDS.md主版本 | King + 全体验收 | 全部15个Agent |
| STANDARDS.md次版本 | King | 全部11个核心Agent |
| STANDARDS.md补丁 | 无需审批 | 受影响Agent |
| Agent级文件变更 | 对应Agent负责人 | 关联Agent |

### 12.10 标准下发同步机制

**背景**：`openclaw standards sync` 命令当前不存在，采用**人工SOP + Cron Job触发**的替代方案。

**标准文件层级**：

| 层级 | 文件 | 同步策略 |
|------|------|---------|
| L0-总纲 | `STANDARDS.md` | 强制推送，变更后立即全量同步至全部15个Agent |
| L1-核心人格 | `SOUL.md` | 强制推送，变更后立即全量同步至全部11个核心Agent |
| L2-操作手册 | `AGENTS.md` | 智能合并，保留Agent特有内容 |
| L3-专项配置 | `HEARTBEAT.md`/`TOOLS.md`/`IDENTITY.md` | 按需同步 |

**同步执行流程（6步）**：
1. **变更检测**：Cron Job `daily_standards_sync`（每日06:30）检查文件时间戳差异
2. **版本升级**：更新目标版本号，生成变更摘要
3. **影响评估**：分析变更影响范围，确定同步Agent列表
4. **审批**：根据变更类型（12.9审批矩阵）获取必要审批
5. **执行同步**：并行推送至所有目标Agent（每Agent先备份）
6. **验证+通知**：验证文件完整性，发送变更通知

**冲突解决策略**：
- 优先级：**Agent特有内容 > 通用更新**（保留差异化）
- 冲突标记：`# <<<<<<< STANDARDS` / `# =======` / `# >>>>>>> AGENT_SPECIFIC`
- 冲突报告：发送给King + 对应Agent负责人

**回滚机制**：
- 触发条件：同步后Agent响应异常 / 验证失败超过3次 / 管理员手动触发
- 执行：定位备份副本 → 恢复文件 → 验证恢复 → 记录回滚日志
- 回滚命令：`openclaw config apply <备份配置文件路径>`

**完整同步SOP**：详见 `knowledge/system/standards-sync.md`

*v2.4.1 · King · 2026-06-30 · 第十二章：组织治理与生态标准（新增）*

---

## 第十三章 工作流与安全深度标准化（第六轮补充）

> 来源：第六轮全角色标准化深度补充（2026-06-30）
> 本章为268项检查清单的P1-P33扩展，共33项新增检查项

### 13.1 工作流成熟度等级

| 等级 | 特征 | 你的现状 |
|------|------|---------|
| 第一阶段 | 部署定时任务 + 简单API调用 | ✅ 19个Cron Job |
| 第二阶段 | 引入多Agent协作 + Task Flow | ⬜ 通道合约已完成，TaskFlow待配置 |
| 第三阶段 | 构建完整工作流 + 闭环自优化 | ⬜ 待启动 |

### 13.2 通道合约已完成确认

| Agent | 通道合约注入 | 行数 |
|-------|------------|------|
| CEO/COO/CTO/CA | ✅ 已注入 | 191/147/145/167 |
| PM/QM/RS/SA | ✅ 已注入 | 148/152/151/152 |
| PE/HE/SE | ✅ 已注入 | 162/161/162 |

### 13.3 Task Flow部署状态

| Task Flow | 模式 | 状态 |
|-----------|------|------|
| 晨间运营简报 | 托管模式 | ⬜ 待配置 |
| 周度质量闭环 | 托管模式 | ⬜ 待配置 |
| 知识库同步链 | 托管模式 | ✅ knowledge_backup→sync→audit |
| 月度全面审计 | 托管模式 | ⬜ 待配置 |
| 标准下发同步 | 托管模式 | ✅ daily_standards_sync |

### 13.4 安全审计结果（2026-06-30执行）

```bash
openclaw security audit
# 结果：0 critical · 3 warn · 1 info
```

**告警处置**：

| checkId | 警告 | 处置结论 |
|---------|------|---------|
| `gateway.trusted_proxies_missing` | 反向代理头未信任 | ✅ 无风险，Gateway绑定127.0.0.1 |
| `plugins.installs_unpinned_npm_specs` | feishu/voice-call未固定版本 | ⚠️ 需npm访问修复，社区插件限制 |
| `channels.feishu.doc_owner_open_id` | 飞书文档创建可授权请求者权限 | ⚠️ 业务需要，已知风险 |

**已修复**：
- ✅ `gateway.nodes.allow_commands_dangerous`：camera.snap/screen.record/camera.clip 已移除
- 修复前：4 WARN；修复后：3 WARN

### 13.5 P1-P33检查清单状态

| 状态 | 数量 | 说明 |
|------|------|------|
| ✅ 已完成 | 7 | 通道合约、安全守则、凭证管理、本地明文禁止、HITL、安全组、调试日志 |
| ⬜ 待配置 | 19 | TaskFlow、队列优先级、Skills扫描、CI/CD、监控等 |
| ⠐ 部分覆盖 | 6 | 预检清单、凭据销毁、DLP、Tools Call检测、401排查、网络SOP |
| ⚠️ 已知风险 | 1 | npm spec固定（社区插件限制） |

### 13.6 架构设计哲学

**Gateway-First核心原则**：
- 单一多路复用端口（18789）
- 编排层与模型层分离
- 会话序列化执行（命令队列限制全局并行度）
- 状态持久化（Task Flow重启后进度可恢复）

**1+N微服务架构**：

| 服务 | 职责 |
|------|------|
| Agent Runtime Core | 主智能体运行时 |
| Memory Sub-Agent | 记忆管理 |
| Command Discovery Engine | 命令发现 |
| Skill Registry | 技能注册中心 |

### 13.7 平行专家工作线三阶段

| 阶段 | 内容 | 状态 |
|------|------|------|
| 第一阶段 | 通道合约 + 背景重型工作 | ✅ 已完成 |
| 第二阶段 | 优先级与并发控制 | ⬜ 待配置 |
| 第三阶段 | 协调器/流量控制器 | ⬜ 待配置 |

### 13.8 五级风险防护架构

| 级别 | 根因 | 防护措施 | 状态 |
|------|------|---------|------|
| 第一层 | 高权限 | Gateway绑定127.0.0.1 | ✅ |
| 第二层 | 弱认证 | 共享密钥+配对机制 | ✅ |
| 第三层 | 明文凭据 | 环境变量注入+自动脱敏 | ✅ |
| 第四层 | 运行失控 | exec.ask+exec.security | ✅ |
| 第五层 | 缺少溯源 | 双层日志+审计报告 | ✅ |

### 13.9 12套高频工作流模板

| 模板 | 场景 | 核心原则 |
|------|------|---------|
| 模板1 | 公众号写作 | 提纲→正文≤5行→标题/封面/配图/转发文案 |
| 模板2 | 教程改稿增强 | 保留结构→表达增强→改动说明+改后全文 |
| 模板3 | OpenClaw排错助手 | 报错→排查树→预期现象→防复发清单 |
| 模板4 | AI编程教学助手 | 术语→生活类比→先讲为什么→可复制命令 |
| 模板5 | 飞书自动化助手 | 确认目标→流程图（触发器→动作→结果） |

### 13.10 SOUL.md人格工程标准

**应放内容**：语气、观点、简洁程度、幽默、边界、默认直率程度

**不应放内容**：人生故事、Changelog堆砌、安全策略堆砌、巨大氛围文字墙

**好的规则 vs 差的规则**：

| 好的规则 | 差的规则 |
|---------|---------|
| "有明确观点" | "始终保持专业" |
| "跳过填充话" | "提供全面且周到的协助" |
| "合适的时候幽默" | "确保积极且支持性的体验" |
| "尽早指出糟糕想法" | — |

**核心原则**：短胜过长。鲜明胜过含糊。有个性不等于可以草率。

*v2.4.2 · King · 2026-06-30 16:11 · 第十三章（通道合约×11已落地）：工作流与安全深度标准化（第六轮补充）*
