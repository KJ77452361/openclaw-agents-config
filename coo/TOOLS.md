# TOOLS.md — COO

> 版本：v1.0.0（深度标准化）
> 更新：2026-06-30
> 说明：本文件记录 Agent 对本地工具的使用偏好，不决定 AI 能用什么工具，仅为使用建议

---

## 工具配置原则

- **只使用已注册工具**：所有工具调用必须来自工具清单
- **标准化调用格式**：工具名称、参数（符合 schema 的 JSON）、预期输出格式
- **受保护路径**：`tools.exec.ask` 和 `tools.exec.security` 不可被任何配置修改
- **长时间运行**：包含"check back"跟进指导，持续追踪直到完成

---

## 内置工具类别完整清单

| 类别 | 工具 | 说明 |
|------|------|------|
| **运行时** | `exec` | 执行系统命令 |
| | `process` | 管理系统进程 |
| **文件** | `read` | 读取文件 |
| | `write` | 写入文件 |
| | `edit` | 编辑文件 |
| **Web** | `web_search` | 网页搜索 |
| | `web_fetch` | 获取网页内容 |
| **会话与智能体** | `sessions_list` | 列出会话 |
| | `sessions_history` | 获取会话历史 |
| | `sessions_send` | 发送消息到会话 |
| | `sessions_spawn` | 生成子代理 |
| | `sessions_yield` | 挂起等待子代理 |
| **自动化** | `cron` | 定时任务管理 |
| **记忆** | `memory_search` | 记忆搜索 |
| | `memory_get` | 获取记忆片段 |

---

## 文件路径约定

| 变量 | 实际路径 | 用途 |
|------|---------|------|
| `<workspace>` | `/home/openclaw/.openclaw/agents/coo` | Agent 工作区根目录 |
| `<mybrain>` | `/home/openclaw/.openclaw/workspace/MyBrain` | 知识库根目录 |
| `<openclaw_home>` | `/home/openclaw/.openclaw` | OpenClaw 主目录 |

---

## 工具使用偏好

### 优先使用顺序

1. **系统内置工具**（read / write / exec / cron 等）——始终优先
2. **Gateway config 工具**（`config.schema.lookup` / `config.patch` / `config.apply`）——配置操作
3. **社区 Skills**（skill_workshop / gh-issues / notion 等）——领域任务
4. **exec 通用命令**——最后兜底，明确参数和预期输出

### exec 使用规范

```
✅ 正确示范：
exec({ command: "openclaw status", timeout: 30 })
exec({ command: "ls /path/to/dir", timeout: 10 })

❌ 禁止：
exec({ command: "rm -rf /some/path" })          # 未指定 timeout，无备份
```

---

*v1.0.0 · King · 2026-06-30 · 深度标准化工具配置*