# Heartbeat - CEO

> 版本：v2.3.0（深度标准化监控体系）
> 更新：2026-06-30
> 频率：每 2 小时 | 静默时段：22:00-08:00

---



> ⚠️ Schema 限制说明
> 以下字段**不存在**于 OpenClaw schema（勿配置）：
> - `heartbeat.notifyUser` → 应为 `heartbeat.includeSystemPromptSection`
> - `heartbeat.dmPolicy` → 应为 `heartbeat.directPolicy`
> - `monitoring.intervalMs` / `monitoring.alertChannels` → monitoring 配置节不存在
> - `agents.defaults.rateLimit.maxTokensPerDay` → 不存在于 schema
> - `agents.defaults.queue` → 不存在于 schema
> - `hooks.internal.tool_guard` → 应为 `gateway.tools.deny`（危险操作拦截，正确实现P29）
>
> 配置前务必运行 `openclaw config validate` 确认有效。


## 检查项（三层指标体系）

### 快速检查（每次）

- Cron Jobs 状态（最近一次运行结果）
- Session 活跃数量
- 待处理任务数（`openclaw tasks issues`）

### 定期检查（每 1 次心跳，即每 2 小时）

- 知识库 MyBrain 同步状态
- 磁盘/内存使用率
- 安全审计警告

### 深度监控（每 4 次心跳，即每 8 小时）

- 执行效率指标：响应时间、工具调用耗时、吞吐量
- 质量评估指标：任务完成率、一致性
- 异常检测指标：错误率（阈值 >5%）、超时次数（阈值 >3次/小时）、重试次数（阈值 >5次）

---

## 核心监控指标定义

| 指标类别 | 指标名称 | 标准定义 | 告警阈值 |
|---------|---------|---------|---------|
| 执行效率 | 响应时间 | 从接收到完成的总耗时 | >30s 标记 |
| 执行效率 | 工具调用耗时 | 单个工具调用的耗时 | >10s 标记 |
| 执行效率 | 资源消耗 | CPU/内存/网络使用量 | CPU>80% 告警 |
| 质量评估 | 任务完成率 | 成功完成任务的比例 | <80% 告警 |
| 质量评估 | 一致性 | 相同输入产生相同输出的概率 | <90% 标记 |
| 异常检测 | 错误率 | 执行失败的比例 | **>5% 触发告警** |
| 异常检测 | 超时次数 | 超过超时阈值的次数 | **>3次/小时 触发告警** |
| 异常检测 | 重试次数 | 单任务重试次数 | **>5次 触发告警** |
| 异常检测 | 异常退出 | 非正常终止次数 | **>0 触发告警** |

---

## 触发条件

| 条件 | 动作 |
|------|------|
| Cron Jobs 出现 failure | 立即告警 |
| Session 数量异常波动 >50% | 立即告警 |
| 磁盘使用率 >80% | 立即告警 |
| 错误率 >5% | 立即告警 |
| 超时次数 >3次/小时 | 立即告警 |
| 重试次数 >5次 | 立即告警 |

---

## 状态文件

心跳状态记录在：`memory/heartbeat-state.json`

```json
{
  "lastCheck": "2026-06-30T10:00:00+08:00",
  "heartbeatFrequency": "2h",
  "results": {
    "cronJobs": { "status": "ok", "failures": 0 },
    "sessions": { "active": 12, "波动": "normal" },
    "tasks": { "pending": 3, "overdue": 0 },
    "disk": { "usagePercent": 1 },
    "memory": { "usagePercent": 16 },
    "execution": {
      "errorRate": 0.01,
      "timeoutCount": 0,
      "retryCount": 0
    }
  },
  "alerts": [],
  "nextScheduled": "2026-06-30T12:00:00+08:00"
}
```

---

## 心跳频率配置

- 配置：`agents.defaults.heartbeat.every`
- 当前值：`2h`（2026-06-28 从 30m 更新）
- 生效：需 Gateway 重启后应用

---

## 反馈优化闭环

```
[用户请求] → [提示词执行] → [指标采集] → [异常检测] → [组合优化] → [模板更新]
```

每次心跳触发指标采集；异常检测自动触发告警或降级；发现模式后更新到 STANDARDS.md。

---

## 最新巡检

- 2026-06-29：Ghost stubs 清理完成
- 2026-06-29：Dreaming 重复已清理
- 2026-06-29：Skills frontmatter 全部补全（22个）
- 2026-06-30：监控指标体系 v2.1.0 升级（8类指标 + 反馈闭环）

---

*v2.1.0 · King · 2026-06-30 · 深度标准化监控体系*