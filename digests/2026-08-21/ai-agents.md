# OpenClaw 生态日报 2026-08-21

> Issues: 500 | PRs: 500 | 覆盖项目: 12 个 | 生成时间: 2026-08-21 03:30 UTC

- [OpenClaw](https://github.com/openclaw/openclaw)
- [NanoBot](https://github.com/HKUDS/nanobot)
- [Hermes Agent](https://github.com/nousresearch/hermes-agent)
- [PicoClaw](https://github.com/sipeed/picoclaw)
- [NanoClaw](https://github.com/qwibitai/nanoclaw)
- [NullClaw](https://github.com/nullclaw/nullclaw)
- [IronClaw](https://github.com/nearai/ironclaw)
- [LobsterAI](https://github.com/netease-youdao/LobsterAI)
- [Moltis](https://github.com/moltis-org/moltis)
- [CoPaw](https://github.com/agentscope-ai/CoPaw)
- [ZeptoClaw](https://github.com/qhkm/zeptoclaw)
- [ZeroClaw](https://github.com/zeroclaw-labs/zeroclaw)

---

## OpenClaw 项目深度报告

# OpenClaw 项目动态日报 | 2026-08-21

## 1. 今日速览

OpenClaw 今日维持**极高社区活跃度**，24小时内 Issues 与 PR 各更新 500 条，但**合并吞吐严重滞后**——仅 125 条 PR 关闭/合并，375 条待合并，38 条 Issues 关闭 vs 462 条活跃/新开，形成显著的"进多出少"积压态势。无新版本发布，v2026.8.1-beta.2 仍处于验证阶段。核心风险集中在**网关稳定性**（Cron 退出处理、心跳阻塞、子进程泄漏）与**多平台运行时一致性**（Windows SQLite 句柄泄漏、Docker 重启循环）。社区对**成本管控、内存管理、认证状态持久化**的诉求持续升温。

---

## 2. 版本发布

**无新版本发布**

v2026.8.1-beta.2 验证仍在进行中（[#125626](https://github.com/openclaw/openclaw/issues/125626)），尚未达到稳定发布标准。

---

## 3. 项目进展

### 今日合并/关闭的关键 PR

| PR | 状态 | 核心改进 | 链接 |
|:---|:---|:---|:---|
| **#120900** feat(ui): review install policy warnings | **已关闭** | 管理员可在 Control UI 中审查安装策略警告并继续安装，安全边界提升 | [PR #120900](https://github.com/openclaw/openclaw/pull/120900) |
| **#125471** fix(models): keep Claude CLI OAuth available in Control UI | **已关闭** | 修复 Gateway 重启后 Claude CLI OAuth 刷新所有权丢失问题，解决认证状态漂移 | [PR #125471](https://github.com/openclaw/openclaw/pull/125471) |
| **#126966** improve(plugins): avoid duplicate web-channel manifest discovery | **已关闭** | 消除插件元数据生命周期中的重复清单发现，降低启动/登录延迟 | [PR #126966](https://github.com/openclaw/openclaw/pull/126966) |

### 待合并的重要推进（Ready for Maintainer Look）

| PR | 优先级 | 改进领域 | 链接 |
|:---|:---|:---|:---|
| **#126853** fix(agents): heartbeat runs no longer block visible turns | **P1** | 解决心跳嵌入运行占用 `main` 车道导致用户可见通道等待数分钟的**可用性瓶颈** | [PR #126853](https://github.com/openclaw/openclaw/pull/126853) |
| **#126640** fix(gateway): give scheduler-owned agent runs a Gateway request context | **P1** | 为调度器拥有的 Agent 运行提供 Gateway 请求上下文，修复安全边界与可用性缺陷 | [PR #126640](https://github.com/openclaw/openclaw/pull/126640) |
| **#126830** fix(doctor): repair plugin host links before startup migration | **P1** | 修复长期运行实例升级时 Codex 插件主机链接失效导致的**崩溃循环** | [PR #126830](https://github.com/openclaw/openclaw/pull/126830) |
| **#126963** fix(gateway): await cron exit watcher drain | **P2** | Cron 退出观察器生命周期完全等待，消除子进程残留与终端持久化竞态 | [PR #126963](https://github.com/openclaw/openclaw/pull/126963) |
| **#126967** improve(ui): skip discarded workspace loading during first-run setup | **P2** | 首屏体验优化：避免未配置 Gateway 时短暂显示错误状态 | [PR #126967](https://github.com/openclaw/openclaw/pull/126967) |

**整体评估**：今日合并量偏低，但 P1 级修复密集进入"待维护者审查"状态，若明日批量合并可显著改善网关稳定性。

---

## 4. 社区热点

### 讨论最活跃的 Issues（按评论数排序）

| 排名 | Issue | 评论 | 核心诉求 | 链接 |
|:---|:---|:---|:---|:---|
| 1 | **#42475** Per-agent cost budget enforcement at gateway level | **23** | **成本管控刚需**：在网关层实施每日/每月成本上限，防止模型调用失控支出 | [Issue #42475](https://github.com/openclaw/openclaw/issues/42475) |
| 2 | **#91009** Codex PreToolUse native hook relay spawns CPU-bound processes | **21** | **性能灾难**：Codex 工具调用衍生 `openclaw-hooks` 进程消耗 100%+ CPU，阻塞 Gateway RPC | [Issue #91009](https://github.com/openclaw/openclaw/issues/91009) |
| 3 | **#48788** Centralized filename encoding utility | **20** | **国际化架构债**：飞书中文文件名乱根因未根治，需统一处理 Shift-JIS/EUC-KR/GB18030 等多编码 | [Issue #48788](https://github.com/openclaw/openclaw/issues/48788) |
| 4 | **#125626** Release validation: v2026.8.1-beta.2 | **17** | **版本质量把关**：社区参与 beta 验证，但进度受限于测试环境搭建复杂度 | [Issue #125626](https://github.com/openclaw/openclaw/issues/125626) |
| 5 | **#108435** Gateway fails to start w/ error (2026.7.1 regression) | **14** | **升级阻断**：systemd/ollama/手动启动均失败，影响稳定版用户升级路径 | [Issue #108435](https://github.com/openclaw/openclaw/issues/108435) |

### 背后诉求分析

- **成本可观测性 > 成本限制**：#42475 的 23 条评论显示用户不仅需要"事后看账单"，更需要在**请求分发前拦截**，反映企业级部署的合规需求
- **Codex 集成深度耦合**：#91009 揭示 Codex 原生钩子与 OpenClaw 进程模型的架构冲突，短期缓解需限制并发，长期需重构为常驻服务而非派生进程
- **国际化非边缘场景**：#48788 的持久讨论证明东亚市场（中日韩）的文件名处理是核心体验，非"off-meta"可敷衍

---

## 5. Bug 与稳定性

### P0（发布阻断/数据丢失）

| Issue | 描述 | Fix PR 状态 | 链接 |
|:---|:---|:---|:---|
| **#108435** | 2026.7.1 升级后网关无法启动（systemd/ollama/手动均失败） | ❌ **无** | [Issue #108435](https://github.com/openclaw/openclaw/issues/108435) |
| **#48920** | Live Docs 超前于发布版本（文档与代码不同步） | ❌ **无** | [Issue #48920](https://github.com/openclaw/openclaw/issues/48920) |
| **#119270** | 文件工具剥离前导 `@` 导致写入/删除错误文件（**数据损坏风险**） | ⚠️ 有 linked PR 但未明确 | [Issue #119270](https://github.com/openclaw/openclaw/issues/119270) |

### P1（崩溃循环/消息丢失/认证失效）

| Issue | 描述 | Fix PR 状态 | 链接 |
|:---|:---|:---|:---|
| **#91009** | Codex 钩子进程 CPU 占满 + RPC 阻塞 | ❌ **无** | [Issue #91009](https://github.com/openclaw/openclaw/issues/91009) |
| **#97616** | 子进程泄漏为僵尸进程，运行时持续退化 | ❌ **无** | [Issue #97616](https://github.com/openclaw/openclaw/issues/97616) |
| **#113306** | SQLite 快照恢复缺乏崩溃保证与身份保证（**数据丢失**） | ❌ **无** | [Issue #113306](https://github.com/openclaw/openclaw/issues/113306) |
| **#123073** | dev 通道更新失败：`workspace:*` 协议与 npm 不兼容 | ⚠️ 有 linked PR | [Issue #123073](https://github.com/openclaw/openclaw/issues/123073) |
| **#119475** | WhatsApp LID 地址聊天消息静默丢弃（24小时 79 位发送者丢失） | ❌ **无** | [Issue #119475](https://github.com/openclaw/openclaw/issues/119475) |
| **#126246** | Telegram 持久外发投递卡在 `send_attempt_started`，重启后丢失 | ❌ **无**（**今日新建，高优先级**） | [Issue #126246](https://github.com/openclaw/openclaw/issues/126246) |
| **#124284** | vLLM + thinking 启用时子 Agent 生成畸形 XML 工具调用 | ❌ **无** | [Issue #124284](https://github.com/openclaw/openclaw/issues/124284) |
| **#86612** | Docker 容器重启循环（`OPENCLAW_SANDBOX=1` + `OPENCLAW_HOME=/mnt/...`） | ❌ **无** | [Issue #86612](https://github.com/openclaw/openclaw/issues/86612) |

### P2（用户体验摩擦/功能降级）

| Issue | 描述 | Fix PR 状态 | 链接 |
|:---|:---|:---|:---|
| **#43747** | 内存管理混乱：同团队 3 人出现 3 种不同存储行为 | ❌ **无** | [Issue #43747](https://github.com/openclaw/openclaw/issues/43747) |
| **#119796** | Windows vitest 拆卸失败：agent state DB 句柄未释放（`EBUSY unlink`） | ⚠️ 有 linked PR | [Issue #119796](https://github.com/openclaw/openclaw/issues/119796) |
| **#124751** | iOS 应用重复渲染助手回复且不自动滚动 | ❌ **无**（**今日活跃**） | [Issue #124751](https://github.com/openclaw/openclaw/issues/124751) |

---

## 6. 功能请求与路线图信号

### 高可能性纳入下一版本（有 PR 或 maintainer 关注）

| 需求 | 信号强度 | 依据 | 链接 |
|:---|:---|:---|:---|
| **Gateway 层成本预算强制** | ⭐⭐⭐⭐⭐ | 评论最多 Issue（23条），企业部署刚需，架构位置明确 | [Issue #42475](https://github.com/openclaw/openclaw/issues/42475) |
| **可配置 Control UI 上传大小限制**（当前硬编码 5MB） | ⭐⭐⭐⭐⭐ | 有 PR #71142 待合并，媒体理解场景直接受阻 | [Issue #71142](https://github.com/openclaw/openclaw/issues/71142) |
| **Perplexity search context size 配置** | ⭐⭐⭐⭐☆ | PR #96937 已提交，扩展性明确 | [PR #96937](https://github.com/openclaw/openclaw/pull/96937) |
| **macOS 实时 Gateway-relay Talk 支持** | ⭐⭐⭐⭐☆ | PR #118499 跨平台大 PR（XL），多应用面影响 | [PR #118499](https://github.com/openclaw/openclaw/pull/118499) |

### 中期路线图信号（讨论充分但需产品决策）

| 需求 | 关键阻碍 | 链接 |
|:---|:---|:---|
| **集中式文件名编码工具** | 需跨所有 channel adapter 改造，影响面大 | [Issue #48788](https://github.com/openclaw/openclaw/issues/48788) |
| **Provider 按故障类别降级**（认证失败单独隔离） | 需重构 failover 状态机 | [Issue #47910](https://github.com/openclaw/openclaw/issues/47910) |
| **暴露解析后的后端模型到 session_status** | LiteLLM 代理场景的信息盲区 | [Issue #51441](https://github.com/openclaw/openclaw/issues/51441) |
| **Reasoning stream 实时覆盖行显示** | 需协议层支持流式元数据 | [Issue #42276](https://github.com/openclaw/openclaw/issues/42276) |

### 用户体验优化（低阻力，高回报）

| 需求 | 链接 |
|:---|:---|
| `/new` 和 `/reset` 添加确认步骤防误触 | [Issue #45564](https://github.com/openclaw/openclaw/issues/45564) |
| 可配置 session 启动消息（`session.resetPrompt`） | [Issue #45501](https://github.com/openclaw/openclaw/issues/45501) |
| lane wait 诊断阈值可配置（当前硬编码 2s） | [Issue #14747](https://github.com/openclaw/openclaw/issues/14747) |

---

## 7. 用户反馈摘要

### 🔴 核心痛点

| 痛点 | 典型场景 | 来源 Issue |
|:---|:---|:---|
| **"升级即崩溃"恐惧** | 2026.7.1 升级后网关完全无法启动，无回滚路径 | #108435 |
| **认证状态神秘失效** | OAuth 刷新在 #73682 修复后仍失效，所有流量死胡同 | #83598 |
| **内存行为不可预测** | 同团队 3 人 3 种存储模式，无法同步知识库状态 | #43747 |
| **消息"成功"实则丢失** | Telegram/WhatsApp 外发卡在中间状态，重启后彻底消失 | #126246, #119475 |
| **Windows 二等公民体验** | DB 句柄未释放、CLI 进程残留、vitest 拆卸失败 | #119796, #74378 |

### 🟡 摩擦与困惑

| 反馈 | 根源 | 来源 |
|:---|:---|:---|
| "文档写的功能实际没有" | Live Docs 超前发布版本 | #48920 |
| "XDG_CONFIG_HOME 在 .env 里设置了但无效" | Docker 安装环境变量未传递 | #53628 |
| "模型切换后静默失败" | 上下文过大无明确错误提示 | #58957 |
| "Doctor 警告无法修复" | NVM vs Homebrew Node 路径冲突，每次重启重置 | #60612 |

### 🟢 积极信号

- **社区深度参与验证**：beta.2 验证 Issue 有结构化工作表，多测试者协作
- **AI 辅助开发成熟**：多个 PR 明确标注"AI-assisted: Yes — implemented and validated with Codex/Claude"，开发效率提升

---

## 8. 待处理积压

### 长期未响应的高价值 Issue（>5个月，仍 open，无 fix PR）

| Issue | 创建时间 | 当前状态 | 风险 | 链接 |
|:---|:---|:---|:---|:---|
| **#42475** Gateway 层成本预算 | 2026-03-10（**164天**） | 需产品决策，无 PR | 企业销售阻碍 | [Issue #42475](https://github.com/openclaw/openclaw/issues/42475) |
| **#43747** 内存管理混乱 | 2026-03-12（**162天**） | 多用户复现，无诊断工具 | 团队协作断裂 | [Issue #43747](https://github.com/openclaw/openclaw/issues/43747) |
| **#44134** Google Antigravity 反滥用误封 | 2026-03-12（**162天**） | 需实时复现，架构级修复 | 商业合作关系 | [Issue #44134](https://github.com/openclaw/openclaw/issues/44134) |
| **#47910** Provider 按故障类别降级 | 2026-03-16（**158天**） | 有明确日志证据，无 PR | 故障恢复效率 | [Issue #47910](https://github.com/openclaw/openclaw/issues/47910) |
| **#50490** 飞书群聊 activation 模式切换无效 | 2026-03-19（**155天**） | 中文用户场景，有复现步骤 | 中国市场体验 | [Issue #50490](https://github.com/openclaw/openclaw/issues/50490) |

### 维护者行动建议

1. **紧急**：为 #108435（P0 升级阻断）分配专项诊断，考虑发布 2026.7.1 热修复或回滚建议
2. **本周**：批量审查 Ready for Maintainer Look 的 P1 PR（#126853, #126640, #126830），释放稳定性改进
3. **本月**：就 #42475 成本预算召开产品决策会议，或明确路线图时间节点以管理企业用户预期
4. **持续**：建立"Windows 运行时"专项，系统解决句柄泄漏、进程残留、路径处理等问题族

---

*日报基于 GitHub 公开数据生成，所有链接指向 github.com/openclaw/openclaw*

---

## 横向生态对比

# 个人 AI 助手/自主智能体开源生态横向对比分析

**报告日期**：2026-08-21 | **分析范围**：11 个活跃项目

---

## 1. 生态全景

个人 AI 助手开源生态正经历**从"功能竞赛"向"生产就绪"的集体转型**：头部项目（OpenClaw、ZeroClaw、CoPaw）日均 50+ PR/Issues 的高强度迭代背后，是合并瓶颈、技术债务与安全合规的三重压力；中型项目（NanoBot、Hermes Agent、IronClaw）聚焦架构成熟化，推进沙箱持久化、生命周期钩子等基础设施；尾部项目（PicoClaw、NanoClaw、Moltis、LobsterAI）则陷入维护模式或社区动能衰减。整体呈现**"高活跃度、低释放效率"**的结构性特征——开发吞吐远超合并消化能力，企业级可靠性成为共同瓶颈。

---

## 2. 各项目活跃度对比

| 项目 | Issues (24h) | PRs (24h) | 待合并 PR | 已合并/关闭 | 新版本 | 健康度评估 |
|:---|:---:|:---:|:---:|:---:|:---:|:---|
| **OpenClaw** | 500 (38关/462活跃) | 500 (125关/375待合) | 375 | 125 | ❌ 无 | 🔴 **高活跃-高积压**：合并吞吐仅 25%，P0 升级阻断未解，网关稳定性危机 |
| **ZeroClaw** | 50 (5关/45活跃) | 50 (2关/48待合) | 48 | 2 | ❌ 无 | 🟡 **极高活跃-极低释放**：48/50 PR 待审，安全架构密集推进但评审瓶颈严重 |
| **CoPaw** | 32 (13关/19活跃) | 50 (28关/22待合) | 22 | 28 | ✅ v2.1.1-beta.1 | 🟢 **高活跃-健康释放**：56% 合并率，MCP 恢复等生产级修复快速落地 |
| **Hermes Agent** | 50 (9关/41活跃) | 50 (18关/32待合) | 32 | 18 | ❌ 无 | 🟡 **高活跃-中等释放**：36% 合并率，SQLite 损坏、安装失败等可靠性债务突出 |
| **IronClaw** | 21 (4关/17活跃) | 37 (14关/23待合) | 23 | 14 | ❌ 无 | 🟢 **中高活跃-结构化推进**：38% 合并率，v1.4.0 双支柱（沙箱持久化+钩子系统）清晰 |
| **NanoBot** | 5 (3关/2开) | 29 (12关/17待合) | 17 | 12 | ❌ 无 | 🟢 **中等活跃-低积压**：41% 合并率，提供商生态扩张为主，技术债务可控 |
| **NanoClaw** | 3 (0关/3活跃) | 50 (15关/35待合) | 35 | 15 | ❌ 无 | 🟡 **高 PR 量-低 Issue 反馈**：30% 合并率，v2 架构容器边界假设与实际部署错位 |
| **PicoClaw** | 0 (0/0) | 9 (4关/5待合) | 5 | 4 | ❌ 无 | 🟡 **低活跃-维护模式**：依赖更新堆积，核心性能问题 stale 近月 |
| **Moltis** | 0 (0/0) | 4 (0关/4待合) | 4 | 0 | ✅ 20260820.01 | 🟡 **低活跃-发布驱动**：0% 合并率，Windows 兼容 PR 积压 150 天 |
| **LobsterAI** | 2 (2关/0活跃) | 7 (6关/1待合) | 1 | 6 | ❌ 无 | 🔴 **极低活跃-历史清理**：全部为 4 月积压项关闭，网关重启 Bug 无修复即关闭 |
| **NullClaw / ZeptoClaw** | 0 | 0 | 0 | 0 | ❌ 无 | ⚪ **无活动** |

---

## 3. OpenClaw 在生态中的定位

### 核心参照优势
| 维度 | OpenClaw 表现 | 生态位 |
|:---|:---|:---|
| **社区规模** | 500 Issues/PR 日更，绝对量级第一 | **生态流量入口**，个人 AI 助手的"默认选项" |
| **功能完整性** | 15+ 渠道适配（WhatsApp/Telegram/Slack/Matrix/iOS...）、内置 Doctor 诊断、Control UI | **全栈整合者**，降低部署门槛 |
| **企业场景覆盖** | 成本预算（#42475）、认证持久化（#125471）、多平台运行时 | **B2B 预备队**，但稳定性未达标 |

### 技术路线差异
| 对比项 | OpenClaw | ZeroClaw | IronClaw | CoPaw |
|:---|:---|:---|:---|:---|
| **安全模型** | 网关层集中管控 | 插件级出口策略（ADR-014）+ shell 分级确认 | Capability + 域隔离身份 | 沙箱时序控制 |
| **架构哲学** | **单体网关**：所有流量经统一网关调度，便于集中策略 | **分层解耦**：Runtime 拥有会话，网关退化为传输层 | **Actor 模型**：Agent 作为持久化实体，生命周期钩子扩展 | **任务流驱动**：多步骤任务可视化，强调执行连续性 |
| **代码质量** | 功能优先，P0 阻断频发 | 系统性质量清理（#10118: 307 处 panic 候选） | Rust 工程化，Clippy 级联阻塞即修复 | 测试加固投入大（flaky test 专项） |

### 关键差距
- **合并效率**：25% vs CoPaw 56%、NanoBot 41%，375 待合并 PR 形成**创新堰塞湖**
- **稳定性信誉**：2026.7.1 升级阻断（#108435）73 天未解，"升级即崩溃"恐惧蔓延
- **Windows 体验**：DB 句柄泄漏、进程残留等"二等公民"问题族未建立专项

---

## 4. 共同关注的技术方向

| 技术方向 | 涉及项目 | 具体诉求 | 紧迫程度 |
|:---|:---|:---|:---:|
| **成本管控与可观测性** | OpenClaw (#42475)、NanoClaw (#3270)、CoPaw (#6436 自动路由) | 网关层预算拦截、Token 用量追踪、子代理成本归因 | 🔴 **极高** |
| **MCP/插件连接韧性** | CoPaw (#6524 自动恢复)、NanoBot (#5180/#5179 SDK v2 迁移)、Hermes (#37589 Desktop 缺失) | 服务端重启后自动重连、凭证隔离、OAuth 竞态消除 | 🔴 **极高** |
| **容器/沙箱生命周期** | IronClaw (#7732 持久化沙箱)、OpenClaw (#86612 Docker 重启循环)、NanoClaw (v2 挂载隔离) | 从"每次命令创建销毁"到 per-user 持久化，平衡隔离与性能 | 🟡 **高** |
| **国际化与编码** | OpenClaw (#48788 文件名编码)、CoPaw (#7191/#7192 中文文件名)、Hermes (#91261 本地化) | 非 ASCII 文件名、提示词硬编码中文、多语言 UI | 🟡 **高** |
| **Agent 执行可见性** | CoPaw (#6921 静默停止)、OpenClaw (#126853 心跳阻塞可见通道)、Hermes (#91272 /context 可视化) | 多步骤任务状态反馈、lane wait 诊断、加载/截断透明 | 🟡 **高** |
| **安全策略精细化** | ZeroClaw (#7155 shell 分级、#9582 插件出口)、OpenClaw (#120900 安装策略审查) | allow/ask/deny 分层、强制流量策略、供应链锁定 | 🟢 **中** |
| **记忆系统边界** | Hermes (#6850 生命周期解耦)、CoPaw (#7184 跨会话召回开关、#7193 记忆错乱)、OpenClaw (#43747 行为不一致) | 会话隔离、合并逻辑统一、团队级同步 | 🟢 **中** |

---

## 5. 差异化定位分析

| 项目 | 核心用户画像 | 功能侧重 | 技术架构特征 | 关键风险 |
|:---|:---|:---|:---|:---|
| **OpenClaw** | 技术爱好者 → 中小企业运维 | **渠道整合广度**（15+ 平台）、快速部署 | Node.js 单体网关，插件化扩展 | 稳定性债务、合并瓶颈 |
| **ZeroClaw** | 安全敏感型企业、开发者平台 | **可控性**（shell 分级、插件出口策略、WASM 沙箱） | Rust 分层架构，Runtime 会话所有权 | 评审过载、大型 PR 冲突 |
| **CoPaw** | 中文开发者、多模态场景用户 | **任务执行连续性**（视频帧、MCP 恢复、流式优化） | 未知（未披露），测试驱动 | GLM 等国产模型适配深度 |
| **Hermes Agent** | 多角色团队（架构师+执行者）、桌面用户 | **看板调度**、Cron 差异化配置、国际化 | SQLite + WAL，多网关路由 | 数据持久化损坏、安装摩擦 |
| **IronClaw** | 平台开发者、企业集成商 | **基础设施扩展性**（钩子系统、持久沙箱、Design System） | Rust + WASM，Capability 安全模型 | LLM 超时策略缺陷（#7783） |
| **NanoBot** | 个人极客、多提供商尝鲜者 | **提供商生态**（8+ 原生 + 兼容层）、TUI 体验 | Python，轻量模块化 | Docker OAuth 黑箱、冲突 PR 堆积 |
| **NanoClaw** | Slack/企业 IM 集成用户 | **v2 多实例隔离**、Cursor 生态接入 | 容器化边界（installSlug） | 挂载假设与实际部署错位 |
| **PicoClaw** | 硬件受限场景（Sipeed 设备）、多智能体实验 | **协议兼容性**（Anthropic Messages）、技能 CLI | Go，Blackboard 多智能体框架 | 社区动能衰减、性能 stale |
| **Moltis** | 企业合规部门、WhatsApp 商业用户 | **供应链安全**（Snyk 锁定、镜像验证） | Rust，日历版本发布 | Windows 支持长期搁置 |
| **LobsterAI** | 网易有道内部场景、中文办公自动化 | **协作性能**（N+1 优化、重渲染消除） | Electron/React，IM 深度集成 | 社区封闭、反馈周期 4 个月+ |

---

## 6. 社区热度与成熟度分层

```
┌─────────────────────────────────────────────────────────────┐
│  🚀 快速迭代阶段（高活跃 + 明确路线图）                        │
│  ├── ZeroClaw：安全架构密集投入，48 PR 待审，释放瓶颈待破       │
│  ├── CoPaw：56% 合并率，v2.1.x 补丁节奏稳定，MCP 恢复落地       │
│  └── IronClaw：v1.4.0 双支柱清晰，结构化推进                   │
├─────────────────────────────────────────────────────────────┤
│  🔥 高活跃-高债务阶段（量级大 + 积压重 + 稳定性危机）           │
│  ├── OpenClaw：绝对流量第一，375 PR 堰塞，P0 阻断频发           │
│  └── Hermes Agent：50/50 日更，SQLite 损坏、安装失败侵蚀信任    │
├─────────────────────────────────────────────────────────────┤
│  🛠️ 质量巩固阶段（低 Issue + 技术债务清理）                    │
│  ├── NanoBot：Provider 扩张为主，4 个 conflict PR 需 rebase     │
│  └── NanoClaw：v2 架构验证期，技能栈审计批量修复                │
├─────────────────────────────────────────────────────────────┤
│  😴 维护模式/衰减阶段（低活跃 + 历史清理 + 社区动能不足）        │
│  ├── PicoClaw：依赖更新堆积，核心性能 stale                    │
│  ├── Moltis：日历版本发布驱动，0 合并，Windows PR 150 天        │
│  └── LobsterAI：4 月积压项批量关闭，网关 Bug 无修复即关         │
├─────────────────────────────────────────────────────────────┤
│  💀 休眠阶段                                                  │
│  └── NullClaw、ZeptoClaw：24h 零活动                          │
└─────────────────────────────────────────────────────────────┘
```

---

## 7. 值得关注的趋势信号

### 信号一：从"功能丰富"到"生产可信"的范式转移
> **数据支撑**：ZeroClaw #10118（307 处 panic 清理，16 评论）、OpenClaw #42475（164 天成本预算诉求，23 评论）、Hermes #90950（SQLite WAL 并发损坏）

**行业含义**：个人 AI 助手从 Demo 走向 7×24 运行时，**可靠性成为比功能更硬的购买壁垒**。开发者应优先投资：状态持久化崩溃保证、网络中断自动恢复、配置-运行时语义一致性。

### 信号二：安全从"事后补丁"到"架构原生"
> **数据支撑**：ZeroClaw ADR-014 插件出口策略 + #7155 shell 分级（23 评论）、OpenClaw #120900 安装策略审查、IronClaw Capability 域隔离

**行业含义**：Claude Code 的 `allow/ask/deny` 成为事实标准，企业部署要求**最小权限原则贯穿数据流全路径**。建议将安全策略设计为可配置、可审计、可回滚的基础设施，而非硬编码规则。

### 信号三：多智能体从"概念验证"到"编排复杂性"
> **数据支撑**：PicoClaw #423 Blackboard 框架（WIP 关闭）、NanoClaw #3330 动态模型覆盖、Hermes #91259 看板 per-profile 并发、OpenClaw #126640 调度器上下文安全

**行业含义**：多智能体系统的核心挑战从"如何让 Agent 通信"转向**"如何控制并发、成本、故障传播边界"**。动态模型选择（轻任务轻模型）和精细化并发配额将成为标配。

### 信号四："合并瓶颈"成为开源项目的新型技术债务
> **数据支撑**：OpenClaw 375/500、ZeroClaw 48/50、Hermes 32/50 的待合并比例

**行业含义**：高活跃度不等于高健康度。**评审带宽不足导致创新堰塞**，大型 PR 长期并行增加集成冲突概率。建议引入：分领域代码所有者、PR 规模上限（<400 行）、自动化合并队列（merge queue）。

### 信号五：Windows 与企业环境成为"隐性淘汰线"
> **数据支撑**：OpenClaw #119796/#74378、Moltis #468（150 天）、Hermes #91021（WSL 重连失败）、NanoClaw #3414（29 并发进程超时）

**行业含义**：开源项目若不能通过**企业桌面合规（Windows）、容器化部署（Docker/K8s）、代理环境（ corporate proxy ）**三重门槛，将自动丧失 B2B 市场。建议设立"企业就绪"专项，而非依赖社区零散修复。

---

*报告基于 2026-08-21 各项目 GitHub 公开数据生成。建议技术决策者关注 CoPaw 的高释放效率模式、ZeroClaw 的安全架构投入、以及 OpenClaw 的合并瓶颈突破作为生态风向标。*

---

## 同赛道项目详细报告

<details>
<summary><strong>NanoBot</strong> — <a href="https://github.com/HKUDS/nanobot">HKUDS/nanobot</a></summary>

# NanoBot 项目动态日报 | 2026-08-21

> **项目**: [HKUDS/nanobot](https://github.com/HKUDS/nanobot) | **日期**: 2026-08-21 | **分析周期**: 过去24小时

---

## 1. 今日速览

NanoBot 今日保持**高活跃度**，PR 吞吐量达 29 条（12 条已合并/关闭，17 条待审），Issues 处理效率良好（3 关/2 开）。核心进展集中在**稳定性修复**（Codex 流式重试、Matrix 日志、Agent 路径处理）与**基础设施清理**（依赖精简）。社区对新提供商接入需求持续涌现（SenseNova、Vertex AI Claude），但暂无版本发布计划。整体健康度：**稳健推进，积压可控**。

---

## 2. 版本发布

**无新版本发布**

---

## 3. 项目进展

### ✅ 已合并/关闭的关键 PR

| PR | 作者 | 核心贡献 | 项目推进 |
|:---|:---|:---|:---|
| [#5455](https://github.com/HKUDS/nanobot/pull/5455) | akinolur | **fix(provider): retry Codex `server_error`** | 补齐流式传输中的瞬态错误重试盲区，提升 Codex 可靠性 |
| [#5452](https://github.com/HKUDS/nanobot/pull/5452) | chengyongru | **feat(tui): print resume command on exit** | 改善 TUI 用户体验，会话恢复更直观 |
| [#1203](https://github.com/HKUDS/nanobot/pull/1203) | mameikagou | **fix(cli): workaround 'Event loop is closed' on Linux** | 解决长期存在的 Linux 关闭崩溃问题（#550） |

**里程碑意义**: #1203 关闭了**6个月前**的遗留问题 #550，标志项目对平台兼容性债务的清理进入收尾阶段。#5455 与 #5454 的联动（Issue→PR→关闭）展现了社区自驱修复的高效闭环。

---

## 4. 社区热点

| 条目 | 热度指标 | 核心诉求分析 |
|:---|:---|:---|
| [#5459](https://github.com/HKUDS/nanobot/issues/5459) **Feature request: Add native Google Vertex AI provider for Claude models** | 新开，0 评论，0 👍 | **企业级多云部署需求**：用户明确对比了现有 8 家提供商覆盖，指出 Vertex AI 是唯一缺失的主流 Claude 托管渠道。诉求背后是**合规与成本优化**（利用现有 GCP 合同/折扣），而非技术能力缺口 |
| [#5444](https://github.com/HKUDS/nanobot/issues/5444) **[bug] Failed to login OpenAI via OAuth in Docker** | 1 评论，0 👍 | **容器化场景的身份验证痛点**：Docker 内 OAuth 回调地址绑定 `localhost:1455` 导致流程中断，反映云原生部署的认证链路设计缺陷 |
| [#5453](https://github.com/HKUDS/nanobot/pull/5453) **feat(providers): add SenseNova (商汤日日新) provider** | 新 PR，待审 | **国产大模型生态接入**：覆盖 sensenova-6.8-flash-lite、deepseek-v4-flash、glm-5.2，显示项目对亚太市场/中文场景的扩展意图 |

**趋势判断**: 提供商生态扩张成为近期主线，但维护者需权衡"原生支持" vs "OpenAI-compatible 网关复用"的维护成本。

---

## 5. Bug 与稳定性

| 严重程度 | Issue/PR | 描述 | 状态 |
|:---|:---|:---|:---|
| 🔴 **高** | [#5454](https://github.com/HKUDS/nanobot/issues/5454) → [#5455](https://github.com/HKUDS/nanobot/pull/5455) | Codex 流式传输中 `server_error` 跳过重试，导致对话中断 | **✅ 已修复并合并** |
| 🟡 **中** | [#5444](https://github.com/HKUDS/nanobot/issues/5444) | Docker 内 OpenAI OAuth 登录失败（回调地址绑定问题） | **⏳ 待处理**，无 PR |
| 🟡 **中** | [#5458](https://github.com/HKUDS/nanobot/pull/5458) | Matrix 错误日志使用 `%s` 占位符导致上下文丢失（Loguru 兼容性问题） | **🔧 PR 待审** |
| 🟡 **中** | [#5460](https://github.com/HKUDS/nanobot/pull/5460) | Agent 默认提示路径使用绝对路径，跨工作空间场景下泄露系统信息 | **🔧 PR 待审** |
| 🟢 **低** | [#5425](https://github.com/HKUDS/nanobot/issues/5425) | `socks://` 代理 URL 别名不被识别（遗留格式兼容） | **✅ 已关闭** |

**稳定性评估**: 流式重试和日志格式化属于**防御性修复**，显示项目正从"功能可用"向"生产可靠"过渡。Docker OAuth 问题若持续无响应，可能阻碍云原生用户 adoption。

---

## 6. 功能请求与路线图信号

| 需求 | 来源 | 可行性信号 | 纳入概率 |
|:---|:---|:---|:---|
| **Google Vertex AI native provider for Claude** | [#5459](https://github.com/HKUDS/nanobot/issues/5459) | 已有 AWS Bedrock、Azure OpenAI 先例；Anthropic 官方推 Vertex 渠道 | **高** — 预计下一季度 |
| **SenseNova (商汤) provider** | [#5453](https://github.com/HKUDS/nanobot/pull/5453) | PR 已提交，含完整文档+测试，OpenAI-compatible 端点降低维护成本 | **高** — 预计近期合并 |
| **Turn observability & safe recovery (WebUI)** | [#5420](https://github.com/HKUDS/nanobot/pull/5420) | 大型 PR，涉及 UX 重构，已开 3 天 | **中** — 需设计评审 |
| **Telegram reusable sticker replies** | [#5387](https://github.com/HKUDS/nanobot/pull/5387) | 社区驱动，频道功能增强，开 8 天 | **中** — 维护者优先级待确认 |
| **Paid security-scan MCP integration (ScanPay x402)** | [#5447](https://github.com/HKUDS/nanobot/issues/5447) | 商业提案，非核心功能，已关闭 | **低** — 超出项目范围 |

---

## 7. 用户反馈摘要

### 😤 痛点
- **"Docker 里的 OAuth 像黑箱"** — #5444 用户描述了完整的调试困境：回调 URL 已复制，但 token 交换阶段静默失败，缺乏诊断输出
- **"代理配置太挑剔"** — #5425 反映 `socks://` vs `socks5://` 的格式差异导致请求在到达提供商前即失败，配置解析的容错性不足

### 😊 满意
- **"终于不用手动找 session ID 了"** — #5452 的 resume 命令打印功能，解决 TUI 用户会话恢复的常见摩擦

### 💡 场景洞察
- **企业混合云**: #5459 的 Vertex AI 请求显示用户正在将 NanoBot 从"个人助手"推向"企业基础设施"，需要与现有云供应商合同对齐
- **Agent 经济实验**: #5447 的关闭表明社区对"AI 代理+区块链支付"的跨界尝试持开放但审慎态度，项目边界清晰

---

## 8. 待处理积压

| 条目 | 创建时间 | 最后更新 | 风险 | 行动建议 |
|:---|:---|:---|:---|:---|
| [#5180](https://github.com/HKUDS/nanobot/pull/5180) **chore(mcp): evaluate minimal SDK v2 migration** | 2026-07-30 | 2026-08-20 | **技术债务累积** | 与 #5179 形成竞争方案，需维护者决策合并哪条路径，或提取共性部分 |
| [#5179](https://github.com/HKUDS/nanobot/pull/5179) **Migrate MCP integration to SDK v2 with legacy compatibility** | 2026-07-30 | 2026-08-20 | **冲突标记，P1 优先级悬空** | 标记 `conflict` + `priority: p1` 却 22 天未合并，需 rebase 或降级优先级 |
| [#5379](https://github.com/HKUDS/nanobot/pull/5379) **fix(memory): preserve full consolidation input** | 2026-08-13 | 2026-08-20 | **功能回退风险** | 同样标记 `conflict`，记忆模块的数据完整性修复，建议优先解决 |
| [#5338](https://github.com/HKUDS/nanobot/pull/5338) **fix(mcp): preserve credentials when OAuth store read fails** | 2026-08-11 | 2026-08-20 | **安全相关** | OAuth 凭证隔离失败可能导致多服务器凭证覆盖，安全修复不应长期搁置 |

**维护者提醒**: 4 个 `conflict` 标记 PR 中，2 个涉及 MCP（#5179/#5180）、1 个涉及记忆核心（#5379）、1 个涉及安全（#5338）。建议安排专项 rebase sprint，避免技术债务滚雪球。

---

*日报生成基于 GitHub 公开数据，链接均指向 HKUDS/nanobot 仓库。如需深度分析特定 PR 的代码变更，可进一步展开 diff 审查。*

</details>

<details>
<summary><strong>Hermes Agent</strong> — <a href="https://github.com/nousresearch/hermes-agent">nousresearch/hermes-agent</a></summary>

# Hermes Agent 项目动态日报 | 2026-08-21

---

## 1. 今日速览

Hermes Agent 今日呈现**高强度开发态势**：24小时内 50 条 Issues 更新（41 活跃/新开，9 关闭）、50 条 PR 更新（32 待合并，18 已合并/关闭），无新版本发布。社区焦点集中在**架构可靠性**（state.db 并发安全、多网关路由身份）、**安装体验**（Debian 安装脚本、Windows PowerShell 约束模式）以及**看板调度并发控制**三大主题。值得关注的是，今日出现多个由核心贡献者 `andrexibiza` 发起的架构级议题，显示项目正从功能迭代向**工程成熟度**深度演进。

---

## 2. 版本发布

**无新版本发布**

---

## 3. 项目进展

### 已合并/关闭的重要 PR

| PR | 作者 | 说明 | 关联 Issue |
|:---|:---|:---|:---|
| [#91268](https://github.com/NousResearch/hermes-agent/pull/91268) | teknium1 | **修复自定义 OpenCode 提供商路由**：`opencode-go-*` 系列提供商现在正确路由至 `/v1/responses` 端点，避免 503；同时解决保留工具名冲突导致的 400 错误。 salvaged #85619 | [#85589](https://github.com/NousResearch/hermes-agent/issues/85589) |
| [#91262](https://github.com/NousResearch/hermes-agent/pull/91262) | teknium1 | **内存存储独立性加固**：禁用内置存储后不再接受写入或出现在工具 schema，salvage #90550 | [#90550](https://github.com/NousResearch/hermes-agent/pull/90550) |
| [#91261](https://github.com/NousResearch/hermes-agent/pull/91261) | wiggins-kong | **桌面端国际化**：会话侧边栏筛选菜单本地化，消除硬编码英文 | — |
| [#85619](https://github.com/NousResearch/hermes-agent/pull/85619) | Lesnak1 | 原 OpenCode 提供商修复方案，被 #91268 salvage 后关闭 | [#85589](https://github.com/NousResearch/hermes-agent/issues/85589) |

### 架构里程碑：God-File 分解完成

[#78647](https://github.com/NousResearch/hermes-agent/issues/78647) **大型文件分解 Epic 正式关闭**（20/20 完成，77 条评论）。这标志着项目确立"所有 god files 必须分片、不可回退"的永久政策，代码库可维护性获得结构性提升。

---

## 4. 社区热点

### 🔥 讨论最活跃的 Issues

| 排名 | Issue | 评论 | 核心诉求 |
|:---|:---|:---|:---|
| 1 | [#78647](https://github.com/NousResearch/hermes-agent/issues/78647) 大型文件分解完成 | 77 | **代码健康度**：社区对技术债务清理的高度共识 |
| 2 | [#87093](https://github.com/NousResearch/hermes-agent/issues/87093) Debian 13.6 安装失败 | 15 | **安装体验**：`uv.lock` 与 `npm install` 在纯净 Debian 环境失败，阻碍新用户入门 |
| 3 | [#88584](https://github.com/NousResearch/hermes-agent/issues/88584) Nous 自动化集成被阻塞 | 12 | **CI/CD 可靠性**：cron 合并冲突导致发布流水线停滞 |
| 4 | [#23524](https://github.com/NousResearch/hermes-agent/issues/23524) 定时任务推理强度覆盖 | 9 | **精细化控制**：不同 cron 任务需要差异化推理成本配置 |
| 5 | [#90950](https://github.com/NousResearch/hermes-agent/issues/90950) SQLite WAL 并发损坏 | 6 | **数据可靠性**：state.db 在生产环境反复损坏，威胁会话持久化 |

### 背后诉求分析

- **新用户漏斗危机**：#87093 的 Debian 安装失败 + #22054 的 PATH 污染问题 + #81620 的 dashboard TUI 依赖反复安装，形成**安装-首次使用**链路的系统性摩擦
- **企业级可靠性焦虑**：#90950 的 SQLite 损坏、#90386 的 Telegram 网关自愈失效、#75756 的会话编辑崩溃，显示项目在**长时运行、高并发、网络波动**场景下的韧性不足
- **多网关架构的深层矛盾**：#90149 提出路由身份应为"不可变权威证明"而非"可重建元数据"，反映多网关功能交付后，**安全模型与架构一致性**的滞后

---

## 5. Bug 与稳定性

### P1（紧急）

| Issue | 描述 | 状态 | Fix PR |
|:---|:---|:---|:---|
| [#87093](https://github.com/NousResearch/hermes-agent/issues/87093) | Debian 13.6 安装脚本崩溃：`uv.lock` 解析失败 + npm 依赖缺失 | 🔴 Open | 无 |
| [#90950](https://github.com/NousResearch/hermes-agent/issues/90950) | `state.db` SQLite 3.53.1 WAL 并发损坏，builder/reviewer 双 profile 同时中招 | 🔴 Open | [#90892](https://github.com/NousResearch/hermes-agent/pull/90892)（配置化 synchronous） |
| [#90386](https://github.com/NousResearch/hermes-agent/issues/90386) | Telegram 网关网络中断后自愈楔死，重连监听器未接管 | 🔴 Open | 无 |
| [#91221](https://github.com/NousResearch/hermes-agent/issues/91221) | `@` 上下文引用展开异常吞没整个 `asyncio.gather`，兄弟协程泄漏 | 🟢 **Closed** | 已修复 |

### P2（高优先级）

| Issue | 描述 | 状态 | Fix PR |
|:---|:---|:---|:---|
| [#75756](https://github.com/NousResearch/hermes-agent/issues/75756) | Desktop 编辑历史消息失败：session not found，rewind 缺乏 resume+retry | 🔴 Open | 无 |
| [#37589](https://github.com/NousResearch/hermes-agent/issues/37589) | Desktop 会话缺失 MCP 工具，macOS GUI PATH 下 `uvx` 服务器启动失败 | 🔴 Open | 无 |
| [#22054](https://github.com/NousResearch/hermes-agent/issues/22054) | venv PATH 前置污染系统 Python，捆绑过时 3.11 | 🔴 Open | 无 |
| [#91265](https://github.com/NousResearch/hermes-agent/issues/91265) | MCP-OAuth 新鲜进程 mtime==0 竞态，kanban worker 崩溃 | 🔴 Open | 无 |
| [#89836](https://github.com/NousResearch/hermes-agent/issues/89836) | muse-spark-1.2 返回 EmptyStream，73s 延迟 | 🟢 **Closed** | 无（外部提供商问题） |

### P3（中等）

| Issue | 描述 | 状态 | Fix PR |
|:---|:---|:---|:---|
| [#70778](https://github.com/NousResearch/hermes-agent/issues/70778) | Desktop HTTP 非安全源下麦克风录制被拒绝 | 🔴 Open | 无 |
| [#91021](https://github.com/NousResearch/hermes-agent/issues/91021) | Windows+WSL 应用内更新后无法自动重连后端 | 🔴 Open | 无 |
| [#90297](https://github.com/NousResearch/hermes-agent/issues/90297) | `auto_tts` 桌面端双重播放 | 🔴 Open | 无 |

---

## 6. 功能请求与路线图信号

| 功能请求 | 热度 | 技术可行性 | 纳入下一版本信号 |
|:---|:---|:---|:---|
| [#20765](https://github.com/NousResearch/hermes-agent/issues/20765) **WebRTC 浏览器语音捕获** | 👍 6 | 高（Web 标准） | ⭐⭐⭐ 强：#54352 同类需求，远程无麦场景刚需 |
| [#14821](https://github.com/NousResearch/hermes-agent/issues/14821) **Cron 命名/可恢复会话** | 👍 1 | 中 | ⭐⭐ 中：session bloat 问题明确，需架构评审 |
| [#91263](https://github.com/NousResearch/hermes-agent/issues/91263) **Codex 风格内联批注** | 👍 0（新） | 中 | ⭐ 观察：UX 创新，但需评估与现有编辑流的整合 |
| [#91259](https://github.com/NousResearch/hermes-agent/issues/91259) **看板 per-profile 并发覆盖** | 👍 0（新） | 高 | ⭐⭐⭐ 强：PR [#91266](https://github.com/NousResearch/hermes-agent/pull/91266) 已提交，今日创建 |
| [#87473](https://github.com/NousResearch/hermes-agent/issues/87473) **桌面通知音量控制** | 👍 0 | 高 | ⭐⭐ 中：体验打磨，实现简单 |

### 已提交的功能 PR

- **[#91266](https://github.com/NousResearch/hermes-agent/pull/91266)** `kanban.max_in_progress_per_profile_overrides`：配置化 per-profile 并发上限，直接响应 #91259
- **[#91272](https://github.com/NousResearch/hermes-agent/pull/91272)** `/context` 指令文件加载可视化：显示加载/截断/优先级竞争状态，提升可调试性
- **[#91271](https://github.com/NousResearch/hermes-agent/pull/91271)** `hermes -z` 子代理花费归因：按子代理记录成本，支持 `--usage-file` 导出

---

## 7. 用户反馈摘要

### 💢 痛点

> *"Basic Debian 13.6 installation. Only Yum installed additionally."* — [#87093](https://github.com/NousResearch/hermes-agent/issues/87093)
- **安装脚本在主流 Linux 发行版上不可靠**，curl\|bash 的简洁承诺被依赖地狱打破

> *"The relaunched app fails to reconnect to the WSL backend, requires manual close and reopen"* — [#91021](https://github.com/NousResearch/hermes-agent/issues/91021)
- **Windows 生态的"最后一公里"断裂**：更新完成但状态未恢复，用户信任损耗

> *"state.db has corrupted multiple times on this host across two profiles"* — [#90950](https://github.com/NousResearch/hermes-agent/issues/90950)
- **数据持久化是生产部署的拦路虎**，SQLite WAL 的并发语义在不同构建间不一致

### ✅ 认可

> *"Standing policy (2026-08): all god files are sharded, never reverted. The only correct answer..."* — [#78647](https://github.com/NousResearch/hermes-agent/issues/78647)
- 社区对**代码质量政策**的强烈支持，77 条评论无反对声音

### 🎯 场景诉求

- **远程/无头部署**：#54352 的"笔记本连接无麦 Mac mini"场景，推动浏览器侧音频采集
- **多角色团队协作**：#91259 的"1 架构师 + 3 执行者"并发差异化配置
- **企业审计合规**：#91271 的子代理成本归因，满足多租户计费需求

---

## 8. 待处理积压

### ⚠️ 需维护者关注的高价值长期 Issue

| Issue | 创建时间 | 最后更新 | 风险 |
|:---|:---|:---|:---|
| [#70778](https://github.com/NousResearch/hermes-agent/issues/70778) Desktop HTTP 源麦克风支持 | 2026-07-24 | 今日 | 阻碍远程开发工作流，28天无实质进展 |
| [#37589](https://github.com/NousResearch/hermes-agent/issues/37589) MCP 工具在 Desktop 缺失 | 2026-06-02 | 今日 | **80天未解决**，macOS 用户持续受影响，👍 3 |
| [#22054](https://github.com/NousResearch/hermes-agent/issues/22054) PATH 污染系统 Python | 2026-05-08 | 今日 | **105天**，安装体验的慢性毒药，影响升级路径 |
| [#47188](https://github.com/NousResearch/hermes-agent/issues/47188) Telegram 代理 NO_PROXY 绕过失效 | 2026-06-16 | 今日 | 企业代理环境刚需，66天 |
| [#88435](https://github.com/NousResearch/hermes-agent/pull/88435) F1-F10 安全加固守卫 | 2026-08-17 | 今日 | **安全 PR 被 block**，两个代码阻塞点待解决，涉及凭证 ACL、MCP 信任边界 |

### 特别警示

- **PR #88435**（安全加固）标记为 "Not merge-ready"，但覆盖凭证 ACL、hooks 门控、cron 脚本隔离、MCP 信任等 10 项安全守卫。在 [#90950](https://github.com/NousResearch/hermes-agent/issues/90950) 数据损坏、[#91265](https://github.com/NousResearch/hermes-agent/issues/91265) OAuth 竞态等安全相关事件频发的背景下，**安全债务的累积速度可能超过功能交付**。

---

*日报生成时间：2026-08-21 | 数据来源：NousResearch/hermes-agent GitHub 公开活动*

</details>

<details>
<summary><strong>PicoClaw</strong> — <a href="https://github.com/sipeed/picoclaw">sipeed/picoclaw</a></summary>

# PicoClaw 项目动态日报 | 2026-08-21

> **项目**: [sipeed/picoclaw](https://github.com/sipeed/picoclaw) | **日期**: 2026-08-21 | **角色**: AI 智能体与个人 AI 助手开源项目

---

## 1. 今日速览

今日 PicoClaw 项目呈现**中等活跃度**，以依赖维护和社区问题跟进为主。PR 侧完成 4 项合并/关闭操作，包括一项重要的 Anthropic 原生协议支持和技能系统 CLI 重构，显示核心功能仍在推进；但 5 个待合并 PR 全部为 Dependabot 依赖更新，且 Issues 侧 3 条均为 stale 状态无新进展，反映**社区驱动的新功能开发和 Bug 修复节奏有所放缓**。无新版本发布，整体处于"维护性迭代"阶段。

---

## 2. 版本发布

**无新版本发布**

---

## 3. 项目进展

### 今日合并/关闭的重要 PR

| PR | 作者 | 状态 | 核心贡献 |
|:---|:---|:---|:---|
| [#1158](https://github.com/sipeed/picoclaw/pull/1158) | hyperwd | **已关闭** | **新增 `anthropic-messages` 协议前缀**，支持 Anthropic 原生 Messages API 格式（`/v1/messages`），解决仅支持原生格式的代理服务兼容性问题（Fixes #269） |
| [#714](https://github.com/sipeed/picoclaw/pull/714) | seanly | **已关闭** | **技能系统 CLI 重构**：新增 `install`/`reinstall` 子命令，支持 `repo@branch` 语法、可选子路径、GitHub Trees API 完整目录获取，生产环境安装更健壮 |
| [#423](https://github.com/sipeed/picoclaw/pull/423) | Leeaandrob | **已关闭** | **多智能体协作框架基座**（WIP）：基于 #213 和 #131 构建共享上下文池（Blackboard）、智能体交接与发现工具，为复杂多智能体场景奠基 |
| [#3318](https://github.com/sipeed/picoclaw/pull/3318) | nuestraai | **已关闭** | **修复 pnpm-lock.yaml 重复键问题**：`semver@7.8.5` 重复声明导致 pnpm 拒绝解析，影响前端构建 |

**整体推进评估**：今日关闭的 PR 覆盖**协议兼容性**（Anthropic）、**开发者体验**（技能 CLI）、**架构演进**（多智能体框架）三个维度，属于项目中期建设的关键拼图。但 #423 标记为 WIP 即关闭，可能意味着多智能体功能将拆分重组，需关注后续替代 PR。

---

## 4. 社区热点

### 最活跃讨论：Web UI 输入延迟问题
- **[#3281 [BUG] Web UI chat input is very laggy when history has a little bit long](https://github.com/sipeed/picoclaw/issues/3281)**
  - 作者: xpader | 6 条评论 | 👍 1 | 创建于 2026-07-21，最后更新 2026-08-20
  - **核心诉求**：会话历史累积后 Web UI 输入框严重卡顿，影响基础使用体验
  - **背后分析**：这是**高频使用场景的性能瓶颈**——用户实际深度使用产品后必然触发，而非边缘 case。1 个点赞反映社区共鸣度有限，但 6 条评论显示维护者与用户有持续交互。问题 stale 近一个月无修复 PR，存在**体验恶化风险**。

### 功能扩展诉求：ASR 模型解耦
- **[#3331 [Feature] 支持非 Whisper 模型的 /audio/transcriptions 端点](https://github.com/sipeed/picoclaw/issues/3331)**
  - 作者: stanislavvv | 1 条评论 | 创建于 2026-08-13
  - **核心诉求**：当前 ASR 强制匹配 `*whisper*` 模型名，用户希望使用更新更快的替代模型（如更快的转写模型）
  - **背后分析**：反映**硬编码模型选择策略**对技术迭代的束缚，社区期望更灵活的模型配置机制。

### 架构灵活性：动态模型覆盖
- **[#3330 [Feature] delegate/spawn/subagent 工具支持调用时动态指定模型](https://github.com/sipeed/picoclaw/issues/3330)**
  - 作者: v2up-32mb | 1 条评论 | 创建于 2026-08-13
  - **核心诉求**：多智能体工具链中打破静态模型绑定，实现"轻任务用轻模型、重任务用重模型"的精细化成本控制
  - **背后分析**：与 #3331 形成呼应——用户期望**从"配置驱动"转向"调用驱动"的模型选择范式**，这是多智能体系统成熟度的关键指标。

---

## 5. Bug 与稳定性

| 严重程度 | 问题 | 状态 | Fix PR | 链接 |
|:---|:---|:---|:---|:---|
| 🔴 **高** | Web UI 输入卡顿（历史记录较长时） | **OPEN, stale** | ❌ 无 | [#3281](https://github.com/sipeed/picoclaw/issues/3281) |
| 🟡 中 | pnpm-lock.yaml 重复键导致构建失败 | **已修复** | ✅ [#3318](https://github.com/sipeed/picoclaw/pull/3318) | — |

**风险评估**：#3281 是唯一活跃 Bug，但 stale 状态且影响核心交互路径。建议优先排查前端状态管理（可能的 React 渲染优化或虚拟滚动缺失）。

---

## 6. 功能请求与路线图信号

| 需求 | 关联 PR/Issue | 纳入可能性 | 判断依据 |
|:---|:---|:---|:---|
| Anthropic 原生 Messages API 支持 | ✅ [#1158](https://github.com/sipeed/picoclaw/pull/1158) 已合并 | **已落地** | 关闭状态，功能可用 |
| 技能系统 CLI 重构 | ✅ [#714](https://github.com/sipeed/picoclaw/pull/714) 已合并 | **已落地** | 生产级安装流程 |
| 多智能体协作框架 | ⚠️ [#423](https://github.com/sipeed/picoclaw/pull/423) WIP 关闭 | **重构中** | 架构基座已建，待拆分 PR |
| 动态模型覆盖（delegate/spawn/subagent） | [#3330](https://github.com/sipeed/picoclaw/issues/3330) | **高** | 与 #423 多智能体路线强相关，技术债务明显 |
| ASR 模型解耦（非 Whisper 支持） | [#3331](https://github.com/sipeed/picoclaw/issues/3331) | **中** | 需修改 `asr.go` 硬编码逻辑，改动面可控 |

**路线图信号**：项目正从"单智能体工具"向"多智能体协作平台"演进，#423 的 Blackboard 架构与 #3330 的动态模型需求形成**技术-需求闭环**，预计下一版本将重点完善智能体编排层。

---

## 7. 用户反馈摘要

### 真实痛点
- **性能瓶颈**："会话历史多一点，输入框就开始卡"（#3281）—— 深度用户的日常摩擦
- **模型锁定焦虑**：Whisper 被描述为"too old and slow"（#3331），用户感知到技术债
- **成本/灵活性权衡**：希望在子任务级别灵活切换模型，而非全局配置（#3330）

### 使用场景
- **长会话工作流**：用户持续在同一 session 中积累上下文，非短问答模式
- **多模型混合部署**：同时使用商业 API 和自托管服务，需要精细路由
- **语音交互扩展**：语音输入场景对 ASR 延迟敏感，驱动模型替换需求

### 满意度暗示
- 技能安装 CLI 重构（#714）的详细设计表明**开发者工具链体验受重视**
- Anthropic 协议支持的快速落地显示**协议兼容性响应积极**

---

## 8. 待处理积压

### 依赖更新堆积（需维护者决策）

| PR | 内容 | 滞留时间 | 风险 |
|:---|:---|:---|:---|
| [#3336](https://github.com/sipeed/picoclaw/pull/3336) | AWS Bedrock Runtime 1.53.3 → 1.57.1 | 8 天 | 云服务 API 兼容性 |
| [#3335](https://github.com/sipeed/picoclaw/pull/3335) | AWS SDK Config 1.32.25 → 1.32.35 | 8 天 | 凭证链行为变更 |
| [#3334](https://github.com/sipeed/picoclaw/pull/3334) | Anthropic SDK 1.55.1 → 1.62.0 | 8 天 | ⚠️ **与 #1158 功能相关，建议优先** |
| [#3333](https://github.com/sipeed/picoclaw/pull/3333) | Matrix 协议库 mautrix 0.27.0 → 0.29.0 | 8 天 | 即时通讯功能稳定性 |
| [#3332](https://github.com/sipeed/picoclaw/pull/3332) | AWS SDK Core 1.42.0 → 1.43.4 | 8 天 | 基础依赖，影响面广 |

**提醒**：5 个 Dependabot PR 均标记 stale 且同日更新，可能存在**批量阻塞**（CI 失败或维护者未审阅）。建议优先处理 #3334（Anthropic SDK），与新合并的 #1158 形成完整功能链。

### 长期未响应 Issue
- **[#3281](https://github.com/sipeed/picoclaw/issues/3281)** Web UI 性能问题：stale 近 1 个月，影响核心用户体验，建议分配前端专项排查。

---

> **日报生成依据**：GitHub Issues/PRs 元数据、标签状态、时间戳、评论数量及内容摘要。健康度评分：🟡 **中等**（功能推进正常，社区响应速度待提升）。

</details>

<details>
<summary><strong>NanoClaw</strong> — <a href="https://github.com/qwibitai/nanoclaw">qwibitai/nanoclaw</a></summary>

# NanoClaw 项目动态日报 | 2026-08-21

> **项目**: [NanoClaw](https://github.com/qwibitai/nanoclaw) — AI 智能体与个人 AI 助手开源框架  
> **数据周期**: 过去 24 小时（截至 2026-08-21）

---

## 1. 今日速览

NanoClaw 今日呈现**高强度维护态势**：50 个 PR 更新（35 待合并/15 已处理），3 个 Issue 更新，核心团队正集中推进 **v2 安装体系的稳定性修复**。今日无新版本发布，但 gavrielc 主导的"技能栈审计"系列 PR（#3414-#3420）批量修复了 7 个技能的配置失效、并发崩溃和文档漂移问题，显示项目正从功能扩张期转入**质量加固期**。Slack 集成与 Cursor 提供商的新功能同步推进，社区对多平台适配的诉求持续升温。

---

## 2. 版本发布

**无新版本发布。** 最新 Release 仍为历史版本。

---

## 3. 项目进展

### 已合并/关闭的重要 PR

| PR | 作者 | 说明 | 推进价值 |
|:---|:---|:---|:---|
| [#1311](https://github.com/nanocoai/nanoclaw/pull/1311) `Feature create new session` | wmalgadey | 关闭（未合并） | 会话创建功能探索终止，团队可能选择其他实现路径 |
| [#3421](https://github.com/nanocoai/nanoclaw/pull/3421) `docs+setup: announce one-click Slack agents` | gavrielc | **关闭**（被 #3404 取代） | Slack 一键部署的宣传层方案重构，说明产品化叙事在调整 |

### 待合并的关键 PR（核心团队主导）

| PR | 说明 | 影响面 |
|:---|:---|:---|
| [#3402](https://github.com/nanocoai/nanoclaw/pull/3402) `fix(codex): deliver provider-generated files` | 新增 `send_file` 显式路径与所有权契约，修复文件传递失败 | **核心文件传输架构** |
| [#3403](https://github.com/nanocoai/nanoclaw/pull/3403) `fix(matrix): use a refresh-safe ESM patch` | Node 22 兼容的 ESM 补丁机制 | **Matrix 渠道稳定性** |
| [#3355](https://github.com/nanocoai/nanoclaw/pull/3355) + [#3356](https://github.com/nanocoai/nanoclaw/pull/3356) `add /add-cursor agent provider skill` | Cursor Agent SDK 集成 | **新提供商生态扩展** |

**整体推进评估**: 今日核心团队以"修复即功能"模式推进，7 个技能修复 PR 构成 **#3408 技术债清偿波次**，为 v2 安装体系（基于 `installSlug` 的多实例隔离）扫除落地障碍。

---

## 4. 社区热点

### 讨论焦点：Slack 集成的权限缺口

| 条目 | 热度指标 | 核心诉求 |
|:---|:---|:---|
| [#3423](https://github.com/nanocoai/nanoclaw/pull/3423) `fix(add-slack): add missing app_mentions:read bot scope` | 当日新建，直指文档错误 | **Slack 事件订阅与权限范围的文档-实现一致性** |

**分析**: 该 PR 揭示 NanoClaw 的 Slack 设置向导存在**步骤间依赖断裂**——步骤 6 要求订阅 `app_mention` 事件，但步骤 2 的 scope 列表遗漏必需的 `app_mentions:read`。这是典型的"快速迭代型文档漂移"，反映社区用户（marcelomarra）在实际配置中遭遇阻塞后主动修复。结合 #3421 的关闭，说明 Slack 一键部署的产品化叙事正从"宣传优先"转向"配置可靠性优先"。

### 路由逻辑争议：mention-sticky 的语义边界

| 条目 | 热度指标 | 核心诉求 |
|:---|:---|:---|
| [#3422](https://github.com/nanocoai/nanoclaw/pull/3422) `fix(router): mention-sticky subscribes on a mention, not on a session` | 当日新建，关联活跃 Issue | **线程粘性行为的可预测性** |

**分析**: 该 PR 直接回应 [#3369](https://github.com/nanocoai/nanoclaw/issues/3369)（见 Bug 节），社区开发者 teran13 在 Issue 报告当日即提交修复，显示**路由层 engage_mode 的语义设计存在系统性模糊**——`accumulate` 作为"静默上下文"存储机制，意外创建了会话订阅关系。

---

## 5. Bug 与稳定性

| 严重度 | Issue/PR | 描述 | 状态 |
|:---|:---|:---|:---|
| 🔴 **高** | [#2715](https://github.com/nanocoai/nanoclaw/issues/2715) `Inbound WhatsApp media unreachable` | WhatsApp 附件下载到未挂载的 `DATA_DIR/attachments`，agent 容器内路径 `/workspace/attachments/...` 不存在，导致**图像/文档/音频完全无法访问** | **开放**（创建于 2026-06-08，已挂起 73 天） |
| 🔴 **高** | [#3369](https://github.com/nanocoai/nanoclaw/issues/3369) `mention-sticky engages without a mention` | `accumulate` 策略在 Slack 线程中**无需 @提及即自动回复**，违反 `mention-sticky` 契约 | **开放**，[#3422](https://github.com/nanocoai/nanoclaw/pull/3422) **待合并修复** |
| 🟡 **中** | [#2606](https://github.com/nanocoai/nanoclaw/issues/2606) `engage_mode='always' silently drops messages` | `evaluateEngage()` 缺少 `always` case，消息被静默丢弃（`no_agent_engaged`） | **已关闭**（2026-08-20） |
| 🟡 **中** | [#3247](https://github.com/nanocoai/nanoclaw/pull/3247) `retire malformed cron string` | 无效 cron 表达式（如 `0 21-5 * * *`）导致**每 sweep tick 重复报错**，而非优雅退役 | **待合并** |
| 🟡 **中** | [#3414](https://github.com/nanocoai/nanoclaw/pull/3414) `cap refresh fan-out` | CLI 仪表板刷新时 29 个并发 `bin/ncl` 进程导致 **27/29 超时**，2-vCPU 主机上**几乎每个 tab 报错** | **待合并** |

**稳定性评估**: 今日暴露的 bug 呈现 **"v2 架构挂载/隔离假设"与"实际部署环境"的系统性错位**——WhatsApp 附件路径、installSlug 多实例、容器内外路径映射等问题集中爆发，说明 v2 的容器化边界设计需要更严格的集成测试覆盖。

---

## 6. 功能请求与路线图信号

| 来源 | 信号 | 纳入可能性评估 |
|:---|:---|:---|
| [#3355](https://github.com/nanocoai/nanoclaw/pull/3355) + [#3356](https://github.com/nanocoai/nanoclaw/pull/3356) Cursor Agent SDK | 新提供商集成（代码编辑器生态） | **高** — 核心团队 zvi-fried 主导，已进入双 PR 并行阶段 |
| [#3189](https://github.com/nanocoai/nanoclaw/pull/3189) `add-why` skill | 消息级可解释性工具（"这条消息发生了什么"） | **中** — teran13 的 utility skill，8 月 5 日创建，持续更新中，符合可观测性趋势 |
| [#3270](https://github.com/nanocoai/nanoclaw/pull/3270) `Feat/ncl token usage` | NCL token 用量追踪 | **中** — 计费/成本管控需求，16 日创建，可能关联未来企业版功能 |
| [#3196](https://github.com/nanocoai/nanoclaw/pull/3196) `Fix/add mount readonly` | 只读挂载安全加固 | **高** — 安全基线修复，符合当前质量加固主题 |

**路线图判断**: 短期（1-2 周）聚焦 **Cursor 提供商上线 + v2 技能栈稳定性**；中期（1 个月）可能释放 **token 计费可观测性** 作为商业化前奏。

---

## 7. 用户反馈摘要

### 真实痛点

| 来源 | 场景 | 情绪 |
|:---|:---|:---|
| [#2715](https://github.com/nanocoai/nanoclaw/issues/2715) jon-ruth | "Agent 完全无法打开用户发送的图片/文档/音频" — **核心功能断裂** | 😤 挫败（73 天未解决） |
| [#3369](https://github.com/nanocoai/nanoclaw/issues/3369) nilsborg | "Agent 在我从未 @它的线程里突然开始回复" — **行为不可预测，隐私/噪音焦虑** | 😰 困惑+担忧 |
| [#3423](https://github.com/nanocoai/nanoclaw/pull/3423) marcelomarra | "按文档步骤走，Slack bot 权限不足" — **文档信任损耗** | 😤 阻塞性挫败 |

### 隐含需求

- **路径透明性**: 用户需要理解"文件在宿主机 vs 容器内"的映射关系（#2715）
- **engage_mode 行为契约文档化**: 当前模式组合（`mention-sticky` × `accumulate`）的交互效应缺乏形式化说明
- **多安装实例运维**: `installSlug` 引入后，全局 `ncl` shim 与实例隔离的冲突（#3419 等批量修复反映）

---

## 8. 待处理积压

| 条目 | 挂起时间 | 风险说明 | 建议动作 |
|:---|:---|:---|:---|
| [#2715](https://github.com/nanocoai/nanoclaw/issues/2715) WhatsApp 媒体不可达 | **73 天**（2026-06-08） | v2 核心渠道功能断裂，直接影响 WhatsApp 场景可用性；可能与 #3402 文件传输重构存在架构关联 | **优先评估**是否可被 #3402 的 `send_file` 机制覆盖，或需独立的挂载方案设计 |
| [#3189](https://github.com/nanocoai/nanoclaw/pull/3189) `add-why` skill | 16 天 | 可解释性工具，社区贡献质量高，但缺乏维护者 review 带宽 | 分配 UX/可观测性模块负责人 |
| [#3247](https://github.com/nanocoai/nanoclaw/pull/3247) cron 错误处理 | 7 天 | 日志噪音问题，影响运维体验，修复方案清晰（退役而非重试） | 快速合并，低风险 |

---

> **健康度评分**: 🟢🟢🟢🟡⚪ (3.5/5)  
> **优势**: 核心团队响应极快（当日 Issue → 当日 PR #3422），技能栈审计展现质量意识  
> **风险**: 长期挂起 Issue（#2715）侵蚀渠道可靠性信任，v2 架构的容器边界假设需更多生产验证

---

</details>

<details>
<summary><strong>NullClaw</strong> — <a href="https://github.com/nullclaw/nullclaw">nullclaw/nullclaw</a></summary>

过去24小时无活动。

</details>

<details>
<summary><strong>IronClaw</strong> — <a href="https://github.com/nearai/ironclaw">nearai/ironclaw</a></summary>

# IronClaw 项目动态日报 | 2026-08-21

## 1. 今日速览

IronClaw 今日呈现**高强度工程推进态势**：24小时内 Issues 活跃21条（17条新开/活跃，4条关闭）、PR 更新37条（23条待合并，14条已合并/关闭），无新版本发布。核心特征为**大规模基础设施重构与生命周期架构升级并行**——用户沙箱代理化（#7732 Step 2）、Agent 生命周期钩子系统（#7770 Phase 1）、WebUI 设计系统五阶段计划全面启动，同时伴随密集的代码清理债务偿还。项目健康度良好，但合并队列曾受 Clippy 1.98 lint 级联阻塞（已修复 #7777），需注意工具链浮动策略的稳定性风险。

---

## 2. 版本发布

**无新版本发布**

---

## 3. 项目进展

### 已合并/关闭的重要 PR

| PR | 作者 | 核心贡献 | 项目推进意义 |
|:---|:---|:---|:---|
| [#7729](https://github.com/nearai/ironclaw/pull/7729) | serrrfirat | **Automation "run-now" 手动触发功能** — 原子化手动点火路径，保留自动化调度计划的同时创建域隔离的点火身份与溯源，贯通 capability → assistant service → WebUI API → 本地化 UI | 补齐自动化产品核心缺口（对应关闭 Issue #7193），使"模型、WebUI、产品表面"均可按需触发自动化，从"只能看不能动"进化为完整 CRUD+Fire 能力 |
| [#7777](https://github.com/nearai/ironclaw/pull/7777) | henrypark133 | **紧急修复 CI 阻塞** — 清除 Clippy 1.98 提升的多项 lint 导致的级联失败，恢复合并队列通行 | 保障工程流水线健康，避免主分支持续变红阻断交付 |
| [#7786](https://github.com/nearai/ironclaw/pull/7786) | henrypark133 | **SEV 级修复：OpenAI 模型建议生成崩溃** — `uniqueItems` 破坏所有 OpenAI 结构化输出；清理死亡 allowlist ID；基于扩展连接状态门控卡片 | 消除生产环境 OpenAI 路径的完全失效，属高优先级热修复 |
| [#7738](https://github.com/nearai/ironclaw/pull/7738) | thisisjoshford | Slack 部署配置卡片逐字段帮助文本 | 运营者体验提升，延续 #7550 的 admin configuration seam 模式 |
| [#7763](https://github.com/nearai/ironclaw/pull/7763) | henrypark133 | **子代理设计文档大整合** — 7份文档、7000+行、三代设计、多处矛盾 → 单一 canonical README，净减 9,713 行 | 大幅降低子代理领域的认知负荷与文档债务，为 #7770 钩子系统的子代理生命周期扩展扫清上下文障碍 |

**整体迈进评估**：今日合并聚焦"产品功能补齐（run-now）+ 工程基础设施（CI/文档）+ 生产稳定性（OpenAI SEV）"三线，属于典型的"还债与筑基"日。无突破性架构落地，但为 #7779（沙箱代理化）和 #7765（AfterTurn 钩子）两大 XL 级 PR 的合并创造了清洁基线。

---

## 4. 社区热点

### 讨论最活跃的 Issues

| 排名 | Issue | 评论数 | 热度分析 |
|:---|:---|:---|:---|
| 🔥1 | [#7732](https://github.com/nearai/ironclaw/issues/7732) Epic: Persistent per-user sandbox with iron-proxy | **8 评论** | **v1.4.0 核心基础设施**。社区高度关注"每次 shell 命令创建销毁容器"的性能与状态持久化问题。Step 2 PR #7779 已开，但 Step 1 的持久化 `/workspace` 与 `(tenant, user)` 隔离策略仍是开放设计空间，评论围绕 Docker 网络模型与多租户安全边界展开 |
| 🔥2 | [#7770](https://github.com/nearai/ironclaw/issues/7770) Epic: hook the agent lifecycle | **3 评论** | **架构扩展性核心议题**。将"when X happens, do Y"从引擎硬编码转为钩子注册，直接决定第三方扩展能力天花板。Phase 1 PR #7765 已提交，评论聚焦 `AfterTurn` 的权限模型（Builtin/Trusted vs Installed/SelfAuthored 的拒绝策略） |
| 🔥3 | [#7038](https://github.com/nearai/ironclaw/issues/7038) / [#7781](https://github.com/nearai/ironclaw/issues/7781) / [#7782](https://github.com/nearai/ironclaw/issues/7782) WebUI Design System 五阶段拆分 | **2-1 评论** | **大规模计划重组信号**。原 Epic #7038 被拆分为 #7038(Phase1) → #7781(Phases2-3) → #7782(Phases4-5)，#7733 因重复被关闭。反映设计系统从"一次性交付"转向"波浪式迭代"，社区需适应新的跟踪粒度 |

**背后诉求分析**：
- **开发者体验优先**：#7732 的容器生命周期问题、#7770 的钩子系统，均指向"让外部开发者不碰核心引擎即可扩展"
- **可视化/可维护性焦虑**：Design System 的激进拆分（5 阶段 → 3 个 Epic）反映前端团队对"长期并行跟踪清晰度"的诉求
- **安全与性能的张力**：沙箱代理化方案中，per-user Docker 网络隔离 vs 共享代理的吞吐量权衡是隐性争论点

---

## 5. Bug 与稳定性

| 严重程度 | Issue | 描述 | 状态 | Fix PR |
|:---|:---|:---|:---|:---|
| 🔴 **SEV（已修复）** | [#7786](https://github.com/nearai/ironclaw/pull/7786) 隐含 | OpenAI 模型 `uniqueItems` 导致结构化输出验证失败，建议生成功能完全不可用 | **已合并** #7786 | ✅ #7786 |
| 🟡 **Medium** | [#7783](https://github.com/nearai/ironclaw/issues/7783) | **LLM 超时策略缺陷**：非流式 HTTP 客户端的 structured-output finalization 无法测量 TTFT（首 token 时间），60s 墙钟上限与 75s finalization deadline 冲突，单点传输故障摧毁运行 | **Open**，1 评论 | ❌ 无 |
| 🟡 **Medium** | [#7776](https://github.com/nearai/ironclaw/issues/7776) | `memory.write` 的 CAS 保护不完整：`append: false` 的全文档重写可能静默覆盖并发写入（read-modify-write 的 ABA 问题） | **Open**，0 评论 | ❌ 无 |
| 🟢 **Low** | [#7767](https://github.com/nearai/ironclaw/issues/7767) | Automation presenter 日期测试时区脆弱性，Asia/Shanghai 等时区失败 | **Open**，0 评论 | ❌ 无 |
| 🟢 **Low（已关闭）** | [#7308](https://github.com/nearai/ironclaw/issues/7308) | Attio MCP OAuth 注册 scope 无效且无法修正 | **已关闭** | 未明确，可能由配套 issue 解决 |

**稳定性风险评估**：#7783 的 LLM 超时策略属于**系统性可靠性缺陷**——非流式路径的观测盲区 + 重试预算与 deadline 的硬冲突，可能导致高价值运行被误杀。建议优先分配资源，因其影响所有依赖结构化输出的生产工作负载。

---

## 6. 功能请求与路线图信号

| 功能方向 | 载体 | 纳入 v1.4.0 概率 | 判断依据 |
|:---|:---|:---|:---|
| **Persistent per-user sandbox** | Epic #7732 + PR #7779 | 🔒 **已锁定** | 明确标注 `v1.4.0`，Step 2 PR 已开，serrrfirat 主导 |
| **Agent 生命周期钩子系统** | Epic #7770 + PR #7765 | 🔒 **已锁定** | Phase 1 PR 已提交，AfterTurn + memory curation 作为首个消费者，架构扩展性属核心战略 |
| **WebUI Design System 全五阶段** | #7038/#7781/#7782 + PR #7750 | 🟡 **高概率，但分期交付** | Phase 1 PR #7750 待合并，Phase 2-3 已拆分为独立 Epic，rdisandro 持续投入 |
| **Unbound runs 门控跳过** | Issue #7775 | 🟡 **可能跟进** | #7770 Phase 1 的"故意开放决策"，需看 #7765 合并后的反馈 |
| **AfterTurn 调度器侧失败终端化** | Issue #7780 | 🟢 **技术债务，后续补齐** | 明确标记为 #7770 Phase 1 的 follow-up，非阻塞但需追踪 |
| **Extension setup phase/blockers 暴露** | Issue #7769 | 🟢 **产品体验优化，中等优先级** | Configure 模态的完整性问题，影响扩展配置可靠性 |

**路线图信号**：v1.4.0 的两大支柱（沙箱持久化、钩子架构）已进入工程执行阶段，Design System 作为用户体验基础设施并行推进。无证据表明有功能被挤出或延期。

---

## 7. 用户反馈摘要

> ⚠️ 注：今日 Issues 以工程团队内部跟踪为主，直接用户声音有限。以下从 issue 描述与评论中提炼**隐含用户/开发者体验信号**：

| 痛点/场景 | 来源 | 本质诉求 |
|:---|:---|:---|
| **"无法按需触发自动化"** | #7193（已关闭） | 用户需要"测试/调试/紧急响应"时的手动干预能力，而非仅被动等待调度 |
| **"每次 shell 命令创建销毁容器"** | #7732 | 开发者期望"像本地终端一样"的持久化状态，当前模型破坏工作流连续性 |
| **"扩展配置错误地报告无需配置"** | #7769 | 运营者被误导性 UI 状态困扰，需要权威的单真相源（setup phase + blockers） |
| **"OpenAI 模型完全无法生成建议"** | #7786（隐含用户影响） | 模型选择不应导致功能级降级，多提供商兼容性需作为一等公民 |
| **"时区敏感的测试在 CI 外失败"** | #7767 | 全球贡献者的本地开发体验被忽视，"在我机器上能跑"的反模式 |

**满意度信号**：run-now 功能的快速落地（#7193 创建 8/4 → 关闭 8/20）反映产品-工程协作效率；Design System 的文档化程度（PR #7257 的 north-star proposal）显示前端团队的专业成熟度。

---

## 8. 待处理积压

| 风险等级 | 项目 | 创建时间 | 最后更新 | 阻塞原因/提醒 |
|:---|:---|:---|:---|:---|
| 🟠 **高** | [PR #7700](https://github.com/nearai/ironclaw/pull/7700) Notifications: authoritative run outcomes | 2026-08-17 | 2026-08-21 | XL 级，评论 `undefined`（数据异常？），与 #7699/#7698 形成通知中心三部曲，需确认依赖关系避免合并顺序错误 |
| 🟠 **高** | [PR #7456](https://github.com/nearai/ironclaw/pull/7456) Reborn durable storage profile-agnostic | 2026-08-10 | 2026-08-21 | 运行 11 天，XL 级重构，可能受 #7732 沙箱代理化方案影响，需评估是否需 rebase 协调 |
| 🟡 **中** | [PR #7711](https://github.com/nearai/ironclaw/pull/7711) WASM typed tool response + guest migration | 2026-08-17 | 2026-08-21 | 明确 supersede #7703，需确认 0.3.0 兼容 shim 的删除是否影响下游消费者 |
| 🟡 **中** | [Issue #7755](https://github.com/nearai/ironclaw/issues/7755) / [PR #7752](https://github.com/nearai/ironclaw/pull/7752)（未展示） | 2026-08-19 | 2026-08-20 | 明确标注"**Neither should land until #7752 merges**"，需追踪 #7752 状态防止误合并 |
| 🟢 **低** | [PR #7749](https://github.com/nearai/ironclaw/pull/7749) Benchmark trigger PR | 2026-08-19 | 2026-08-21 | 自述"Close once the run finishes"，需确认 benchmark 运行状态，避免长期悬挂 |

---

**日报生成时间**：2026-08-21  
**数据覆盖**：过去 24 小时 GitHub 活动  
**项目地址**：[github.com/nearai/ironclaw](https://github.com/nearai/ironclaw)

</details>

<details>
<summary><strong>LobsterAI</strong> — <a href="https://github.com/netease-youdao/LobsterAI">netease-youdao/LobsterAI</a></summary>

# LobsterAI 项目动态日报 | 2026-08-21

> **项目**: [netease-youdao/LobsterAI](https://github.com/netease-youdao/LobsterAI)  
> **日期**: 2026-08-21  
> **分析师**: AI 智能体与开源项目分析助手

---

## 1. 今日速览

今日 LobsterAI 项目呈现**低活跃、高清理**特征：24小时内无新 Issue/PR 创建，所有活动均为**历史积压项的批量关闭**（2个 Issue、6个 PR）。唯一待合并 PR #1550 为4月遗留的定时任务修复，已停滞4个月。整体判断：项目处于**维护模式收缩期**，核心团队可能正集中处理8月中旬版本发布后的技术债务，但社区贡献动能明显不足，需关注长期健康度。

---

## 2. 版本发布

**无新版本发布**

最新 Release 仍为历史版本，今日无更新。

---

## 3. 项目进展

### 今日合并/关闭的 6 个 PR 全览

| PR | 作者 | 领域 | 核心贡献 | 状态 |
|:---|:---|:---|:---|:---|
| [#2513](https://github.com/netease-youdao/LobsterAI/pull/2513) | liugang519 | 多区域（renderer/docs/main/artifacts） | **8.17 版本库更新** — 疑似8月中旬版本的功能聚合 PR | ✅ 关闭 |
| [#1215](https://github.com/netease-youdao/LobsterAI/pull/1215) | mingoLzm | IM 模块 | **修复配置热更新失效** — `setConfig` 在平台特定保存（钉钉/电报凭证）时因缺少 `settings` 键跳过 `updateChatHandler()`，导致系统提示词、技能等变更不生效 | ✅ 关闭 |
| [#1218](https://github.com/netease-youdao/LobsterAI/pull/1218) | gongzhi-netease | 定时任务 | **重构任务列表排序** — 解决 UUID 随机排序导致新建任务位置不可预测的问题，改为按 `nextRunAtMs` + 创建时间 + 状态优先级排序 | ✅ 关闭 |
| [#1219](https://github.com/netease-youdao/LobsterAI/pull/1219) | choyuenga | 协作模块（性能） | **消除会话列表无效重渲染** — `React.memo` + `useSelector` 合并优化，减少流式输出时的 CPU 占用 | ✅ 关闭 |
| [#1220](https://github.com/netease-youdao/LobsterAI/pull/1220) | choyuenga | 协作模块（性能） | **消除 N+1 查询** — `recentChats()` / `conversationSearch()` 从每会话2次查询降为批量查询，数据库负载优化 | ✅ 关闭 |
| [#1224](https://github.com/netease-youdao/LobsterAI/pull/1224) | MaoQianTu | Agent 模块 | **i18n 修复 + 交互优化** — 硬编码中文标签国际化、Escape 键关闭弹窗、删除操作防重复点击 | ✅ 关闭 |

**进展评估**：今日关闭的 PR 均为 **4月1日-7日创建的积压项**，集中在 **IM 稳定性、定时任务 UX、协作性能、国际化** 四大主题。技术价值中等偏上（尤其 N+1 查询和重渲染优化），但延迟4个月才处理，反映维护节奏偏慢。PR #2513 的"2026.8.17 library"标签暗示团队刚完成一个版本周期，现处于收尾阶段。

---

## 4. 社区热点

### 最高讨论 Issue：[#1223](https://github.com/netease-youdao/LobsterAI/issues/1223) — 硬编码中文标签与 Agent 交互缺陷

| 指标 | 数值 |
|:---|:---|
| 评论数 | 2 |
| 👍 | 0 |
| 关联 PR | [#1224](https://github.com/netease-youdao/LobsterAI/pull/1224)（已关闭）|

**背后诉求分析**：
- **国际化成熟度不足**：核心组件 `CoworkPromptInput.tsx` 存在硬编码中文 `'输入文件'`，直接污染英文用户的 AI 提示词，暴露 i18n 审计漏洞
- **桌面端交互标准缺失**：Agent 弹窗不支持 Escape 键关闭、删除无防重复保护，反映前端交互规范执行不严格
- **修复模式**：三问题捆绑单 PR 修复，符合小步快跑，但 Issue 创建4个月后才处理，响应速度存疑

### 次热 Issue：[#1217](https://github.com/netease-youdao/LobsterAI/issues/1217) — 网关偶发重启

| 指标 | 数值 |
|:---|:---|
| 评论数 | 2 |
| 👍 | 0 |
| 日志附件 | 有（2026-04-01 18:04）|

**核心矛盾**：用户提供了完整日志压缩包，但 Issue 被标记 `stale` 关闭，**无明确修复结论**。网关一天重启 3-5 次属高频故障，关闭动作可能引发用户不满。

---

## 5. Bug 与稳定性

| 严重程度 | 问题 | 来源 | 修复状态 | 风险说明 |
|:---|:---|:---|:---|:---|
| 🔴 **高** | **网关偶发重启**（一天3-5次，Win10 平台） | [#1217](https://github.com/netease-youdao/LobsterAI/issues/1217) | ❌ **无修复，stale 关闭** | 生产环境稳定性威胁，用户已提供日志但未获响应即关闭，可能复发 |
| 🟡 中 | IM 配置热更新失效（钉钉/电报凭证保存后聊天处理器不刷新） | [#1215](https://github.com/netease-youdao/LobsterAI/pull/1215) | ✅ PR #1215 已关闭 | 平台集成场景下的状态同步 Bug，修复逻辑清晰但延迟4个月 |
| 🟡 中 | 定时任务"不通知"模式触发网关校验错误 | [#1550](https://github.com/netease-youdao/LobsterAI/pull/1550) | 🟡 **PR 待合并，停滞4个月** | 会话创建 vs 表单创建的路径不一致，运行时才会暴露，测试覆盖不足 |
| 🟢 低 | 硬编码中文标签混入英文提示词 | [#1223](https://github.com/netease-youdao/LobsterAI/issues/1223) / [#1224](https://github.com/netease-youdao/LobsterAI/pull/1224) | ✅ PR #1224 已关闭 | 影响国际化体验，非功能性故障 |

**稳定性警示**：网关重启问题（#1217）作为唯一高严重度项被无修复关闭，是今日最大风险点。建议维护者重新评估该 Issue 或确认是否在 #2513 版本库更新中间接修复。

---

## 6. 功能请求与路线图信号

**今日无新功能请求**（无新开 Issue/PR）。

从已关闭 PR 推断的**潜在路线图信号**：

| 方向 | 证据 | 纳入下一版本可能性 |
|:---|:---|:---|
| **性能优化（协作模块）** | PR #1219、#1220 连续优化渲染与查询 | ⭐⭐⭐⭐⭐ 高 — 已合并，或已在 8.17 版本 |
| **定时任务 UX 重构** | PR #1218 排序规则重写、PR #1550 投递模式修复 | ⭐⭐⭐⭐☆ 中高 — 基础修复完成，待合并项为边缘场景 |
| **国际化（i18n）加固** | PR #1224 单点修复 | ⭐⭐⭐☆☆ 中 — 需系统性审计，非一次性修复 |
| **IM 多平台集成稳定性** | PR #1215 钉钉/电报热更新 | ⭐⭐⭐☆☆ 中 — 企业用户刚需，但优先级不突出 |

---

## 7. 用户反馈摘要

### 直接痛点（来自 Issue 描述）

| 用户 | 场景 | 痛点 | 情绪信号 |
|:---|:---|:---|:---|
| blueb0ne | Win10 日常使用 | 网关一天随机重启 3-5 次，打断工作流 | 😤  frustration — 提供日志后 Issue 被 stale 关闭，无闭环 |
| MaoQianTu | 英文界面用户 | 提示词中混入中文「输入文件」，专业场景下显得不专业 | 😐 困惑/质量质疑 — 基础 i18n 未做到 |
| 未具名（PR #1218 描述） | 定时任务管理 | 新建任务后找不到任务位置，需全列表扫描 | 😫 效率损耗 — UUID 排序的 UX 反模式 |

### 隐含满意度信号

- **正面**：性能优化 PR（#1219、#1220）由社区贡献者 choyuenga 主动提交，说明存在技术粘性用户愿意深度参与
- **负面**：所有4月 PR 延迟至8月关闭，4个月反馈周期远超社区友好阈值（通常期望 2-4 周）

---

## 8. 待处理积压

### 🚨 需维护者重点关注

| 项目 | 类型 | 创建时间 | 最后更新 | 风险说明 |
|:---|:---|:---|:---|:---|
| [#1550](https://github.com/netease-youdao/LobsterAI/pull/1550) | PR（待合并） | 2026-04-07 | 2026-08-21 | **定时任务"不通知"模式运行时崩溃**，PR 已提供完整修复（去除多余 channel/to 字段），但停滞4个月。该 Bug 为**会话创建路径特有**，表单创建正常，测试覆盖盲区，生产环境可能持续触发 |
| [#1217](https://github.com/netease-youdao/LobsterAI/issues/1217) | Issue（已关闭，建议重开） | 2026-04-01 | 2026-08-21 | **网关偶发重启无修复结论**，用户日志未被分析即 stale 关闭。若 #2513（8.17 library）未包含修复，应重开并关联到网关稳定性专项 |

### 积压健康度评估

- **PR 积压**：1 个（#1550），4个月+
- **Issue 无响应关闭模式**：今日2个 Issue 均为 `stale` 标签关闭，需确认是否为自动化清理策略，避免误伤有效 Bug 报告

---

## 附录：今日数据总览

```
Issues:  更新 2 | 新开 0 | 关闭 2 | 净变化 -2
PRs:     更新 7 | 待合并 1 | 已合并/关闭 6 | 净变化 -5
Releases: 0
活跃贡献者（24h）: 0（全部为历史项的维护者操作）
```

---

> **明日关注**：PR #1550 是否会获得合并或进一步 review；网关重启 Issue #1217 是否有用户要求重开或维护者补充说明。

</details>

<details>
<summary><strong>Moltis</strong> — <a href="https://github.com/moltis-org/moltis">moltis-org/moltis</a></summary>

# Moltis 项目动态日报 | 2026-08-21

> **项目**: [moltis-org/moltis](https://github.com/moltis-org/moltis)  
> **分析日期**: 2026-08-21（数据覆盖 2026-08-20 24h）

---

## 1. 今日速览

Moltis 今日呈现**中等活跃度**，核心特征为"安全加固 + 跨平台修复 + 发布节奏稳定"。过去24小时内无 Issue 活动，但产生 **4 条待合并 PR** 且全部处于 Open 状态，显示代码审查队列存在积压；同时发布 **20260820.01 版本**，表明项目维持着稳定的迭代节奏。整体健康度良好，但 PR 合并效率值得关注——4 条 PR 均创建于昨日且尚未进入合并流程。

---

## 2. 版本发布

### 🔖 [20260820.01](https://github.com/moltis-org/moltis/releases/tag/20260820.01)

| 属性 | 详情 |
|:---|:---|
| **发布时间** | 2026-08-20 |
| **版本号** | 20260820.01（日历版本号，表明日常迭代发布） |

> **⚠️ 数据限制说明**: 该 Release 的详细 changelog、破坏性变更及迁移指南在当前数据中未提供具体内容。建议维护者补充 release notes 以提升透明度。基于同日 PR 内容推断，本次发布可能包含以下方向的更新：
> - Web 模块的沙箱镜像请求验证
> - Gateway 模块的 Snyk 安全扫描固定
> - WhatsApp Markdown 渲染支持
> - Windows 平台 shell hook 兼容性修复

**迁移建议**: 采用日历版本号发布的项目通常保证向后兼容，建议用户常规升级；生产环境建议等待上述 4 条安全相关 PR 合并后再评估。

---

## 3. 项目进展

> **今日合并/关闭 PR**: 0 条  
> **待合并 PR**: 4 条（全部处于审查阶段）

尽管无 PR 完成合并，但 4 条 Open PR 代表了明确的工程推进方向：

| PR | 模块 | 技术价值 | 进展状态 |
|:---|:---|:---|:---|
| [#1222](https://github.com/moltis-org/moltis/pull/1222) `fix(web): validate sandbox image requests` | Web | **供应链安全**: 镜像引用与包名预验证，权限最小化（仅限 operator admin） | 测试完成，待审查 |
| [#1221](https://github.com/moltis-org/moltis/pull/1221) `fix(gateway): pin Snyk Agent Scan` | Gateway | **供应链安全**: 固定 Snyk Agent Scan 0.5.17，移除 mcp-scan fallback，强制 uv | 格式化检查通过，单元测试待完成 |
| [#1220](https://github.com/moltis-org/moltis/pull/1220) `fix(whatsapp): render Markdown in outbound messages` | WhatsApp | **用户体验**: 模型生成的 Markdown 自动转换为 WhatsApp 原生标记，保留原始数据 | 待审查 |
| [#468](https://github.com/moltis-org/moltis/pull/468) `fix(plugins): use cmd.exe on Windows for shell hooks` | Plugins | **跨平台兼容**: Windows 运行时检测，cmd.exe /C 替代 sh -c | 已测试，长期 PR 更新 |

**整体评估**: 项目在安全加固（#1221、#1222）和平台兼容性（#468、#1220）两条主线同步推进，但 **PR 合并吞吐量为零** 暗示审查资源可能成为瓶颈——尤其是 #468 创建于 5 个月前（2026-03-23），昨日才更新，存在明显的积压风险。

---

## 4. 社区热点

> **今日评论/反应数据**: 全部 PR 评论数为 `undefined`，👍 均为 0

由于数据层面无显性社区互动热点，以下从**技术影响面**角度分析最受关注的 PR：

### 🔥 最高优先级: [#1221](https://github.com/moltis-org/moltis/pull/1221) - Snyk Agent Scan 固定
**背后诉求**: 供应链安全攻击（如恶意 MCP 扫描工具注入）已成为 AI 基础设施的核心威胁。社区对"工具版本固定"和"移除 fallback 机制"的需求反映了：
- 企业用户对**可重现安全扫描**的合规要求
- 对 `uv` 工具链标准化的推动（降低 Python 依赖管理攻击面）

### 📱 用户可见度最高: [#1220](https://github.com/moltis-org/moltis/pull/1220) - WhatsApp Markdown 渲染
**背后诉求**: AI 助手输出普遍采用 Markdown，但 WhatsApp 使用 `*bold*`、`_italic_` 等标记。该 PR 的"即时转换 + 原始数据保留"设计表明：
- 用户需要**多渠道一致的内容体验**
- 开发者关注**数据可追溯性**（session history 保留原始格式）

---

## 5. Bug 与稳定性

| 严重程度 | PR/Issue | 描述 | Fix 状态 |
|:---|:---|:---|:---|
| 🔴 **高** | [#1221](https://github.com/moltis-org/moltis/pull/1221) | Snyk 扫描未固定版本，存在供应链投毒风险 | **PR 待合并**，单元测试未完成 ❗ |
| 🟡 **中** | [#1222](https://github.com/moltis-org/moltis/pull/1222) | 沙箱镜像请求缺乏验证，可能导致未授权容器构建 | **PR 待合并**，测试通过 ✓ |
| 🟡 **中** | [#468](https://github.com/moltis-org/moltis/pull/468) | Windows 平台 shell hook 完全失效（`sh -c` 不可用） | **PR 待合并**，已手动测试 ✓ |
| 🟢 **低** | [#1220](https://github.com/moltis-org/moltis/pull/1220) | WhatsApp 消息 Markdown 原样显示，可读性差 | **PR 待合并**，功能增强 |

> **关键风险**: #1221 的单元测试未完成即进入待合并状态，建议维护者要求补充 `cargo test -p moltis-gateway snyk_agent_scan` 验证后再合并。

---

## 6. 功能请求与路线图信号

**基于现有 PR 推断的下一版本方向**：

| 信号来源 | 可能纳入功能 | 置信度 |
|:---|:---|:---|
| #1220 WhatsApp Markdown | **多渠道消息格式自适应引擎**（扩展至 Telegram、Slack 等） | 中 |
| #1222 镜像验证 | **统一沙箱安全策略框架**（覆盖所有容器化工作负载） | 高 |
| #1221 Snyk 固定 | **SBOM 与依赖锁定标准化**（全工具链版本固定） | 高 |
| #468 Windows 兼容 | **原生 Windows 支持**（从"能运行"到"一等公民"） | 中 |

**缺失信号**: 无 Issue 活动意味着缺乏用户直接提出的功能请求，建议维护者开启 GitHub Discussions 收集社区输入。

---

## 7. 用户反馈摘要

> **数据限制**: 今日无 Issue 评论数据，以下从 PR 描述反推用户场景痛点

| 痛点/场景 | 来源 PR | 推断依据 |
|:---|:---|:---|
| **"AI 输出在 WhatsApp 里全是 Markdown 符号，用户看不懂"** | [#1220](https://github.com/moltis-org/moltis/pull/1220) | PR 明确解决"common model-generated Markdown to WhatsApp-native markup" |
| **"Windows 部署完全跑不起来，shell hook 直接崩溃"** | [#468](https://github.com/moltis-org/moltis/pull/468) | 手动测试记录显示 Windows 10 + v0.9.10 环境验证 |
| **"安全扫描工具版本不透明，合规审计过不了"** | [#1221](https://github.com/moltis-org/moltis/pull/1221) | 企业级供应链安全诉求，强制 uv 工具链 |
| **"任何人都能触发镜像构建，权限太松"** | [#1222](https://github.com/moltis-org/moltis/pull/1222) | 明确限制至 operator administrators |

---

## 8. 待处理积压

### ⚠️ 长期未响应 PR

| PR | 创建日期 | 最后更新 | 积压天数 | 风险说明 |
|:---|:---|:---|:---|:---|
| [#468](https://github.com/moltis-org/moltis/pull/468) `fix(plugins): use cmd.exe on Windows for shell hooks` | 2026-03-23 | 2026-08-20 | **150 天** | 🔴 **严重**: Windows 平台基础功能损坏，社区贡献者已提供完整修复 + CI 通过，但 5 个月未合并 |

**维护者行动建议**:
1. **立即审查 #468**: 该 PR 已满足测试要求（Windows 10 手动测试 + CI 通过），延迟合并无技术理由
2. **优先完成 #1221 测试**: 安全相关 PR 不应在单元测试未完成时合并
3. **建立 PR 审查 SLA**: 当前 4 条 PR 全部 pending，建议设定 48h/72h 响应目标

---

## 附录：今日 PR 完整索引

| # | 状态 | 标题 | 作者 | 链接 |
|:---|:---|:---|:---|:---|
| 1222 | OPEN | fix(web): validate sandbox image requests | [tsauvajon](https://github.com/tsauvajon) | [PR #1222](https://github.com/moltis-org/moltis/pull/1222) |
| 1221 | OPEN | fix(gateway): pin Snyk Agent Scan | [tsauvajon](https://github.com/tsauvajon) | [PR #1221](https://github.com/moltis-org/moltis/pull/1221) |
| 1220 | OPEN | fix(whatsapp): render Markdown in outbound messages | [rubenssoto](https://github.com/rubenssoto) | [PR #1220](https://github.com/moltis-org/moltis/pull/1220) |
| 468 | OPEN | fix(plugins): use cmd.exe on Windows for shell hooks | [jmikedupont2](https://github.com/jmikedupont2) | [PR #468](https://github.com/moltis-org/moltis/pull/468) |

---

*本日报基于公开 GitHub 数据生成，Release 详细内容因原始数据缺失未完全覆盖。建议关注 [moltis-org/moltis](https://github.com/moltis-org/moltis) 官方 Release 页面获取完整信息。*

</details>

<details>
<summary><strong>CoPaw</strong> — <a href="https://github.com/agentscope-ai/CoPaw">agentscope-ai/CoPaw</a></summary>

# CoPaw 项目动态日报 | 2026-08-21

## 1. 今日速览

CoPaw 项目今日保持**高活跃度**：24小时内 Issues 更新 32 条（19 活跃/新开，13 关闭），PR 更新 50 条（22 待合并，28 已合并/关闭），并发布 v2.1.1-beta.1 版本。社区讨论集中在**任务执行中断恢复**、**MCP 连接稳定性**、**非 ASCII 文件名处理**三大痛点。PR 侧以测试加固、控制台修复和火山引擎/MiMo 新 provider 集成为主，项目整体处于**快速迭代、密集修 bug** 的阶段，稳定性优先于新功能。

---

## 2. 版本发布

### [v2.1.1-beta.1](https://github.com/agentscope-ai/QwenPaw/releases/tag/v2.1.1-beta.1)

| 变更项 | 详情 |
|--------|------|
| **feat(console)** | 优化编辑器标签页溢出导航（[#6983](https://github.com/agentscope-ai/QwenPaw/pull/6983) by @rayrayraykk） |
| **fix(providers)** | 降低 rate limiter 初始化日志级别，减少启动噪音（[#6988](https://github.com/agentscope-ai/QwenPaw/pull/6988) by @rayrayraykk） |
| **chore** | 更新 release notes |

**评估**：此为**补丁级 beta 版本**，无破坏性变更，主要改善控制台体验和日志噪音。适合正在使用 2.1.x 的用户平滑升级。

---

## 3. 项目进展

### 今日合并/关闭的关键 PR（28 条中精选）

| PR | 作者 | 核心贡献 | 项目意义 |
|:---|:---|:---|:---|
| [#6586](https://github.com/agentscope-ai/QwenPaw/pull/6586) | @wananing | **MCP 会话过期自动恢复** — 服务端重启后自动重连，而非 stuck 在失效 session | 🔴 解决生产环境核心痛点（对应 Issue [#6524](https://github.com/agentscope-ai/QwenPaw/issues/6524)）|
| [#7061](https://github.com/agentscope-ai/QwenPaw/pull/7061) | @xiaoka76 | **修复 OpenAI Responses API 视频帧丢失** — `view_video` 工具结果正确投递给模型 | 修复静默失败，多媒体能力可用性提升 |
| [#7192](https://github.com/agentscope-ai/QwenPaw/pull/7192) / [#7191](https://github.com/agentscope-ai/QwenPaw/pull/7191) | @zhaozhuang521 / @wananing | **非 ASCII（中文）文件名乱码修复** — 从 DataBlock 正确解码 `name` 字段 | 中文用户体验关键修复 |
| [#7187](https://github.com/agentscope-ai/QwenPaw/pull/7187) | @niceIrene | **标题生成禁用 thinking** — 避免推理模型返回 thinking 文本污染标题 | 细节体验优化 |
| [#7152](https://github.com/agentscope-ai/QwenPaw/pull/7152) / [#7155](https://github.com/agentscope-ai/QwenPaw/pull/7155) / [#7178](https://github.com/agentscope-ai/QwenPaw/pull/7178) | @yutai78786 | **测试加固** — 修复 spawn 递归、沙箱时序、浏览器会话竞争等 flaky test | CI 可靠性提升，开发效率保障 |
| [#7092](https://github.com/agentscope-ai/QwenPaw/pull/7092) | @yutai78786 | **CI 精简** — PR gate 移除 console build，缩减依赖安装 | 反馈循环加速 |

**整体迈进**：今日合并 PR 以**"稳定性+国际化"**为主线，MCP 自动恢复和视频帧修复是两大生产级改进，测试/CI 加固为后续快速迭代奠基。

---

## 4. 社区热点

### 讨论最活跃的 Issues

| 排名 | Issue | 评论 | 状态 | 核心诉求 |
|:---|:---|:---|:---|:---|
| 🔥1 | [#6921](https://github.com/agentscope-ai/QwenPaw/issues/6921) 多步骤任务无提示停止，需用户说"继续" | 10 | OPEN | **任务连续性机制缺陷** — Agent 规划完子任务后静默挂起，无视觉反馈，用户不知该等待还是干预 |
| 🔥2 | [#7102](https://github.com/agentscope-ai/QwenPaw/issues/7102) GLM 5.3 冻结超 10 分钟 | 9 | **CLOSED** | Provider 侧响应停滞，已关闭但根因可能未完全消除（用户切换模型解决）|
| 🔥3 | [#6524](https://github.com/agentscope-ai/QwenPaw/issues/6524) MCP 后端重启后客户端无法自动恢复 | 6 | OPEN | **基础设施韧性** — 对应 PR [#6586](https://github.com/agentscope-ai/QwenPaw/pull/6586) 已合并，待验证关闭 |
| 🔥4 | [#6643](https://github.com/agentscope-ai/QwenPaw/issues/6643) 任务产出物按任务分目录存放 | 6 | **CLOSED** | 文件管理组织性需求，已解决 |

**诉求分析**：社区最强烈的呼声是 **"Agent 执行可见性"**（[#6921](https://github.com/agentscope-ai/QwenPaw/issues/6921) 10 评论）和 **"连接韧性"**（[#6524](https://github.com/agentscope-ai/QwenPaw/issues/6524)、[#6932](https://github.com/agentscope-ai/QwenPaw/issues/6932)）。前者是 UX 设计问题，后者是系统工程问题。

---

## 5. Bug 与稳定性

| 严重程度 | Issue | 描述 | Fix PR | 状态 |
|:---|:---|:---|:---|:---|
| 🔴 **P0-核心功能中断** | [#6921](https://github.com/agentscope-ai/QwenPaw/issues/6921) | 多步骤任务规划后**静默停止**，无提示、无错误，用户需手动触发"继续" | 无 | **待诊断** — 疑似 Agent 循环/流式输出状态机 bug |
| 🔴 **P0-连接恢复失败** | [#6932](https://github.com/agentscope-ai/QwenPaw/issues/6932) | 网络瞬断恢复后，LLM 请求**永久失败**，必须重启进程 | 无 | **待修复** — httpx 连接池未重建 |
| 🟡 **P1-健康检查误杀** | [#7156](https://github.com/agentscope-ai/QwenPaw/issues/7156) | Embedding health check **硬编码 5s 超时**，预热后端仍被判定失败，降级 BM25 | 无 | **待修复** — 需暴露配置项 |
| 🟡 **P1-会话数据污染** | [#7193](https://github.com/agentscope-ai/QwenPaw/issues/7193) | Agent **跨会话记忆错乱**，搜索到另一会话内容，执行错误任务 | 无 | **新报告** — 记忆隔离边界问题 |
| 🟡 **P1-文件编码** | [#7136](https://github.com/agentscope-ai/QwenPaw/issues/7136) | 中文文件名 percent-encoded 乱码 | [#7191](https://github.com/agentscope-ai/QwenPaw/pull/7191) [#7192](https://github.com/agentscope-ai/QwenPaw/pull/7192) | **已修复，待合并** |
| 🟢 **P2-历史库膨胀** | [#7168](https://github.com/agentscope-ai/QwenPaw/issues/7168) | `history.db` 被重复落库撑爆至 **7.6GB** | 无 | **已关闭** — 需确认是否彻底解决 |
| 🟢 **P2-流式中断** | [#7162](https://github.com/agentscope-ai/QwenPaw/issues/7162) | `httpx.ReadError` 流式中途断开，**未触发重试** | 无 | **已关闭** — `_get_httpx_retryable()` 漏判 |

**稳定性评估**：今日暴露 **2 个 P0 级系统性缺陷**（任务挂起、网络恢复失败），均涉及核心 Agent 执行引擎和 HTTP 连接管理，需优先投入。MCP 恢复已修复是积极信号。

---

## 6. 功能请求与路线图信号

| Issue | 需求 | 可行性信号 | 纳入下一版本概率 |
|:---|:---|:---|:---|
| [#6436](https://github.com/agentscope-ai/QwenPaw/issues/6436) **自动模型路由** | 按任务复杂度/模态自动选择模型（小模型简单对话、大模型推理、视觉模型看图） | 架构层面需求，影响 core/backend | ⭐⭐⭐⭐ 高 — 与多 provider 战略契合 |
| [#7184](https://github.com/agentscope-ai/QwenPaw/issues/7184) **Agent 级跨会话召回开关** | Scroll 策略下控制是否允许新会话召回历史 | 有详细配置提案 | ⭐⭐⭐⭐ 高 — 记忆系统精细化 |
| [#7182](https://github.com/agentscope-ai/QwenPaw/issues/7182) **Workspace 常驻 Skill** | 减少重复指令，Skill 预加载到 system prompt | 明确 opt-in 设计 | ⭐⭐⭐☆ 中高 — 体验优化 |
| [#7181](https://github.com/agentscope-ai/QwenPaw/issues/7181) **Qwen_Code 作为第三方 harness** | 替代 ACP，服务网络受限用户 | 有 PR 基础 | ⭐⭐⭐☆ 中高 — 生态扩展 |
| [#7196](https://github.com/agentscope-ai/QwenPaw/issues/7196) **推理过程默认折叠** | 减少视觉干扰，类似 Hermes 可配置 | UI 层面，实现成本低 | ⭐⭐⭐⭐⭐ 极高 — 社区呼声高，改动小 |
| [#7159](https://github.com/agentscope-ai/QwenPaw/issues/7159) **QQ 群定时主动消息** | 基于 QQ Bot 开放能力的定时任务 | 依赖外部平台能力 | ⭐⭐☆☆ 中 — 需评估合规性 |

---

## 7. 用户反馈摘要

### 😫 核心痛点

> *"执行多步骤任务时经常自己停止且无任何提示消息...需要我说'继续'才会继续任务"* — [#6921](https://github.com/agentscope-ai/QwenPaw/issues/6921) @rerbin

> *"网络短暂中断并恢复后，QwenPaw 无法自动恢复...必须手动重启"* — [#6932](https://github.com/agentscope-ai/QwenPaw/issues/6932) @tina0501853

> *"任务的产出物全部堆积在 media 目录下，很混乱"* — [#6643](https://github.com/agentscope-ai/QwenPaw/issues/6643) @rerbin（已解决）

### 😐 体验摩擦

> *"一直显示推理过程是严重的视觉干扰，希望可以设置默认是否折叠"* — [#7196](https://github.com/agentscope-ai/QwenPaw/issues/7196) @rerbin

> *"Windows 安装脚本...'删除本地应用缓存'不知道干嘛的，只能猜"* — [#7188](https://github.com/agentscope-ai/QwenPaw/issues/7188) @rerbin

> *"智能体较多的时候，切换智能体要上下滑动，很不方便"* — [#7179](https://github.com/agentscope-ai/QwenPaw/issues/7179) @henryliuwork

### ✅ 满意点

- MCP 自动恢复能力获认可（[#6586](https://github.com/agentscope-ai/QwenPaw/pull/6586) 合并）
- 中文文件名问题得到快速响应（2 个 PR 同时提交）

---

## 8. 待处理积压

| Issue/PR | 创建时间 | 最后更新 | 风险 | 建议行动 |
|:---|:---|:---|:---|:---|
| [#6524](https://github.com/agentscope-ai/QwenPaw/issues/6524) MCP 重启恢复 | 2026-07-28 | 2026-08-21 | PR [#6586](https://github.com/agentscope-ai/QwenPaw/pull/6586) 已合并，Issue 仍 OPEN | **立即验证关闭** |
| [#6436](https://github.com/agentscope-ai/QwenPaw/issues/6436) 自动模型路由 | 2026-07-24 | 2026-08-20 | 4 评论，高价值需求，无官方回应 | 维护者评估纳入 v2.2 路线图 |
| [#6515](https://github.com/agentscope-ai/QwenPaw/pull/6515) Volcengine/MiMo Provider | 2026-07-28 | 2026-08-21 | [Under Review] 近一个月，模型目录已过期 | 加速 review 或拆分合并 |
| [#6581](https://github.com/agentscope-ai/QwenPaw/pull/6581) 冗余多模态警告移除 | 2026-07-30 | 2026-08-21 | OPEN 近一个月，改动小 | 快速 review 合并 |
| [#6921](https://github.com/agentscope-ai/QwenPaw/issues/6921) 任务静默停止 | 2026-08-12 | 2026-08-20 | **10 评论，最高热度，无 assignee** | **紧急分配诊断**，可能是状态机死锁 |

---

**日报编制说明**：基于 GitHub 公开数据，Issues/PR 链接指向 `agentscope-ai/QwenPaw` 仓库。项目健康度评分：**活跃度 A / 稳定性 B+ / 响应速度 B**（P0 issue 响应需加速）。

</details>

<details>
<summary><strong>ZeptoClaw</strong> — <a href="https://github.com/qhkm/zeptoclaw">qhkm/zeptoclaw</a></summary>

过去24小时无活动。

</details>

<details>
<summary><strong>ZeroClaw</strong> — <a href="https://github.com/zeroclaw-labs/zeroclaw">zeroclaw-labs/zeroclaw</a></summary>

# ZeroClaw 项目动态日报 | 2026-08-21

> **项目**: [zeroclaw-labs/zeroclaw](https://github.com/zeroclaw-labs/zeroclaw)  
> **日期**: 2026-08-21  
> **分析师**: AI 智能体与个人 AI 助手领域开源项目分析师

---

## 1. 今日速览

ZeroClaw 今日维持**极高活跃度**，24 小时内 50 条 Issues 更新（45 活跃/新开，5 关闭）与 50 条 PR 更新（48 待合并，2 已合并/关闭），但**合并吞吐量极低**（仅 2/50 PR 完成闭环），显示项目处于**密集开发-评审瓶颈期**。安全与架构领域成为绝对焦点：高风险的 shell 命令确认策略（#7155）、插件出口流量管控（#9582/#9584）、WASM 插件架构（#10076）等核心设计并行推进，而 Rust 代码质量债务清理（#10118）和代理可移植性（#10069）等新议题快速涌入。社区诉求明显从"功能丰富"转向"安全可控"与"生产就绪"。

---

## 2. 版本发布

**无新版本发布**。

---

## 3. 项目进展

| PR | 状态 | 进展说明 | 链接 |
|:---|:---|:---|:---|
| #10194 | **已关闭** | CI 修复：阻止 PR 合并后继续发布 AI 评审结果，消除竞态条件 | [Issue #10194](https://github.com/zeroclaw-labs/zeroclaw/issues/10194) |
| #10111 | **已关闭** | Windows 桌面端 `TaskDialogIndirect` 入口点问题，标记为重复/支持类 | [Issue #10111](https://github.com/zeroclaw-labs/zeroclaw/issues/10111) |
| #9016 | **已关闭** | OpenAI 工具调用因 reasoning effort 参数被拒的阻塞性 Bug 修复完成 | [Issue #9016](https://github.com/zeroclaw-labs/zeroclaw/issues/9016) |

**整体评估**：今日合并/关闭量（2 Issues + 0 PR）与 48 个待合并 PR 形成鲜明对比。项目**前进动能充足但释放受阻**——大量高价值功能（插件出口策略、多模态验证、配置系统重构）卡在评审队列，存在"开发饱和、合并饥饿"的风险。

---

## 4. 社区热点

### 🔥 最高讨论热度

| 排名 | Issue/PR | 评论数 | 核心诉求 | 链接 |
|:---|:---|:---:|:---|:---|
| 1 | **#7155** RFC: 高风险 shell 命令逐执行确认层级（allow/ask/deny） | **23** | 用户要求**Claude Code 级别的命令安全策略**，拒绝"一刀切"的 shell 权限；需与现有 `allowed_commands` 允许列表共存 | [Issue #7155](https://github.com/zeroclaw-labs/zeroclaw/issues/7155) |
| 2 | **#9487** RFC: Runtime 拥有的对话会话与传输层适配器 | **22** | 架构解耦诉求：将通道所有权从网关下放到 runtime，支持 ACP/Web 等多通道统一接入 | [Issue #9487](https://github.com/zeroclaw-labs/zeroclaw/issues/9487) |
| 3 | **#10118** [Tracker] Rust 反 slop 策略债务清理 | **16** | 生产代码质量危机：307 处 panic 候选、202 处生产 panic，要求系统性消除"能跑就行"的 Rust 坏味道 | [Issue #10118](https://github.com/zeroclaw-labs/zeroclaw/issues/10118) |
| 4 | **#6850** RFC: 内存生命周期策略与存储后端解耦 | **14** | 避免每个网关重复实现合并/治理逻辑，需清晰的分层边界 | [Issue #6850](https://github.com/zeroclaw-labs/zeroclaw/issues/6850) |
| 5 | **#8780** RFC: Gemini Live 实时语音通道 | **14** | 对标 OpenAI Realtime API，要求原生 speech-to-speech 能力，当前仅文本通道不够用 | [Issue #8780](https://github.com/zeroclaw-labs/zeroclaw/issues/8780) |

**诉求分析**：社区已从"想要更多功能"转向**"要求基础设施成熟"**——安全策略精细化、架构分层清晰、代码质量可维护。这与项目从实验性工具向企业级平台演进的阶段匹配。

---

## 5. Bug 与稳定性

| 严重程度 | Issue | 描述 | 状态 | Fix PR |
|:---|:---|:---|:---|:---|
| **S1 - 阻塞工作流** | ~~#9016~~ | OpenAI `gpt-5.6-sol` 因 reasoning effort 参数导致工具调用完全失败 | ✅ **已修复** | 已合并 |
| **S2 - 行为降级** | #10068 | 交互式代理会话硬编码 32K token 上限，无视 131K 配置 | 🔄 进行中 | 待验证 |
| **S2 - 行为降级** | #10106 | 代理选择器误拒绝支持的转录服务（Groq/OpenAI/Deepgram 等） | 🔄 进行中 | 待验证 |
| **S2 - 行为降级** | ~~#10194~~ | AI 评审在 PR 合并后仍发布结果，造成状态混乱 | ✅ **已修复** | 已关闭 |
| **S3 - 轻微问题** | #10103 | ZeroCode Health 面板法语/西班牙语本地化布局错位 | 🔄 已接受 | good first issue |

**新增风险信号**：
- **#10074** (`SECURITY.md` 记录已移除的 CI 任务)：文档-实现漂移，容器安全合规沦为"约定"而非强制检查，**合规风险上升**。

---

## 6. 功能请求与路线图信号

### 高概率纳入下一版本（已有活跃 PR 或 maintainer 接受）

| 功能 | 信号强度 | 依据 | 链接 |
|:---|:---:|:---|:---|
| **插件出口流量强制策略** | ⭐⭐⭐⭐⭐ | #9582（Stage 2 实现）+ #9584（安装仪式）双 PR 并行，ADR-014 已提出 | [PR #9582](https://github.com/zeroclaw-labs/zeroclaw/pull/9582), [PR #9584](https://github.com/zeroclaw-labs/zeroclaw/pull/9584) |
| **web_search 安全加固** | ⭐⭐⭐⭐⭐ | #9831 已标记 `do-not-merge` 但设计完整（500 字符结果上限、DuckDuckGo 加固） | [PR #9831](https://github.com/zeroclaw-labs/zeroclaw/pull/9831) |
| **shell 子进程防逃逸** | ⭐⭐⭐⭐⭐ | #9827 修复 4 个沙箱边界漏洞，P1 优先级 | [PR #9827](https://github.com/zeroclaw-labs/zeroclaw/pull/9827) |
| **多模态图片像素级验证** | ⭐⭐⭐⭐☆ | #9819 防止损坏图片导致提供商请求失败，需作者动作 | [PR #9819](https://github.com/zeroclaw-labs/zeroclaw/pull/9819) |
| **默认流式响应** | ⭐⭐⭐⭐☆ | #10168 已接受，stall watchdog 默认启用 + #10166 stream_mode 默认 `partial` | [Issue #10168](https://github.com/zeroclaw-labs/zeroclaw/issues/10168), [Issue #10166](https://github.com/zeroclaw-labs/zeroclaw/issues/10166) |

### 中长期架构方向（RFC 阶段）

| 功能 | 成熟度 | 关键阻塞 | 链接 |
|:---|:---|:---|:---|
| **WASM 插件"万物皆可插"架构** | 草案 v1 | 需与现有 #8850 运行时插件迁移、#8398 权限模型统一 | [Issue #10076](https://github.com/zeroclaw-labs/zeroclaw/issues/10076) |
| **Agent 可移植性（导出/共享）** | 早期 RFC | 3 阶段设计，需维护者评审 | [Issue #10069](https://github.com/zeroclaw-labs/zeroclaw/issues/10069) |
| **zeroclaw swarm（临时代理集群）** | 早期 RFC | 与现有 `GoalTaskRecord` 控制平面整合方案待细化 | [Issue #10025](https://github.com/zeroclaw-labs/zeroclaw/issues/10025) |

---

## 7. 用户反馈摘要

### 真实痛点

> **"我配置了 131K 上下文，但代理只认 32K"**  
> — #10068 报告者 `icemann521`，反映**配置-运行时语义断裂**，用户信任受损

> **"DuckDuckGo 搜索结果太长，淹没上下文窗口"**  
> — #9831 隐含诉求，当前无内容上限导致**隐形成本失控**

> **"插件装到一半配置写入失败，重试不了"**  
> — #10162 揭示**原子性缺失**，安装与配置播种非事务性

> **"Code 会话历史不共享 Chat 记忆，但 UI 没告诉我"**  
> — #9341 用户心智模型与实现边界错位，**文档外显不足**

### 满意信号

- **#10168/#10166** 的默认行为改进（stall 超时、流式输出）显示维护者**主动优化开箱体验**
- **#7155** 的 allow/ask/deny 分级策略获社区强烈响应，证明**安全可控性**是核心购买动机

---

## 8. 待处理积压

### 需维护者紧急关注（>30 天未闭环 + P1/P2 + 高评论）

| Issue/PR | 天数 | 风险 | 行动建议 |
|:---|:---:|:---|:---|
| **#7155** shell 命令确认层级 | **79 天** | 安全核心设计，社区最高评论（23），已 accepted 但实现未启动 | 指派实现者，与 #9826/#9827 安全 PR 协调 |
| **#9487** Runtime 会话所有权 | **24 天** | 架构基础设施，阻塞多通道统一 | 确认 #9488/#9600 边界后进入实现 |
| **#6850** 内存生命周期解耦 | **91 天** | 技术债务累积，每个网关重复实现 | 与 #10118 质量清理统筹 |
| **#8780** Gemini Live 语音通道 | **46 天** | 竞品功能差距（OpenAI Realtime 已 GA） | 评估 v2 broker contract 是否可进入原型 |
| **#9582/#9584** 插件出口策略 | **21 天** | P1 + XL 规模，stacked 依赖 | 优先评审合并，释放后续 PR 队列 |

### 系统性风险提醒

- **48 个待合并 PR** 中 **XL 规模占 7 个**，大型变更长期并行增加集成冲突概率
- **"needs-author-action" 标签覆盖 8 个 PR**，作者-评审者反馈循环可能停滞
- **#8692** Maintainer 决策队列 Tracker 显示 RFC 评审负载过重，建议引入**分领域代码所有者**分流

---

> **健康度评分**: 🟡 **7.2/10**  
> 优势：社区活跃度高、安全架构投入坚决、代码质量意识觉醒  
> 风险：合并瓶颈、大型 PR 堆积、文档-实现漂移、本地化细节欠打磨

</details>

---
*本日报由 [agents-radar](https://github.com/brysoh435-dev/agents-radar) 自动生成。*