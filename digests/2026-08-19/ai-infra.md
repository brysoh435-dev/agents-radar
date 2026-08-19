# AI 基础设施日报 2026-08-19

> 生成时间: 2026-08-19 05:56 UTC | 覆盖项目: 6 个

- [vLLM](https://github.com/vllm-project/vllm)
- [SGLang](https://github.com/sgl-project/sglang)
- [llama.cpp](https://github.com/ggml-org/llama.cpp)
- [Ollama](https://github.com/ollama/ollama)
- [LiteLLM](https://github.com/BerriAI/litellm)
- [Unsloth](https://github.com/unslothai/unsloth)

---

## 横向对比

# AI 基础设施生态横向对比分析 | 2026-08-19

---

## 1. 生态全景

当前 AI 基础设施呈现**"推理引擎军备竞赛白热化、AMD 生态急起直追、PD 分离架构从功能可用向生产可靠演进"**的三重态势。vLLM 与 SGLang 在 ROCm 路径上密集投入 AITER 优化，试图弥合与 NVIDIA 的 gap；llama.cpp 持续深耕多后端算子融合与边缘推理；Ollama 和 Unsloth 则因快速迭代引入多项严重回归，稳定性承压。LiteLLM 作为网关层聚焦供应链安全与多租户计费精度，差异化明显。

---

## 2. 各项目活跃度对比

| 项目 | 今日 Issues（活跃/严重） | 今日 PR（核心/合并） | Release 情况 | 关键特征 |
|:---|:---|:---|:---|:---|
| **vLLM** | 8+ 活跃（2 严重静默损坏、6 高优先级无 fix） | 7+ 性能优化 PR（ROCm 占 5）、2 紧急回滚 | 无 | ROCm 优化爆发，但 Kimi-K3 稳定性危机 |
| **SGLang** | 7 活跃（3 高严重） | 6+（PD 可靠性、多模态、AMD 修复） | 无 | PD 分离架构加固，HiCache 长会话失效待解 |
| **llama.cpp** | 11 活跃（3 高严重） | 8+（CUDA 融合、Metal/SYCL/Vulkan 补齐） | v0.1.2（语义化推进中，实际 b10488） | 多后端算子融合密集，ROCm TOP_K 阻断长上下文 |
| **Ollama** | 12+ 活跃（4 严重） | 3 核心修复（内存核算、元数据缓存、Qwen 兼容） | 无（v0.32.14 多项回归） | sm_86 静默回退 CPU、MLX 无 prompt caching |
| **LiteLLM** | 5 活跃（2 高严重） | 6+（UI 迁移、CI 修复、安全加固） | **v1.99.0-dev.1**（Docker cosign 签名） | 网关层聚焦供应链安全与计费精度 |
| **Unsloth** | 12+ 活跃（2 Critical、4 High） | 5+（ROCm 修复、Studio UI 稳定） | 无（v0.1.800-beta 多项回归） | Studio 桌面端稳定性危机，安全扫描器误杀 |

> **活跃度排序**：vLLM ≈ Unsloth > Ollama > llama.cpp > SGLang > LiteLLM（按 issue/PR 密度）
> **稳定性风险排序**：Unsloth（Critical×2）> Ollama（严重回归）> vLLM（静默损坏）> llama.cpp（ROCm 阻断）> SGLang > LiteLLM

---

## 3. 模型支持竞速

| 模型/架构 | 首发支持 | 跟进状态 | 落后方 |
|:---|:---|:---|:---|
| **DeepSeek-V4 / V3.2** | **vLLM**（DSA 路径路由、FP8 量化融合、稀疏 MLA 索引器） | SGLang（HiCache 长会话缓存失效修复中）；llama.cpp（ROCm TOP_K 阻断 >128K） | Ollama、Unsloth 无专项支持 |
| **Kimi-K3** | **SGLang**（NVFP4/FP8 混合精度） | vLLM（ROCm AITER 优化密集，但 **CUDA Graph 静默损坏** 阻断生产） | llama.cpp 无专项；Ollama 无 |
| **GLM-5 / GLM-5.2 MoE DSA** | **vLLM**（DSA 路径自动路由、MTP AMD MI300X 死锁修复中） | SGLang 无专项 | llama.cpp（GLM-5.2 多节点 RPC 崩溃）、Ollama 无 |
| **MiniMax-H3 / M3** | **SGLang**（ComfyUI 视频+音频联合去噪） | vLLM（MiniMax-M3 MTP AITER PA gluon decode） | llama.cpp、Ollama、Unsloth 无 |
| **Qwen3.6-27B / 3.8-27B** | **vLLM**（推理后独立采样参数、MTP 工具调用修复中） | SGLang 无专项；Ollama（system message 归一化修复中）；Unsloth 无 | llama.cpp（连续图像合并 bug） |
| **DFlash2 推测解码** | **vLLM**（分组动态深度卷积 + 候选选择器，向后兼容） | **llama.cpp**（PR 阶段，公式已公开） | SGLang、Ollama 无 |
| **PARD-2 并行草稿** | **vLLM**（AMD-AGI 目标对齐，独立于目标架构） | 无跟进 | — |
| **Nemotron-H** | **llama.cpp**（LoRA GGUF 转换修复、Mamba SSD 后端路由） | SGLang（Mamba SSD 严格后端路由） | vLLM（SSM 模型 PD 分离需 `--no-async-scheduling`） |

**领跑方**：**vLLM** 在新模型首发支持上最为激进（DeepSeek 全系、Kimi-K3 ROCm、GLM DSA、DFlash2、PARD-2）；**SGLang** 在多模态视频生成（MiniMax-H3）和混合精度（Kimi-K3 NVFP4）上差异化领先；**llama.cpp** 在边缘/Apple Silicon 场景（Nemotron-H、DFlash2 跟进）保持独特价值。

---

## 4. 性能优化前沿

| 方向 | 核心项目 | 具体手段 | 目标场景 |
|:---|:---|:---|:---|
| **KV Cache 架构革新** | vLLM、SGLang | vLLM #50779 **可扩展（按需增长）KV Cache**；SGLang #35360 **deferred KV release** 防跨节点污染 | 长上下文首次请求显存压力、PD 分离可靠性 |
| **ROCm/AMD 算子追赶** | **vLLM（最密集）**、SGLang、llama.cpp | AITER 集成（v0.20.0）、batched_gemm_bf16、fused MoE `e_per_rank_topk`、gluon paged-attention、sparse indexer top-k 分发 | DeepSeek-V4/Kimi-K3 在 MI300X/MI355X 上对标 B200 |
| **CUDA 算子融合** | vLLM、llama.cpp | vLLM: Q-LoRA RMSNorm + FP8 量化融合为不透明自定义算子；llama.cpp: **FFN Gate+SwiGLU/GEGLU MMQ epilogue 融合**（消除 gate 张量内存占用） | 降低内存带宽瓶颈、兼容分段 CUDA Graph |
| **推测解码效率** | vLLM、SGLang、llama.cpp | vLLM: 3D split-KV attention 解除 `max_seqlen_q>1` 限制；SGLang: Ngram v2 预计算 draft 完全重叠调度；llama.cpp: DFlash2 动态深度卷积 | 降低 draft-verification 流水线气泡 |
| **量化精度-效率权衡** | SGLang、vLLM、llama.cpp | SGLang: Kimi-K3 **NVFP4/FP8 混合精度**（MoE 专家 NVFP4，attention FP8）、MXFP8×BF16 MegaMOE；vLLM: DeepSeek V3.2 FP8 1x128 打包；llama.cpp: Q4_K/Q5_K branchless 反量化 | 新硬件世代（SM100/SM103）的精度陷阱规避 |
| **多模态预处理并行化** | SGLang | VLM 双 worker 池，37 个调用点中 33 个此前绕过 worker 池导致 tokenizer 事件循环阻塞 | 高并发视觉对话 TTFT |
| **SYCL/Intel GPU** | llama.cpp、Unsloth | llama.cpp: TILE kernel 门控（Qwen3.6-35B +42%~+169%）、Mamba-2 SSM_SCAN 上 GPU；Unsloth: XPU 自动检测文档化 | BMG/Alchemist 平台推理加速 |

**优化火力集中度**：**ROCm 路径 > CUDA 融合 > KV Cache 架构 > 推测解码 > 量化格式创新**。值得注意的是，vLLM 和 SGLang 的 ROCm 优化已非"能用"级别，而是针对特定模型（Kimi-K3、DeepSeek-V4）的**点对点性能对标**（B200 作为参照系）。

---

## 5. 分层定位差异

| 层级 | 代表项目 | 核心抽象 | 今日动态体现的定位 |
|:---|:---|:---|:---|
| **推理引擎（ datacenter-grade ）** | **vLLM** | Python 生态、PyTorch 原生、最大化吞吐与硬件利用率 | 极致的模型首发支持 + 多硬件路径（CUDA/ROCm）+ 复杂调度（PP/TP/PD 分离）。代价：快速迭代引入严重回归（Kimi-K3 静默损坏） |
| **推理引擎（ production-reliability ）** | **SGLang** | 结构化生成优化、PD 分离架构先驱、Rust/C++ 核心 | 从"功能可用"向"生产可靠"演进：deferred KV release、FlashAttention 边界硬化、NIXL 统一协议层。长会话缓存失效（#35129）是最后一道关卡 |
| **本地运行时 / 边缘推理** | **llama.cpp** | GGUF 格式标准、C/C++ 跨平台、最小依赖 | 多后端（Metal/SYCL/Vulkan/OpenCL）算子补齐，Apple Silicon 与 Intel GPU 的差异化价值。ROCm 长上下文阻断（#27021）是 datacenter 场景的天花板 |
| **本地运行时 / 开发者体验** | **Ollama** | 一键运行、模型管理、多引擎封装（llama.cpp/MLX） | **当前处于信任危机**：v0.32.14 sm_86 静默回退 CPU、MLX 无 prompt caching、`ollama ps` VRAM 统计长期不可靠。GGUF 元数据缓存重构（#17858）是结构性修复 |
| **LLM 网关 / 多租户路由** | **LiteLLM** | 统一 API 层、成本核算、密钥管理、observability | 供应链安全（Docker cosign）、计费精度（stream usage 细节）、MCP/Agent 工具链冲突（#37031）。与推理引擎解耦，聚焦"连接与治理" |
| **训练/微调框架** | **Unsloth** | LoRA/QLoRA 优化、Studio 桌面端、消费级 GPU 友好 | **Studio 产品化阵痛**：安全扫描器误杀、SQLite 死锁、图像变换回归。ROCm 修复（AOTriton、llama-server 重试）改善基础设施，但前端稳定性拖累核心定位 |

**关键洞察**：vLLM 与 SGLang 的边界正在模糊——两者都支持 PD 分离、都追逐 ROCm 性能、都服务 datacenter 场景。差异在于 **vLLM 优先"功能广度"**（更多模型、更多优化开关），**SGLang 优先"架构深度"**（结构化生成原生、NIXL 统一后端、生产可靠性加固）。llama.cpp 的不可替代性在于**边缘部署的格式标准与跨平台覆盖**，Ollama 若不能解决稳定性危机将面临被上游（llama.cpp/MLX）直接替代的风险。

---

## 6. 值得关注的趋势信号

### 🔥 行业趋势

| 信号 | 证据 | 含义 |
|:---|:---|:---|
| **AMD ROCm 从"能用"到"对标"** | vLLM 7 项 ROCm PR 明确提及 B200 性能参照（"~15× 慢于 B200"→AITER 优化）；SGLang AITER EAGLE GQA 打包 | AMD GPU 在 LLM 推理中的定位从"成本替代选项"升级为"性能竞争参与者"，MI350X/MI355X 世代是关键窗口 |
| **PD 分离进入"可靠性深水区"** | SGLang deferred KV release（#35360）、vLLM NIXL-only vs MooncakeStore+NIXL 组合乱码（#52627） | 功能演示阶段结束，跨节点缓存一致性、故障恢复、性能隔离成为决胜点 |
| **量化格式与硬件世代强绑定** | SGLang SM100 vs SM103 兼容性陷阱（#34340）；vLLM NVFP4/FP8 混合精度；llama.cpp i-quant | 量化不再是"通用压缩"，而是**硬件特化的性能契约**，选型需明确目标 GPU 架构 |
| **推测解码架构分化** | vLLM: DFlash2（动态深度卷积）、PARD-2（AMD 并行草稿）、EAGLE3；SGLang: Ngram v2；llama.cpp: adaptive MTP | 无统一标准，各引擎押注不同技术路线，应用层面临**锁定风险** |
| **Agent 工作流倒逼推理引擎特性** | vLLM 推理后独立采样参数（#52876）、SGLang HiCache 长会话、Ollama MLX 无 caching | "think 高 temperature / output 低 temperature" 等 Agent 特定需求，正从应用层 hack 下沉为引擎原生能力 |

### 🎯 Agent/应用开发者行动清单

| 优先级 | 行动 | 风险/机会 |
|:---|:---|:---|
| **P0** | **生产环境禁用 vLLM Kimi-K3 CUDA Graph**，验证 `--enforce-eager` | 静默输出损坏（#52531）无 fix PR，数据正确性风险 |
| **P0** | **SGLang PD 分离启用 NIXL-only**，回避 MooncakeStore+NIXL MultiConnector | 间歇性乱码（#52627），分离式部署场景 |
| **P1** | **评估 vLLM 可扩展 KV Cache（#50779）** 对长上下文 SaaS 的显存收益 | 按需增长替代预分配，首次请求延迟与成本结构变化 |
| **P1** | **锁定 Ollama v0.32.13 或迁移至 llama.cpp 直接部署**，规避 sm_86/MLX 回归 | v0.32.14 多项严重问题无 fix，GPU 调度信任崩塌 |
| **P1** | **暂缓 Unsloth v0.1.800-beta 升级**，关注 #9239 #9241 修复 | 安全扫描器误杀、图像变换失效阻断微调流程 |
| **P2** | **LiteLLM MCP 配置避免 `require_approval: "never"`**，等待 #37031 修复 | 代理端 auto-execute 与客户端 tool 冲突，重复副作用 |
| **P2** | **验证 Qwen3.6-27B + MTP 工具调用**，临时禁用 MTP 或降级 | vLLM #46249 已知回归，Agent 工具链稳定性 |
| **P2** | **Docker 镜像启用 cosign 验证**（LiteLLM v1.99.0-dev.1+） | 供应链安全基线，密钥自 commit `0112e53` 固定 |

---

*报告生成时间：2026-08-19 | 数据来源：各项目 GitHub 公开数据*

---

## 各项目详细报告

<details>
<summary><strong>vLLM</strong> — <a href="https://github.com/vllm-project/vllm">vllm-project/vllm</a></summary>

# vLLM 动态日报 | 2026-08-19

## 今日速览

今日 vLLM 社区活跃度极高，**Kimi-K3 与 DeepSeek-V4 的 ROCm 优化**成为绝对焦点，同时出现多起 CUDA Graph 相关的静默输出损坏回归问题需要警惕。Rust 前端功能补齐和可扩展 KV Cache 两大长期架构项目持续推进。

---

## 版本发布与破坏性变更

**无新版本发布。**

⚠️ **潜在破坏性变更预警**：
- PR #52870 / #52881 紧急回滚了昨日合并的 MM `keep_on_cpu=True` 变更，该变更导致 H200 上 `test_beam_search_passes_multimodal_data` 等采样器测试失败，并引发多模态元数据设备放置问题。[#52870](https://github.com/vllm-project/vllm/pull/52870) [#52881](https://github.com/vllm-project/vllm/pull/52881)

---

## 新模型与硬件支持

| 项目 | 说明 | 链接 |
|:---|:---|:---|
| **DeepSeek V3.2 注意力 FP8 量化融合** | 将 Q-LoRA RMSNorm 与 1x128 FP8 打包量化融合，Q 投影与稀疏索引器共享量化激活；CuTe DSL Q kernel 注册为不透明自定义算子以兼容分段 CUDA Graph | [#51943](https://github.com/vllm-project/vllm/pull/51943) |
| **PARD-2 并行草稿模型** | 新增 AMD-AGI/PARD 目标对齐并行草稿模型推测解码支持，独立于目标模型架构 | [#49406](https://github.com/vllm-project/vllm/pull/49406) |
| **DFlash2 架构扩展** | 新增分组动态深度卷积 + 候选选择器，现有 DFlash 检查点向后兼容 | [#52816](https://github.com/vllm-project/vllm/pull/52816) |
| **DSA 模型 NVIDIA 路径路由** | `DeepseekV32ForCausalLM`、`GlmMoeDsaForCausalLM` 及其 MTP 草稿模型默认走 NVIDIA 优化 CUDA 路径 | [#52861](https://github.com/vllm-project/vllm/pull/52861) |

---

## 性能与优化

### ROCm/AMD 深度优化（Kimi-K3 / DeepSeek-V4 专项）

| PR | 优化内容 | 链接 |
|:---|:---|:---|
| **AITER 升级至 0.20.0** | 集成最新 AITER 版本，含多项 kernel 优化 | [#52826](https://github.com/vllm-project/vllm/pull/52826) |
| **DSv4 综合性能提升** | ① AITER topk decode 替换 vLLM 自研扩展；② AITER batched_gemm_bf16 替换小 decode batch(T≤32) 的 `torch.einsum`；③ 启用 AITER fused moe 的 `e_per_rank_topk` 路径 | [#52878](https://github.com/vllm-project/vllm/pull/52878) |
| **C4A top-k AITER 化** | C4A 预填充/decode top-k 走 AITER v0.1.19，长上下文回退到 graph-safe 调优原生路径 | [#52882](https://github.com/vllm-project/vllm/pull/52882) |
| **MiniMax-M3 MTP 与 dense 层 AITER PA gluon decode** | 启用 gluon paged-attention decode kernel 处理多 token query 长度，EAGLE3 推测解码不再回退到原生 unified_attention | [#52849](https://github.com/vllm-project/vllm/pull/52849) |
| **稀疏索引器 decode top-k AITER 分发** | DeepSeek-V3.2/GLM-5 稀疏注意力索引器每 decode 步的 top-k 从 HIP radix kernel（随上下文宽度线性增长，~15× 慢于 B200）迁移到 AITER | [#50470](https://github.com/vllm-project/vllm/pull/50470) |
| **稀疏 MLA 索引器优化** | 针对 topk-token=2048 优化，基于 ATOM speed of light 实现 | [#46172](https://github.com/vllm-project/vllm/pull/46172) |
| **非整除小头 MLA decode 路由修复** | Kimi-K3 TP8 时每 rank 12 头非 16 整除，修复后正确路由到 gluon 而非 padded ASM persistent 路径 | [#51403](https://github.com/vllm-project/vllm/pull/51403) |

### 通用优化

| PR | 优化内容 | 链接 |
|:---|:---|:---|
| **可扩展（可增长）KV Cache** | 可选的按需增长 KV cache 分配，替代预分配，显著降低长上下文启动显存压力；基于 #51718 堆叠 | [#50779](https://github.com/vllm-project/vllm/pull/50779) |
| **MoE-LoRA shrink reduction 外提** | `_fused_moe_lora_small_batch_kernel` 将 K 循环内的 warp-shuffle 归约改为循环后单次归约，减少 ~K 倍 shuffle 开销 | [#52880](https://github.com/vllm-project/vllm/pull/52880) |
| **推测解码启用 3D split-KV attention** | 解除 `max_seqlen_q > 1` 的过度保守限制，改为按 query row 宽度判断，推测解码场景可用更高效的 split-KV 路径 | [#52879](https://github.com/vllm-project/vllm/pull/52879) |
| **推理后独立采样参数** | 支持 `<think>...</think>` 结束后使用不同采样参数（如 Qwen 3.8 27B 推理阶段需要高 temperature，输出阶段需要低 temperature） | [#52876](https://github.com/vllm-project/vllm/pull/52876) |

---

## 稳定性与回归

### 🔴 严重：静默输出损坏（Silent Corruption）

| Issue | 描述 | 状态 | 链接 |
|:---|:---|:---|:---|
| **Kimi-K3 CUDA Graph 静默损坏** | batch=1 时三种 cudagraph 模式均出现静默输出损坏，三种不同失败模式 | **无 fix PR，活跃调查中** | [#52531](https://github.com/vllm-project/vllm/issues/52531) |
| **Kimi-K3 1P1D NIXL Direct-PD 静默损坏** | TP=8 1P1D 分离式部署中，NIXL-only PD 正常，但 MooncakeStoreConnector + NixlConnector MultiConnector 组合出现间歇性乱码生成 | **无 fix PR** | [#52627](https://github.com/vllm-project/vllm/issues/52627) |

### 🟡 高优先级：崩溃与正确性

| Issue | 描述 | 状态 | 链接 |
|:---|:---|:---|:---|
| **Prefix Cache 0% 命中（DSv4-Flash）** | DeepSeek-V4-Flash hybrid groups 每次请求重新分配时丢失所有首块 cache key，属于 #32802 的 DSv4 变体 | **无 fix PR** | [#42948](https://github.com/vllm-project/vllm/issues/42948) |
| **Qwen3.6-27B MTP 工具调用失败** | Responses API 启用 MTP 时工具调用回归失败 | **无 fix PR** | [#46249](https://github.com/vllm-project/vllm/issues/46249) |
| **Qwen3.5 JSON Schema 输出乱码空格** | `response_format=json_schema` 时空格乱码 | **无 fix PR** | [#38696](https://github.com/vllm-project/vllm/issues/38696) |
| **qwen3_next_mtp 非法地址访问** | `num_speculative_tokens=5` 高负载下 `cudaErrorIllegalAddress` | **无 fix PR** | [#37035](https://github.com/vllm-project/vllm/issues/37035) |
| **GLM 5.2 MTP AMD MI300X 死锁** | RCCL `no transport for peer` 导致首步推测解码死锁 | **无 fix PR** | [#48568](https://github.com/vllm-project/vllm/issues/48568) |
| **PD 双向 KV 传输 + 推理痕迹剥离** | 多轮对话中剥离 thinking traces 导致错误结果 | **无 fix PR** | [#43094](https://github.com/vllm-project/vllm/issues/43094) |
| **SSM 模型 PD 分离需 `--no-async-scheduling`** | TP>1 时 NemotronH/Qwen3.5 等 SSM 模型精度下降，同步调度绕过 | **根因已识别，无 fix PR** | [#37285](https://github.com/vllm-project/vllm/issues/37285) |

### 🟢 已修复/有缓解

| Issue/PR | 描述 | 状态 | 链接 |
|:---|:---|:---|:---|
| **MM `keep_on_cpu` 回滚** | 昨日合并导致 CI 回归，今日紧急回滚 | ✅ **已 revert** | [#52870](https://github.com/vllm-project/vllm/pull/52870) [#52881](https://github.com/vllm-project/vllm/pull/52881) |
| **aarch64 NVFP4 CUDA illegal instruction** | GB10 上 V1 Engine + NVFP4 decode 崩溃 | ✅ **已关闭** | [#39761](https://github.com/vllm-project/vllm/issues/39761) |

### ⚠️ 性能回归

| Issue | 描述 | 链接 |
|:---|:---|:---|
| **v0.20 MoE 延迟吞吐回归** | v0.20.0 vs v0.19.0，8×H200 上 MoE 模型延迟增加、吞吐下降 | [#41306](https://github.com/vllm-project/vllm/issues/41306) |
| **DFlash 高并发性能倒挂** | Qwen3.5-35B-A3B 并发>8 时 DFlash 慢于基线，并发=1 加速比远低于论文 | [#42505](https://github.com/vllm-project/vllm/issues/42505) |

---

## 对应用开发者的意义

### 1. **Kimi-K3 / DeepSeek-V4 生产部署建议**
- **ROCm 用户**：今日大量 AITER 优化 PR 表明 AMD 路径正快速追赶，但 **Kimi-K3 存在 CUDA Graph 静默损坏和 PD 分离乱码两大严重问题**，建议生产环境：
  - 禁用 CUDA Graph（`--enforce-eager` 或 `--no-enable-prefix-caching` 组合验证）
  - PD 分离部署暂时使用 NIXL-only，避免 MooncakeStore + NIXL MultiConnector 组合
- **NVIDIA 用户**：DSA 模型（DeepSeek V3.2、GLM MoE DSA）自动走优化 CUDA 路径，无需手动配置

### 2. **推测解码策略更新**
- DFlash2 引入动态深度卷积 + 候选选择器，高并发场景下建议关注 #42505 的修复进展后再评估生产部署
- PARD-2 作为 AMD 主导的并行草稿方案，为 AMD GPU 用户提供新的推测解码选项

### 3. **多模态应用注意回滚**
- 若昨日基于 main 构建了多模态服务，今日 `keep_on_cpu` 回滚可能影响行为，建议重新验证 beam search 等多模态采样路径

### 4. **Agent/工具调用开发者**
- Qwen3.6-27B + MTP + 工具调用存在已知回归，建议临时禁用 MTP 或降级到稳定版本
- 新增"推理后独立采样参数"功能（#52876），对需要 `<think>` 阶段高探索性、输出阶段高确定性的 Agent 工作流非常有价值

### 5. **长上下文服务**
- 可扩展 KV Cache（#50779）即将落地，可显著降低长上下文首次请求的显存预分配压力，适合按需扩展的 SaaS 场景

---

*日报基于 vllm-project/vllm 2026-08-19 GitHub 公开数据生成*

</details>

<details>
<summary><strong>SGLang</strong> — <a href="https://github.com/sgl-project/sglang">sgl-project/sglang</a></summary>

# SGLang 动态日报 | 2026-08-19

## 今日速览

今日社区聚焦 **PD 分离架构的可靠性加固** 与 **多模态推理基础设施扩展**。核心进展包括：NIXL 后端新增 deferred KV release 机制防止跨节点缓存污染（#35360），FlashAttention CUDA Graph 元数据边界硬化修复潜在越界（#35454），以及 ComfyUI 插件扩展至 MiniMax-H3 视频生成模型（#35352）。AMD 路径持续活跃，ROCm 修复 FLUX 预热崩溃并优化 a8w8 BMM 内存布局（#34481、#34498）。

---

## 版本发布与破坏性变更

**无新版本发布。**

---

## 新模型与硬件支持

| 条目 | 说明 | 链接 |
|:---|:---|:---|
| **MiniMax-H3 ComfyUI 节点** | 新增视频+音频联合去噪模型支持，支持 `t2va`/`fl2va`/`ref2va` 任务路由，突破此前仅图像模型的限制 | [#35352](https://github.com/sgl-project/sglang/pull/35352) |
| **Intel XPU 嵌入/视觉模型扩展** | 支持 `bge-base-en-v1.5`、`nomic-embed-text-v1.5`、`granite-embedding-english-r2`、`InternVL3_5-30B-A3B`；修复 XPU 上 BertModel 输出错误及 InternVL 图像预处理崩溃 | [#35304](https://github.com/sgl-project/sglang/pull/35304) |
| **Kimi-K3 NVFP4/FP8 混合精度** | 支持 ModelOpt 混合精度 checkpoint：MoE 专家用 NVFP4（SiTU β=4），attention 投影用 FP8 weight-only 128x128 block | [#35077](https://github.com/sgl-project/sglang/pull/35077) |
| **MXFP8 × BF16 MegaMOE** | 新增量化格式支持，扩展 DeepSeek 系列 MoE 的精度-效率权衡选项 | [#35459](https://github.com/sgl-project/sglang/pull/35459) |
| **Mamba SSD 严格后端路由** | 新增 `flashinfer_ssd` 和 `cake` 预填充后端，Nemotron-H C256 逻辑元数据映射至物理 C128 域 | [#35444](https://github.com/sgl-project/sglang/pull/35444) |

---

## 性能与优化

| 条目 | 优化点 | 链接 |
|:---|:---|:---|
| **AMD a8w8 BMM 直接写消除 transpose copy** | MI355X 上 Kimi-K2.7-Code-MXFP4 验证：吸收 a8w8 BMM 输出直接按 `o_proj` epilogue 布局，下游 `flatten` 变为零拷贝 view | [#34498](https://github.com/sgl-project/sglang/pull/34498) |
| **Ngram Speculative Decoding v2** | 预计算 draft 并完全重叠调度，降低投机解码的流水线气泡 | [#22332](https://github.com/sgl-project/sglang/pull/22332) |
| **VLM 多模态预处理并行化** | 默认启用双 worker 池，`process_and_combine_mm_data` 37 个调用点中 33 个此前绕过 worker 池导致 tokenizer 事件循环阻塞 | [#35342](https://github.com/sgl-project/sglang/pull/35342) [#35349](https://github.com/sgl-project/sglang/pull/35349) |
| **AMD EAGLE target-verify GQA 打包** | AITER 路径加载共享 TP-local KV head 一次覆盖 query head block，延续 #34517 Triton 优化思路 | [#35457](https://github.com/sgl-project/sglang/pull/35457) |

---

## 稳定性与回归

| 严重程度 | 问题 | 状态 | 链接 |
|:---|:---|:---|:---|
| 🔴 **高** | **HiCache + PP 多 rank 发散崩溃** — Pipeline Parallelism 与 HiCache L3 交互导致 rank 状态不一致 | **Fix PR 开放** #27010 | [#22607](https://github.com/sgl-project/sglang/issues/22607) |
| 🔴 **高** | **DeepSeek-V4-Flash + DSPARK + HiCache 长会话缓存失效** — agentic 多轮对话中 `#cached-token: 0`，短请求可达 98% 命中率 | 待修复 | [#35129](https://github.com/sgl-project/sglang/issues/35129) |
| 🔴 **高** | **B300 (sm_103) 两内核失败** — `is_sm100_supported()` 家族检查误让 sm_103 走 SM100 路径，cutedsl TGV BF16 GEMM 报 Xid 13 CGA，trtllm-gen MoE finalize 静默挂起 | 待修复 | [#34340](https://github.com/sgl-project/sglang/issues/34340) |
| 🟡 **中** | **FlashAttention CUDA Graph 越界** — aggregate batch tokens 超单请求 max_context_len，sliding-window write-location buffer 分配不足 | **Fix PR 开放** #35454 | [#35454](https://github.com/sgl-project/sglang/pull/35454) |
| 🟡 **中** | **DCP 下 req_to_token row headroom 缩放错误** — `attn_dcp_size` 未计入 headroom，近上下文限制时写入越界到相邻 row | **Fix PR 开放** #35424 | [#35424](https://github.com/sgl-project/sglang/pull/35424) |
| 🟡 **中** | **MoRI EP 后端静默输出损坏** — 小的 `SGLANG_MORI_NUM_MAX_DISPATCH_TOKENS_PER_RANK` 导致 gsm8k=0，推测接受长度仍高但语义错误 | 待修复 | [#27194](https://github.com/sgl-project/sglang/issues/27194) |
| 🟡 **中** | **Kimi-K3 [PAD] token 风暴 + DSPARK inf/nan** — 已发布镜像未含 #32477 修复，`allowed_special="all"` 允许注入 [PAD] | 待修复 | [#32968](https://github.com/sgl-project/sglang/issues/32968) |
| 🟢 **低** | **ROCm FLUX 预热崩溃** — PTX inline asm diffusion norm fusion 误启用 | **已修复** #34481 | [#34481](https://github.com/sgl-project/sglang/pull/34481) |
| 🟢 **低** | **Intel XPU EAGLE/NEXTN TP=2 预热挂起** — #34238 将 verify-decision TP broadcast 移出 sampling branch 引入 | **已修复** #35144 | [#35144](https://github.com/sgl-project/sglang/pull/35144) |
| 🟢 **低** | **ROCm aiter attention 拒绝 GQA** — `cosmos3_nano` 启动失败 | **Fix PR 开放** #35456 | [#35456](https://github.com/sgl-project/sglang/pull/35456) |

---

## 对应用开发者的意义

| 场景 | 影响 |
|:---|:---|
| **Agent 长会话部署** | DeepSeek-V4-Flash + HiCache 的缓存失效问题（#35129）直接影响多轮 agent 的 TTFT 和成本，生产环境建议监控 `#cached-token` 指标并暂缓长会话场景的全量 rollout |
| **PD 分离生产可靠性** | #35360 的 deferred KV release 机制解决了 abort 场景下跨节点缓存污染的关键竞态，配合 #34510 的统一协议层推进，PD 分离从"功能可用"向"生产可靠"演进 |
| **多模态应用集成** | ComfyUI 扩展至视频生成（#35352）+ VLM 预处理并行化（#35349），意味着 SGLang 正从"文本 LLM 后端"向"统一多模态服务引擎"转型，应用层可期待更一致的 API 体验 |
| **量化模型选型** | Kimi-K3 的 NVFP4/FP8 混合精度（#35077）和 MXFP8 MegaMOE（#35459）提供了新的精度-效率权衡点，但需注意量化格式与硬件世代的绑定（SM100 vs SM103 的兼容性陷阱 #34340） |
| **AMD 平台迁移** | ROCm 路径近期修复密集（FLUX 崩溃、GQA 拒绝、内存拷贝优化），但 MI350X 的 block-fp8 数值正确性问题（#28685）提示复杂量化路径仍需充分验证后再上线 |

---

*数据来源：sgl-project/sglang GitHub 仓库，统计窗口 2026-08-18 至 2026-08-19 UTC*

</details>

<details>
<summary><strong>llama.cpp</strong> — <a href="https://github.com/ggml-org/llama.cpp">ggml-org/llama.cpp</a></summary>

# llama.cpp 动态日报 | 2026-08-19

## 今日速览

今日 llama.cpp 密集推进多后端算子融合与推理优化：CUDA 侧完成 FFN Gate+SwiGLU/GEGLU 的 MMQ epilogue 融合（消除中间张量），Metal 补齐 DIAG_MASK_INF 与 i-quant 小 batch 解码支持；同时社区高度关注 ROCm gfx1151 预填充性能瓶颈与 Vulkan 多 buffer 崩溃等稳定性问题。版本管理方面，v0.1.2 发布但语义化版本仍在推进中，主构建线已推进至 b10488。

---

## 版本发布与破坏性变更

| 项目 | 详情 |
|:---|:---|
| **v0.1.2 发布** | 语义化版本仍在进行中，参考 [ggml#1579](https://github.com/ggml-org/ggml/discussions/1579)。实际构建以 nightly build **b10485** 为基线，最新 CI 构建为 **b10488** |
| **CMake 清理** | [b10483](https://github.com/ggml-org/llama.cpp/releases/tag/b10483) 修复 xcframework 构建，引入 `vendor::hash` alias target，下游若直接依赖 `vendor-hash` 目标需迁移至新命名 |

---

## 新模型与硬件支持

| 类别 | 详情 | 链接 |
|:---|:---|:---|
| **模型架构** | **DFlash2 推测解码** 支持进入 PR 阶段：新增 grouped dynamic depthwise convolution + candidate selector，公式为 `out[i,c] = Σ_t (base[t,c] + δ[i,t,g(c)]) · x[i−t,c]` | [#27342](https://github.com/ggml-org/llama.cpp/pull/27342) |
| **模型架构** | **Nemotron-H LoRA GGUF 转换修复**：解决 hparams 解析与 LoRA 键名映射两处独立缺陷 | [#27356](https://github.com/ggml-org/llama.cpp/pull/27356) |
| **Metal (Apple Silicon)** | 新增 `GGML_OP_DIAG_MASK_INF` 支持，补齐注意力掩码算子缺口 | [#27197](https://github.com/ggml-org/llama.cpp/pull/27197) |
| **Metal (Apple Silicon)** | `mul_mv_ext`（BS 2-8 小 batch 解码路径）新增 **i-quant 支持**（IQ1/IQ2/IQ3/IQ4_XS），解决投机解码在此批宽下的 fallback 问题 | [#27350](https://github.com/ggml-org/llama.cpp/pull/27350) |
| **OpenCL** | 移植 fused `ssm_scan` kernel（Mamba-2, d_state ∈ {128,256}），此前 fallback 至 CPU | [#26439](https://github.com/ggml-org/llama.cpp/pull/26439) |
| **OpenCL** | 修复 norm kernel local size 计算：当 `ne00 < 64` 时可能破坏"必须为 2 的倍数"约束 | [#27339](https://github.com/ggml-org/llama.cpp/pull/27339) |
| **XDNA (AMD NPU)** | 社区 Feature Request：请求 AMD XDNA 后端支持，👍 30，持续跟踪中 | [#21725](https://github.com/ggml-org/llama.cpp/issues/21725) |

---

## 性能与优化

| 优化项 | 详情 | 预期收益 | 链接 |
|:---|:---|:---|:---|
| **CUDA: FFN Gate + GLU epilogue 融合** | 将 `ffn_gate` 与 SwiGLU/GEGLU 融合至 `mul_mat_q` write-back epilogue，gate 结果驻留寄存器完成 `silu/gelu(gate) * up`，**消除 gate 张量内存占用** | 减少内存带宽，关闭 decode 与 prefill 的融合缺口 | [#27341](https://github.com/ggml-org/llama.cpp/pull/27341) |
| **CUDA: Q4_K/Q5_K branchless 反量化** | 移除 scale unpack 的分支判断，避免 nvcc 为每列重复执行 | **batch size > 1 时 mmvq 性能提升**（具体数字待 CI 基准） | [#26705](https://github.com/ggml-org/llama.cpp/pull/26705) |
| **SYCL: TILE kernel 门控调整** | 量化 KV decode（q4_0/q8_0）从 VEC 切换至 TILE kernel，BMG 平台实测 | **Qwen3.6-35B: +42%~+169%**（32K/118K context），零回归 | [#26689](https://github.com/ggml-org/llama.cpp/pull/26689) |
| **Vulkan: Q8_0 KV 单次反量化** | coopmat1 prefill 中 KV 反量化从 32 次/ workgroup 降至 1 次，scratch 重组为 per-head contiguous | 减少冗余计算与内存操作 | [#25494](https://github.com/ggml-org/llama.cpp/pull/25494) |
| **Vulkan: 0<->2 permute CONT 分块转置** | `ggml_cont(ggml_permute(x, 2, 1, 0, 3))` 从逐元素 strided copy 升级至 shared-memory tiled transpose | 显著改善内存访问模式 | [#26585](https://github.com/ggml-org/llama.cpp/pull/26585) |
| **SYCL: Alchemist OneDNN 门控** | 修复 SPDA 路由在 Alchemist GPU 上的错误结果，重新开放性能优化路径 | 恢复 Alchemist 的 prompt processing 加速 | [#26635](https://github.com/ggml-org/llama.cpp/pull/26635) |
| **ROCm gfx1151 预填充瓶颈** | 社区识别 Strix Halo (gfx1151) 默认 cost model 导致预填充性能损失，存在明确优化空间 | 待官方合并优化方案 | [#21284](https://github.com/ggml-org/llama.cpp/issues/21284) |

---

## 稳定性与回归

| 严重程度 | 问题 | 状态 | 链接 |
|:---|:---|:---|:---|
| 🔴 **高** | **CUDA illegal memory access** (`cudaStreamSynchronize`)：Qwen3.6-35B MoE + partial expert offload + flash-attn，第二请求确定性崩溃，`-fa off` 规避 | **待修复**，交叉验证于 b10107/b10243 | [#26609](https://github.com/ggml-org/llama.cpp/issues/26609) |
| 🔴 **高** | **ROCm TOP_K 崩溃**：`ncols > 1024` 时 bitonic kernel block-size 溢出，**阻断 DeepSeek V4 ctx > 128K** | **PR 待审**，影响 RPC worker | [#27021](https://github.com/ggml-org/llama.cpp/issues/27021) |
| 🔴 **高** | **ROCm gfx1151 RPC worker 崩溃**：`GGML_OP_TOP_K` 在 DeepSeek V4 prefill > 4096 tokens 时崩溃 | 与 #27021 同源，待合并修复 | [#26746](https://github.com/ggml-org/llama.cpp/issues/26746) |
| 🟡 **中** | **CUDA kernel stall / watchdog kill**：RTX Pro 6000 Blackwell MAX-Q，模型执行中 GPU 挂起 | **待诊断**，需更多复现信息 | [#27102](https://github.com/ggml-org/llama.cpp/issues/27102) |
| 🟡 **中** | **Vulkan: multi buffers unsupported → segfault**：多 GPU 场景下 `ggml-backend-meta` 断言失败 | **长期 open**，影响 Polaris 等多卡聚合 | [#22197](https://github.com/ggml-org/llama.cpp/issues/22197) |
| 🟡 **中** | **Vulkan: GET_ROWS misaligned offset hard crash**：view tensor 非零 `view_offs` 触发 `GGML_ASSERT` | **PR #26854 提供 CPU fallback 修复** | [#26854](https://github.com/ggml-org/llama.cpp/pull/26854) |
| 🟡 **中** | **Gemma 4 31B MTP draft 崩溃**：Vulkan 后端 "pre-allocated tensor cannot run operation NONE" | **待修复** | [#24492](https://github.com/ggml-org/llama.cpp/issues/24492) |
| 🟡 **中** | **Gemma 4 官方 QAT GGUF vocab 加载失败**：`id_to_token.size() == token_to_id.size()` 断言 | **待修复**，影响官方量化模型 | [#25739](https://github.com/ggml-org/llama.cpp/issues/25739) |
| 🟡 **中** | **Qwen3.6-35B-A3B 连续图像合并为 super-frames**：llama-server 中 4 图变 2 图，视觉理解不完整 | **待修复** | [#24303](https://github.com/ggml-org/llama.cpp/issues/24303) |
| 🟡 **中** | **GLM-5.2 多节点 CUDA RPC 崩溃**：`invalid data ptr` / `ggml_backend_rpc_buffer_get_tensor` abort | **待修复**，阻断分布式推理 | [#26583](https://github.com/ggml-org/llama.cpp/issues/26583) |
| 🟢 **低** | **Qwen3.8-27B Ridge quantizations 加载 abort**：Vulkan 后端 | 新上报，待分类 | [#27299](https://github.com/ggml-org/llama.cpp/issues/27299) |
| 🟢 **低** | **MTP decoding pre-Ampere 崩溃**：SM < 5.0 不支持，社区提供可用 patch | **有 patch，待合并** | [#25713](https://github.com/ggml-org/llama.cpp/issues/25713) |
| 🟢 **低** | **SYCL 后端模型加载回归**：b10451 起 Ministral-3-14B 无法加载 | **已关闭**，可能为临时构建问题 | [#27253](https://github.com/ggml-org/llama.cpp/issues/27253) |

---

## 对应用开发者的意义

| 场景 | 影响与行动建议 |
|:---|:---|
| **高并发推理服务 (llama-server)** | **DFlash2 支持** [#27342](https://github.com/ggml-org/llama.cpp/pull/27342) 与 **adaptive MTP draft** [#27210](https://github.com/ggml-org/llama.cpp/pull/27210) 即将落地，建议提前测试投机解码配置；**`dedup-cache-models` 预设** [#27346](https://github.com/ggml-org/llama.cpp/pull/27346) 解决缓存模型重复暴露问题，多租户场景关注 |
| **Apple Silicon 部署** | **i-quant + small batch 解码** [#27350](https://github.com/ggml-org/llama.cpp/pull/27350) 直接利好投机解码（verify batch 通常为 2-8），IQ 系列量化模型在 Mac 上的吞吐将显著提升；**DIAG_MASK_INF** [#27197](https://github.com/ggml-org/llama.cpp/pull/27197) 补齐特定注意力变体的支持 |
| **AMD GPU (ROCm) 生产环境** | **gfx1151 预填充性能** [#21284](https://github.com/ggml-org/llama.cpp/issues/21284) 与 **TOP_K 崩溃** [#27021](https://github.com/ggml-org/llama.cpp/issues/27021) 是当前最大风险，DeepSeek V4 长上下文 (>128K) 部署需规避或等待修复；建议跟踪 [b10488](https://github.com/ggml-org/llama.cpp/releases/tag/b10488) 的 OpenVINO 2026.3 更新 |
| **Intel GPU (SYCL/OpenCL)** | **Mamba-2 SSM_SCAN 上 GPU** [#26439](https://github.com/ggml-org/llama.cpp/pull/26439) 与 **TILE kernel 门控** [#26689](https://github.com/ggml-org/llama.cpp/pull/26689) 显著改善 state-space 模型与量化 KV 的推理效率，BMG 平台优先升级 |
| **量化模型选型** | **Q4_K/Q5_K CUDA 优化** [#26705](https://github.com/ggml-org/llama.cpp/pull/26705) 改善 batch 推理效率，但 **SM_60 (P100) FP32→FP16 静默降级** [#25593](https://github.com/ggml-org/llama.cpp/issues/25593) 仍在影响老卡正确性，老旧数据中心 GPU 需验证输出质量 |
| **多模态应用** | **LFM2 图像 tiling 阈值修复** [b10486](https://github.com/ggml-org/llama.cpp/releases/tag/b10486) 与 **Qwen3.6 图像合并 bug** [#24303](https://github.com/ggml-org/llama.cpp/issues/24303) 并存，视觉对话应用建议验证多图输入的 token 计数与模型感知一致性 |

---

*日报基于 ggml-org/llama.cpp 公开 GitHub 数据生成，构建号与 PR 状态以官方仓库为准。*

</details>

<details>
<summary><strong>Ollama</strong> — <a href="https://github.com/ollama/ollama">ollama/ollama</a></summary>

# Ollama 动态日报 | 2026-08-19

## 1. 今日速览

今日 Ollama 无新版本发布，但社区活跃度极高：**v0.32.14 出现多项严重回归**——包括 sm_86 GPU 静默回退 CPU、MLX 引擎无 prompt caching、以及 Qwen3.8 系列的多重兼容性问题。核心团队正密集修复，dhiltgen 主导的 GGUF 元数据缓存重构和 llama-server 内存核算修复是今日技术重点。

---

## 2. 版本发布与破坏性变更

**无新 Release**。但需注意 v0.32.14 的已知破坏性变更：

| 问题 | 影响范围 | 状态 |
|:---|:---|:---|
| CUDA 13 构建链移除 sm_86 支持，且 CUDA 12 回退路径损坏 | RTX 30/A40/A6000 用户 | 🔴 无 fix，需降级或等待补丁 |
| `think` 参数类型校验收紧 | CLI/API 用户 | 🟡 PR #17855 修复中 |

- [Issue #17841](https://github.com/ollama/ollama/issues/17841): Ollama 0.32.14 silently falls back to CPU on sm_86 GPUs
- [Issue #17837](https://github.com/ollama/ollama/issues/17837): `/set think true\|false` accepted by CLI but rejected by backend

---

## 3. 新模型与硬件支持

| 动态 | 详情 |
|:---|:---|
| **Intel iGPU 长期悬而未决** | Iris Xe 支持请求 #3113 持续 17 个月，75 👍，无官方回应 |
| **AMD Strix Halo (gfx1151) 新 Bug** | ROCm 后端出现 KV cache 污染，跨请求内容泄漏 |
| **RX 9060 XT (gfx1200) TensileLibrary 缺失** | ROCm 新卡支持缺口 |

- [Issue #3113](https://github.com/ollama/ollama/issues/3113): Integrated Intel GPU support
- [Issue #17847](https://github.com/ollama/ollama/issues/17847): ROCm backend on Strix Halo bleeds KV state
- [Issue #17782](https://github.com/ollama/ollama/issues/17782): qwen3.8:27b Could not load "TensileLibrary_lazy_gfx1200.dat"

---

## 4. 性能与优化

| PR | 优化内容 | 预期收益 |
|:---|:---|:---|
| **[#17858](https://github.com/ollama/ollama/pull/17858)** GGUF 元数据提取与能力统一 | 将 metadata 缓存从内存移至 `<OLLAMA_MODELS>/metadata/sha256-*.json`，消除双缓存不一致 | 减少每请求 ~300ms 开销（承接 #17752） |
| **[#17857](https://github.com/ollama/ollama/pull/17857)** llama-server 内存核算修复 | 修复多模型加载时 buffer line 去重错误（speculative draft 覆盖 target 统计、hybrid attention 双 KV cache 丢失其一） | `ollama ps` VRAM 显示准确，调度器决策正确 |
| **[#17850](https://github.com/ollama/ollama/pull/17850)** MLX 引擎更新 | 临时携带 mlx-c #127 | 待上游合并 |
| **[#17855](https://github.com/ollama/ollama/pull/17855)** Qwen3.8 system message 归一化 | 合并分散的 system/developer 消息为单一前置消息 | 修复对话历史渲染错误 |

**进行中但待解决：**
- [Issue #17829](https://github.com/ollama/ollama/issues/17829): MLX 引擎**无 prompt/prefix caching**，agent 多步场景每请求全量 re-prefill（20-30K tokens），TTFT 线性增长
- [Issue #17833](https://github.com/ollama/ollama/issues/17833): v0.32.14 模型完全载入 VRAM 时仍**高 CPU 占用**（50-80%），0.32.13 正常

---

## 5. 稳定性与回归（按严重程度排列）

### 🔴 严重：数据正确性 / 静默失败

| Issue | 描述 | Fix PR |
|:---|:---|:---|
| [#17847](https://github.com/ollama/ollama/issues/17847) | **ROCm KV cache 污染**：Strix Halo 上请求 B 的回复包含请求 A 的内容 | 无 |
| [#17841](https://github.com/ollama/ollama/issues/17841) | **sm_86 静默回退 CPU**，GPU 显存占用为 0，用户无感知 | 无 |
| [#17856](https://github.com/ollama/ollama/issues/17856) | **qwen35moe 层数溢出**：`n_layer=4294967274`（uint32 下溢），0.31.x 起存在 | 无，已关闭（可能误关） |
| [#17778](https://github.com/ollama/ollama/issues/17778) | Qwen3.8 流式报错 "no user query found in messages"，205K context 下触发 | 无 |

### 🟡 中等：功能异常 / 兼容性问题

| Issue | 描述 | Fix PR |
|:---|:---|:---|
| [#14645](https://github.com/ollama/ollama/issues/14645) | Qwen3.5 系列 `think` 关闭时 `format` 被忽略 | **已关闭**（0.17.6 修复） |
| [#17839](https://github.com/ollama/ollama/issues/17839) | macOS 本地 Qwen 模型在 Agent 集成中无限挂起，原生 API 正常 | 无 |
| [#17811](https://github.com/ollama/ollama/issues/17811) | `ollama launch claude` 非交互模式失败：要求 stdin 或 prompt 参数 | 无 |
| [#17717](https://github.com/ollama/ollama/issues/17717) | Claude Code 不识别 `kimi-k2.7-code:cloud`，强制 200K auto-compact | 无 |
| [#17251](https://github.com/ollama/ollama/issues/17251) | `ollama ps` VRAM 与 `rocm-smi` 差异显著（23GB vs 实际更多），mtp-accelerator 模型 | #17857 可能修复 |
| [#17816](https://github.com/ollama/ollama/issues/17816) | qwen3.8 下载 defunct，`pull manifest` 返回 EOF | 无 |

### 🟢 轻微：安装 / CLI / 文档

| Issue | 描述 | Fix PR |
|:---|:---|:---|
| [#17860](https://github.com/ollama/ollama/issues/17860) | Ubuntu 26.04 安装脚本因缺 `zstd` 静默失败 | 无 |
| [#17842](https://github.com/ollama/ollama/issues/17842) | macOS Monterey (12.x) 被强制要求 14.0+ | 无 |
| [#3185](https://github.com/ollama/ollama/issues/3185) | MIT 许可证合规：静态链接 llama.cpp 但未分发版权声明（54 评论，269 👍） | 无，2024-03 至今 |

---

## 6. 对应用开发者的意义

### Agent/多步推理开发者
- **MLX 引擎当前不可用**：prompt caching 缺失导致每步 20-30K tokens 全量重算，TTFT 爆炸。[#17829](https://github.com/ollama/ollama/issues/17829) 阻塞复杂 agent 工作流，建议暂用 llama.cpp 后端或降低上下文窗口。
- **Qwen3.8 系列风险**：system message 处理、think 模式、tool calling 均存在边界 case，生产环境建议锁定 0.32.13 或等待 #17855 合并。

### 云/企业部署
- **GPU 调度信任危机**：`ollama ps` 的 VRAM 统计在 multi-model / speculative decoding / hybrid attention 场景下长期不可靠，[#17857](https://github.com/ollama/ollama/pull/17857) 是结构性修复，但需验证。
- **许可证合规审计**：[#3185](https://github.com/ollama/ollama/issues/3185) 持续 17 个月未解决，企业分发 Ollama 二进制需自行评估 MIT 归属风险。

### 集成开发者
- **Claude Code 集成脆弱**：`ollama launch claude` 对非交互模式、模型识别、cloud 模型支持均有缺口，当前更适合演示而非生产自动化。
- **新元数据缓存路径**：#17858 引入的 `<OLLAMA_MODELS>/metadata/` 目录可能成为新的运维关注点（磁盘空间、权限、备份策略）。

---

*日报基于 GitHub 公开数据生成，未包含未公开的内部讨论或安全补丁。*

</details>

<details>
<summary><strong>LiteLLM</strong> — <a href="https://github.com/BerriAI/litellm">BerriAI/litellm</a></summary>

# LiteLLM 动态日报 | 2026-08-19

## 今日速览

今日 LiteLLM 核心工作集中在 **UI 架构迁移**（antd → react-hook-form）和 **CI 稳定性修复**（Vertex batch 成本测试硬编码问题）。同时，v1.99.0-dev.1 发布，引入 Docker 镜像 cosign 签名验证机制，提升供应链安全。

---

## 版本发布与破坏性变更

| 版本 | 关键变更 | 迁移注意 |
|:---|:---|:---|
| **[v1.99.0-dev.1](https://github.com/BerriAI/litellm/releases/tag/v1.99.0-dev.1)** | Docker 镜像启用 [cosign](https://docs.sigstore.dev/cosign/overview/) 签名 | 生产环境需验证签名：`cosign verify --key litellm.pub ghcr.io/berriai/litellm:v1.99.0-dev.1`。签名密钥自 commit `0112e53` 起固定。 |

---

## 新模型与硬件支持

| 新增支持 | 来源 | 备注 |
|:---|:---|:---|
| **Z.AI 模型目录** | [PR #37433](https://github.com/BerriAI/litellm/pull/37433) | 新增 6 个当前 Z.AI 模型，Provider 搜索同时匹配 slug `zai` 与显示名称 |
| **Bedrock Converse GPT-5.6 成本映射** | [PR #37307](https://github.com/BerriAI/litellm/pull/37307)（已合并） | 补全 `gpt-5.6-luna/sol/terra` 在常规 Bedrock Converse 路由的成本条目，此前仅 `bedrock_mantle/openai.gpt-5.6-*` 有注册 |

---

## 性能与优化

| 优化项 | 状态 | 细节 |
|:---|:---|:---|
| **Vertex Batch 成本测试去硬编码** | 3 个 PR 并行推进 | [PR #37444](https://github.com/BerriAI/litellm/pull/37444)、[PR #37447](https://github.com/BerriAI/litellm/pull/37447)、[PR #37448](https://github.com/BerriAI/litellm/pull/37448) 从不同角度解决：从成本映射动态读取 / 自注册固定费率 / 使用 batch-tier 费率断言，消除因 gemini-3.6-flash 费率下调导致的 CI 阻塞 |
| **流式 Usage 细节保留** | [PR #36370](https://github.com/BerriAI/litellm/pull/36370)（Open） | `CompletionUsage` 的 `prompt_tokens_details` 与 `completion_tokens_details` 不再丢失，避免 cache-read token 误按 fresh-input 计费、reasoning token 从用量统计消失 |
| **多副本注册表一致性** | [PR #36263](https://github.com/BerriAI/litellm/pull/36263)（Open） | 注册表 miss 时直读 DB，消除新创建 model/guardrail/agent 在副本间的秒级 400/404 |

---

## 稳定性与回归

| 严重程度 | 问题 | 状态 | 关键影响 |
|:---|:---|:---|:---|
| 🔴 **高** | **[#37273](https://github.com/BerriAI/litellm/issues/37273)** `/v1/messages` 流式 tool_use 重复触发 | **已修复**（当日关闭） | Claude Code 等客户端同一 tool 执行两次，可能导致重复副作用 |
| 🔴 **高** | **[#37031](https://github.com/BerriAI/litellm/issues/37031)** MCP auto-execute 劫持客户端 tool_use | Open | `require_approval: "never"` 时，代理端自动执行循环覆盖客户端（如 Claude Code 的 Read/Bash/Edit）工具调用，所有非 MCP 工具报错 |
| 🟡 **中** | **[#36898](https://github.com/BerriAI/litellm/issues/36898)** `GET /health` 泄露 `extra_headers` 与 `aws_session_token` | Open | 健康检查端点明文返回敏感凭证，`api_key` 已脱敏但其他头部未处理 |
| 🟡 **中** | **[#35590](https://github.com/BerriAI/litellm/issues/35590)** adaptive_router 持久化 alpha/beta=0 导致永久 500 | Open | 单条脏数据使整个路由组崩溃，`gammavariate` 参数校验需防御性处理 |
| 🟡 **中** | **[#37202](https://github.com/BerriAI/litellm/pull/37202)**（PR）Key 缓存驱逐失败导致已提交更新报 400 | Open（PR） | Redis 抖动时 `/key/update` 误报失败，引发客户端重试重复更新 |
| 🟢 **低** | **[#31243](https://github.com/BerriAI/litellm/issues/31243)** `reasoning_effort='none'` 忽略 `base_model` | **已修复** | Azure 自定义部署名被误判为不支持 reasoning |
| 🟢 **低** | **[#37132](https://github.com/BerriAI/litellm/issues/37132)** Bedrock OpenAI GPT-5.6 Responses 路由挂起 | **已修复** | FastAPI 依赖解析崩溃 + chat/stream 转换缺失 |
| 🟢 **低** | **[#27434](https://github.com/BerriAI/litellm/issues/27434)** `openai/*` 字面量 api_key 发送空 Bearer | **已修复** | `Authorization: Bearer ` 被 httpx 拒绝 |

---

## 对应用开发者的意义

| 维度 | 影响与行动建议 |
|:---|:---|
| **Agent 开发** | MCP auto-execute 与客户端 tool 冲突（[#37031](https://github.com/BerriAI/litellm/issues/37031)）是**阻断性问题**——若你的 Agent 同时依赖代理端 MCP 工具和客户端原生能力（如 Claude Code），需避免 `require_approval: "never"`，或等待修复。流式 tool 重复执行（[#37273](https://github.com/BerriAI/litellm/issues/37273)）已修复，建议升级。 |
| **成本核算** | 流式 usage 细节修复（[PR #36370](https://github.com/BerriAI/litellm/pull/36370)）直接影响 cache hit / reasoning token 的计费准确性，多租户场景需关注。Vertex batch 测试修复意味着成本映射变更将更快进入主线。 |
| **安全合规** | Docker 签名验证（v1.99.0-dev.1）是基础设施硬要求；`/health` 凭证泄露（[#36898](https://github.com/BerriAI/litellm/issues/36898)）未修复前，避免在公网暴露该端点。 |
| **UI 自定义** | 大规模 antd → react-hook-form 迁移（今日 6+ PR）将解除主题定制限制，但短期内表单行为变更（mounted-field 语义差异）可能引入 payload 形状回归，自定义前端需跟进测试。 |

---

*日报基于 GitHub 公开数据生成，PR 评论数为 `undefined` 表示未在原始数据中提供。*

</details>

<details>
<summary><strong>Unsloth</strong> — <a href="https://github.com/unslothai/unsloth">unslothai/unsloth</a></summary>

# Unsloth 动态日报 | 2026-08-19

## 今日速览

今日 Unsloth 社区活跃度极高，**Studio 桌面端和前端稳定性成为焦点**：v0.1.800-beta 引入多项回归问题（安全扫描器误杀、图像变换失效），同时 AMD ROCm 生态持续收到深度修复，包括 AOTriton attention gate 和 llama-server 启动重试机制。CI 基础设施也迎来关键加固，针对 apt 步骤超时导致的隐性失败进行了系统性治理。

---

## 版本发布与破坏性变更

**无新版本发布。** 但需注意 v0.1.800-beta 的已知回归：
- 安全扫描器**误将无自定义代码的模型标记为 CRITICAL**（[#9239](https://github.com/unslothai/unsloth/issues/9239)）
- 图像变换功能完全失效：`Casting a quantized model to a new dtype is unsupported`（[#9241](https://github.com/unslothai/unsloth/issues/9241)）

---

## 新模型与硬件支持

| 进展 | 详情 | 链接 |
|:---|:---|:---|
| **Intel XPU 支持文档化** | README 新增 Intel GPU (XPU) 支持说明，依赖 #9084 的 XPU 自动检测与 llama.cpp SYCL 编译 | [#9250](https://github.com/unslothai/unsloth/pull/9250) |
| **ROCm AOTriton Attention 解锁** | 修复库用户无法使用 ROCm 实验性 flash/mem-efficient SDPA 的问题，此前 `TORCH_ROCM_AOTRITON_ENABLE_EXPERIMENTAL` 门控导致所有 sub-quadratic backend 拒绝后回退到 MATH (eager) | [#8821](https://github.com/unslothai/unsloth/pull/8821) |
| **VRAM 显示精度修复** | Windows ROCm 系统现在区分**专用 VRAM** 与**共享 GPU 内存**，此前将 iGPU 共享内存累加导致 28 GiB 虚高显示 | [#9247](https://github.com/unslothai/unsloth/pull/9247) |

---

## 性能与优化

| 优化项 | 状态 | 链接 |
|:---|:---|:---|
| **macOS 统一内存保护** | 手动设置超出统一内存容量的 context length 现在会被**拒绝而非 kernel panic**（M1 Max 32GB 实测触发） | [#9172](https://github.com/unslothai/unsloth/pull/9172) |
| **Studio 重载闪烁消除** | 阻止 Chat UI 先销毁为空白再重建的可见 tearing：`rendered Chat -> blank -> Loading... -> replacement Chat` | [#9251](https://github.com/unslothai/unsloth/pull/9251) |
| **ROCm llama-server 启动重试** | HIP/ROCR 版本不匹配时自动回退 bundled HIP，解决 `hsa_amd_queue_create@ROCR_1` 符号解析失败 | [#9002](https://github.com/unslothai/unsloth/pull/9002) |

---

## 稳定性与回归（按严重程度排列）

| 严重程度 | 问题 | 状态 | 链接 |
|:---|:---|:---|:---|
| 🔴 **Critical** | **安全扫描器误杀所有训练模型**：v0.1.800-beta macOS 桌面端将包括无自定义代码的模型全部标记为 "Custom code blocked / CRITICAL"，阻断微调流程 | **待修复** | [#9239](https://github.com/unslothai/unsloth/issues/9239) |
| 🔴 **Critical** | **Studio SQLite 死锁**：所有线程阻塞于 `sqlite3.connect()/close()`，服务停止接受连接，curl 超时，CPU 占用 100% | **待修复** | [#9008](https://github.com/unslothai/unsloth/issues/9008) |
| 🟡 **High** | **AMD GPU 检测与运行不一致**：安装器报告 `AMD ROCm (gfx1201)`，后端实际 CPU-only 运行，Live Monitor 显示 "No visible GPU" | **待修复** | [#8473](https://github.com/unslothai/unsloth/issues/8473) |
| 🟡 **High** | **macOS 桌面端二次启动崩溃** | **待修复** | [#8610](https://github.com/unslothai/unsloth/issues/8610) |
| 🟡 **High** | **API /embeddings 极少正常工作** | **待修复** | [#9128](https://github.com/unslothai/unsloth/issues/9128) |
| 🟡 **High** | **llama.cpp 预构建安装静默回退 CPU**：无 `nvcc` 时从源码编译 CPU-only 构建，安装仍报告成功，GPU 推理丢失无警告 | **待修复** | [#9255](https://github.com/unslothai/unsloth/issues/9255) |
| 🟡 **High** | **安装器 shell profile 不可写时错误回滚**：PATH 追加失败触发 venv 回滚陷阱，成功横幅已打印后仍执行回滚 | **待修复** | [#9254](https://github.com/unslothai/unsloth/issues/9254) |
| 🟢 **Medium** | **图像变换功能回归**：v0.1.800-beta 中 `Transform images` 完全失效，量化模型 dtype 转换报错 | **待修复** | [#9241](https://github.com/unslothai/unsloth/issues/9241) |
| 🟢 **Medium** | **Ollama 模型未出现在聊天选择器**：前端故意排除 `source="ollama"` 的库存条目 | **PR 已开** [#9237](https://github.com/unslothai/unsloth/pull/9237) | [#9226](https://github.com/unslothai/unsloth/issues/9226) |
| 🟢 **Medium** | **Linux 桌面端不信任系统自签名证书**：`SSL_CERT_FILE`/`REQUESTS_CA_BUNDLE` 设置后反而更糟 | **PR 已开** [#9240](https://github.com/unslothai/unsloth/pull/9240) | [#9218](https://github.com/unslothai/unsloth/issues/9218) |
| 🟢 **Medium** | **连续两次网页搜索失败**：使用云模型（如 ollama cloud）时同一会话无法执行第二次搜索 | **待修复** | [#9108](https://github.com/unslothai/unsloth/issues/9108) |
| 🟢 **Medium** | **拖拽上传 JPG/PNG 间歇失效** | **待修复** | [#9036](https://github.com/unslothai/unsloth/issues/9036) |
| 🟢 **Low** | **菜单焦点管理多项 a11y 问题**：非模态菜单关闭后焦点丢失至 `<body>`，Escape 关闭 More 菜单不返回触发器 | **PR 已开** [#9243](https://github.com/unslothai/unsloth/pull/9243) [#9248](https://github.com/unslothai/unsloth/pull/9248) | [#9245](https://github.com/unslothai/unsloth/issues/9245) [#9156](https://github.com/unslothai/unsloth/issues/9156) |

**已关闭的关键问题：**
- LLVM `fdot2.bf16` intrinsic 选择失败（AMD RX 6600）— [#5337](https://github.com/unslothai/unsloth/issues/5337)
- PID 1 环境下 `PDEATHSIG` 守卫杀死所有子进程 — [#6756](https://github.com/unslothai/unsloth/issues/6756)
- 工具调用误触发（无调用时 nudge）— [#8907](https://github.com/unslothai/unsloth/issues/8907)

---

## 对应用开发者的意义

| 场景 | 影响与建议 |
|:---|:---|
| **基于 Unsloth Studio 构建 Agent 应用** | v0.1.800-beta 存在**多项阻断性回归**，建议暂缓升级或锁定版本。若已升级，注意：① 安全扫描器可能阻断任何模型加载 ② 图像变换 pipeline 完全不可用 ③ 网页搜索工具链状态不持久。关注 [#9243](https://github.com/unslothai/unsloth/pull/9243) 等修复 PR 的合并进度。 |
| **AMD ROCm 部署** | 今日多项修复显著改善可用性：AOTriton attention 解锁（[#8821](https://github.com/unslothai/unsloth/pull/8821)）、llama-server 启动重试（[#9002](https://github.com/unslothai/unsloth/pull/9002)）、VRAM 显示精度（[#9247](https://github.com/unslothai/unsloth/pull/9247)）。但仍需验证 `gfx1201` 等较新架构的实际 GPU 利用率问题（[#8473](https://github.com/unslothai/unsloth/issues/8473)）。 |
| **企业内网/离线部署** | ① 自签名证书信任问题有修复 PR（[#9240](https://github.com/unslothai/unsloth/pull/9240)）② 远程访问仍强制 Cloudflare 隧道，社区 workaround 为手动 `.bat` 端口转发（[#9207](https://github.com/unslothai/unsloth/issues/9207)）③ 建议关注即将支持的 systemd 服务自启动（[#9258](https://github.com/unslothai/unsloth/issues/9258)）。 |
| **多模型并发服务** | 社区已提出支持请求（[#9257](https://github.com/unslothai/unsloth/issues/9257)），当前 Studio 为单模型推理服务器架构，需自行管理多实例或等待官方支持 per-model device pinning。 |
| **CI/CD 集成** | 上游修复了 apt 步骤无界超时导致的隐性 job cancellation（[#9256](https://github.com/unslothai/unsloth/pull/9256)），若自行维护 Unsloth 构建流水线，建议同步引入 `timeout-minutes` 约束。 |

---

*数据来源：github.com/unslothai/unsloth | 生成时间：2026-08-19*

</details>

---
*本日报由 [agents-radar](https://github.com/brysoh435-dev/agents-radar) 自动生成。*