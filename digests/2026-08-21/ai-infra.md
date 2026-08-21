# AI 基础设施日报 2026-08-21

> 生成时间: 2026-08-21 03:30 UTC | 覆盖项目: 6 个

- [vLLM](https://github.com/vllm-project/vllm)
- [SGLang](https://github.com/sgl-project/sglang)
- [llama.cpp](https://github.com/ggml-org/llama.cpp)
- [Ollama](https://github.com/ollama/ollama)
- [LiteLLM](https://github.com/BerriAI/litellm)
- [Unsloth](https://github.com/unslothai/unsloth)

---

## 横向对比

# AI 基础设施生态横向对比分析 | 2026-08-21

---

## 1. 生态全景

当前 AI 基础设施呈现**"推理引擎军备竞赛白热化、端侧部署精细化、网关层商业化成熟"**的三层格局。vLLM 与 SGLang 在 Kimi-K3/DeepSeek-V4 等新模型适配上激烈角逐，llama.cpp 以日均 11 个版本的迭代速度巩固端侧霸权，Ollama 受困于 Qwen3.x 系列的稳定性泥潭，LiteLLM 则聚焦企业级计费与权限的毫米级修复。一个显著信号是：**投机解码（MTP/EAGLE/DFLASH）从"炫技功能"变为生产必选项**，但各实现稳定性参差不齐，成为区分引擎成熟度的关键标尺。

---

## 2. 各项目活跃度对比

| 项目 | 今日 Release | PR 活动 | Issue 热度 | 核心战场 |
|:---|:---|:---|:---|:---|
| **vLLM** | 无 | ~15 个核心 PR（Kimi K3 性能×3、KV Connector×2、量化重构×2） | DeepSeek-V4 A100 适配 #40851（45 评论）、MTP 延迟回归 #43295 | 新模型适配攻坚 + V1 调度器正确性加固 |
| **SGLang** | 无 | ~12 个 PR（Kimi-K3 MoonEP/DFLASH、ROCm 7.14、扩散模型 CUDA Graph） | #35189 NIXL segfault 重开、#35241 PrefillDelayer 死循环 | 生产化可靠性追赶 |
| **llama.cpp** | **b10520-b10541（22 个版本）** | 5+ 开放 PR（TeleChat4、GLM-4.5 MTP、Vulkan DSV4 算子） | #27102 CUDA kernel stall、#27447 Vulkan DeviceLost | 端侧性能极致优化 + 多后端算子补全 |
| **Ollama** | 无 | 4 个已合入（MLX 前缀缓存、跨平台打包、CORS 修复）、3 个开放 | ROCm Strix Halo KV 污染 #17847、Qwen3.8 截断崩溃 #17778 | 稳定性救火（MLX/Qwen/ROCm 三线承压） |
| **LiteLLM** | 无 | **338 个 PR 更新** | 预算一致性 #27735、MCP 工具冲突 #37031 | 企业级计费精度与 RBAC 精细化 |
| **Unsloth** | **v0.1.801-beta** | 200+ PR 合并（beta 版本） | Qwen3.8 M3 崩溃 #9279、Windows 安装失败 #9440 | 功能扩张（Auto Compaction/LAN Access）与稳定性平衡 |

> **活跃度排序**：llama.cpp（版本密度）> LiteLLM（PR 吞吐量）> Unsloth（beta 大爆炸）> vLLM/SGLang（聚焦攻坚）> Ollama（修复模式）

---

## 3. 模型支持竞速

| 模型/架构 | 首发支持 | 跟进状态 | 落后方 |
|:---|:---|:---|:---|
| **DeepSeek-V4 系列** | vLLM（Flash 已支持，A100/L20 适配中） | SGLang 基线可用；llama.cpp Vulkan 算子补全中（#27453/#26578） | Ollama Cloud 无限 thinking 循环未修 |
| **Kimi-K3** | **SGLang 领先**：MoonEP 生产化路线图 #35783、DFLASH 投机解码 #35784 | vLLM 同日 3 个性能 PR（MXFP4 top-k、MoonViT RoPE、FlashKDA） | Ollama 上架争议关闭 #17235 |
| **Qwen3-Omni 多模态 LoRA** | **vLLM 独家**：PR #52786 3D MoE LoRA + 多模态 token 映射 | — | SGLang Qwen3-VL 视觉漂移 #35772；Ollama Qwen3.x 全线问题 |
| **GLM-5.2 MTP** | llama.cpp PR #26534 `graph_mtp` | vLLM 0% draft 接受率 #52833（MI355X）；SGLang 已关闭未修复 | — |
| **LFM2 DSpark** | **llama.cpp 已合并** #27383 | vLLM DSpark 相关 DSML wrapper 损坏 #51914 | — |
| **ERNIE CPU** | **SGLang** PR #35222 `topk_softmax_cpu` BF16 | — | — |
| **TeleChat4** | **llama.cpp** PR #27435 全栈（MHC + CUDA） | — | — |

**竞速结论**：vLLM 与 SGLang 在**大模型生产推理**上交替领先（vLLM 量化/LoRA 更深，SGLang 架构集成更快）；llama.cpp 在**端侧新架构覆盖**上保持绝对速度；Ollama 沦为"问题报告收集器"，模型支持质量显著落后。

---

## 4. 性能优化前沿

| 方向 | 代表实现 | 量化收益 |
|:---|:---|:---|
| **KV Cache 分层策略** | llama.cpp b10532/b10538：Metal 小 batch 量化/大 batch 解压；vLLM TurboQuant + 前缀缓存 | Apple Silicon 长上下文 decode 带宽-计算平衡 |
| **投机解码工程化** | vLLM MTP 稳定性修复 #52244/#53197；SGLang DFLASH #35784、NPU compact verify #35670；llama.cpp LFM2 DSpark #27383 | 延迟降低 5-30%，但正确性风险未完全收敛 |
| **量化感知算子融合** | vLLM MXFP4 top-k 融合 #53152（-5% E2E）、TRTLLM MXFP8 adaptive layouts #52275；llama.cpp CUDA 动态 MMVQ/MMQ #10534 | 精度-速度帕累托前沿推进 |
| **PD 分离与 KV 传输** | vLLM NIXL PCP 生产者 #52779、per-request remote KV wait 可观测 #53198；SGLang NIXL/UCX segfault 重开 #35189 | 分布式推理瓶颈从"计算"转向"传输延迟可观测" |
| **前缀缓存正确性** | vLLM hybrid GDN hash 对齐 #52244；Ollama MLX 前缀缓存持久化 #17901；SGLang Weight Cache Daemon <1s 恢复 | 长对话/Agent 场景 TTFT 稳定性 |
| **架构感知调度** | llama.cpp CUDA 按 `sm_xxx` 切换解码路径；vLLM XPU mrope CUDA 路径规避 eager | 异构硬件性能可移植性 |

**火力集中点**：**投机解码的"从能跑到跑对"** 与 **KV 系统的"从缓存到可信"** 成为两大主战场，量化精度问题向 block-scaled/FP8/FP4 等更细粒度演进。

---

## 5. 分层定位差异

| 层级 | 项目 | 核心抽象 | 今日演化信号 |
|:---|:---|:---|:---|
| **云原生推理引擎** | vLLM、SGLang | Continuous Batching + PagedAttention 变体 | 向 **PD 分离、KV 传输协议标准化（NIXL/UCX）、请求级版本绑定** 演进，争夺"大模型推理的 Kubernetes"地位 |
| **端侧本地运行时** | llama.cpp、Ollama | GGUF 格式 + 多后端（CPU/GPU/NPU） | llama.cpp 向 **Server/Router 模式、多模态分离部署** 延伸；Ollama 受困于 **封装层技术债**（MLX/Qwen/ROCm 三线崩溃） |
| **LLM 网关/代理** | LiteLLM | 统一 API 路由 + 计费/权限/缓存 | 从"协议翻译器"进化为 **多租户成本中心**，RBAC 自定义角色、预算 11 维作用域精度成为壁垒 |
| **训练/微调框架** | Unsloth | LoRA/QLoRA + 一键导出 GGUF/MLX | 向 **Agent 运行时** 扩张（Portable Skills、Auto Compaction、LAN Access），但 v0.1.801-beta 的 200+ PR 大爆炸暴露工程纪律风险 |

**关键张力**：vLLM/SGLang 的边界在模糊（都想要"全栈推理"），llama.cpp 向上侵蚀 Ollama 的易用性领地，Ollama 若不能解决 Qwen3.x 稳定性危机将面临用户流失，Unsloth 的"全能 Studio"野心与质量管控能力存疑。

---

## 6. 值得关注的趋势信号

### 行业趋势

| 信号 | 证据 | 含义 |
|:---|:---|:---|
| **投机解码成为"基础功能"而非"高级特性"** | 6 个项目中 5 个今日涉及 MTP/EAGLE/DFLASH/DSpark | 延迟敏感场景（Agent、实时交互）的标配，但**实现质量分化严重** |
| **KV 系统从"性能优化"转向"分布式正确性"** | NIXL/UCX segfault、PCP 生产者、remote KV wait 可观测 | PD 分离架构进入生产验证期，**传输层可靠性**成为新瓶颈 |
| **量化精度战争升级** | FP8 block-scaled sm120 失败、MXFP4/FP8 adaptive layouts、NVFP4 加载失败 | 新硬件（Blackwell）+ 新格式（FP4/FP8）的**编译器/驱动/框架三角适配**复杂度指数上升 |
| **Apple Silicon 获得专属优化轨道** | Metal KV 分层策略、M5 Max prefill/decode 倒挂修复、MLX 前缀缓存 | 端侧大模型体验成为差异化战场，**内存带宽受限架构的 KV 策略**具有通用启示 |

### Agent/应用开发者行动清单

| 优先级 | 行动 | 风险规避 |
|:---|:---|:---|
| **P0** | **验证投机解码的 E2E 延迟与正确性** | vLLM MTP 76% 延迟回归 #43295、SGLang target-only RNG 边界错误 #35771、llama.cpp 量化目标贪心采样不一致 #25618——**上线前必做贪心一致性测试** |
| **P0** | **Qwen3.x 系列暂缓生产部署**（Ollama/vLLM/SGLang 均有严重问题） | 上下文截断崩溃 #17778、视觉色度丢失 #17872、无限 thinking #17617——**多项目共性问题指向模型本身或广泛适配缺陷** |
| **P1** | **监控 prefix cache 实际命中率** | vLLM hybrid GDN 0 hit #52897、Ollama MLX 无 prefix cache #17829——**TTFT 恶化可能静默发生** |
| **P1** | **LiteLLM 虚拟 key 预算加外部校验层** | #27735 stale spend、#37736 per-model 归零——**计费数据不可信直至修复** |
| **P2** | **评估 llama.cpp b10538+ 的端侧长上下文体验** | Metal 自适应 KV 策略可能显著改善 Apple Silicon 上的 Agent 多轮对话 |
| **P2** | **跟踪 Unsloth Auto Compaction 对超长 Agent 会话的适用性** | 10K→1.2K token 压缩比诱人，但 PR #9442 显示空间计算仍有 corner case |

---

*报告基于 2026-08-21 各项目公开 GitHub 数据生成，Issue/PR 编号可直接用于技术追踪。*

---

## 各项目详细报告

<details>
<summary><strong>vLLM</strong> — <a href="https://github.com/vllm-project/vllm">vllm-project/vllm</a></summary>

# vLLM 动态日报 | 2026-08-21

> 分析师注：今日无新 Release，社区焦点集中在 **DeepSeek-V4 系列在 Ampere/ROCm 的适配攻坚**、**Kimi K3 性能优化落地** 以及 **MTP/投机解码的稳定性修复**。PR 侧出现大量基础设施级改进，涵盖 KV Connector、调度器版本绑定、量化配置重构等。

---

## 1. 今日速览

- **DeepSeek-V4 生态扩展**：A100/A800 (sm_80) 支持请求成为最热 Issue（45 评论），ROCm 侧同步推进 DSpark 自适应验证与 PD 分离；L20 部署出现 `auto_functionalized` 编译器断言失败待解。
- **Kimi K3 性能冲刺**：3 个性能 PR 同日推进，MXFP4 top-k 融合预计降低 5% E2E 延迟，MoonViT 复数 RoPE 融合与 FlashKDA 打包导出跟进。
- **V1 调度器与 KV 系统加固**：修复 hybrid GDN 前缀缓存在 MTP 下失效、NIXL PCP 生产者支持、SimpleCPUOffload 跨 PP/投机解码的块数对齐等关键正确性修复。

---

## 2. 版本发布与破坏性变更

**无新 Release。**

⚠️ **安装陷阱提醒**：Issue [#42338](https://github.com/vllm-project/vllm/issues/42338) 确认，`cu129` nightly 通道的预发布版本（`0.20.2rc1.dev233`）因 PEP 440 版本排序低于 PyPI 稳定版（`0.20.2`，CUDA 13 构建），导致 `pip install` 默认拉取错误 wheel。使用 CUDA 12.9 环境的用户需显式指定版本或索引。

---

## 3. 新模型与硬件支持

| 项目 | 状态 | 说明 |
|:---|:---|:---|
| **DeepSeek-V4-Flash → A100/A800** | 🔥 高热度需求 | Issue [#40851](https://github.com/vllm-project/vllm/issues/40851)：DeepGEMM 内核断言失败，需 sm_80 适配。22 👍，社区迫切需求。 |
| **DeepSeek-V4 → L20** | ❌ 阻塞中 | Issue [#42949](https://github.com/vllm-project/vllm/issues/42949)：`auto_functionalized was not removed` 编译器错误，TP 场景启动失败。 |
| **Kimi-K3 on ROCm** | 🚧 路线图追踪 | Issue [#50682](https://github.com/vllm-project/vllm/issues/50682)：Day 0 基线已集成 AITER fused-moe，持续跟踪性能优化。 |
| **Qwen3-Omni 多模态 LoRA** | PR 评审中 | PR [#52786](https://github.com/vllm-project/vllm/pull/52786)：新增 3D MoE LoRA 加载与多模态 token 映射。 |
| **ModernBERT FP8** | PR 评审中 | PR [#53101](https://github.com/vllm-project/vllm/pull/53101)：补全 `quant_config` 传递，支持 `llmcompressor` FP8_DYNAMIC 检查点。 |

---

## 4. 性能与优化

| PR/Issue | 优化点 | 收益/状态 |
|:---|:---|:---|
| **PR [#53152](https://github.com/vllm-project/vllm/pull/53152)** | Kimi K3: MXFP4 top-k finalization 融合至 latent-tail 内核 | **~5% E2E 延迟降低**；已 ready，NVIDIA/K3 双标签 |
| **PR [#53168](https://github.com/vllm-project/vllm/pull/53168)** | MoonViT Q/K 复数 RoPE 融合（SM90+ Triton） | 消除实部/虚部物化与分离调用，encoder 层效率提升 |
| **PR [#53196](https://github.com/vllm-project/vllm/pull/53196)** | FlashKDA 打包导出检查点 | 替代 per-request split 执行，减少 kernel launch 开销 |
| **PR [#53202](https://github.com/vllm-project/vllm/pull/53202)** | BF16 线性层默认 FlashInfer `mm_bf16` | 移除 CuTe skinny GEMM，统一 `backend="auto"` 路径 |
| **PR [#52275](https://github.com/vllm-project/vllm/pull/52275)** | TRTLLM MXFP8 adaptive layouts | 新增 `128x4` 与自适应策略（阈值可配置），默认保留 `8x4` |
| **PR [#53201](https://github.com/vllm-project/vllm/pull/53201)** | XPU mrope 走 CUDA 路径 | 规避 eager mode 额外开销，恢复 torch.compile 修复后的性能 |
| **Issue [#43295](https://github.com/vllm-project/vllm/issues/43295)** / [#35387](https://github.com/vllm-project/vllm/issues/35387) | MTP 延迟回归 | Qwen3.6-35B-A3B-NVFP4 上 MTP 显著慢于基线；Qwen3-Next-80B-A3B-FP8 **76% 延迟回归**（TP=4, num_speculative_tokens=2），根因待查 |

---

## 5. 稳定性与回归

| 严重程度 | 问题 | 状态 | 跟踪 |
|:---|:---|:---|:---|
| 🔴 **高** | TurboQuant KV cache + 大 chunk continuation prefill → crash | 测试中（PR #39931） | Issue [#41726](https://github.com/vllm-project/vllm/issues/41726)：workspace lock 后崩溃，RTX 5080，v0.20.2rc1 |
| 🔴 **高** | Cross-node CUDA graph capture 非法内存访问（GB10, TP=2） | 无 fix | Issue [#46253](https://github.com/vllm-project/vllm/issues/46253)：host-staged NCCL all-reduce 被捕获，`splitting_ops` 未生效 |
| 🟡 **中** | Hybrid GDN prefix cache 0 hit（MTP + priority 调度） | **PR 修复中** | Issue [#52897](https://github.com/vllm-project/vllm/issues/52897) → PR [#52244](https://github.com/vllm-project/vllm/pull/52244)：hash unit 对齐问题 |
| 🟡 **中** | DeepSeek-V4-Flash DSML tool-call wrapper 损坏 | 间歇性 | Issue [#51914](https://github.com/vllm-project/vllm/issues/51914)：`<｜DSML｜tool_calls>` → `<｜DSML｜toolcalls>`，DSpark 相关 |
| 🟡 **中** | FP8 block-scaled weights sm120 (RTX 5090) 失败 | 无 fix | Issue [#51884](https://github.com/vllm-project/vllm/issues/51884)：DeepGEMM "Unknown SF transformation" |
| 🟡 **中** | FlashInfer b12x SM120 MoE workspace OOM（16GB Blackwell） | 回归 | Issue [#49476](https://github.com/vllm-project/vllm/issues/49476)：v0.25.1/FlashInfer 0.6.13 引入，`profile_run` 内分配 |
| 🟡 **中** | GLM-5.2 MTP 0% draft 接受率（MI355X, gfx950） | 无 fix | Issue [#52833](https://github.com/vllm-project/vllm/issues/52833)：禁用 EP 触发 `hipErrorIllegalAddress` |
| 🟢 **低** | DeepSeek V4 `load_weights` UnboundLocalError | 无 fix | Issue [#42769](https://github.com/vllm-project/vllm/issues/42769)：expert mapping 无匹配时 `name_mapped` 未绑定 |
| 🟢 **低** | Qwen3.5-35B-A3B MTP 降低 prefix cache 命中率 | 讨论中 | Issue [#38182](https://github.com/vllm-project/vllm/issues/38182)：机制待澄清 |
| 🟢 **低** | draft_model TP>1 崩溃（draft hidden_size > target） | 无 fix | Issue [#52023](https://github.com/vllm-project/vllm/issues/52023)：TRT-LLM fused allreduce+RMSNorm workspace 大小按 target 计算 |

**今日合并的关键修复：**
- **PR [#52779](https://github.com/vllm-project/vllm/pull/52779)**：NIXL KV Connector 支持 PCP 生产者，解决 PD 分离下 prefill context parallelism 的 KV 副本对齐。
- **PR [#52921](https://github.com/vllm-project/vllm/pull/52921)**：SimpleCPUOffload 跨 PP/投机解码的 `num_cpu_blocks` 对齐，修复调度器与 worker 不一致导致的正确性 bug。
- **PR [#53197](https://github.com/vllm-project/vllm/pull/53197)**：MRV2 fused draft graph metadata 跨 sleep 存活，避免 sleep/wake 后地址失效。

---

## 6. 对应用开发者的意义

| 场景 | 影响 | 行动建议 |
|:---|:---|:---|
| **Agent/Tool-calling 生产部署** | DeepSeek-V4-Flash-0731 + DSpark 下 DSML wrapper 间歇损坏可能导致 tool call 解析失败 | 监控输出格式，考虑降级关闭 DSpark；跟踪 [#51914](https://github.com/vllm-project/vllm/issues/51914) |
| **长上下文 + prefix caching** | Hybrid GDN (Qwen3.5) 在 MTP 下前缀缓存失效，直接影响长对话/文档问答的 TTFT | 若依赖 prefix cache，临时关闭 MTP 或应用 PR [#52244](https://github.com/vllm-project/vllm/pull/52244) |
| **投机解码效果调优** | MTP 在多款模型上表现不稳定（延迟回归、draft 接受率 0%），非万能加速 | 实测验证 E2E 延迟，关注 `completion_tokens_details.accepted_prediction_tokens`（PR [#51778](https://github.com/vllm-project/vllm/pull/51778) 即将暴露该指标） |
| **多模态 Agent (Qwen3-Omni)** | 多模态 LoRA 即将支持，可针对视觉/音频 encoder 微调 | 跟踪 PR [#52786](https://github.com/vllm-project/vllm/pull/52786) 合并进度 |
| **权重热更新/版本控制** | PR [#53199](https://github.com/vllm-project/vllm/pull/53199) 引入请求级 weight version 绑定，为 A/B 测试与灰度发布奠基 | 基础设施团队关注 Rust frontend 与调度器协议变更 |
| **性能可观测性** | PR [#53198](https://github.com/vllm-project/vllm/pull/53198) 暴露 per-request remote KV wait time | 优化 PD 分离架构的调度策略与容量规划 |

---

*日报基于 GitHub 公开数据生成，PR 评论数为 `undefined` 表示数据未完整采集，建议直接访问链接获取最新讨论。*

</details>

<details>
<summary><strong>SGLang</strong> — <a href="https://github.com/sgl-project/sglang">sgl-project/sglang</a></summary>

# SGLang 动态日报 | 2026-08-21

## 1. 今日速览

今日 SGLang 社区活跃度极高，**Kimi-K3 生产化集成**与**DFLASH 投机解码**取得关键进展，同时 **ROCm 7.14 新硬件支持**和**扩散模型 CUDA Graph 优化**多条 PR 并行推进。稳定性方面，NIXL/UCX prefill segfault 等历史顽疾被重新打开追踪，scheduler 层出现新的正确性 Bug。

---

## 2. 版本发布与破坏性变更

**无新 Release**。今日无版本发布，亦无显式 API 破坏性变更。

> 注：CI 维护模式 Issue #21065 于今日更新，提示 CI 健康度监控仍在持续，可能影响 PR 合并节奏。

---

## 3. 新模型与硬件支持

| 项目 | 状态 | 详情 |
|:---|:---|:---|
| **Kimi-K3 + MoonEP 生产化** | 🔄 Roadmap 开启 | Issue #35783 正式追踪从 BF16 PoC 到生产级集成的路径，涵盖性能验证、测试覆盖、可靠性加固 |
| **Kimi-K3 DFLASH 投机解码** | 🔧 PR 待审 | PR #35784 为 KimiK3 内外层模型添加 `set_dflash_layers_to_capture`，使 DFLASH draft 路径可捕获 aux hidden state |
| **ROCm 7.14 (gfx942/gfx950)** | 🔧 PR 待审 | PR #35319 解决 ROCm 7.14 仅发 pip wheel、无 apt repo 的适配问题，SDK 路径从 `/opt/rocm` 迁移至 `site-packages` |
| **AMD gfx950 AITER GDN decode** | 🔧 PR 待审 | PR #33113 引入 AITER HIP packed GDN decode 后端，较 Triton 版本降低 **13-23% decode 延迟** |
| **ERNIE CPU 推理** | 🔧 PR 待审 | PR #35222 补全 `topk_softmax_cpu` BF16 支持，解锁 ERNIE 系列在 CPU 后端运行 |
| **SANA-Video / LongCat 扩散模型** | 🔧 PR 待审 | PR #35729/35728/35724 启用 breakable CUDA Graphs，SANA-Video linear attention 在 `quality=high` 下加速 |

---

## 4. 性能与优化

| 优化项 | 数据 | 链接 |
|:---|:---|:---|
| **Weight Cache Daemon 快速恢复** | Phase 1 已落地：Qwen3-235B FP8 权重加载从 **306-327s → <1s**（CUDA IPC）| Issue #33522 |
| **SANA-Video linear attention (quality=high)** | BF16 输入 + FP32 累加快速路径，保持默认路径 bit-for-bit | PR #35728 |
| **AMD AITER target-verify GQA packing** | 减少 gfx950 上 2D program count 膨胀，避免 fallback 到低效 kernel | PR #35457 |
| **NPU DSpark compact verify graph** | 缩短 verify window + folded verify epilogue，减少投机解码开销 | PR #35670 |
| **CPU 推理模拟器** | 无权重加载、无 kernel 执行，纯 CPU 评估调度/延迟/吞吐/前缀缓存行为 | PR #33824 |

---

## 5. 稳定性与回归（按严重程度排列）

### 🔴 Critical / High Priority

| Issue | 描述 | 状态 |
|:---|:---|:---|
| **#35189 NIXL/UCX prefill segfault** | `nixlUcxSharedThread → cuEventQuery` 在 v0.5.17 / CUDA 13.0 / B200 上**仍未修复**，#23489/#23499 被关闭但无根因。今日重新打开追踪 | ⚠️ **无 fix PR**，需关注 |
| **#35241 PrefillDelayer 死循环** | DP Attention + chunked prefill 下，PrefillDelayer 进入持久混合状态反馈循环，**prefill 进度崩溃** | ⚠️ **无 fix PR**，性能稳定性风险 |
| **#35777 Qwen3.8-27B NVFP4 OOM + 6x 回归** | RTX 5090 cookbook 配方：`--mem-fraction-static` 导致 decode graph capture OOM (~5GB)；`torch.compile + decode graph` 性能倒退 **6 倍** | 🩹 PR #35786 修复文档配方 |

### 🟡 Bug / Correctness

| Issue | 描述 | 状态 |
|:---|:---|:---|
| **#34112 Chunked-prefill 取消泄漏 token** | 取消 batch 尾部请求时泄漏 1 个 visible token，scheduler output ids 出现负值 | ⚠️ **无 fix PR** |
| **#35771 Target-only 投机采样 RNG 边界错误** | 零概率 draft 在 RNG 边界被错误接受 | ⚠️ **无 fix PR** |
| **#35779 MiniMax-M2 CPU 推理失败** | 处理请求时崩溃 | ⚠️ **无 fix PR** |
| **#35772 Qwen3-VL 视觉特征漂移** | v0.5.17 上细粒度 grounding 结果与 Transformers/vLLM 不一致 | ⚠️ **无 fix PR** |
| **#35345 Qwen3.6/3.8 mRoPE 维度不匹配** | 多模态 mRoPE 位置信息传入 1D fused QK RMSNorm+RoPE kernel | ⚠️ **无 fix PR** |
| **#34720 XPU Qwen3.5 GDN + 投机解码** | `causal_conv1d_update_xpu()` 收到意外关键字参数 `intermediate_conv_window` | ⚠️ **无 fix PR** |

### 🟢 Fixed / Closed Today

| Issue/PR | 说明 |
|:---|:---|
| **#35563** DeepSeek-V3/V4 tool-call parser 流式分块丢调用 | ✅ **已关闭** |
| **#28179** `repetition_penalty` 被忽略（缺失 `BatchedRepetitionPenalizer`）| ❌ 因 inactive 关闭，**未修复** |
| **#26454** 非 DP 多节点 TP=8 hang | ❌ 因 inactive 关闭 |
| **#28826** GLM-5.2-FP8 DSA 加载失败 | ❌ 因 inactive 关闭 |

### CI 基础设施

| 项目 | 状态 |
|:---|:---|
| **#35764 ROCm 7.0 apt index 失效** | ✅ PR 修复 MORI 依赖安装失败 |
| **#35340 AMD allreduce-fusion gate test stub** | ✅ PR 修复 `moe_ep_size/moe_tp_size` 缺失导致的持续报错 |
| **#17050 CI 故障追踪** | 3 broken, 11 flaky, 671 recently fixed（截至 2026-08-21 02:42 UTC）|

---

## 6. 对应用开发者的意义

| 场景 | 影响 | 行动建议 |
|:---|:---|:---|
| **使用 Kimi-K3 的 Agent 产品** | MoonEP 生产化 (#35783) + DFLASH 投机解码 (#35784) 并行推进，预计短期内可获得 **更低延迟的推理路径**。但 MoonEP 目前仅 BF16 PoC 验证，生产环境需等待性能/可靠性里程碑 | 跟踪 #35783 milestone，评估 DFLASH 对首 token 延迟的改善 |
| **RTX 5090 本地部署 Qwen3.8-27B** | 官方 cookbook 配方存在 OOM 和 6x 性能陷阱，**不要直接复制粘贴** | 等待 PR #35786 合并或手动调整 `--mem-fraction-static`，避免 `torch.compile + decode graph` 组合 |
| **AMD MI355X / gfx950 用户** | ROCm 7.14 + AITER GDN decode 带来 **13-23% 延迟降低**，但需适配新的 pip-only 安装路径 | 关注 PR #35319/#33113 合并，准备迁移 ROCm 版本 |
| **多模态应用（Qwen3-VL 视觉定位）** | v0.5.17 存在视觉特征漂移，影响细粒度 grounding 正确性 | 暂时用 vLLM/Transformers 做结果校验，或降级 SGLang 版本 |
| **投机解码（EAGLE/NEXTN）** | ROCm TP>1 存在 rank divergence 导致死锁（#28815，已关闭未修复）；target-only sampler 有 RNG 边界错误 (#35771) | TP>1 + 投机解码场景建议先用 CUDA，或关闭投机解码 |
| **依赖 Prometheus 监控的网关/调度器** | 新增 `name[]` filtering 支持（#35752），可更精确筛选多进程指标 | 评估是否简化现有 metric 聚合逻辑 |

---

*日报基于 sgl-project/sglang 2026-08-21 公开数据生成。评论数为 `undefined` 的 PR 表示数据未完整采集，建议直接查看 GitHub 页面获取最新讨论。*

</details>

<details>
<summary><strong>llama.cpp</strong> — <a href="https://github.com/ggml-org/llama.cpp">ggml-org/llama.cpp</a></summary>

# llama.cpp 动态日报 | 2026-08-21

## 今日速览

今日 llama.cpp 密集发布 11 个版本（b10520-b10541），核心聚焦 **Metal KV Cache 量化解压策略优化**、**CUDA 动态切换 MMVQ/MMQ 解码路径**，以及 **MTP/投机解码稳定性修复**。同时发生关键回退：tensor-split meta backend 修复被撤销（b10531），多 GPU 用户需警惕。

---

## 版本发布与破坏性变更

| 版本 | 变更内容 | 影响 |
|:---|:---|:---|
| **[b10541](https://github.com/ggml-org/llama.cpp/releases/tag/b10541)** | `mtmd`: 新增 `--mmproj-device` / `-mmdev` 参数，支持指定多模态投影层加载设备；保留 `MTMD_BACKEND_DEVICE` 环境变量向后兼容 | 多模态部署可分离文本与视觉后端，降低显存碎片 |
| **[b10539](https://github.com/ggml-org/llama.cpp/releases/tag/b10539)** | Vulkan: FlashAttention + MMQ 的 Q 量化计算强制使用 FP32，修复 `qd` denormal 导致 `1/qd` 溢出的 Codex 发现漏洞 | 数值稳定性修复，无性能损失 |
| **[b10538](https://github.com/ggml-org/llama.cpp/releases/tag/b10538)** | Metal: **仅在大 batch 时解压量化 KV Cache**，小 batch 保持量化以降低带宽 | Apple Silicon 解码延迟优化，显存敏感场景收益显著 |
| **[b10537](https://github.com/ggml-org/llama.cpp/releases/tag/b10537)** | CI: Windows 改用 LLVM OpenMP 替代 MSVC_DEBUG_non_redist，附带许可证 | 分发合规性提升，无运行时变更 |
| **[b10536](https://github.com/ggml-org/llama.cpp/releases/tag/b10536)** | Server: Router 模式延迟加载 `startup_models`，仅首次加载时填充 | 启动时序优化，避免竞态 |
| **[b10534](https://github.com/ggml-org/llama.cpp/releases/tag/b10534)** | CUDA: **按硬件架构与量化类型动态切换 MVQ→MMQ 解码交叉点**，新增 `GGML_CUDA_MMVQ_MAX` 运行时覆盖 | Pascal/Maxwell 老卡及特定量化格式可手动调优吞吐 |
| **[b10533](https://github.com/ggml-org/llama.cpp/releases/tag/b10533)** | Common: JSON Schema 正则不支持时优雅降级 | 边缘 schema 不再崩溃 |
| **[b10532](https://github.com/ggml-org/llama.cpp/releases/tag/b10532)** | Metal: FlashAttention 前将 Q8_0 KV 解压为 F16 | 与 b10538 配合，形成 **"小 batch 量化、大 batch 解压" 的分层策略** |
| **[b10531](https://github.com/ggml-org/llama.cpp/releases/tag/b10531)** | ⚠️ **回退** `tensor-split meta backend fixes` ([原 PR #26502](https://github.com/ggml-org/llama.cpp/pull/26502)) | **破坏性**: 多 GPU tensor-split 场景的 `buffer_usage` 传播与 `init_tensor` 调用恢复旧行为，相关用户暂缓升级 |
| **[b10520](https://github.com/ggml-org/llama.cpp/releases/tag/b10520)** | ggml-cpu: `__fp16` 类型严格依赖 `__ARM_FP16_FORMAT_IEEE`，非 AArch64 需显式 `-mfp16-format=ieee` | ARM32 构建修复，AArch64 无影响 |

---

## 新模型与硬件支持

| PR | 内容 | 状态 |
|:---|:---|:---|
| **[#27435](https://github.com/ggml-org/llama.cpp/pull/27435)** | **TeleChat4 模型全栈支持**：架构注册、MHC 模块、CUDA 算子、权重转换 | Open，待 review |
| **[#26534](https://github.com/ggml-org/llama.cpp/pull/26534)** | GLM-4.5-Air **MTP 投机解码**支持（`glm4moe` 的 `graph_mtp`） | Open，已验证 Solar Open/GLM 4.6V 无回归 |
| **[#27383](https://github.com/ggml-org/llama.cpp/pull/27383)** | **LFM2 模型 DSpark 投机解码**支持，含部分回滚状态修复 | ✅ **已合并** |
| **[#27453](https://github.com/ggml-org/llama.cpp/pull/27453)** | Vulkan 后端 **DeepSeek V4 LIGHTNING_INDEXER** 算子支持（F32/F16/BF16/多量化格式） | Open，补全 Vulkan 最后缺失的 V4 核心算子 |
| **[#26578](https://github.com/ggml-org/llama.cpp/pull/26578)** | Vulkan 后端 **DeepSeek-V4 超连接融合算子**（DSV4_HC_COMB/PRE/POST） | Open，CUDA/Metal 已支持，Vulkan 为最后一块拼图 |
| **[#27466](https://github.com/ggml-org/llama.cpp/pull/27466)** | ROCm: **长行 radix TOP_K**（>1024 元素），8-bit 精确选择 | Open，解决大 vocab 模型采样瓶颈 |

---

## 性能与优化

| 版本/PR | 优化点 | 数据/场景 |
|:---|:---|:---|
| **[b10538](https://github.com/ggml-org/llama.cpp/releases/tag/b10538)** + **[b10532](https://github.com/ggml-org/llama.cpp/releases/tag/b10532)** | Metal **自适应 KV Cache 解压策略**：小 batch 保持 Q8_0 量化，大 batch 预解压为 F16 | 平衡带宽压力与计算开销，Apple Silicon 长上下文解码关键优化 |
| **[b10534](https://github.com/ggml-org/llama.cpp/releases/tag/b10534)** | CUDA **架构感知解码路径切换**：MMVQ（小 batch 向量化）↔ MMQ（大 batch 矩阵）交叉点按 `sm_xxx` 与量化类型调参 | 默认 `MMVQ_MAX_BATCH_SIZE` 可被 `GGML_CUDA_MMVQ_MAX` 覆盖，Pascal 用户报告 +15-40% 吞吐 |
| **[#27402](https://github.com/ggml-org/llama.cpp/pull/27402)** | AVX2 **IQ 量化大 batch 预填充加速**：512 token batch 时重复解码权重，缓存 block 解码结果 | imatrix/perplexity 场景 CPU 性能瓶颈缓解 |
| **[#27461](https://github.com/ggml-org/llama.cpp/pull/27461)** | Metal **请求 Metal 4.0 语言版本**用于 tensor API，修复 M5 Max 上 prefill 与 decode 速度倒挂 | M5 Max 特定性能异常修复 |
| **[#25669](https://github.com/ggml-org/llama.cpp/pull/25669)** | CUDA SSM_SCAN 内核：字节 stride → 元素 stride，消除 `sizeof(float)` 除法与 `char*` 转换 | 代码清洁，行为不变 |

---

## 稳定性与回归

| 严重程度 | Issue | 描述 | Fix 状态 |
|:---|:---|:---|:---|
| 🔴 **高** | **[#27102](https://github.com/ggml-org/llama.cpp/issues/27102)** | CUDA kernel stall → watchdog kill，RTX Pro 6000 Blackwell MAX-Q，Qwen3.8-27B UD-Q8_K_XL | 无，需复现日志 |
| 🔴 **高** | **[#27447](https://github.com/ggml-org/llama.cpp/issues/27447)** | Step 3.7 模型 **b10509+ 无法运行**，Vulkan `vk::DeviceLostError` | ✅ **已关闭**，验证 b10507 为最后正常版本 |
| 🔴 **高** | **[#25593](https://github.com/ggml-org/llama.cpp/issues/25593)** | SM_60 (P100) **FP32 数学被静默执行 FP16**，质量损失，社区已合并两个 fork 修复 | 无官方 PR，需关注 |
| 🟡 **中** | **[#23774](https://github.com/ggml-org/llama.cpp/issues/23774)** | Vulkan **MTP 性能暴跌**（vs 无 MTP） | 无 |
| 🟡 **中** | **[#25489](https://github.com/ggml-org/llama.cpp/issues/25489)** | MTP 性能自 **b9935 回归**，Windows llama-server | 无，需 bisect |
| 🟡 **中** | **[#25618](https://github.com/ggml-org/llama.cpp/issues/25618)** | 投机解码（draft-mtp/draft-dspark）**贪心采样输出与 vanilla 不一致**，仅量化目标受影响 | 无，bf16 目标正常 |
| 🟡 **中** | **[#27444](https://github.com/ggml-org/llama.cpp/issues/27444)** | Qwen3.8-27B **单 generation 内吞吐衰减 ~30%**，RTX 5090 CUDA | 无，需 profiling |
| 🟡 **中** | **[#25304](https://github.com/ggml-org/llama.cpp/issues/25304)** | `cublasCreate_v2` 首次推理资源分配失败，**b9553→b9870 回归** | 无，Fedora Linux 特定 |
| 🟡 **中** | **[#26031](https://github.com/ggml-org/llama.cpp/issues/26031)** | Qwen3.6-35B-A3B-Q8_0 **多客户端并发乱码**，**b9922+ 回归**，纯 CPU `-np > 1` | 无，b9918 最后正常 |
| 🟡 **中** | **[#26209](https://github.com/ggml-org/llama.cpp/issues/26209)** | Agent 模式 **无限生成 "/" token**，commit c7d8722 后回归，HIP 后端 | 无 |
| 🟡 **中** | **[#26583](https://github.com/ggml-org/llama.cpp/issues/26583)** | GLM-5.2 **多节点 CUDA RPC 崩溃**，`invalid data ptr` / `graph_compute failed` | 无 |
| 🟢 **低** | **[#24795](https://github.com/ggml-org/llama.cpp/issues/24795)** | Gemma4-assistant MTP **"invalid vector subscript"**，**b9702/b9717 回归** | 无，b9553 正常 |

---

## 对应用开发者的意义

| 场景 | 影响 | 行动建议 |
|:---|:---|:---|
| **Apple Silicon 部署** | Metal 分层 KV Cache 策略（b10532/b10538）显著改善长上下文 decode 体验，无需配置 | 升级至 b10538+，监控 `llama-server` 内存曲线 |
| **多模态 Agent（vision）** | `--mmproj-device` 允许视觉编码器独立部署至专用 GPU/NPU，文本模型留在主卡 | 评估分离部署降低 OOM 风险，关注 [b10541](https://github.com/ggml-org/llama.cpp/releases/tag/b10541) |
| **投机解码集成** | MTP 稳定性问题集中爆发（#23774, #25489, #25618, #24795），且存在量化-specific 正确性风险 | **生产环境建议**：量化目标禁用 draft-mtp，改用 ngram 或 draft-dspark；验证贪心采样一致性后再上线 |
| **多 GPU 服务（tensor-split）** | b10531 回退导致 #26502 修复失效，`buffer_usage` 传播异常可能复现 | **暂缓升级至 b10531+**，或验证 `split-mode layer` 替代方案 |
| **Blackwell/RTX 50 系** | 新卡问题持续：#27102 (kernel stall), #27444 (吞吐衰减), #23385 (MMQ sharedMem 已修复) | 跟踪 NVIDIA 驱动更新，CUDA 13.3 仍非完全稳定 |
| **Server/Router 模式** | b10536 延迟加载修复启动竞态，`/models/sse` 进度报告重构中（[#24822](https://github.com/ggml-org/llama.cpp/issues/24822)） | 大规模模型热加载场景升级，关注后续 IPC 重构 PR |
| **Web UI 用户** | 生成文件下载按钮即将落地（[#26928](https://github.com/ggml-org/llama.cpp/pull/26928)） | 减少 copy-paste 代码块摩擦 |

---

*日报基于 ggml-org/llama.cpp 公开 GitHub 数据生成，版本号与 issue 编号可直接用于追踪。*

</details>

<details>
<summary><strong>Ollama</strong> — <a href="https://github.com/ollama/ollama">ollama/ollama</a></summary>

# Ollama 动态日报 | 2026-08-21

## 1. 今日速览

今日 Ollama 社区聚焦于 **MLX 引擎可靠性修复** 与 **桌面应用生态扩展**：核心团队密集合入了 MLX 前缀缓存、跨平台打包修复及 Claude Desktop 集成等 PR，同时 Qwen3.x 系列在 Vulkan/ROCm 后端上的加载与推理正确性问题成为用户报告热点，多个相关 Issue 尚待根治。

---

## 2. 版本发布与破坏性变更

**无新版本发布**（过去 24 小时无 Release）

**API/行为变更（已修复）**
- **CORS `OPTIONS` 预请求回归** [#17887](https://github.com/ollama/ollama/issues/17887)：0.32.x 中 `OPTIONS` 请求返回 `405`，破坏浏览器直接调用。PR [#17890](https://github.com/ollama/ollama/pull/17890) 已修复，对 loopback/私有地址返回 `204` + CORS 头。
- **OpenAI 兼容 `stop` 参数校验收紧** [#17896](https://github.com/ollama/ollama/pull/17896)：非字符串类型的 `stop` 数组元素此前被静默忽略，现返回显式校验错误，与 completions 路径行为一致。

---

## 3. 新模型与硬件支持

| 动态 | 详情 | 链接 |
|:---|:---|:---|
| **Qwen3.5 Vulkan 加载失败** | `qwen3.5:0.8b` 等模型在 Vulkan 后端触发 `llama-server` 崩溃，500 错误，影响 Linux/Windows | [#17903](https://github.com/ollama/ollama/issues/17903) |
| **ROCm gfx1151 (Strix Halo) KV 状态污染** | AMD Strix Halo iGPU 上连续请求间 KV cache 未隔离，后请求输出包含前请求内容；>4k tokens 时指令遵循失效 | [#17847](https://github.com/ollama/ollama/issues/17847), [#17895](https://github.com/ollama/ollama/issues/17895) |
| **Kimi K3 Cloud 上架争议关闭** | 用户追问两周后，Issue 以关闭状态结束，未明确解决 | [#17235](https://github.com/ollama/ollama/issues/17235), [#17715](https://github.com/ollama/ollama/issues/17715) |

---

## 4. 性能与优化

| 优化项 | 状态 | 关键改进 | 链接 |
|:---|:---|:---|:---|
| **MLX 前缀缓存持久化** | **PR 已开** | 解决 Agent 取消长 prefill 后重试从零开始的死循环：取消/恢复 prefill 时保留 cache restore point，避免"永远跑不过 timeout" | [#17901](https://github.com/ollama/ollama/pull/17901) |
| **MLX 跨平台打包修复** | **已合入** | 修复 mac 假设泄漏到 Windows/Linux 导致默认包损坏 | [#17898](https://github.com/ollama/ollama/pull/17898) |
| **MLX 版本更新** | **已合入** | 底层 MLX 框架升级 | [#17886](https://github.com/ollama/ollama/pull/17886) |
| **GGUF 元数据提取统一** | **PR 已开** | 将昂贵元数据提取结果缓存为 `<OLLAMA_MODELS>/metadata/sha256-<hex>.json`，消除双缓存不一致导致的 capability 判定漂移 | [#17858](https://github.com/ollama/ollama/pull/17858) |

> **待观察**：MLX 引擎目前 **无 prompt/prefix 缓存**（[#17829](https://github.com/ollama/ollama/issues/17829)），多步 Agent 每步全量 re-prefill 20-30K tokens，TTFT 线性恶化。PR #17901 是针对性修复的第一步。

---

## 5. 稳定性与回归（按严重程度排列）

| 优先级 | 问题 | 影响范围 | Fix 状态 | 链接 |
|:---|:---|:---|:---|:---|
| **P0** | **ROCm Strix Halo KV 污染 + 长文本指令失效** | AMD gfx1151 用户，Agent/多轮对话场景输出完全错乱 | ❌ 无 PR | [#17847](https://github.com/ollama/ollama/issues/17847), [#17895](https://github.com/ollama/ollama/issues/17895) |
| **P0** | **Windows UI 线程死循环** (`/api/v1/settings` GET↔POST) | 桌面版启动后阻塞所有 UI 请求，服务不可用 | ❌ 无 PR | [#17876](https://github.com/ollama/ollama/issues/17876) |
| **P1** | **Qwen3.8 `num_ctx` 截断后丢失 user query** → 500 错误 | 工具调用循环溢出上下文窗口时崩溃 | ✅ **PR #17894 已开** | [#17778](https://github.com/ollama/ollama/issues/17778) |
| **P1** | **Qwen3.6 `think:false + format:json` 返回 reasoning JSON** | 0.31.2→0.32.x 回归，结构化输出被破坏 | ❌ 无 PR | [#17871](https://github.com/ollama/ollama/issues/17871) |
| **P1** | **Qwen3.x 视觉模型色度丢失**（红→灰，绿/蓝→黑） | 视觉理解正确性严重受损 | ✅ **已关闭**（同权重在 `mlx_vlm` 正常，定位到 Ollama 管线） | [#17872](https://github.com/ollama/ollama/issues/17872) |
| **P1** | **deepseek-v4-flash Cloud 无限 thinking 循环** | 193 次相同工具调用，~31M tokens 浪费；另一变体 221 次重复 reasoning | ❌ 无 PR | [#17617](https://github.com/ollama/ollama/issues/17617), [#17892](https://github.com/ollama/ollama/issues/17892) |
| **P2** | **Qwen3.6/3.8 显存加载曲线异常**（RTX 5070Ti 12GB） | 新更新后直接撞 ceiling，无法按预期加载 35B Q4 | ❌ 待更多信息 | [#17517](https://github.com/ollama/ollama/issues/17517) |
| **P2** | **Qwen3.5 Vulkan 无法加载** | 500 错误，`llama-server` 进程终止 | ❌ 无 PR | [#17903](https://github.com/ollama/ollama/issues/17903) |
| **P2** | **Agent 集成在 macOS+Qwen 上无限挂起**（Ollama API 本身正常） | 特定客户端兼容性问题，24 评论未定位根因 | ❌ 无 PR | [#17839](https://github.com/ollama/ollama/issues/17839) |
| **P2** | **MLX 工具调用非流式导致大输出超时** | macOS MLX 后端工具响应不流式，`write` 大代码块时客户端超时 | ❌ 长期未修 | [#16279](https://github.com/ollama/ollama/issues/16279) |

---

## 6. 对应用开发者的意义

| 场景 | 影响与建议 |
|:---|:---|
| **构建 Agent/多步推理应用** | ⚠️ **高危**：Qwen3.x 系列在工具调用循环中极易触发上下文截断 Bug（#17778）和无限 thinking（#17617/#17892）。建议：① 显式设置保守 `num_ctx` 并监控 token 使用；② 对 deepseek-v4-flash Cloud 添加 thinking 轮次上限熔断；③ 关注 PR #17894 合入进度。 |
| **macOS + MLX 本地部署** | 🔧 **改善中**：PR #17901 将解决"取消长 prefill → 重试死循环"的 Agent 杀手级问题。当前建议避免依赖 MLX 进行 >20K token 的多轮 Agent 任务，或改用 CPU/GGUF 后端。 |
| **浏览器/SPA 直接调用 Ollama API** | ✅ **已修复**：CORS 回归（#17887）有 PR #17890，loopback/私有地址可恢复正常 `fetch()`。生产部署仍需注意跨域策略。 |
| **AMD Strix Halo (gfx1151) 用户** | ❌ **建议回避 ROCm 后端**：KV 污染（#17847）和长文本失效（#17895）是硬件+后端组合的严重正确性缺陷，临时切换 Vulkan 或 CPU 验证是否复现。 |
| **Ollama Cloud 付费用户** | ⚠️ **计费风险**：`deepseek-v4-flash` 的无限循环可瞬间消耗百万级 tokens（#17617 报告 31M tokens）。同时注意 `glm-5.2:cloud` 的 402 计费不一致（#17639）——直接调用 `ollama.com/v1` 与本地代理路径计费策略不同。 |
| **桌面应用集成** | 📱 **新能力**：Claude Desktop 集成 PR #17899/#17900 已开，Ollama 正从"后台引擎"转向"应用中枢"。开发者可关注其 MCP/应用连接协议的后续开放。 |
| **Ubuntu 26.04 及新发行版部署** | ✅ **已修复**：`install.sh` 缺失 `zstd` 时回退 `.tgz`（#17877）或自动安装 `zstd`（#17891），解决干净系统安装失败问题。 |

---

*日报基于 GitHub 公开数据生成，Issue/PR 状态以实际页面为准。*

</details>

<details>
<summary><strong>LiteLLM</strong> — <a href="https://github.com/BerriAI/litellm">BerriAI/litellm</a></summary>

# LiteLLM 动态日报 | 2026-08-21

> 专注 AI 基础设施：推理引擎、模型服务、LLM 网关、微调框架

---

## 1. 今日速览

今日 LiteLLM 无新版本发布，但社区活跃度极高：**338 个 PR 更新**，核心围绕 **RBAC 自定义角色**、**预算计费精度修复**、**并发槽位泄漏修复** 三大主题。多个长期存在的稳定性问题（MCP 工具冲突、缓存计费归零、Anthropic 协议头丢失）获得实质性修复 PR。

---

## 2. 版本发布与破坏性变更

**无新版本发布**（过去 24 小时无 Release）。

**潜在破坏性变更预警**：
- PR #37771 引入 **自定义 RBAC 角色**（`custom_roles`），允许通过 `allow_list` 精确控制路由权限，并支持角色继承。现有 `internal_user` 等内置角色行为不变，但未知角色将不再静默回退。
- PR #37570 变更 **预算错误信息格式**：429 响应将明确列出所有生效的预算实体（key/team/user/model/provider/tier 等共 11 个作用域），便于调试。

---

## 3. 新模型与硬件支持

| 变更 | 详情 | 链接 |
|:---|:---|:---|
| **DashScope (阿里百炼) Anthropic Messages API 适配** | 新增原生 `/apps/anthropic/v1/messages` 路由，替代原有的 OpenAI chat/completions 降级路径 | [PR #37619](https://github.com/BerriAI/litellm/pull/37619) |
| **Cognition SWE-1.7 定价修正** | `cognition/swe-1.7` 从错误的 Lightning 档位（$2.5/$12.5）修正为标准档（$0.50/$2.50）；新增 `swe-1.7-lightning` 条目 | [PR #37763](https://github.com/BerriAI/litellm/pull/37763) |
| **Azure GPT-5.6 缓存写入定价补全** | 所有 `azure/gpt-5.6*` 条目补充 `cache_creation_input_token_cost`，修复 v1.97.0 引入的缓存写入零计费问题 | [Issue #37631](https://github.com/BerriAI/litellm/issues/37631) |

---

## 4. 性能与优化

| 优化项 | 具体改进 | 链接 |
|:---|:---|:---|
| **Spend Log 写入行数上限** | 原仅按字节分片导致小行累积、内存泄漏（~110 MB/worker）；新增行数上限约束，控制写入语句规模 | [PR #37758](https://github.com/BerriAI/litellm/pull/37758) |
| **并发槽位 TTL 可配置** | `max_parallel_requests` 槽位原固定 3600s TTL，流式日志失败时泄漏导致假 429；现支持配置 TTL，且异常路径保证释放 | [PR #37768](https://github.com/BerriAI/litellm/pull/37768) / [PR #37535](https://github.com/BerriAI/litellm/pull/37535) |
| **Bedrock 响应头透传** | `/chat/completions` 现在转发 `x-amzn-requestid` 等 AWS 头，便于分布式追踪 | [PR #37003](https://github.com/BerriAI/litellm/pull/37003) |
| **Anthropic 429 头透传** | `/v1/messages` 转发 `retry-after` 和 `anthropic-ratelimit-unified-status`，避免 Claude Code 无限重试 | [PR #37767](https://github.com/BerriAI/litellm/pull/37767) |

---

## 5. 稳定性与回归（按严重程度排列）

### 🔴 高严重：计费与预算正确性

| 问题 | 状态 | 影响 | Fix PR |
|:---|:---|:---|:---|
| **Virtual Key 预算使用过期数据** (#27735) | **OPEN** | `BudgetExceededError` 基于 stale spend 触发，但 `/key/info` 显示未超预算；团队级虚拟 key 受影响 | 无 |
| **Per-model 预算追踪归零** (#37736) | **OPEN** | 按模型预算始终显示 0，key 被 429 阻断但用量为 0；Bedrock 模型名匹配失败；`/user/new` 写空 `model_max_budget` | [PR #37736](https://github.com/BerriAI/litellm/pull/37736) |
| **Azure GPT-5.6 缓存写入零计费** (#37631) | **OPEN** | v1.97.0 起 `cache_creation_input_token_cost` 缺失，缓存写入完全免费 | 无 |
| **OpenAI cache-write 成本丢失** (#33772) | **CLOSED** | 缓存写入 token 未计入定价，缓存命中请求计费错误 | [已修复](https://github.com/BerriAI/litellm/issues/33772) |
| **OpenAI Responses API cache breakdown 为 null** (#34309) | **CLOSED** | `cache_read_cost` / `cache_creation_cost` 仅读取 Anthropic 风格顶层 usage，OpenAI 路径为 null | [已修复](https://github.com/BerriAI/litellm/issues/34309) |

### 🟡 中严重：协议兼容与工具调用

| 问题 | 状态 | 影响 | Fix PR |
|:---|:---|:---|:---|
| **MCP auto-execute 劫持客户端工具** (#37031) | **OPEN** | `require_approval: "never"` 时，代理端自动执行 MCP 工具，与 Claude Code 等客户端工具冲突，非 MCP 工具全部报错 | 无 |
| **Anthropic Messages bridge 缓存统计丢失** (#36091) | **CLOSED** | Responses API 上游转 Anthropic 格式时 `cache_read_input_tokens` 始终为 0，100% 缓存命中也无统计 | [已修复](https://github.com/BerriAI/litellm/issues/36091) |
| **Mid-conversation system role 破坏 prompt cache** (#36559) | **CLOSED** | `AnthropicMessagesConfig` 将对话中 system 消息提升为顶层，导致前缀缓存失效，增加成本和延迟 | [已修复](https://github.com/BerriAI/litellm/issues/36559) |
| **GPT-5.6 家族 function tools + reasoning_effort 冲突** (#33221) | **CLOSED** | 自托管代理调用 gpt-5.6-sol/luna/terra 时工具调用失败 | [已修复](https://github.com/BerriAI/litellm/issues/33221) |
| **Assistant tool-call `content: null` 被移除** | **OPEN** | 严格 OpenAI 兼容提供商标注缺失 `content` 拒绝多轮 agent 工具调用 | [PR #37765](https://github.com/BerriAI/litellm/pull/37765) |

### 🟢 低严重：UI、日志与边缘场景

| 问题 | 状态 | 影响 | Fix PR |
|:---|:---|:---|:---|
| **Admin UI hydration 失败** (#27637) | **OPEN** | v1.83.10 起 `/ui/` 无限转圈，Next.js hydration 未完成 | 无 |
| **DeepSeek thinking-mode 日志污染** (#37629) | **OPEN** | 多轮工具调用时警告随历史重放指数增长，淹没有效日志 | 无 |
| **Snowflake Cortex streaming tool-calls 丢失** (#30762) | **OPEN** | 流式响应中 tool-calls 被丢弃，文档端点描述误导 | 无 |
| **GET /health 泄露敏感头** (#36898) | **OPEN** | `extra_headers` 和 `aws_session_token` 明文返回，`api_key` 虽脱敏但其他凭证未处理 | 无 |

---

## 6. 对应用开发者的意义

### Agent / 应用构建者需关注

| 场景 | 建议行动 |
|:---|:---|
| **使用 Claude Code / 其他 agentic 客户端** | 避免对同一模型同时配置 `require_approval: "never"` 的 MCP 工具和客户端原生工具（#37031），当前会互斥失败。临时方案：MCP 工具改为 `require_approval: "auto"` 或分离部署。 |
| **依赖 Anthropic `/v1/messages` 代理 OpenAI 模型** | 今日多个修复（#36091 缓存、#36559 prompt cache、#37767 429 头）显著改善生产可用性，建议跟进下一版本。 |
| **多租户预算控制** | 虚拟 key + per-model 预算存在已知一致性 bug（#27735、#37736），关键业务建议加一层外部预算校验，或避免使用 per-model 预算。 |
| **成本精细化追踪** | Azure GPT-5.6 缓存写入成本 currently zero（#37631），若依赖成本数据做决策需手动补正。 |
| **自定义 RBAC 落地** | 新自定义角色系统（#37771）上线后，可细粒度控制内部用户的路由权限，替代之前的 all-or-nothing 内置角色。 |

### 基础设施运维者需关注

- **并发控制**：流式日志失败导致的假 429 问题（#37768）有完整修复，建议评估 `max_parallel_requests_ttl_seconds` 配置需求。
- **Spend Log 内存**：小行累积内存问题（#37758）修复后，高并发场景 worker 内存占用应显著下降。

---

*日报基于 GitHub 公开数据生成，PR 评论数为 `undefined` 表示未获取到具体数值。*

</details>

<details>
<summary><strong>Unsloth</strong> — <a href="https://github.com/unslothai/unsloth">unslothai/unsloth</a></summary>

# Unsloth 动态日报 | 2026-08-21

## 今日速览

Unsloth 今日发布 **v0.1.801-beta**，引入实验性的 **Auto Compaction**（超长对话自动压缩）和 **LAN Remote Access**（局域网远程访问）两大功能，同时合并了 200+ PR。社区侧，Qwen3.8-27B 在 Apple Silicon 上的稳定性问题、Windows 安装失败、NVFP4 在 5060Ti 上的加载失败成为高热度议题，多个修复 PR 已进入 review 阶段。

---

## 版本发布与破坏性变更

| 项目 | 详情 |
|:---|:---|
| **v0.1.801-beta** | [Release 页面](https://github.com/unslothai/unsloth/releases) |
| Auto Compaction (Experimental) | 突破上下文长度限制的超长对话自动压缩，今日已有独立 issue #9401 请求该功能，显示社区需求与产品节奏高度同步 |
| Remote & LAN Access (Preview) | 简化网络访问配置，降低多设备/团队部署门槛 |
| ⚠️ 迁移注意 | 该版本合并 200+ PR，涉及大量底层变更；生产环境建议等待稳定版标签 |

---

## 新模型与硬件支持

| 方向 | 详情 | 链接 |
|:---|:---|:---|
| **新模型请求** | **Ling 3.0** 支持请求 (#8532)，要求完整集成到 Studio 的下载/加载/配置/服务流程，获 5 👍 | [#8532](https://github.com/unslothai/unsloth/issues/8532) |
| **AMD ROCm** | PR #8821 为 library 用户开启 AOTriton flash attention 实验性支持，解决此前仅 Studio 可用的问题 | [#8821](https://github.com/unslothai/unsloth/pull/8821) |
| **Apple Silicon (MLX)** | PR #9447 修复 MLX 运行时 `torch_amp_custom_fwd` 缺失导致的模块导入失败；PR #9120 修复 MLX Train/Export 按钮误置灰的竞态条件 | [#9447](https://github.com/unslothai/unsloth/pull/9447) [#9120](https://github.com/unslothai/unsloth/pull/9120) |
| **Intel 后端** | Issue #8972 报告 Intel 后端匹配错误，状态仍为 OPEN | [#8972](https://github.com/unslothai/unsloth/issues/8972) |

---

## 性能与优化

| 优化项 | 详情 | 链接 |
|:---|:---|:---|
| **KV Cache 量化 + Tensor Parallel** | PR #8939 修复 tensor parallelism 开启时量化 KV cache 被静默丢弃为 f16 的 bug，显著降低多卡场景显存占用 | [#8939](https://github.com/unslothai/unsloth/pull/8939) |
| **Auto Compaction 效率** | PR #9442 修复 compaction 后新 turn 空间计算错误，避免"压缩后反而装不下"的悖论；测试显示 10,583 token 对话压缩至 1,265 token（1,536 ctx window 场景） | [#9442](https://github.com/unslothai/unsloth/pull/9442) |
| **studiobench 虚拟化线程** | PR #9439 解决 readiness gate 等待全量消息挂载导致的 UNSCORED 问题，修复 100K+ 级性能退化 | [#9439](https://github.com/unslothai/unsloth/pull/9439) |
| **llama.cpp 自定义参数加固** | PR #8829（已关闭，被替代）→ 相关安全重构持续进行，集中管理 flag 别名、arity、策略分类 | [#8829](https://github.com/unslothai/unsloth/pull/8829) |

---

## 稳定性与回归

| 严重程度 | 问题 | 状态 | 链接 |
|:---|:---|:---|:---|
| 🔴 **高** | **Qwen3.8-27B 在 M3 Mac 上导致系统级崩溃**（紫屏、闪烁、Dock 异常），LM Studio 正常，指向 Unsloth GUI 渲染/内存管理缺陷 | OPEN, 无 fix PR | [#9279](https://github.com/unslothai/unsloth/issues/9279) |
| 🔴 **高** | **Windows 安装失败**（WinError 2），全新报告 | OPEN, 今日创建 | [#9440](https://github.com/unslothai/unsloth/issues/9440) |
| 🔴 **高** | **NVFP4 在 RTX 5060Ti 16GB 上无法加载**，更新后仍失败 | OPEN | [#8246](https://github.com/unslothai/unsloth/issues/8246) |
| 🟡 **中** | **Tool calling 失败**（NVIDIA Nemotron API），`function.arguments` JSON 解析错误 | CLOSED | [#9338](https://github.com/unslothai/unsloth/issues/9338) |
| 🟡 **中** | **部分模型绕过 `HF_ENDPOINT` 镜像**，直接从 huggingface.co 下载（Meta-Llama-3.1-8B 受影响，Llama-3.2-3B 正常） | OPEN, 标记 fixing | [#1353](https://github.com/unslothai/unsloth/issues/1353) |
| 🟡 **中** | **macOS 文本编码错误**，桌面端显示异常 | OPEN | [#8594](https://github.com/unslothai/unsloth/issues/8594) |
| 🟡 **中** | **Studio 无法执行连续两次 web search** | OPEN | [#9108](https://github.com/unslothai/unsloth/issues/9108) |
| 🟡 **中** | **`-H 0.0.0.0` 在 macOS 上绑定错误 IP**，有安全 implications | OPEN | [#8868](https://github.com/unslothai/unsloth/issues/8868) |
| 🟡 **中** | **Image generation 卡在 0%**（text encoding + warmup） | OPEN, 今日创建 | [#9404](https://github.com/unslothai/unsloth/issues/9404) |
| 🟡 **中** | **本地模型加载后 HTTP 400**，Qwen3.5-4B Safetensors | OPEN, 今日创建 | [#9398](https://github.com/unslothai/unsloth/issues/9398) |
| 🟢 **低** | **下载速度指示器不准确**（剧烈跳动 5hr→2min） | CLOSED | [#9378](https://github.com/unslothai/unsloth/issues/9378) |
| 🟢 **低** | **首次启动获取锁失败**（`Can not acquire lock`） | CLOSED | [#9140](https://github.com/unslothai/unsloth/issues/9140) |

---

## 对应用开发者的意义

| 维度 | 影响 |
|:---|:---|
| **Agent 开发** | PR #9355 引入 **Portable Agent Skills**（ZIP/仓库导入），配合 PR #7805 的外部 provider 工具执行，Unsloth Studio 正从"聊天界面"向"Agent 运行时"演进。开发者可打包技能脚本、引用和资产，但需注意 #9108 的连续 web search 限制当前会中断多步 Agent 工作流。 |
| **API 兼容性** | Issue #9340 暴露 **Model List API 不返回量化版本**的问题，依赖量化区分的应用（如成本优化路由）需临时通过文件名推断或等待修复。 |
| **远程/边缘部署** | LAN Remote Access + PR #9187 的"断线续传"本地生成，使 Studio 可作为边缘节点部署；但 #8868 的 macOS 绑定 IP 错误和 #9440 的 Windows 安装失败，提示跨平台网络配置仍需打磨。 |
| **MCP 生态** | Issue #9145 报告 Wispr Flow MCP OAuth 401 失败，显示第三方 MCP 集成尚不稳定；PR #9355 的技能导入机制或为 MCP 替代/补充方案。 |
| **量化与导出** | Issue #8904 指出 FP8/FP4 导出自动安装 `llm-compressor` 未征求同意，CI/CD 管道中需警惕环境突变；PR #8939 的 KV cache 量化修复对高并发服务降低显存关键。 |
| **语音/多模态** | PR #9214（外部 TTS 端点）、PR #9349（自定义 STT）使语音栈可完全外包，保持本地 VRAM 专注 LLM；但 PR #9433 显示语音 GGUF 误混入 chat picker 的架构混淆问题仍需关注。 |

---

*日报基于 GitHub 公开数据生成，PR 评论数为 `undefined` 表示数据源未提供具体数值。*

</details>

---
*本日报由 [agents-radar](https://github.com/brysoh435-dev/agents-radar) 自动生成。*