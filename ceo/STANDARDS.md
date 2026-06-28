# OpenClaw 生态标准化总纲

> 版本：v1.1.0
> 建立：2026-06-27
> 更新：2026-06-27
> 依据：docs.openclaw.ai + github.com/openclaw 官方规范

---

## 1. 规范依据

| 规范来源 | 版本 | 关键内容 |
|---------|------|---------|
| OpenClaw 官方文档 | 2026.6 | Tools/Skills/Plugins/Agent 系统完整规范 |
| ClawHub skill-format | main | SKILL.md frontmatter + body 标准 |
| claw-mem releases | v6.13+ | TypeScript-only 项目标准化（参考） |
| openclaw.plugin.json | manifest v1 | Plugin 声明式规范 |
| ACP (Agent Client Protocol) | current | Session/Agent 间通信协议 |

---

## 2. 核心原则

### 2.1 标准化优先
所有新建文件、目录、技能必须符合官方标准。存量文件逐步改造。

### 2.2 SKILL.md 是技能的唯一入口
- 每个 Skill 是包含 `SKILL.md` 的目录
- Frontmatter 用 YAML，body 用 Markdown
- 禁止在 body 中嵌入 secrets 或真实 API key
- 禁止在 body 中写入过多技术细节——SKILL.md 是给 AI 读的指令，不是给人类的手册

### 2.3 技能加载优先级（高→低）
```
1. <workspace>/skills/               ← Agent 工作区技能（最高）
2. <workspace>/.agents/skills/       ← 项目 Agent 技能
3. ~/.agents/skills/                 ← 全局个人 Agent 技能
4. ~/.openclaw/skills/               ← 全局托管技能
5. 打包/插件技能                      ← 最低
```

### 2.4 Plugin 声明式规范
- 所有 Plugin 必须有 `openclaw.plugin.json` manifest
- 工具必须同时在 manifest 的 `contracts.tools` 和运行时 `registerTool()` 中声明
- Plugin 版本必须声明 `openclaw.compat.pluginApi` 和 `openclaw.build.openclawVersion`

### 2.5 Agent 工作区标准化结构
```
AGENTS.md     ← 工作区说明（必须）
SOUL.md       ← 人格定义（必须）
IDENTITY.md   ← 身份卡（必须）
USER.md       ← 用户信息（必须）
MEMORY.md     ← 长时记忆（建议）
TOOLS.md      ← 本地工具备注（可选）
HEARTBEAT.md  ← 心跳任务（可选）
memory/       ← 日志记忆目录
  └── YYYY-MM-DD.md
knowledge/    ← 知识库目录
  ├── index.md
  ├── system/
  │   ├── agents.md
  │   ├── cron.md
  │   └── channels.md
  ├── sandbox/
  └── memory/
skills/       ← 技能目录（创建 SKILL.md）
DREAMS.md     ← 梦想/愿景文件（可选）
```

---

## 3. 标准化领域

### 3.1 Skill 技能标准

**SKILL.md Frontmatter 规范**

```yaml
---
name: skill-name                  # 必须：小写字母+数字+连字符，全局唯一
description: 简短描述（≤160字）   # 必须：单行，说明功能
version: 1.0.0                    # 建议标注
metadata:
  openclaw:
    requires:                     # 可选：依赖声明
      env:                        # 需要的 Env 变量
        - MY_API_KEY
      bins:                       # 需要的命令行工具
        - git
        - curl
    always: false                 # 可选：true=无条件激活
---
```

**SKILL.md body 规范**
- 用 Markdown 格式书写
- 包含## 章节结构（背景、流程、规范、禁止等）
- 路径用 `<workspace>`、`<agent>` 等变量，不硬编码
- 示例代码用标准工具调用格式

**有效 Skill 示例**
```
skills/
├── memory-manager/
│   └── SKILL.md        ← name: memory-manager
├── cron-manager/
│   └── SKILL.md        ← name: cron-manager
└── knowledge-manager/
    └── SKILL.md        ← name: knowledge-manager
```

### 3.2 Cron Job 命名标准（v2.0.0）

```
{domain}_{action}_{frequency}
  domain:     sys | knowledge | agent | channel | quality | memory
  action:     health | backup | sync | cleanup | check | audit | report | dreaming
  frequency:  daily | hourly | weekly | monthly

示例：
sys_health_daily          ✅ 标准
knowledge_backup_daily    ✅ 标准
sys_session_cleanup_daily ✅ 标准
quality_weekly_check      ✅ 标准
memory_dreaming_promotion 🔒 插件托管，不可改名
session_cleanup           ⚠️ 历史遗留，暂保留
```

**状态标识**
| 标识 | 含义 |
|------|------|
| ✅ | 符合 v2.0.0 规范 |
| ⚠️ | 历史遗留，暂不强制改名（避免丢失运行历史） |
| 🔒 | 插件托管，名称由插件控制，不可手动修改 |

### 3.3 知识库目录标准

```
knowledge/
├── index.md              ← 知识库总索引（必须）
├── system/               ← 系统文档（必须）
│   ├── agents.md         ← 智能体架构
│   ├── cron.md           ← Cron Job 清单
│   └── channels.md       ← 渠道配置
├── memory/               ← 临时记忆引用
├── sandbox/              ← 沙盒验证区
└── projects/             ← 项目文档（可选）
    └── {project}/
```

**index.md 规范**
- 包含目录结构树
- 包含变更日志
- 包含系统文档链接

### 3.4 ClawHub 发布标准
- 包作用域必须与发布者 handle 完全匹配
- 包 slug 必须小写、npm-safe
- MIT-0 许可证（ClawHub 强制）
- SKILL.md frontmatter `name` 即为包名
- 发布命令：`clawhub skill publish` 或 `clawhub package publish`

### 3.5 Agent 命名规范
- ID 使用小写字母 + 数字 + 下划线
- 全局唯一，不与其他 Agent 冲突
- 特殊保留：`ceo` / `coo` / `cto` / `ca` / `main`

---

## 4. 禁止事项

- ❌ 禁止在 SKILL.md 中写入 secrets 或真实 API key
- ❌ 禁止在 workspace 根目录存放大型二进制文件
- ❌ 禁止同一 Skill 名称出现在多个加载路径（以优先级高者为准）
- ❌ 禁止 plugin manifest 与运行时声明不一致
- ❌ 禁止 cron job 投递给未配置的 channel
- ❌ 禁止在群聊中共享子 Agent 的私人上下文
- ❌ 禁止派发需要 CEO 亲自处理的敏感任务给子 Agent

---

## 5. 落地清单

| 编号 | 任务 | 状态 | 备注 |
|------|------|------|------|
| STD-001 | CEO/skills/ 目录建立 + 核心 Skill 创建 | ✅ 完成 | 5个核心 Skill |
| STD-002 | 所有 Agent 工作区标准化结构改造 | ✅ 完成 | 21/21 通过审计 |
| STD-003 | Cron Job 命名体系规范化 | ✅ 完成 | v2.0.0 命名规范生效 |
| STD-004 | 知识库目录标准化 | ✅ 完成 | index/system 目录建立 |
| STD-005 | Skill Workshop 集成 | ✅ 可用 | skill_workshop 工具已就绪 |
| STD-006 | ClawHub 发布工作流 | ⬜ 待建设 | 发布命令已知，待落地 |
| STD-007 | 外部工具（Codex++/Hermes/豆包）集成规范 | ✅ 完成 | 已写入 agents.md |
| STD-008 | VP1/VP2 撤销与归档 | ✅ 完成 | 2026-06-27 执行 |

---

## 6. 系统概念（OpenClaw 核心）

### 6.1 核心实体关系
```
Agent（智能体）
├─ 拥有独立工作区（workspace）
├─ 持有 Skill（技能）
├─ 运行在 Session（会话）中
└─ 通过 Tool（工具）操作

Session（会话）
├─ 绑定一个 Agent
├─ 分 main / isolated / current 类型
└─ 可通过 sessions_send 跨 Agent 通信

Skill（技能）
├─ 以 SKILL.md 为入口
├─ 属于某个 Agent 的工作区
└─ 被 Agent 加载后生效

Tool（工具）
├─ 系统内置或 Plugin 提供
├─ 通过 manifest 声明
└─ Agent 调用时不感知实现细节

Plugin（插件）
├─ 含 openclaw.plugin.json manifest
└─ 打包工具+技能+配置
```

### 6.2 Session 类型说明
| 类型 | 说明 | 适用场景 |
|------|------|---------|
| main | 主会话，持久化 | CEO 唯一入口 |
| isolated | 临时会话，自动化任务 | Cron Job 子任务 |
| current | 当前会话绑定 | 临时后台任务 |

### 6.3 跨 Agent 通信
- `sessions_spawn`：派发独立子任务
- `sessions_send`：向指定 Agent 发送消息
- `sessions_yield`：等待子 Agent 完成
- 禁止在群聊中暴露子 Agent 私人上下文

---

## 7. 外部工具集成

| 工具 | 定位 | 接入方式 |
|------|------|---------|
| 豆包 | Windows 桌面 AI，快速响应前台 | 桌面集成 |
| Hermes | 本地大模型，深度分析智囊 | SSH→java -jar |
| Codex++ | 开发者编程工具，代码执行 | SSH→codex_task.bat |

**Codex++ 调用规范**
```bash
ssh -i ~/.ssh/id_ed25519 Administrator@172.21.112.1 "cmd /c codex_task.bat \"prompt\""
# 结果文件：C:\Users\Administrator\codex_result.txt
```

---

*King · 2026-06-27 · 全面标准化 v1.1.0*