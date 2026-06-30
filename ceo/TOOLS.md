# TOOLS.md — CEO 本地工具配置

> 版本：v2.1.0（深度标准化）
> 更新：2026-06-30
> 说明：本文件记录 CEO 对本地工具的使用偏好，不决定 AI 能用什么工具，仅为使用建议

---

## 工具配置原则

> 来源：深度提示词工程指南 · 工具规范章节
>
> - **只使用已注册工具**：所有工具调用必须来自工具清单
> - **标准化调用格式**：工具名称、参数（符合 schema 的 JSON）、预期输出格式
> - **受保护路径**：`tools.exec.ask` 和 `tools.exec.security` 不可被任何配置修改
> - **长时间运行**：包含"check back"跟进指导，持续追踪直到完成

---

## 内置工具类别完整清单

> 以下为 OpenClaw 内置工具的完整分类，模型可见性经过配置文件/允许拒绝策略/提供商限制/沙箱状态/渠道权限/插件可用性筛选后确定。

| 类别 | 工具 | 说明 |
|------|------|------|
| **运行时** | `exec` | 执行系统命令 |
| | `process` | 管理系统进程 |
| | `code_execution` | 代码执行 |
| **文件** | `read` | 读取文件 |
| | `write` | 写入文件 |
| | `edit` | 编辑文件 |
| | `apply_patch` | 应用多文件补丁 |
| **Web** | `web_search` | 网页搜索 |
| | `x_search` | 跨平台搜索 |
| | `web_fetch` | 获取网页内容 |
| **浏览器** | `browser` | 浏览器自动化 |
| **消息与渠道** | `message` | 发送消息 |
| **会话与智能体** | `sessions_list` | 列出会话 |
| | `sessions_history` | 获取会话历史 |
| | `sessions_send` | 发送消息到会话 |
| | `sessions_spawn` | 生成子代理 |
| | `sessions_yield` | 挂起等待子代理 |
| | `subagents` | 子代理管理 |
| | `agents_list` | 列出智能体 |
| | `session_status` | 会话状态 |
| **自动化** | `cron` | 定时任务管理 |
| | `heartbeat_respond` | 响应心跳事件 |
| **Gateway与节点** | `gateway` | 网关控制 |
| | `nodes` | 节点管理 |
| **记忆** | `memory_search` | 记忆搜索 |
| | `memory_get` | 获取记忆片段 |

**约束**：
- 只能使用**预先在工具清单中注册的工具**，禁止动态发明新工具
- `optional: true` 的工具需要加入白名单才能使用
- `tools.deny` 优先级**高于** `tools.allow`（拒绝优先）
- 如果允许列表链导致没有可调用工具，应在提交提示词给模型**之前停止**（大声失败）

---

## SSH 外部工具集成

### Windows 宿主机（DESKTOP-QQMDSC1）

| 别名 | 目标 | 用途 | 调用规范 |
|------|------|------|---------|
| `codex++` | `Administrator@172.21.112.1` | 开发者编程工具，代码执行 | `ssh -i ~/.ssh/id_ed25519 ... "cmd /c codex_task.bat \"$PROMPT\""` |
| `hermes` | `Administrator@172.21.112.1` | 本地大模型，深度分析智囊 | `ssh ... "cmd /c hermes_task.bat \"$PROMPT\""` |
| `claude` | `Administrator@172.21.112.1` | Windows 桌面 AI（待配置） | 桌面集成，未启用 |

**SSH Key**：统一使用 `~/.ssh/id_ed25519`（Ed25519 最佳实践）

**调用流程**：
```bash
# 标准调用格式
ssh -i ~/.ssh/id_ed25519 Administrator@172.21.112.1 "cmd /c codex_task.bat \"$PROMPT\""

# 结果文件
C:\Users\Administrator\codex_result.txt
```

---

## 飞书渠道工具

| 配置项 | 值 | 说明 |
|-------|-----|------|
| 渠道 | feishu | 当前活跃渠道之一 |
| 连接模式 | websocket | 实时双向，已验证 |
| dmPolicy | ~~open~~ → **allowlist** | **已收紧**，仅白名单用户可 DM |
| allowFrom | 管理员指定 | 待配置 |

> ⚠️ dmPolicy 已由 `open` 变更为白名单模式（v2.0.0 更新）

---

## TTS / 语音工具

> 当前配置：未启用
>
> 如需启用，配置项在 `channels` → `feishu` 或独立 TTS 插件

---

## 文件路径约定

| 变量 | 实际路径 | 用途 |
|------|---------|------|
| `<workspace>` | `/home/openclaw/.openclaw/agents/ceo` | CEO 工作区根目录 |
| `<mybrain>` | `/home/openclaw/.openclaw/workspace/MyBrain` | 知识库根目录 |
| `<openclaw_home>` | `/home/openclaw/.openclaw` | OpenClaw 主目录 |

---

## 工具使用偏好

### 优先使用顺序

1. **系统内置工具**（read / write / exec / cron 等）——始终优先
2. **Gateway config 工具**（`config.schema.lookup` / `config.patch` / `config.apply`）——配置操作
3. **社区 Skills**（skill_workshop / gh-issues / notion 等）——领域任务
4. **SSH 外部工具**（Codex++ / Hermes）——需要 Windows 本地执行的场景
5. **exec 通用命令**——最后兜底，明确参数和预期输出

### exec 使用规范

```
✅ 正确示范：
exec({ command: "openclaw status", timeout: 30 })
exec({ command: "ls /path/to/dir", timeout: 10 })

❌ 禁止：
exec({ command: "rm -rf /some/path" })          # 未指定 timeout，无备份
exec({ command: "curl http://..." })             # 网络调用未评估风险
```

---

## 凭证管理

| 凭证类型 | 存储位置 | 说明 |
|---------|---------|------|
| SSH Private Key | `~/.ssh/id_ed25519` | 文件系统存储，不写入任何 .md |
| 飞书 AppSecret | `~/.openclaw/auth store` | OpenClaw 内置凭证存储 |
| API Keys | 环境变量注入 | 不得硬编码 |

> 凭证管理原则：不得硬编码；不得出现在任何 .md 文件中；不得出现在 SKILL.md body 中

---

## 相关文件

| 文件 | 作用 |
|------|------|
| `knowledge/system/channels.md` | 渠道配置详情 |
| `knowledge/system/security.md` | 安全体系（SkillSpector / MITRE ATLAS） |
| `STANDARDS.md` | OpenClaw 生态标准化总纲 |

---

*v2.0.0 · King · 2026-06-30 · 深度标准化工具配置*