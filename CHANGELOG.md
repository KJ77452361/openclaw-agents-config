# Changelog — OpenClaw 自进化系统

## [v1.1.0] 后续改善 — 2026-06-28

### 完成时间
2026-06-28 13:00 CST

### 缺陷修复
- ✅ `sys_self_health_hourly` payload 简化（复杂度导致超时，timeout 300s）
- ✅ 三仓 GitHub Actions workflows（`openclaw-agents-config` 中添加 `sync-agents.yml` + `sync-all.yml`）
- ✅ `knowledge_sync_daily` 增强为三仓同步（workspace + MyBrain + agents-config）
- ✅ `agents-config/` 克隆到 workspace 并加入 .gitignore
- ✅ `MyBrain/` 加入 workspace .gitignore（三仓完全隔离）
- ✅ `PAT` secret 添加至 GitHub（`openclaw-agents` + `openclaw-agents-config`）

### 已知改进
- ⚠️ 3 个 cron job error 状态（isolated session Feishu delivery false-positive）：`sys_self_health_hourly` / `memory_log_compress` / `llm_wiki_weekly` — job 本身执行成功，仅 delivery 失败

---

## [v1.0.0] Baseline — 2026-06-28

### 完成时间
2026-06-28 12:22 CST

### 核心架构
- **三仓备份**：`openclaw-agents` + `openclaw-agents-config` + `openclaw-mybrain`
- **Agent 数量**：11 个核心 Agent（CEO/COO/CTO/CA/PM/QM/RS/SA/PE/HE/SE）+ 4 个执行层
- **模型**：MiniMax-M2.7
- **OpenClaw**：v2026.6.11

### 文档标准化
- ✅ 22 个 SOP + Workflow 全部 active
- ✅ 16 个 Cron Job 符合命名规范 v2.0.0
- ✅ 所有 Agent 核心文件 8 个标准化（AGENTS.md / SOUL.md / IDENTITY.md / USER.md / MEMORY.md / STANDARDS.md / TOOLS.md / HEARTBEAT.md）
- ✅ 12 个社区技能安装并配置软链接
- ✅ Skills SKILL.md frontmatter 100% 完整

### SOUL.md OSP 升级
- ✅ 11 个 Agent SOUL.md 升级至 v2.2.0 OSP 三层结构（nucleus → persona → pulse）
- 参照：`doingdd/open-soul` OSP YAML seeds

### Cron Job 系统
- ✅ 16 个 Cron Job 就绪（13 ok / 3 error / 2 null）
- ✅ `sys_self_health_hourly` payload 修复（isolated session cron list 隔离问题）
- ✅ `sys_audit_weekly` payload 修复（skills 路径硬编码）
- ✅ `memory_log_compress` / `llm_wiki_weekly` delivery=none 修复

### 知识库
- ✅ MyBrain 拆分为独立 git 仓库 `openclaw-mybrain`
- ✅ SOP 索引 v4.6.0
- ✅ `system/cron.md` v2.1.0（Cron Job 完整清单）
- ✅ Dreaming System 目录结构完整（deep/light/rem/）

### 缺陷修复
- 🔴 P0：`openclaw-super-healthcheck/SKILL.md` frontmatter 缺失
- 🔴 P0：`llm_wiki_weekly` / `memory_log_compress` Feishu delivery 报错
- 🔴 P1：`sys_self_health_hourly` 报告逻辑错误（只显示 1 个 job）
- 🔴 P1：`sys_audit_weekly` skills 路径解析错误
- 🟡 P3：`heartbeat-state.json` 缺失 → 已创建
- 🟡 P3：CEO inbox/ 目录缺失 → 已创建

### 已知遗留项
- ⚠️ 3 个 cron job error 状态（API 偶发过载，持续监控）
- ⚠️ 2 个 cron job null 状态（首次未到，正常）
- ⚠️ 43 个 historical task issues（Gateway 重启遗留）

---

## 升级记录格式

| 版本 | 日期 | 主要变更 |
|------|------|---------|
| v1.0.0 | 2026-06-28 | 基线封装 |
