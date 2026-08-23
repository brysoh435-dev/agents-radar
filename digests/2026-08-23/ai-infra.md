# AI 基础设施日报 2026-08-23

> 生成时间: 2026-08-23 03:30 UTC | 覆盖项目: 6 个

- [vLLM](https://github.com/vllm-project/vllm)
- [SGLang](https://github.com/sgl-project/sglang)
- [llama.cpp](https://github.com/ggml-org/llama.cpp)
- [Ollama](https://github.com/ollama/ollama)
- [LiteLLM](https://github.com/BerriAI/litellm)
- [Unsloth](https://github.com/unslothai/unsloth)

---

## 横向对比

# AI 基础设施生态横向对比分析 | 2026-08-23

---

## 1. 生态全景

当前 AI 基础设施呈现**"推理引擎主导创新、端侧运行时追赶、网关层收敛合规"**的三极格局。vLLM 与 SGLang 在 DeepSeek-V4 支持上激烈竞速，PD 分离架构从实验走向生产可用；llama.cpp 以 9 日更的节奏加固多模态底座，但 SYCL 后端质量拖累 Intel GPU 生态；Ollama 和 Unsloth 在桌面端遭遇稳定性反噬，提示"易用性"与"可靠性"的权衡进入关键期；LiteLLM 则聚焦企业级计费准确性与供应链安全，完成从"路由工具"到"金融级网关"的跃迁。

---

## 2. 各项目活跃度对比

| 项目 | 今日 Release | PR 活动 | Issue 活动 | 核心战场 |
|:---|:---|:---|:---|:---|
| **vLLM** | 0 | 密集（3 个 DSv4 SM12x PR 同日迭代） | 高（75 条评论的 Batch Invariant 跟踪、多个 P0 待修复） | DeepSeek-V4 硬件适配、PD 分离落地 |
| **SGLang** | 0 | **309 个 PR 更新**（ServerArgs 重构 8 个堆叠 PR） | 高（NVFP4+投机解码 24 小时内 2 个崩溃 issue） | 配置系统重构、Fast Engine Recovery |
| **llama.cpp** | **9 个版本**（b10580-b10588） | 持续合并 | 中（SYCL 类 issue 占比超 30%） | 多模态基础设施、JSON 抽象层 |
| **Ollama** | 0 | 中（Prompt Cache 指标、MLX 结构化输出） | 高（v0.32.11-0.32.15 回归集中爆发） | 兼容层修复、云模型运营 |
| **LiteLLM** | **v1.99.0-rc.1 + v1.98.0** | **284 个活跃 PR** | 中（成本核算 Bug 密集修复） | 供应链安全（cosign）、计费准确性 |
| **Unsloth** | 0 | 低（长思维链渲染优化 >3000 行待解） | **高（桌面端 4 个 P0 级稳定性问题）** | Studio 桌面端稳定性、企业功能 |

> **活跃度排序**：SGLang (PR) > LiteLLM (PR) > llama.cpp (Release 节奏) > vLLM (技术深度) > Ollama (Issue 压力) > Unsloth (稳定性危机)

---

## 3. 模型支持竞速

| 模型/架构 | vLLM | SGLang | llama.cpp | Ollama | Unsloth |
|:---|:---|:---|:---|:---|:---|
| **DeepSeek-V4** | ✅ 最完整（SM12x Flash、Sparse MLA、Holo2 解析器） | ⚠️ 精度问题、8×H20 预热挂起 | — | — | — |
| **DeepSeek-V4-Flash** | ✅ DP8/TP8 性能对比、KV 容量 7.7x | ⚠️ DSPARK 标识符曾损坏 | — | — | — |
| **Kimi-K3** | — | ⚠️ v0.5.18 崩溃调查中 | — | — | — |
| **Qwen3 系列** | ✅ Batch Invariant 调优、3x decode 提升 | — | — | ✅ 云模型请求中 | — |
| **Gemma 4** | — | — | ⚠️ `<unused49>` 循环（多后端） | ⚠️ `think=false` 触发、内存分配异常 | — |
| **LTX-2.5** | — | ✅ 扩散解码器 | — | — | — |
| **BailingMoE3/DSpark** | — | — | ✅ b10581 | — | — |
| **dots3-note（视觉+音频）** | — | — | ✅ b10580 MTMD | — | — |
| **Ling 3.0** | — | — | — | — | ⏳ 社区高票请求 |

**领跑者判定**：
- **DeepSeek-V4 完整支持**：**vLLM 绝对领先**（SM12x 全路径、PD 分离、结构化输出、投机解码）
- **多模态原生输入**：**llama.cpp 率先**（dots3-note 视觉+音频联合）
- **扩散模型**：SGLang 独占 LTX-2.5
- **端侧易用性**：Ollama 云模型运营滞后成短板

---

## 4. 性能优化前沿

| 方向 | 代表项目 | 具体进展 | 量化收益 |
|:---|:---|:---|:---|
| **KV Cache 效率** | vLLM | DP8 vs TP8：7.7x 容量，3.4x TTFT @ 1M 上下文 | 长上下文并发质变 |
| **PD 分离（Prefill-Decode Disaggregation）** | vLLM、SGLang | NIXL Pull 模式 + PP > 1（#53360）；Derender Endpoints（#42729） | TTFT/TPOT 解耦优化 |
| **量化与格式** | vLLM、SGLang、llama.cpp | NVFP4（SM120）、FP8、BF16 NVFP4 Marlin、Q8_K_XL | 显存压缩，但兼容性风险高 |
| **投机解码** | vLLM、SGLang、llama.cpp | Ngram 全局缓存（#44597）、动态 K=0 跳过（#53426）、DFlash2、adaptive MTP | 延迟降低，但 greedy 一致性未解 |
| **算子/内核** | vLLM、llama.cpp | Batch Invariant 持久化 Matmul（3x decode）、concat 行级 memcpy、Vulkan 算子融合 | 确定性 + 吞吐 |
| **权重加载/冷启动** | SGLang | Weight Cache Daemon：306s → <1s（Qwen3-235B FP8） | 弹性伸缩核心瓶颈 |
| **批处理与调度** | vLLM | Batch Invariant（75 评论跟踪）、动态投机解码 | 可复现性 + 效率 |
| **CPU 卸载精细化** | llama.cpp | `--n-cpu-ffn` 按层卸载 | 边缘设备显存扩展 |

**火力集中度**：KV Cache 重构（PD 分离 + DP 策略）> 量化格式竞速（NVFP4/FP8）> 投机解码工程化 > 冷启动消除

---

## 5. 分层定位差异

| 层级 | 项目 | 核心抽象 | 今日演进信号 |
|:---|:---|:---|:---|
| **数据中心推理引擎** | **vLLM** | Python-first，GPU 集群优化，PP/TP/DP 组合 | 向 Agent 运行时演进（RFC #52567） |
| **数据中心推理引擎** | **SGLang** | RadixAttention + Fast Engine Recovery，配置即代码 | ServerArgs 重构：从"构造时突变"到"不可变配置" |
| **端侧/本地运行时** | **llama.cpp** | C++ 跨平台，单卡/边缘优先，多模态基础设施 | MTMD 管道成熟，但 SYCL 质量拖累 |
| **端侧/本地运行时** | **Ollama** | 封装 llama.cpp/MLX，桌面体验优先 | 兼容层债务爆发，云运营滞后 |
| **端侧训练+运行时** | **Unsloth** | 微调框架 + Studio IDE，LoRA/GGUF 全生命周期 | 桌面端稳定性危机，企业功能追赶 |
| **LLM 网关/路由器** | **LiteLLM** | 统一 API、成本核算、多供应商 fallback | 金融级计费 + 供应链安全（cosign） |

**关键张力**：
- vLLM/SGLang 向上吞噬 Agent 运行时（vs 专用框架如 LangChain）
- Ollama/Unsloth 向下需要更可靠的底层（llama.cpp 版本锁定 vs 快速迭代）
- LiteLLM 横向扩展至 MCP/A2A 协议，成为 Agent 间通信基础设施

---

## 6. 趋势信号与开发者行动建议

### 🔮 行业趋势

| 信号 | 证据 | 推断 |
|:---|:---|:---|
| **PD 分离从论文到生产** | vLLM NIXL Pull + PP > 1 解除限制；SGLang NIXL 角色替换需双重启 | 2026 Q4 将成为标配架构，但运维复杂度陡增 |
| **NVFP4 成为新量化标准，但工具链不成熟** | SM120 三项目同时出现 NVFP4 相关崩溃/乱码 | 早期采用者需承担格式兼容性风险 |
| **Agent 运行时内建化** | vLLM RFC #52567（Agentic API）、LiteLLM A2A/MCP 绑定 | 推理引擎与 Agent 框架边界模糊化 |
| **成本核算成为基础设施核心功能** | LiteLLM 密集修复缓存计费、Batch 成本、零用量区分 | LLM 基础设施进入"金融级"精度要求 |
| **桌面端 AI 工具稳定性反噬** | Ollama v0.32 回归、Unsloth Studio 4 个 P0 | "易用性优先"策略在复杂场景下遭遇可靠性天花板 |

### 🎯 Agent/应用开发者行动清单

| 优先级 | 行动 | 依据 |
|:---|:---|:---|
| **P0** | **生产环境锁定 vLLM/SGLang 版本，避开已知回归** | vLLM #53130（Scheduler 死锁）、#53157（Ray TP8 挂死）；SGLang NVFP4+投机解码崩溃 |
| **P0** | **禁用 Ollama v0.32.11-0.32.15，回退至 0.32.9/0.32.13** | #17910（长生成不停止）、#17833（CPU 异常占用） |
| **P1** | **评估 DP8 策略替代 TP8 服务长上下文** | vLLM #51454：7.7x KV 容量，3.4x TTFT |
| **P1** | **实验 PD 分离架构，但规划双角色重启运维** | vLLM #53360 可用，SGLang #33789 角色替换故障 |
| **P1** | **MLX 后端启用结构化输出，验证 XGrammar 约束** | Ollama #17929：此前为无约束解码，存在隐性风险 |
| **P2** | **审计 LiteLLM Azure 缓存计费历史数据** | #37823：2024-11-20 数据区域模型可能零计费 |
| **P2** | **关注 vLLM Batch Invariant 进展，规划确定性推理迁移** | Issue #27433：75 评论，Agent 测试/审计场景刚需 |
| **P2** | **llama.cpp Server 禁用 `--cache-ram` 直至 #27148 修复** | 跨 slot 数据污染，多租户安全风险 |

---

*分析基于 2026-08-23 公开 GitHub 数据，生产决策建议结合内部测试验证。*

---

## 各项目详细报告

<details>
<summary><strong>vLLM</strong> — <a href="https://github.com/vllm-project/vllm">vllm-project/vllm</a></summary>

# vLLM 动态日报 | 2026-08-23

## 今日速览

今日无新 Release，但社区活跃度极高：DeepSeek-V4 在 SM120/Blackwell 客户端硬件的适配进入密集修复期，3 个相关 PR 同日更新；NIXL KV Connector 解锁 Pipeline Parallel 下的 Pull 模式预填-解码分离，为大规模部署降低复杂度；Batch Invariant 特性持续成为讨论焦点，75 条评论的跟踪 Issue 显示该性能优化方向仍有多项子任务待完成。

---

## 新模型与硬件支持

| 条目 | 说明 | 链接 |
|:---|:---|:---|
| **DeepSeek-V4 Flash 支持 SM12x（RTX PRO 6000 / RTX 5090 / DGX Spark）** | PR #41834 持续迭代，为 Blackwell 客户端 GPU 启用 DSv4 Flash 的 SM120/SM121 fallback 路径，涵盖结构化输出、投机解码、KV Connector、量化等 essential fixes。这是当前最复杂的跨模块新硬件支持 PR 之一。 | [#41834](https://github.com/vllm-project/vllm/pull/41834) |
| **DSv4 Sparse MLA 的 SM12x Triton fallback** | PR #52357 修复 `o_proj` 在 SM12x 上错误调用 DeepGEMM `fp8_einsum` 的问题，为 GB10/DGX Spark 提供可运行的 FP8 矩阵乘 fallback。 | [#52357](https://github.com/vllm-project/vllm/pull/52357) |
| **DSv4 FlashInfer block size 对齐** | PR #53425 修正 `DeepseekV4SparseMLABackend` 的 block size 声明：FlashInfer SM120 decode kernel 编译为 64-token page，但 vLLM 仍广告 `[256]`，导致 `--block-size 256` 无法 split。 | [#53425](https://github.com/vllm-project/vllm/pull/53425) |
| **Holo2 推理解析器（Rust 前端）** | PR #50763 为 Rust 前端添加 Holo2 推理解析器，与 Python 的 `DeepSeekV3ReasoningWithThinkingParser` 行为对齐。 | [#50763](https://github.com/vllm-project/vllm/pull/50763) |

---

## 性能与优化

| 条目 | 数据/细节 | 链接 |
|:---|:---|:---|
| **Batch Invariant 持久化 Matmul 配置调优** | PR #53247 为 Hopper 和 Ada 架构添加按架构族调优的 bf16 launch config，覆盖 Qwen3-1.7B 的 5 种 weight shape，**RTX 4090D/H20 上约 3x decode kernel 提升**。 | [#53247](https://github.com/vllm-project/vllm/pull/53247) |
| **DeepSeek-V4 性能优化跟踪** | Issue #45861 记录 V4 优化任务清单，4 项 PR 已全部合并完成，包括 MLA 优化、通信优化等。 | [#45861](https://github.com/vllm-project/vllm/issues/45861) |
| **DP8 vs TP8 单 KV-head MLA：7.7x KV 容量，3.4x 1M TTFT** | Issue #51454 报告在 8x B200 上，DeepSeek-V4-Flash-0731 使用 Data Parallel 8 相比 Tensor Parallel 8，**KV cache 容量提升 7.7 倍，1M 上下文并发场景 TTFT 快 3.4 倍**（concurrency=8）。 | [#51454](https://github.com/vllm-project/vllm/issues/51454) |
| **PCP O-Proj Tensor Parallelism（实验性）** | PR #53422 添加 `--enable-pcp-o-proj-tp`，将 MLA output-projection 权重分片到 `TP x PCP`，保留 token-parallel PCP prefill 行为，为 decode 和 prefill 分别提供显式路径。 | [#53422](https://github.com/vllm-project/vllm/pull/53422) |
| **Ngram Prompt Lookup 全局缓存** | PR #44597 为投机解码添加 opt-in 全局缓存 scope，跨请求复用 prompt ngram continuation，提升多轮对话场景命中率。 | [#44597](https://github.com/vllm-project/vllm/pull/44597) |
| **动态投机解码 K=0 跳过 draft sync forward** | PR #53426（基于 #51575）实现 opt-in 跳过 K=0 时的 draft model forward，减少无效计算。默认关闭。 | [#53426](https://github.com/vllm-project/vllm/pull/53426) |

---

## 稳定性与回归

| 严重程度 | 条目 | 状态 | 链接 |
|:---|:---|:---|:---|
| 🔴 **高** | **Scheduler 永久停止准入请求**：`running + skipped_waiting` 达到 `max_num_seqs` 后引擎报告健康、KV cache 大量空闲，仅重启恢复。 | **待修复** | [#53130](https://github.com/vllm-project/vllm/issues/53130) |
| 🔴 **高** | **Ray TP8 间歇性死锁**：`shm_broadcast` block starvation 60s 后 `RayWorkerProc rank=[0] died unexpectedly`，API 存活但生成挂死。 | **待修复** | [#53157](https://github.com/vllm-project/vllm/issues/53157) |
| 🟡 **中** | **MNNVL allreduce workspace 初始化挂起 30s + GPU 内存泄漏**（IB-only 多节点） | **PR 已提交 #53377** | [#51986](https://github.com/vllm-project/vllm/issues/51986) → [#53377](https://github.com/vllm-project/vllm/pull/53377) |
| 🟡 **中** | **Hybrid Mamba prefix-cache resume 非法内存访问**：`--block-size` 显式设置时 state column 用错误 block size 初始化。 | **待修复** | [#53142](https://github.com/vllm-project/vllm/issues/53142) |
| 🟡 **中** | **Streaming input 累积 prompt 导致多模态会话二次方复杂度**：每轮 300-600 token delta 的到达成本随轮数线性增长（~150ms/轮）。 | **待修复** | [#52471](https://github.com/vllm-project/vllm/issues/52471) |
| 🟡 **中** | **FP8 checkpoint 比 runtime fp8 quant 慢 ~40%**（Qwen3-4B, RTX 4090, v0.24.0），bs=1 decode 吞吐量回归 28%。 | **待修复** | [#48035](https://github.com/vllm-project/vllm/issues/48035) |
| 🟡 **中** | **BF16 NVFP4 Marlin 在无原生 FP4 支持的 GPU 上输出乱码** | **待修复** | [#34694](https://github.com/vllm-project/vllm/issues/34694) |
| 🟢 **低** | **INT32 token offset 溢出**：`silu_and_mul_per_block_quant` 中大 batch 时 `token_idx * (2 * hidden_size)` 溢出导致 illegal memory access。 | **PR 已提交 #53409** | [#53409](https://github.com/vllm-project/vllm/pull/53409) |
| 🟢 **低** | **Mistral3 图像 placeholder grid 忽略 `mm_processor_kwargs` size override** | **PR 已提交 #52874** | [#52874](https://github.com/vllm-project/vllm/pull/52874) |
| 🟢 **低** | **CPU WNA16 算子未 guard**：加载的 CPU extension 未注册 `_C::cpu_gemm_wna16` 时崩溃。 | **PR 已提交 #52903** | [#52903](https://github.com/vllm-project/vllm/pull/52903) |

---

## 生态与基础设施

| 条目 | 说明 | 链接 |
|:---|:---|:---|
| **NIXL KV Connector 支持 PP > 1 的 Pull 模式** | PR #53360 解除两个 `NotImplementedError` 限制：PP stage > 1 的 prefill 节点可作为 producer，支持 hybrid-Mamba 模型的 disaggregated serving。这是 PD 分离落地的关键路径。 | [#53360](https://github.com/vllm-project/vllm/pull/53360) |
| **NIXL KV Connector 指标聚合语义文档** | Issue #41230 跟踪 metrics aggregation 的文档化需求，涉及 transfer time、throughput 等统计口径。 | [#41230](https://github.com/vllm-project/vllm/issues/41230) |
| **Derender Endpoints 为 PD 分离提供 detokenization** | RFC #42729 讨论 `/render` endpoint 的逆向流程，使 decode tier 能将 token ID 流转换回文本，完善 disaggregated serving 的端到端链路。 | [#42729](https://github.com/vllm-project/vllm/issues/42729) |
| **ROCm CI 基础设施改进** | PR #53355 持久化 AMD 测试缓存，避免冷网络下载；Issue #34994 跟踪 MI325/MI355 专用测试分组、Moriio 测试修复等。 | [#53355](https://github.com/vllm-project/vllm/pull/53355) [#34994](https://github.com/vllm-project/vllm/issues/34994) |
| **离线 Prefix Cache 工作负载分析器** | RFC #47993 提议前端/CLI 工具，帮助用户分析 prefix cache 命中率与配置优化空间。 | [#47993](https://github.com/vllm-project/vllm/issues/47993) |
| **Agentic 推理一等公民支持** | RFC #52567 基于 vLLM Agentic API MVP，提议将 Codex/Claude Code 风格的高性能 Agent 运行时内建到 vLLM。 | [#52567](https://github.com/vllm-project/vllm/issues/52567) |
| **Cold Start Q3 2026 路线图** | Issue #48193 跟踪冷启动优化专项，目标是通过 retained-state frontier 模型逐步减少启动工作量。 | [#48193](https://github.com/vllm-project/vllm/issues/48193) |

---

## 对应用开发者的意义

1. **DeepSeek-V4 部署成本显著下降**：DP8 vs TP8 的 7.7x KV 容量提升意味着同样硬件可服务 **7 倍以上长上下文并发**，1M token TTFT 快 3.4x——这对需要超长上下文窗口的 RAG、代码分析、文档理解应用是质变。建议关注 #51454 的测试配置并评估 DP 策略。

2. **PD 分离（Disaggregated Serving）进入可用阶段**：NIXL Pull 模式 + PP 支持（#53360）+ Derender Endpoints（#42729）组合，意味着应用开发者可以开始实验 **预填-解码分离架构**，将 TTFT 和 TPOT 优化解耦，尤其适合高并发、变长输入场景。

3. **Batch Invariant 是确定性推理的基石**：Issue #27433 跟踪的 batch 无关特性，对需要 **可复现输出** 的 Agent 工作流（如测试、审计、合规场景）至关重要。75 条评论显示社区投入度，建议关注其进展以规划未来迁移。

4. **警惕 v0.24-v0.27 的已知回归**：FP8 checkpoint 性能劣化（#48035）、Scheduler 死锁（#53130）、Ray TP8 挂死（#53157）等 issue 集中在近期版本，生产环境升级前建议对照验证。

5. **Agentic API 前瞻**：RFC #52567 暗示 vLLM 正从"纯推理引擎"向"Agent 运行时"演进，构建 Tool Use、多轮 ReAct 应用的开发者可提前关注其 gRPC 接口和状态管理设计。

</details>

<details>
<summary><strong>SGLang</strong> — <a href="https://github.com/sgl-project/sglang">sgl-project/sglang</a></summary>

# SGLang 动态日报 | 2026-08-23

## 今日速览

今日 SGLang 社区活跃度极高，**309 个 PR 更新**，核心焦点是 `ch-wan` 主导的 **ServerArgs 配置系统重构**（8 个堆叠 PR），旨在将配置从"构造时突变"改为"一次性解析后发布到配置包"。同时，**NVFP4 量化与投机解码的兼容性**成为紧急修复目标，多个相关 Bug 在 24 小时内被报告和关闭。

---

## 版本发布与破坏性变更

**无新版本发布。**

> 注：CI 维护模式已于昨日关闭（[#21065](https://github.com/sgl-project/sglang/issues/21065)），PR 合并恢复正常。

---

## 新模型与硬件支持

| 项目 | 详情 | 链接 |
|:---|:---|:---|
| **AMD gfx1250** | 新增架构支持，包含无 MFMA/WMMA FP4 scale 指令、不同 wave32 布局等针对性适配 | [#32754](https://github.com/sgl-project/sglang/pull/32754) |
| **LTX-2.5 扩散解码器** | 支持并行 tiling 的扩散视频生成 | [#36026](https://github.com/sgl-project/sglang/pull/36026) |
| **Kimi-K3** | v0.5.18 版本出现崩溃，正在调查 | [#36018](https://github.com/sgl-project/sglang/issues/36018) |

---

## 性能与优化

| 优化项 | 效果/细节 | 链接 |
|:---|:---|:---|
| **Fast Engine Recovery: Weight Cache Daemon** | Phase 1 已落地：Qwen3-235B FP8 权重加载从 **306–327s 降至 <1s**（通过 CUDA IPC 共享后量化权重） | [#33522](https://github.com/sgl-project/sglang/issues/33522) |
| **LingBot VAE 解码加速** | H200 上 832x480x9/4 步场景，高质量模式保持 FP32 权重常驻，BF16 仅用于 `quality=high` | [#36024](https://github.com/sgl-project/sglang/pull/36024) |
| **扩散模型自动驻留提升** | `performance-mode auto` 下通过 warmup 校准动态决定组件驻留，避免 80-140GB 显存 GPU 上的不必要卸载 | [#35335](https://github.com/sgl-project/sglang/pull/35335) |
| **MORI CP 状态传输去重** | 消除 `cp_size` 倍的状态传输冗余 | [#36025](https://github.com/sgl-project/sglang/pull/36025) |
| **FlashInfer KDA MTP 融合验证** | 将两阶段状态推进融合为单内核，减少 speculative SSM scratch 的 scatter 开销 | [#33722](https://github.com/sgl-project/sglang/pull/33722) |

---

## 稳定性与回归

### 🔴 严重（无 fix PR / 开放中）

| 问题 | 影响 | 状态 |
|:---|:---|:---|
| **NVFP4 + 投机解码崩溃** `extend_prefix_lens_cpu is None` | SM120 (RTX 5090) 上 `--kv-cache-dtype nvfp4` 与任何投机解码（MTP/DFLASH）不兼容，FlashInfer dequant workspace 空指针 | [#36010](https://github.com/sgl-project/sglang/issues/36010) **开放** |
| **NVFP4 + DFLASH 投机解码崩溃**（重复报告） | 同上，dequant workspace path 中 `extend_prefix_lens_cpu` 缺失 | [#36001](https://github.com/sgl-project/sglang/issues/36001) **已关闭** → 可能未完全修复 |
| **Qwen 3.8 27B NVFP4 图像坐标检测错误** | 多模态场景下坐标识别不正确 | [#35949](https://github.com/sgl-project/sglang/issues/35949) **开放** |
| **NIXL P/D 角色替换后停止服务** | Prefill/Decode 任一角色替换后，请求超时失败，需双角色重启恢复 | [#33789](https://github.com/sgl-project/sglang/issues/33789) **开放** |
| **Kimi-K3 v0.5.18 崩溃** | 新版本回归，详情待补充 | [#36018](https://github.com/sgl-project/sglang/issues/36018) **开放** |
| **ROCm AITER 版本锁定导致 MXFP4 MoE kernel 不可用** | gfx950 上 tuned `AITER_CONFIG_FMOE` 表无法使用 | [#35591](https://github.com/sgl-project/sglang/issues/35591) **开放** |

### 🟡 中等（已有关闭/有 workaround）

| 问题 | 说明 | 链接 |
|:---|:---|:---|
| **DeepSeek-V4 精度问题** | 轻微精度偏差，因 inactive 被关闭 | [#28414](https://github.com/sgl-project/sglang/issues/28414) |
| **DeepSeek V4 Pro 8×H20-3e 预热后挂起** | 无响应，inactive 关闭 | [#26783](https://github.com/sgl-project/sglang/issues/26783) |
| **DSPARK 标识符静默损坏** | DeepSeek-V4-Flash 上投机解码不安全，已修复 | [#34959](https://github.com/sgl-project/sglang/issues/34959) |
| **EAGLE3 CUDA graph 非法内存访问** | gpt-oss-120b 解码时 batch 收缩触发，已关闭 | [#28569](https://github.com/sgl-project/sglang/issues/28569) |
| **HiCache L3 + MTP 接受率下降** | GLM5.1 缓存命中后 speculative 效率降低，已关闭 | [#28895](https://github.com/sgl-project/sglang/issues/28895) |

### 🟢 基础设施

| 问题 | 说明 | 链接 |
|:---|:---|:---|
| **/health 超时路径未取消 scheduler 请求** | 健康检查孤儿请求堆积，导致 paged-prefill batching 崩溃 | [#35884](https://github.com/sgl-project/sglang/issues/35884) |
| **CUDA Coredump 自动追踪** | CI 基础设施，238 条评论的持续追踪 | [#26340](https://github.com/sgl-project/sglang/issues/26340) |
| **CI 状态** | 3 broken, 11 flaky, 671 recently fixed | [#17050](https://github.com/sgl-project/sglang/issues/17050) |

---

## 对应用开发者的意义

| 关注点 | 建议 |
|:---|:---|
| **NVFP4 量化用户** | ⚠️ **立即避免** 在 RTX 5090/SM120 上将 `--kv-cache-dtype nvfp4` 与任何投机解码（`--speculative-algorithm NEXTN` / DFLASH / EAGLE3）组合使用。这是一个已知活跃崩溃，24 小时内已有两个独立 issue。等待 [#36010](https://github.com/sgl-project/sglang/issues/36010) 修复。 |
| **Mistral 模型 API 用户** | 空 assistant turns 将导致请求被拒绝（[#35915](https://github.com/sgl-project/sglang/pull/35915) 准备修复），建议客户端过滤空内容消息。 |
| **P/D 分离部署运维** | NIXL 后端在角色替换后需要**双角色重启**才能恢复，规划滚动更新时需注意（[#33789](https://github.com/sgl-project/sglang/issues/33789)）。 |
| **扩散模型/ComfyUI 集成** | 新版 ComfyUI 模式改为原生 pipeline 的 DiT-only runtime profile（[#35960](https://github.com/sgl-project/sglang/pull/35960)），旧 `comfyui_*_pipeline.py` 将被删除，需迁移。 |
| **配置依赖型应用** | `ServerArgs` 重构为"解析后发布到 config bags"（[#35917](https://github.com/sgl-project/sglang/pull/35917) 系列），依赖构造时突变行为的扩展可能需适配。 |

---

*数据来源：sgl-project/sglang GitHub | 生成时间：2026-08-23*

</details>

<details>
<summary><strong>llama.cpp</strong> — <a href="https://github.com/ggml-org/llama.cpp">ggml-org/llama.cpp</a></summary>

# llama.cpp 动态日报 | 2026-08-23

## 今日速览

今日 llama.cpp 密集发布 9 个版本（b10580-b10588），核心聚焦于**多模态基础设施加固**（MTMD 视觉/音频支持、JSON 抽象层重构）与**推理引擎稳定性修复**（clang LTO、draft-MTP 嵌入修复、ROCm CI 恢复）。SYCL 后端仍是 Bug 重灾区，多 GPU 场景下的正确性与稳定性问题持续发酵。

---

## 版本发布与破坏性变更

| 版本 | 核心变更 | 影响面 | 链接 |
|:---|:---|:---|:---|
| **b10588** | `common/json.h`: 修复 clang LTO 链接失败 | 编译工具链（clang + LTO） | [Release](https://github.com/ggml-org/llama.cpp/releases/tag/b10588) |
| **b10587** | Vulkan 后端新增 `PAD_REFLECT_1D` 算子 | 视觉模型边缘填充场景 | [Release](https://github.com/ggml-org/llama.cpp/releases/tag/b10587) |
| **b10586** | MTMD: 使用 `ggml_rope_set_offset` 替代手动实现 | 多模态位置编码一致性 | [Release](https://github.com/ggml-org/llama.cpp/releases/tag/b10586) |
| **b10585** | **重大重构**: 引入 `common/json.h` 抽象层，解耦 `nlohmann::json` | **所有依赖 JSON 的模块**（server、jinja、tests） | [Release](https://github.com/ggml-org/llama.cpp/releases/tag/b10585) |
| **b10584** | `fit`: 考虑 `n_streams` 参数；draft context 跟随 target context 的 KV cache 配置 | 投机解码 + 非统一 KV cache 场景 | [Release](https://github.com/ggml-org/llama.cpp/releases/tag/b10584) |
| **b10582** | 恢复 Ubuntu ROCm CI 作业（此前因工具链问题禁用） | AMD GPU 持续集成覆盖 | [Release](https://github.com/ggml-org/llama.cpp/releases/tag/b10582) |
| **b10581** | 模型: 支持 BailingMoE3 的 DSpark 架构 | 新增 MoE 模型支持 | [Release](https://github.com/ggml-org/llama.cpp/releases/tag/b10581) |
| **b10580** | **MTMD: 支持 dots3-note 视觉+音频多模态输入** | **多模态推理基础设施** | [Release](https://github.com/ggml-org/llama.cpp/releases/tag/b10580) |
| **b10578** | `ggml`: concat 算子优化——逐元素 memcpy 改为行级 memcpy | 大 tensor concat 吞吐提升 | [Release](https://github.com/ggml-org/llama.cpp/releases/tag/b10578) |
| **b10577** | 修复 draft-MTP 与 embeddings 共用的崩溃/错误输出 | 投机解码 + RAG/embedding 场景 | [Release](https://github.com/ggml-org/llama.cpp/releases/tag/b10577) |

### ⚠️ 迁移注意事项

- **b10585 JSON 抽象层**: 若你的 fork 或插件直接使用了 `nlohmann::json` 头文件，需迁移至 `#include "common/json.h"`。该变更采用 pimpl 模式，未来可能支持替换底层 JSON 库（如 simdjson）。
- **b10584 draft context 变更**: 使用 `--spec-type draft-mtp` 且启用非统一 KV cache（如 `--cache-type-k q8_0`）时，draft model 的 context 大小现在自动与 target 对齐为 `n_ctx_train`，此前可能导致 OOM 或投机失败。

---

## 新模型与硬件支持

| 类别 | 详情 | 链接 |
|:---|:---|:---|
| **新架构** | BailingMoE3 支持 DSpark（稀疏注意力变体） | [b10581](https://github.com/ggml-org/llama.cpp/releases/tag/b10581) |
| **多模态** | dots3-note 视觉+音频联合输入（MTMD pipeline） | [b10580](https://github.com/ggml-org/llama.cpp/releases/tag/b10580) |
| **后端扩展** | **Qualcomm Hexagon NPU 替代后端**（FastRPC-based，与官方 dspqueue 共存） | [PR #27535](https://github.com/ggml-org/llama.cpp/pull/27535) |
| **Vulkan 算子** | `PAD_REFLECT_1D` 补齐视觉模型算子覆盖 | [b10587](https://github.com/ggml-org/llama.cpp/releases/tag/b10587) |
| **Metal 算子** | `DIAG_MASK_INF` 支持（待合并） | [PR #27197](https://github.com/ggml-org/llama.cpp/pull/27197) |
| **CUDA 算子** | `POOL_1D` (avg/max) 支持（待合并） | [PR #27573](https://github.com/ggml-org/llama.cpp/pull/27573) |

---

## 性能与优化

| 优化项 | 细节 | 来源 |
|:---|:---|:---|
| **concat 算子** | 行级 memcpy 替代逐元素拷贝，减少 kernel launch 开销 | [b10578](https://github.com/ggml-org/llama.cpp/releases/tag/b10578) |
| **CUDA Graphs 多 slot decode** | 形状稳定的 padded batching 方案（进行中，尚未合并） | [Issue #27009](https://github.com/ggml-org/llama.cpp/issues/27009) |
| **OpenCL (Adreno)** | 量化 lm_head / decode GEMV 优化，speculative decoding/MTP  skinny GEMM 加速 | [PR #26477](https://github.com/ggml-org/llama.cpp/pull/26477) |
| **OpenCL (Intel Xe-LP)** | TG/PP +11-12% 性能提升 | [PR #26438](https://github.com/ggml-org/llama.cpp/pull/26438) |
| **KV cache 布局自动切换** | FA auto-resolve 为 disabled 时重建 cache，避免 V cache 转置带来的性能损耗 | [PR #26460](https://github.com/ggml-org/llama.cpp/pull/26460) |
| **Vulkan 算子融合** | `UNARY(SIGMOID\|SILU\|SOFTPLUS) + MUL` 融合（匹配 CUDA 行为） | [PR #27220](https://github.com/ggml-org/llama.cpp/pull/27220) |
| **CPU 卸载精细化** | `--n-cpu-ffn` 选项（类似 `--n-cpu-moe`），dense model FFN 层可按数量卸载至 CPU | [PR #26622](https://github.com/ggml-org/llama.cpp/pull/26622) |

---

## 稳定性与回归

### 🔴 严重（崩溃/正确性）

| Issue | 描述 | 后端/硬件 | 状态 |
|:---|:---|:---|:---|
| [#27102](https://github.com/ggml-org/llama.cpp/issues/27102) | **CUDA kernel stall 被 watchdog 杀死**，RTX Pro 6000 Blackwell MAX-Q，Qwen3.8-27B Q8_K_XL | CUDA / Blackwell | **OPEN**, 25 评论，无 fix |
| [#27579](https://github.com/ggml-org/llama.cpp/issues/27579) | **HIP/ROCm gfx1151 输出损坏**，Vulkan 同配置正确，两 dense 架构均失败 | HIP / RDNA3.5 Strix Halo | **OPEN**, 今日新建，3 评论 |
| [#25618](https://github.com/ggml-org/llama.cpp/issues/25618) | **投机解码 greedy 输出与 vanilla 不一致**（量化 target 下），bf16 正常 | 全后端 | **OPEN**, 16 评论，长期未解 |
| [#27198](https://github.com/ggml-org/llama.cpp/issues/27198) | **SYCL tensor split 双 Arc Pro B70 崩溃**，`dev2dev_memcpy` DEVICE_LOST，P2P 实际可用 | SYCL / Arc Pro B70 | **OPEN**, 6 评论 |
| [#26837](https://github.com/ggml-org/llama.cpp/issues/26837) | **3 GPU tensor split 即时崩溃**，2 GPU 正常 | CUDA / 3x3090 | **OPEN**, 3 评论 |

### 🟡 中等（功能异常/性能退化）

| Issue | 描述 | 后端/硬件 | 状态 |
|:---|:---|:---|:---|
| [#27063](https://github.com/ggml-org/llama.cpp/issues/27063) | **SYCL A770 完全不可用**，B60 正常（同代码） | SYCL / Arc A770 | **OPEN**, 17 评论 |
| [#27038](https://github.com/ggml-org/llama.cpp/issues/27038) | SYCL host-pinned memory 大分配时 CPU 占用飙升 | SYCL / 大内存 | **OPEN**, 13 评论 |
| [#26239](https://github.com/ggml-org/llama.cpp/issues/26239) | Gemma4 `<unused49>` 无限生成，gfx1151 HIP Windows，长 prompt 后 | HIP / Strix Halo | **OPEN**, 5 评论 |
| [#26088](https://github.com/ggml-org/llama.cpp/issues/26088) | Gemma4 31B `<unused49>` 循环，多 quant 源复现，长 idle 后触发 | CUDA / 2xA100 | **OPEN**, 5 评论 |
| [#25692](https://github.com/ggml-org/llama.cpp/issues/25692) | **SYCL Arc Pro B70 GPU hang**，FA + 量化 KV cache，持续负载下 xe ccs engine reset | SYCL / Arc Pro B70 | **OPEN**, 5 评论 |
| [#25835](https://github.com/ggml-org/llama.cpp/issues/25835) | **CUDA Graphs VRAM 泄漏**，V100，启用后显存持续增长 | CUDA / V100 | **OPEN**, 3 评论 |
| [#27148](https://github.com/ggml-org/llama.cpp/issues/27148) | **RAM-backed prompt cache 跨 slot 污染**：新请求恢复已结束对话内容 | Server / `--cache-ram` | **OPEN**, 3 评论，**数据安全相关** |

### 🟢 已修复/有进展

| Issue/PR | 修复内容 | 版本 |
|:---|:---|:---|
| [b10577](https://github.com/ggml-org/llama.cpp/releases/tag/b10577) | draft-MTP + embeddings 崩溃/错误输出 | b10577 |
| [b10588](https://github.com/ggml-org/llama.cpp/releases/tag/b10588) | clang LTO 链接失败（json.h） | b10588 |
| [b10582](https://github.com/ggml-org/llama.cpp/releases/tag/b10582) | ROCm Ubuntu CI 恢复 | b10582 |
| [#27574](https://github.com/ggml-org/llama.cpp/pull/27574) | Tensor parallel meta tensor split state 传播修复（`test-llama-archs` 全通过） | 待合并 |

---

## 对应用开发者的意义

### 1. 多模态应用：基础设施就绪但边缘情况待打磨
- **dots3-note 视觉+音频支持**（b10580）意味着 llama.cpp 正在追赶 GPT-4o 类的原生多模态输入，但 [#24394](https://github.com/ggml-org/llama.cpp/issues/24394)、[#24429](https://github.com/ggml-org/llama.cpp/issues/24429) 显示 MP4 解析（MOOV atom 位置）在 Windows/Linux 均有死锁/静默失败，**生产环境视频输入建议先 ffmpeg 预处理为规范格式**。

### 2. 投机解码：自适应深度与正确性博弈
- **adaptive MTP draft depth**（[PR #27210](https://github.com/ggml-org/llama.cpp/pull/27210)）和 **DFlash2**（[PR #27342](https://github.com/ggml-org/llama.cpp/pull/27342)）提供更高质量的 draft model，但 [#25618](https://github.com/ggml-org/llama.cpp/issues/25618) 的 greedy 不一致问题仍未解决——**对确定性输出有强需求的场景（如代码生成），建议关闭投机解码或仅用 bf16 target**。

### 3. JSON 抽象层：Fork 维护者需关注迁移
- b10585 的 `common/json.h` 是长期架构调整，若你维护了基于 llama.cpp 的绑定（如 Python/Go/Rust），需检查是否直接依赖 `nlohmann::json` 的具体类型或异常语义。

### 4. SYCL/Arc GPU：生产环境风险极高
- 今日 84 条 issue 中 SYCL 相关占比超 30%，涵盖 A770 完全不可用、B70 多 GPU 崩溃、host memory 性能陷阱。**Intel Arc GPU 部署建议仅限单卡、非 tensor-split、关闭 FA 的保守配置**。

### 5. Server 缓存安全：紧急关注
- [#27148](https://github.com/ggml-org/llama.cpp/issues/27148) 的 RAM cache 跨 slot 污染是**数据隔离漏洞**，多租户场景下可能导致敏感信息泄露，建议暂时禁用 `--cache-ram` 或等待修复。

---

*日报基于 ggml-org/llama.cpp 公开 GitHub 数据生成，不含未公开 commit 或内部讨论。*

</details>

<details>
<summary><strong>Ollama</strong> — <a href="https://github.com/ollama/ollama">ollama/ollama</a></summary>

# Ollama 动态日报 | 2026-08-23

## 今日速览

今日社区活跃度极高，**MLX 后端结构化输出**与 **Prompt Cache 指标暴露**成为核心进展，多个回归问题在 v0.32.11-v0.32.15 版本窗口集中爆发。桌面端体验优化（设置页返回按钮、应用列表展示）同步推进，但云模型更新滞后问题持续引发用户不满。

---

## 版本发布与破坏性变更

> 无新版本发布。

**关键变更提醒**：多个 PR 针对 v0.32.15 的兼容层行为进行修正，建议关注后续 patch 版本。

---

## 新模型与硬件支持

| 动态 | 详情 | 链接 |
|:---|:---|:---|
| **云模型请求：Qwen3.8-27B Cloud** | 社区强烈呼吁将本地已支持的 `qwen3.8:27b` 上架 Ollama Cloud，对标 `gemma4:31b-cloud` | [#17926](https://github.com/ollama/ollama/issues/17926) |
| **云模型滞后争议** | 用户指出 Cloud 仍托管"古老的 qwen 3.5 27b"，要求自动更新机制或淘汰旧模型 | [#17796](https://github.com/ollama/ollama/issues/17796) |
| **Vulkan gfx1151 (Strix Halo) 稳定性** | AMD Radeon 8060S 长 prompt prefill 触发 `compute-ring watchdog timeout`，`num_batch=128` 可规避 | [#17870](https://github.com/ollama/ollama/issues/17870) |

---

## 性能与优化

| 动态 | 详情 | 链接 |
|:---|:---|:---|
| **Prompt Cache 指标暴露（PR）** | 新增 `prompt_eval_cached_count` 字段，原生 API 及 OpenAI/Anthropic 兼容层同步暴露；`prompt_eval_count` 保持为逻辑输入总量，缓存命中不计入 CLI benchmark 的 prefill 速率 | [#17943](https://github.com/ollama/ollama/pull/17943) |
| **MLX 结构化输出支持（PR）** | 弃用静默忽略行为，通过 XGrammar 在采样每一步对 logits 做 grammar mask，覆盖 JSON/JSON Schema/原始 grammar；[#17933](https://github.com/ollama/ollama/issues/17933) 报告的问题将得修复 | [#17929](https://github.com/ollama/ollama/pull/17929) |
| **Gemma 4 内存分配异常** | `gemma4:e2b` 在 RTX 3060 Ti 上过度使用 CUDA_Host（pinned system memory）而非 VRAM，已关闭待更多信息 | [#17386](https://github.com/ollama/ollama/issues/17386) |

---

## 稳定性与回归

### 🔴 严重：生成行为异常

| 问题 | 影响版本 | 状态 | 链接 |
|:---|:---|:---|:---|
| **长生成永不停止** | v0.32.11-0.32.15（0.32.9 正常） | 待修复，无 PR | [#17910](https://github.com/ollama/ollama/issues/17910) |
| **v0.32.14 CPU 异常占用** | 模型完全载入 VRAM 时 CPU 50-80%，0.32.13 正常 | 待修复 | [#17833](https://github.com/ollama/ollama/issues/17833) |
| **Gemma 4 `<unused49>` token 重复** | `think=false` 时触发，VS Code 扩展受影响 | **已关闭** | [#17459](https://github.com/ollama/ollama/issues/17459) |
| **Q4_0 KV 量化输出垃圾** | 低比特量化导致 "AI AI AI..." 循环输出 | **已关闭** | [#17614](https://github.com/ollama/ollama/issues/17614) |

### 🟡 中等：API/协议层

| 问题 | 详情 | 状态 | 链接 |
|:---|:---|:---|:---|
| **本地 API 误报 "Invalid API Key"** | 无 Cloud 配置时 `/api/embed` `/api/generate` 返回 500，tokenize 层错误路由到 Cloud | **已关闭** | [#17822](https://github.com/ollama/ollama/issues/17822) |
| **`tool_choice` 被静默忽略** | OpenAI/Anthropic 兼容层中 `none` 仍允许工具调用，`required` 返回纯文本 | **PR 待合并** [#17935](https://github.com/ollama/ollama/pull/17935) | [#17921](https://github.com/ollama/ollama/issues/17921) |
| **SYSTEM 消息位置校验错误** | `qwen3.8:27b` 配合 Claude Code 使用时 500 错误 | **已关闭** | [#17754](https://github.com/ollama/ollama/issues/17754) |
| **HF 导入模型 `stop` 参数推导错误** | Muse-Glimmer-30B 输出被截断至 ~3 tokens | 待修复 | [#17939](https://github.com/ollama/ollama/issues/17939) |

### 🟢 一般：体验与文档

| 问题 | 状态 | 链接 |
|:---|:---|:---|
| **HF 导入 Qwen3 thinking 未分离** | `PARSER qwen3` 对 RuadaptQwen3-8B-Hybrid 失效，`message.thinking` 字段未填充 | 待修复 [#17937](https://github.com/ollama/ollama/issues/17937) |
| **NTFS 卷挂载点导致 `ollama create` 400** | 路径解析问题 | 待修复 [#17591](https://github.com/ollama/ollama/issues/17591) |
| **Context deadline exceeded** | 大量 HF 模型下载/运行超时 | 待修复 [#17484](https://github.com/ollama/ollama/issues/17484) |

---

## 对应用开发者的意义

### 1. **Prompt Cache 可观测性（#17943）**
> Agent 开发者终于能在 API 响应中区分 `prompt_eval_count`（总输入 token）与 `prompt_eval_cached_count`（缓存命中），实现：
> - 成本/延迟归因分析
> - 多轮对话缓存策略验证
> - 兼容 OpenAI `usage.prompt_tokens_details.cached_tokens` 语义

### 2. **MLX 结构化输出落地（#17929）**
> 此前在 Apple Silicon 上依赖 `format: "json"` 的 Agent 流水线实际处于**无约束解码**状态，存在隐性解析失败风险。XGrammar 集成后，MLX 后端行为与 llama.cpp 后端对齐，可安全启用 schema 约束。

### 3. **兼容层 `tool_choice` 修复（#17935）**
> 强制工具调用（`tool_choice: "required"`）和禁用工具（`tool_choice: "none"`）在 OpenAI/Anthropic 适配层生效，多 Agent 编排场景（如 ReAct、Plan-and-Execute）的可靠性提升。

### 4. **版本锁定建议**
> v0.32.11-0.32.15 存在**长生成不停止**（#17910）和 **CPU 异常占用**（#17833）两个活跃回归，生产环境建议：
> - 短期：锁定 0.32.9 或 0.32.13
> - 关注：#17929、#17935 合并后的 0.32.16 窗口

### 5. **云模型选型陷阱**
> Ollama Cloud 模型更新存在显著滞后（#17796），构建 Cloud-first Agent 时需显式校验模型版本，避免落入"qwen 3.5 27b"等旧版能力陷阱。免费层 403 前置检测仍无官方方案（#16773）。

---

*日报基于 ollama/ollama 公开 GitHub 数据生成，PR 合并状态以仓库为准。*

</details>

<details>
<summary><strong>LiteLLM</strong> — <a href="https://github.com/BerriAI/litellm">BerriAI/litellm</a></summary>

# LiteLLM 动态日报 | 2026-08-23

## 今日速览

今日 LiteLLM 社区聚焦**供应链安全与成本核算修复**：v1.99.0-rc.1 与 v1.98.0 正式引入 Docker 镜像 cosign 签名验证机制；同时密集修复了 Azure 数据区域缓存计费、Bedrock 工具调用、成本优化统计等关键财务链路 Bug，直接影响企业级部署的计费准确性。

---

## 版本发布与破坏性变更

| 版本 | 关键变更 | 迁移注意 |
|:---|:---|:---|
| **[v1.99.0-rc.1](https://github.com/BerriAI/litellm/releases/tag/v1.99.0-rc.1)** | 所有 Docker 镜像启用 cosign 签名 | 生产环境需配置签名验证，公钥引入于 commit `0112e53` |
| **[v1.98.0](https://github.com/BerriAI/litellm/releases/tag/v1.98.0)** | 同上，稳定版发布 | 从旧版本升级时，若使用私有镜像仓库需更新拉取策略 |

> **基础设施行动项**：建议 CI/CD 流水线集成 `cosign verify` 步骤，防止供应链投毒。

---

## 新模型与硬件支持

| PR | 内容 | 状态 |
|:---|:---|:---|
| [#37067](https://github.com/BerriAI/litellm/pull/37067) | 新增 **RunInfra** 提供商支持，注册为 `openai_like` 类型，附带四款托管模型的成本映射 | Open |
| [#37995](https://github.com/BerriAI/litellm/pull/37995) | **Bedrock Mantle**: 转发原生 `web_search` 工具至 Responses API，解决 Codex 0.148 网页搜索静默失败问题 | Open |
| [#37993](https://github.com/BerriAI/litellm/pull/37993) | **Bedrock Converse**: 支持 `tool_choice="any"` 映射为 Bedrock 的 `{"any": {}}` | Open |

---

## 性能与优化

| PR/Issue | 优化点 | 影响 |
|:---|:---|:---|
| [#37728](https://github.com/BerriAI/litellm/pull/37728) | DeepSeek `reasoning_content` 占位符警告聚合：每请求仅输出一次，替代原有"每条历史消息触发一次"的日志风暴 | 显著降低日志噪声，提升可观测性 |
| [#37994](https://github.com/BerriAI/litellm/pull/37994) | 流式传输保留显式零用量：区分"上游未提供"与"上游明确报告零 token" | 避免错误计费，断开连接场景更准确 |
| [#36672](https://github.com/BerriAI/litellm/pull/36672) | 被阻断模型路由至健康 fallback：主部署完全阻塞时不再直接拒绝 | 提升可用性恢复能力 |

---

## 稳定性与回归

### 🔴 严重（已有 Fix PR）

| Issue | 描述 | Fix PR |
|:---|:---|:---|
| [#37823](https://github.com/BerriAI/litellm/issues/37823) | **Azure 数据区域缓存计费归零**：`azure/us/gpt-4o-2024-11-20` 与 `azure/eu/gpt-4o-2024-11-20` 缺失 `cache_read_input_token_cost`，缓存读取按 $0 计费 | [#37996](https://github.com/BerriAI/litellm/pull/37996) |
| [#37117](https://github.com/BerriAI/litellm/issues/37117) | **成本优化统计恒为零**：`compression_savings_spend` / `prompt_caching_savings_spend` 在多部署共享 `model_name` 时始终返回 $0 | [#37926](https://github.com/BerriAI/litellm/pull/37926) |
| [#37699](https://github.com/BerriAI/litellm/issues/37699) | **工具调用 JSON 解析崩溃**：`split_concatenated_json_objects` 遇截断 JSON 时抛出 `JSONDecodeError`，整轮多工具调用中止 | [#37699](https://github.com/BerriAI/litellm/issues/37699)（已关闭）|
| [#37713](https://github.com/BerriAI/litellm/issues/37713) | **Batch 成本永久丢失**：`output_file_id=None` 时无条件设置 `batch_processed=True`，阻断后续 spend recovery | [#37713](https://github.com/BerriAI/litellm/issues/37713)（已关闭）|

### 🟡 高优先级（待修复/部分修复）

| Issue | 描述 | 状态 |
|:---|:---|:---|
| [#27954](https://github.com/BerriAI/litellm/issues/27954) | **K8s 内存泄漏**：运行一段时间后 RAM 占满导致 Pod OOMKilled，11 条评论持续追踪 | Open，无明确 fix |
| [#36922](https://github.com/BerriAI/litellm/issues/36922) | **uv 工具链兼容断裂**：`uv tool update` 后 FastAPI `get_flat_dependant` 不兼容，Proxy 无法启动 | Open |
| [#34614](https://github.com/BerriAI/litellm/issues/34614) | **Redis SSL 连接回归**：v1.93.0 起 `ssl_check_hostname` 参数引发 `TypeError` | Open，影响缓存与预算计数器 |
| [#28553](https://github.com/BerriAI/litellm/issues/28553) | **Azure Responses API 流式阻断**：`stream_options.include_usage` 被错误传递，Codex 无法使用 | Open |

### 🟢 中等/已关闭

| Issue | 描述 | 状态 |
|:---|:---|:---|
| [#28706](https://github.com/BerriAI/litellm/issues/28706) | Anthropic 提供者应丢弃纯空白系统消息，避免 `invalid_request_error` | Open |
| [#37877](https://github.com/BerriAI/litellm/issues/37877) | Provider 预算不计入 embedding/rerank 开销，`RouterBudgetLimiting` 抛异常 | Open |
| [#37140](https://github.com/BerriAI/litellm/issues/37140) | 非流式请求客户端断开后上游工作未取消，资源浪费 | Open |
| [#36091](https://github.com/BerriAI/litellm/issues/36091) | Anthropic `/v1/messages` 桥接 Responses API 时缓存 token 始终为 0 | Closed |
| [#26428](https://github.com/BerriAI/litellm/issues/26428) | 零长度消息省略 `role` 字段，SGLang 边缘情况兼容 | Closed |

---

## 对应用开发者的意义

| 场景 | 影响 | 行动建议 |
|:---|:---|:---|
| **成本敏感型应用** | Azure 缓存计费修复直接修正财务数据，但历史零计费记录可能无法追溯 | 审计 2024-11-20 数据区域模型的缓存使用账单 |
| **Agent/工具调用** | Bedrock 工具选择、Anthropic 空白消息、JSON 解析等多处边界修复 | 升级后回归测试多轮工具调用流程 |
| **可观测性集成** | New Relic 团队级指标/追踪路由（[#37610](https://github.com/BerriAI/litellm/pull/37610)、[#37603](https://github.com/BerriAI/litellm/pull/37603)）、LangSmith trace_id 修复（[#37269](https://github.com/BerriAI/litellm/issues/37269)） | 多租户 SaaS 可配置团队级监控隔离 |
| **安全合规** | Docker 签名 + UI JWT 不再携带 API key 材料（[#35664](https://github.com/BerriAI/litellm/issues/35664)） | 更新安全基线检查清单 |
| **MCP/A2A 生态** | A2A 协议绑定大小写规范化（[#37917](https://github.com/BerriAI/litellm/pull/37917)）、Vertex AI 系统指令驼峰命名（[#37030](https://github.com/BerriAI/litellm/pull/37030)） | 混合供应商 Agent 部署兼容性提升 |

---

> **基础设施工程师关注项**：今日 284 个活跃 PR 中，成本核算与供应链安全占主导。建议优先验证 [#37996](https://github.com/BerriAI/litellm/pull/37996) 的 Azure 缓存定价修复是否覆盖所有数据区域 SKU，并评估 v1.99.0-rc.1 的 cosign 验证对现有部署流水线的影响。

</details>

<details>
<summary><strong>Unsloth</strong> — <a href="https://github.com/unslothai/unsloth">unslothai/unsloth</a></summary>

# Unsloth 动态日报 | 2026-08-23

## 今日速览

今日 Unsloth 无新版本发布，但 **Studio 桌面端稳定性问题集中爆发**：Windows/Linux 双平台均出现模型加载冻结、IndexedDB 写入失败、同步盘模型文件被误扫描等阻塞性 Bug。同时社区对 **Ling 3.0 模型支持** 的呼声持续升温，API 统计与 LAN 访问等企业级功能需求显著增加。

---

## 稳定性与回归

| 严重程度 | 问题 | 状态 | 链接 |
|:---|:---|:---|:---|
| 🔴 **高** | **Studio Desktop Linux AppImage 打开 Model Hub 即冻结**，需强制退出；v0.1.801-beta 引入，回退旧版本正常 | 已关闭 #9453，但根因（atk-bridge 警告）未明确修复 | [#9453](https://github.com/unslothai/unsloth/issues/9453) |
| 🔴 **高** | **Windows 桌面端启动时遍历并下载同步盘（OneDrive/Google Drive）全部模型文件**，造成不必要的网络/存储开销 | 开放，今日新增 | [#9537](https://github.com/unslothai/unsloth/issues/9537) |
| 🔴 **高** | **聊天窗口 IndexedDB 本地持久化写入失败**，每次发送消息必现，导致历史记录丢失；Tauri/WebKitGTK 构建 | 开放，今日新增 | [#9518](https://github.com/unslothai/unsloth/issues/9518) |
| 🟡 **中** | **GGUF 模型全 VRAM 卸载后系统 RAM 不释放**（`-ngl -1`），"不保留系统内存"选项失效 | 开放 | [#9033](https://github.com/unslothai/unsloth/issues/9033) |
| 🟡 **中** | **GGUF 导出到 HuggingFace 报 524 超时**，LoRA 导出正常，疑似网关/上传逻辑问题 | 开放，今日新增 | [#9516](https://github.com/unslothai/unsloth/issues/9516) |
| 🟡 **中** | **取消生成后产生连续 user 消息**，触发部分 Jinja 模板不兼容（Ministral 等严格模板）| **有 fix PR #9489 待合并** | [#9484](https://github.com/unslothai/unsloth/issues/9484) |
| 🟡 **中** | **XPU 后端 qLoRA 训练张量设备不一致**（`xpu:0` vs `cpu`），offloading 逻辑缺陷 | 开放，今日新增 | [#9524](https://github.com/unslothai/unsloth/issues/9524) |
| 🟢 **低** | **聊天设置 Preset 保存失败**（PUT /api/chat/settings 400），配置无法持久化 | 开放 | [#9500](https://github.com/unslothai/unsloth/issues/9500) |
| 🟢 **低** | **设置页 Remote & LAN 访问选项重复出现**，UI 冗余 | 开放 | [#9519](https://github.com/unslothai/unsloth/issues/9519) |
| 🟢 **低** | **下载新模型时 RAM 用量飙升**（Web UI）| 已关闭 #9032 | [#9032](https://github.com/unslothai/unsloth/issues/9032) |

**已修复**：Vite 构建缺失导出（#9521，commit `e0ae131` 引入，当日关闭）

---

## 性能与优化

| 方向 | 进展 | 链接 |
|:---|:---|:---|
| **长思维链渲染性能** | PR #9477 针对 DeepSeek-R1 类长思考场景优化：为实时推理 DOM 和 Markdown 处理设置硬边界，解决单位数 FPS 和多秒点击延迟问题；**影响 >3000 行代码，存在冲突待解** | [#9477](https://github.com/unslothai/unsloth/pull/9477) |
| **Studio 页面重载白屏** | PR #9251 阻止已渲染 Chat UI 在重载时先 tear down 为空白文档，消除闪烁；**>3000 行，重要级** | [#9251](https://github.com/unslothai/unsloth/pull/9251) |
| **llama-server 发现逻辑** | PR #9325 跳过无启动 revision 时的二进制发现，减少不必要的 VRAM finalizer 断言 | [#9325](https://github.com/unslothai/unsloth/pull/9325) |
| **KV Budget 准入死锁** | PR #9415 修复取消等待者阻塞队列时的重入准入逻辑（#9392 后续） | [#9415](https://github.com/unslothai/unsloth/pull/9415) |

---

## 新模型与硬件支持

| 需求/变更 | 详情 | 链接 |
|:---|:---|:---|
| **Ling 3.0 支持请求** | 社区热门需求（5 👍），要求 Studio 全生命周期支持：下载、加载、配置、服务 | [#8532](https://github.com/unslothai/unsloth/issues/8532) |
| **语音 GGUF 过滤** | PR #9433 将 llama.cpp 不支持的 TTS GGUF 架构从模型选择器中剔除，避免用户下载后无法运行 | [#9433](https://github.com/unslothai/unsloth/pull/9433) |
| **XPU 训练** | 见稳定性 #9524，Intel XPU 后端当前 qLoRA 训练不可用 | [#9524](https://github.com/unslothai/unsloth/issues/9524) |

---

## 功能与 API 演进

| 类别 | 内容 | 链接 |
|:---|:---|:---|
| **API 监听地址绑定** | 社区请求允许修改 `127.0.0.1` 以支持 LAN/WSL 访问；当前硬编码，阻碍多机/容器化开发 | [#9522](https://github.com/unslothai/unsloth/issues/9522) |
| **Token 统计覆盖 API** | 请求将 API 调用纳入用量统计，目前仅聊天计入；建议新增独立统计页面 | [#9071](https://github.com/unslothai/unsloth/issues/9071) |
| **记忆系统** | 请求类 ChatGPT 的 Profile 级长期记忆，按知识库加载 | [#9486](https://github.com/unslothai/unsloth/issues/9486) |
| **Web 搜索图片内联** | PR #9490 实现搜索结果图片直接嵌入聊天，后端缓存缩略图，不依赖第三方 CDN | [#9490](https://github.com/unslothai/unsloth/pull/9490) |
| **媒体 API 监控** | PR #9217 补齐 `/v1/images/generations`、`/v1/audio/speech`、`/v1/audio/transcriptions` 的监控可见性 | [#9217](https://github.com/unslothai/unsloth/pull/9217) |
| **托盘状态同步** | PR #9488 托盘"启动/停止服务器"菜单项实时反映后端状态，避免状态漂移 | [#9488](https://github.com/unslothai/unsloth/pull/9488) |
| **上下文溢出提示细化** | PR #9413 区分提示中哪部分（系统提示/历史/当前消息）超出上下文窗口，替代笼统的"缩短对话" | [#9413](https://github.com/unslothai/unsloth/pull/9413) |
| **LoRA 微调目标默认启用** | PR #9436 将 `finetune_language_layers` 等三项默认改为 `True`，避免 API/MCP 调用者遗漏导致空 LoRA | [#9436](https://github.com/unslothai/unsloth/pull/9436) |

---

## 对应用开发者的意义

| 场景 | 影响与建议 |
|:---|:---|
| **构建 Agent/多步骤工作流** | #9129（项目聊天跨视图保持运行）和 #9510（切换页面取消 Agent 任务）直接相关。当前 Studio 的 Project Chat Runtime 在视图切换时会 unmount 导致生成中断，**建议暂时避免在 Project 内运行长时 Agent 任务**，或监控 #9129 合并进度。 |
| **API 集成与计费** | Token 统计缺失 API 调用（#9071）意味着若你通过 OpenAI-compatible endpoint 对外提供服务，**当前无法准确计量成本**；同时 API 绑定 `127.0.0.1`（#9522）限制了容器化/局域网部署，需评估是否接受本地代理方案。 |
| **企业/隐私敏感部署** | Web Search 的路由细节仍未公开（#6329 关闭后续），且同步盘扫描（#9537）存在数据泄露风险。**建议隔离模型存储路径**，避免挂载云同步目录。 |
| **长思维链模型（R1 类）** | #9477 优化前，生产环境使用长思考模型可能因前端渲染性能导致用户体验崩溃；**可临时限制 reasoning 内容长度或关闭实时渲染**。 |
| **自定义微调（LoRA/API）** | #9436 修复前，通过 API/MCP 启动训练务必显式设置 `finetune_language_layers=True` 等参数，否则产出空适配器。 |

---

*日报基于 GitHub 公开数据生成，PR 评论数因 API 限制显示为 undefined，实际活跃度请以页面为准。*

</details>

---
*本日报由 [agents-radar](https://github.com/brysoh435-dev/agents-radar) 自动生成。*