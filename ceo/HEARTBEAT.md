# Heartbeat - CEO

## 检查项（每 30 分钟轮询，22:00-08:00 静默）

### 快速检查（每次）
- Cron Jobs 状态（最近一次运行结果）
- Session 活跃数量
- 待处理任务数（openclaw tasks issues）

### 定期检查（每 3 次心跳）
- 知识库 MyBrain 同步状态
- 磁盘/内存使用率
- 安全审计警告

### 有变化时通知，无变化回 HEARTBEAT_OK

## 触发条件
- Cron Jobs 出现 failure
- Session 数量异常波动 >50%
- 磁盘使用率 >80%
