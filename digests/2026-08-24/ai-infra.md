# AI 基础设施日报 2026-08-24

> 生成时间: 2026-08-24 03:32 UTC | 覆盖项目: 6 个

- [vLLM](https://github.com/vllm-project/vllm)
- [SGLang](https://github.com/sgl-project/sglang)
- [llama.cpp](https://github.com/ggml-org/llama.cpp)
- [Ollama](https://github.com/ollama/ollama)
- [LiteLLM](https://github.com/BerriAI/litellm)
- [Unsloth](https://github.com/unslothai/unsloth)

---

## 横向对比

# AI 基础设施生态横向对比分析 | 2026-08-24

---

## 1. 生态全景

当前 AI 基础设施呈现**"推理引擎内卷、PD 分离攻坚、量化格式碎片化"**的三重态势。vLLM 与 SGLang 在 MTP 投机解码和 PD 分离架构上激烈竞争，但双方均暴露长序列稳定性危机；llama.cpp 以 7 个版本的高频迭代巩固边缘/端侧霸主地位，Blackwell 新硬件适配成为共同痛点；Ollama 和 LiteLLM 分别聚焦本地体验优化与企业级网关可靠性，Unsloth 则在训练微调层加速 Studio 产品化。整体而言，**生产级部署的稳定性差距仍在拉大社区贡献速度**。

---

## 2. 各项目活跃度对比

| 项目 | Issues 更新 | PRs 更新 | Releases | 今日核心主题 |
|:---|:---|:---|:---|:---|
| **vLLM** | 14+（含 7 个 Critical/High 开放） | 6+（Sleep/Wake 修复集群） | 无 | MTP 长序列崩溃、Sleep/Wake 可靠性、Qwen3.6/DeepSeek V4 适配 |
| **SGLang** | 12+（5 个 Critical/High 开放） | 8+（PD 分离、权重缓存、DSA 后端） | 无 | PD 失败通知缺失、Weight Cache Daemon、SM10x 兼容性 |
| **llama.cpp** | 10+（3 个 High 开放） | 15+（OpenCL 5 个并行） | **7 个**（b10589-b10603） | GLM-4.5-Air MTP、DeepSeek-V4 回滚修复、DFlash2 推测解码 |
| **Ollama** | 12+（2 个 High 开放） | 17+ | 无 | KV Cache 持久化竞争实现、Qwen3.8 系统性崩溃、MLX 升级 |
| **LiteLLM** | 5+（均有关闭/修复 PR） | 11+ | 无 | Anthropic 联邦认证、thinking 块/缓存前缀修复、Bedrock 适配 |
| **Unsloth** | 8+ | **121+**（含 Studio 密集修复） | 无 | Studio 桌面端稳定性、PEFT 回归修复、HTTPS 默认下载 |

> **活跃度洞察**：Unsloth 的 121 PR 为异常值，反映 Studio 产品发布后的 bug 风暴期；llama.cpp 的 7 个版本发布频率为六项目最高，验证其"小步快跑"的端侧策略。

---

## 3. 模型支持竞速

| 模型/架构 | 首发支持 | 跟进状态 | 落后方 |
|:---|:---|:---|:---|
| **Qwen3.6 系列** | vLLM（内核融合 #52676） | SGLang（MTP 崩溃已关闭 #28431）、Ollama（解析器修复 #17945） | llama.cpp 未提及 |
| **DeepSeek V4** | vLLM（FP8 权重适配 #53503） | llama.cpp（多序列回滚修复 b10593） | SGLang 未提及；vLLM TP16 仍崩溃 #40955 |
| **GLM-4.5-Air / GLM-5.x** | llama.cpp（MTP b10603） | vLLM（GLM-5.2 稀疏索引 #49678）、SGLang（GLM5.1 DSA #24959） | — |
| **Gemma 4** | vLLM（基础支持） | Ollama（内存测量修复 #17951）、llama.cpp（SYCL 优化 #26689） | **vLLM 自身镜像冲突 #51744**、Unsloth LoRA 无效 #41754 |
| **Kimi-K3** | vLLM（基础支持） | — | **长上下文全局退化 #51039，无修复** |
| **MiniMax H3** | SGLang（扩散模型 #34893） | — | 其他项目未跟进 |
| **Zamba2** | llama.cpp（PR #21412 长期推进） | — | vLLM/SGLang 未提及 |
| **Mamba SSD** | SGLang（flashinfer_ssd/cake #35444） | llama.cpp（Metal 分块 SSD MMA #26647） | vLLM 未提及 |

> **竞速结论**：**vLLM 在大模型首发适配最快，但稳定性债最重**（Qwen3.6 MTP 崩溃、Kimi-K3 退化、DeepSeek V4 TP 失败）；**llama.cpp 在端侧/新架构覆盖最广**，GLM-4.5-Air MTP 和 Zamba2 显示其生态包容性；SGLang 在差异化模型（MiniMax、Mamba）上建立壁垒。

---

## 4. 性能优化前沿

| 优化方向 | 火力集中度 | 关键项目与进展 | 行业意义 |
|:---|:---|:---|:---|
| **KV Cache 生命周期管理** | 🔥🔥🔥🔥🔥 | vLLM Sleep/Wake（6 PR）、Ollama 持久化预填充（2 竞争 PR #17278/#17953）、SGLang HiCache（死锁 #30760）、vLLM PD Multi Tier（60s 丢弃 #53128） | **PD 分离架构的瓶颈从"能不能跑"转向"KV 不丢、延迟可控"** |
| **投机解码（MTP/EAGLE/DFlash）** | 🔥🔥🔥🔥🔥 | vLLM（调度隔离 #53080、GDN Prefix Cache #52244）、llama.cpp（DFlash2 #27342、GLM-4.5-Air MTP）、SGLang（MTP 权重静默丢弃 #35797） | 长序列场景集体翻车，**接受率与稳定性成为新战场** |
| **量化格式适配** | 🔥🔥🔥🔥 | vLLM（FP8 MoE 边界 #43396、NVFP4 未提及）、SGLang（NVFP4 KV + 投机崩溃 #36010）、llama.cpp（NVFP4/MXFP4 CUTLASS 实验 #26704）、Ollama（q4_0 乱码 #17614） | **NVFP4 在 Blackwell 上"能跑但危险"**，社区缺乏统一验证框架 |
| **分布式通信/序列并行** | 🔥🔥🔥 | vLLM（XPU SP 扩展 #49943/#49009、Elastic EP #53483）、SGLang（PD 角色切换 #28403） | 异构硬件（XPU/AMD）的序列并行追赶 CUDA 生态 |
| **算子/内核融合** | 🔥🔥🔥 | vLLM（Qwen3.6 fused QK-norm + MRoPE #52676）、llama.cpp（OpenCL Adreno 5 PR、SYCL TILE kernel #26689）、SGLang（LoRA MoE 引擎 #34803） | 端侧（Adreno）与数据中心（Blackwell）两端并行优化 |
| **权重服务/动态加载** | 🔥🔥 | SGLang（Weight Cache Daemon <1s #33522）、Ollama（预填充缓存 #17278） | **在线 A/B 测试、增量更新的基础设施就绪中** |

> **优化趋势**：KV Cache 管理已从"性能优化"升级为**架构正确性命题**——PD 分离、Sleep/Wake、多 Tier 存储的交叉复杂度，使得单一项目的修复难以覆盖全链路。

---

## 5. 分层定位差异

```
┌─────────────────────────────────────────────────────────────┐
│  应用/网关层    │  LiteLLM（统一路由、认证、计费、可观测性）      │
│                │  Ollama（本地开发者体验、模型生命周期管理）      │
├─────────────────────────────────────────────────────────────┤
│  推理服务层    │  vLLM（生产级高吞吐、多 GPU 分布式）            │
│                │  SGLang（PD 分离、动态权重、差异化硬件）         │
├─────────────────────────────────────────────────────────────┤
│  端侧/边缘层   │  llama.cpp（跨平台二进制、移动端优先）           │
├─────────────────────────────────────────────────────────────┤
│  训练/微调层   │  Unsloth（PEFT 加速、Studio 产品化）            │
└─────────────────────────────────────────────────────────────┘
```

| 维度 | vLLM | SGLang | llama.cpp | Ollama | LiteLLM | Unsloth |
|:---|:---|:---|:---|:---|:---|:---|
| **核心战场** | 数据中心大模型服务 | PD 分离/动态服务创新 | 端侧/边缘全平台覆盖 | 本地开发者第一入口 | 企业多模型路由治理 | 高效微调训练 |
| **今日痛点** | MTP 长序列崩溃、Sleep/Wake 债 | NIXL 可靠性、SM10x 兼容 | Blackwell decode hang | Qwen3.8 系统性崩溃 | 缓存前缀失效、异常事件丢失 | Studio 稳定性风暴 |
| **差异化壁垒** | 生态最丰富、首发模型最快 | Weight Cache Daemon、PD 运行时切换 | 版本发布频率、跨平台深度 | 易用性、社区集成广度 | 企业级功能完整度（SSO/计费/审计） | 训练速度、Hub 集成 |
| **向上/下延伸** | 下探边缘（困难） | 上探企业功能（进行中） | 上探服务化（llama-server） | 上探生产级（困难） | 下探本地推理（通过 Ollama/vLLM 后端） | 下探推理服务（Studio 尝试） |

---

## 6. 趋势信号与开发者行动指南

### 🔮 行业趋势信号

| 信号 | 证据 | 推断 |
|:---|:---|:---|
| **PD 分离从"演示"进入"生产债"阶段** | vLLM 60s KV 丢弃、SGLang NIXL 失败通知缺失、双方 HiCache/多 Tier 问题集中爆发 | 2026 H2 将是 PD 分离架构的**可靠性分水岭**，不能解决 KV 一致性的项目将被淘汰出生产环境 |
| **NVFP4/Blackwell 成为新"坑王"** | vLLM SM120 崩溃、SGLang NVFP4+投机解码必崩、llama.cpp decode hang、DeepGEMM 无 fallback | 新硬件的"纸面性能"与"生产可用"差距持续 6-12 个月，**FP8 仍是 2026 年最安全选择** |
| **MTP/投机解码的"长序列诅咒"** | vLLM Qwen3.6-27B 非法内存访问、Kimi-K3 全局退化、SGLang MTP 权重丢弃、llama.cpp 接受率 1/633 | 投机解码的**收益假设在长序列下失效**，需重新评估"首 token 延迟 vs 总吞吐"的权衡 |
| **权重服务化（Weight as a Service）萌芽** | SGLang Weight Cache Daemon <1s、Ollama 预填充缓存竞争 | 模型热更新、A/B 测试、增量部署的基础设施正在形成，**可能重塑 MLOps 流水线** |
| **端侧与数据中心的技术鸿沟缩小** | llama.cpp OpenCL Adreno 5 PR 并行、DFlash2 动态卷积、Metal Mamba SSD | 端侧推理不再只是"能跑"，开始向**动态深度、稀疏激活**等先进优化看齐 |

### 🎯 Agent/应用开发者行动清单

| 优先级 | 行动 | 截止日期感 |
|:---|:---|:---|
| **P0** | **暂停 Qwen3.8 + 工具调用组合上线**（vLLM #17778、Ollama #17778/#17961） | 立即 |
| **P0** | **生产环境禁用 NVFP4 KV + 投机解码**（SGLang #36010 必崩） | 立即 |
| **P1** | 评估 llama.cpp b10598+ 的视觉 pipeline，验证 resize 算法变更影响 | 1 周内 |
| **P1** | 为 Anthropic 多轮 system 消息设计 workaround（LiteLLM #38053 缓存前缀失效） | 跟踪 PR 合并 |
| **P2** | 预研 Ollama `OLLAMA_PREFILL_CACHE` 或 SGLang Weight Cache Daemon 的架构适配 | 1 个月内 |
| **P2** | 监控 vLLM Sleep/Wake 6 PR 合并进度，评估弹性伸缩场景可用性 | 持续 |
| **P3** | 关注 llama.cpp DFlash2（#27342）与 agentskills.io 规范，规划 Agent 技能标准化 | 季度规划 |

---

*报告生成时间：2026-08-24 | 数据来源：六项目 GitHub 公开动态 | 建议复核周期：24-48 小时*

---

## 各项目详细报告

<details>
<summary><strong>vLLM</strong> — <a href="https://github.com/vllm-project/vllm">vllm-project/vllm</a></summary>

# vLLM 动态日报 | 2026-08-24

## 1. 今日速览

今日 vLLM 社区聚焦 **Sleep/Wake 模式可靠性修复**（6 个关联 PR 拆分修复 tensor 所有权与状态恢复问题），以及 **MTP 投机解码在长序列场景下的稳定性危机**——Qwen3.6-27B-FP8 出现非法内存访问崩溃，Kimi-K3 长上下文后输出退化至单 token 重复。无新版本发布。

---

## 2. 版本发布与破坏性变更

**无新版本发布。** 过去 24 小时无 Release。

⚠️ **已知兼容性风险**：
- `vllm/vllm-openai:latest` 镜像内嵌 Transformers 5.15.0 与 Gemma4 启动冲突 ([#51744](https://github.com/vllm-project/vllm/issues/51744))
- `OpenAIServingChat` 自 v0.18 起静默要求 `openai_serving_render` 关键字参数，为破坏性变更但未在 release note 中显著标注 ([#43411](https://github.com/vllm-project/vllm/issues/43411))

---

## 3. 新模型与硬件支持

| 方向 | 进展 | 链接 |
|:---|:---|:---|
| **Qwen3.6 内核融合** | 新增 fused QK-norm + partial MRoPE + gate 融合内核，针对 35B-A3B 的 per-head `[q\|gate]` 与零中心 RMSNorm 优化 | [#52676](https://github.com/vllm-project/vllm/pull/52676) |
| **XPU 序列并行扩展** | mxfp4 SP 支持（GPT-OSS-120B）+ block FP8 SP 支持（Qwen3-32B-FP8-block），均通过 `torch.compile`/`inductor` 路径 | [#49943](https://github.com/vllm-project/vllm/pull/49943), [#49009](https://github.com/vllm-project/vllm/pull/49009) |
| **GLM-5.2 稀疏索引复用** | Attention Metadata 缓存稀疏物理索引，跳过 `triton_convert_req_index_to_global_index`，**吞吐 +3.76%** | [#49678](https://github.com/vllm-project/vllm/pull/49678) |
| **DeepSeek V4 FP8 权重适配** | 修复 `sgl-project/DeepSeek-V4-Flash-FP8` 的 `wo_a` 层权重格式差异导致无输出的问题 | [#53503](https://github.com/vllm-project/vllm/pull/53503) |

---

## 4. 性能与优化

| 优化项 | 细节 | 状态 |
|:---|:---|:---|
| **投机解码调度隔离** | 分离 target/draft token budget，解决同请求两阶段处理 token 数不一致导致的调度冲突 | PR [#53080](https://github.com/vllm-project/vllm/pull/53080) |
| **Hybrid GDN Prefix Cache + MTP** | 修复 1072-token GDN page 下缓存提示词命中深度不足、长度为 hash unit 整数倍时零命中的问题 | PR [#52244](https://github.com/vllm-project/vllm/pull/52244) |
| **零 JIT 编译运行时** | 基础设施 PR #47451/#49315/#50174 已合并，DeepSeek V4 模型级去 JIT 化进行中 | [#49349](https://github.com/vllm-project/vllm/issues/49349) |
| **Elastic EP 初始化** | 移除新通信组的 warmup collective，避免与主服务线程 forward collectives 重叠（ROCm 已触发问题） | [#53483](https://github.com/vllm-project/vllm/pull/53483) |

---

## 5. 稳定性与回归（按严重程度排列）

### 🔴 Critical：崩溃 / 数据损坏 / 服务完全不可用

| 问题 | 影响 | Fix PR | 链接 |
|:---|:---|:---|:---|
| **MTP 投机解码长序列非法内存访问** | Qwen3.6-27B-FP8 + `num_spec_tokens=5`，长序列下 CUDA illegal memory access，**生产环境高危** | 无 | [#40756](https://github.com/vllm-project/vllm/issues/40756) |
| **Kimi-K3 长上下文后全局退化** | ~240K prefill 后所有后续请求退化为单 token 无限重复（NaN logits），**需重启服务恢复** | 无 | [#51039](https://github.com/vllm-project/vllm/issues/51039) |
| **DeepSeek V4 TP16 无法启动** | 官方 recipe 直接报错，影响大规模部署 | 无 | [#40955](https://github.com/vllm-project/vllm/issues/40955) |
| **DeepSeek V4 + DSpark RTX PRO 6000 崩溃** | SM120 + FlashInfer sparse MLA decode kernel routing 失败 | 无 | [#50720](https://github.com/vllm-project/vllm/issues/50720) |
| **DeepSeek V4 GB10 (sm_121) 启动失败** | DeepGEMM 假设可用但无 fallback，阻塞边缘部署 | 无 | [#52732](https://github.com/vllm-project/vllm/issues/52732) |
| **FP8 MoE `moe_permute` batch=8192 非法内存访问** | CUTLASS 内核边界条件，**可静默损坏输出** | 无 | [#43396](https://github.com/vllm-project/vllm/issues/43396) |
| **DFlash fused-KV 投影破坏量化 drafter** | `F.linear` 直接切片 `qkv_proj` 权重，量化 drafter 崩溃或静默错误 | 无 | [#51581](https://github.com/vllm-project/vllm/issues/51581) |

### 🟡 High：功能异常 / 性能严重下降

| 问题 | 影响 | Fix PR | 链接 |
|:---|:---|:---|:---|
| **Gemma4 + Transformers 5.15.0 启动失败** | 官方镜像默认环境不兼容 | 无 | [#51744](https://github.com/vllm-project/vllm/issues/51744) |
| **Align-mode prefix caching 零命中** | hybrid GDN + `--scheduling-policy priority` 下 0/996k 命中 | 无 | [#52897](https://github.com/vllm-project/vllm/issues/52897) |
| **XPU MTP/EAGLE + OffloadingConnector 失效** | 存储但永不服务 KV，投机解码退化为无加速 | 无 | [#52735](https://github.com/vllm-project/vllm/issues/52735) |
| **PD Multi Tier 60s 丢弃 parked KV** | 队列消费者强制重算，延迟抖动 | 无 | [#53128](https://github.com/vllm-project/vllm/issues/53128) |
| **Gemma 4 Unsloth LoRA 加载无效** | 成功加载但不生效，用户无感知错误 | 无 | [#41754](https://github.com/vllm-project/vllm/issues/41754) |
| **Gemma-4 尾部 `<turn\|>` token 回归** | v0.25.1→0.26.0 引入，破坏输出格式 | 无 | [#49955](https://github.com/vllm-project/vllm/issues/49955) |

### 🟢 Sleep/Wake 模式修复集群（今日集中产出）

| PR | 修复内容 | 链接 |
|:---|:---|:---|
| #53511 | MiniMax Lamport workspace 唤醒后状态重置 | [#53511](https://github.com/vllm-project/vllm/pull/53511) |
| #53510 | Humming 量化锁与 `processed` 状态恢复 | [#53510](https://github.com/vllm-project/vllm/pull/53510) |
| #53509 | MoE derived buffers（scales/strides/GEMM control）重载恢复 | [#53509](https://github.com/vllm-project/vllm/pull/53509) |
| #53508 | MRV2 KV cache 元数据隔离，避免被误清理 | [#53508](https://github.com/vllm-project/vllm/pull/53508) |
| #53507 | 运行时 buffer 注册到 sleep-managed 体系 | [#53507](https://github.com/vllm-project/vllm/pull/53507) |
| #53344 (已合并) | 顶层修复：tensor 所有权与 recovery 设计 | [#53344](https://github.com/vllm-project/vllm/pull/53344) |

---

## 6. 对应用开发者的意义

| 场景 | 建议 |
|:---|:---|
| **生产部署 MTP 投机解码** | **暂停使用** Qwen3.6/Kimi-K3 的 MTP 长序列场景，等待 [#40756](https://github.com/vllm-project/vllm/issues/40756) / [#51039](https://github.com/vllm-project/vllm/issues/51039) 修复。短序列可继续运行但需监控 |
| **Gemma4 应用** | 锁定 Transformers < 5.15.0 或等待镜像更新；注意 v0.26.0 `<turn\|>` 回归，输出后处理需兼容 |
| **DeepSeek V4 大规模部署** | TP16/DP+EP 配置存在已知崩溃，建议先用 TP8 以下验证稳定性；FP8 权重需确认 `wo_a` 层格式 |
| **LoRA 微调服务** | Unsloth 训练的 Gemma 4 adapter 当前不生效，bitsandbytes 路径为临时替代 |
| **PD 分离/多 tier 架构** | 60s KV 丢弃问题导致尾部延迟恶化，高并发场景建议评估是否回退至单实例或缩短队列深度 |
| **Sleep/Wake 弹性伸缩** | 今日 6 个修复 PR 表明该功能仍在快速迭代，生产启用需验证完整 wake-up 后状态一致性 |
| **OpenAI 兼容 API 封装** | 检查是否直接构造 `OpenAIServingChat`，v0.18+ 需传入 `openai_serving_render` 参数 |

---

*数据来源：vllm-project/vllm GitHub，统计窗口 2026-08-23 至 2026-08-24*

</details>

<details>
<summary><strong>SGLang</strong> — <a href="https://github.com/sgl-project/sglang">sgl-project/sglang</a></summary>

# SGLang 动态日报 | 2026-08-24

## 今日速览

今日社区焦点集中在 **PD 分离架构的可靠性加固** 与 **动态权重服务基础设施** 两大主线：NIXL 后端缺失失败通知的问题被正式暴露，同时 Weight Cache Daemon 的 Phase 2 路线图持续迭代。硬件层面，AMD HIP DSA 图捕获与 Intel XPU DSA Attention 的跨平台支持继续推进，但 SM10x 新架构（B300/sm_103）的 kernel 兼容性问题浮现。

---

## 版本发布与破坏性变更

**无新版本发布。**

---

## 新模型与硬件支持

| 条目 | 详情 | 链接 |
|:---|:---|:---|
| **AMD HIP DSA 图捕获扩展** | PR #35672 为 `DeepseekSparseAttnBackend` 启用 HIP 路径，使 GLM MI355 的 MTP draft_extend 可捕获 CUDA Graph，避免 eager fallback。 | [#35672](https://github.com/sgl-project/sglang/pull/35672) |
| **Intel XPU GLM5.1 DSA Attention** | PR #24959 持续迭代，为 `GlmMoeDsaForCausalLM` 启用 XPU 平台的 DSA 注意力，是 Intel GPU 上稀疏注意力推理的关键拼图。 | [#24959](https://github.com/sgl-project/sglang/pull/24959) |
| **MiniMax H3 扩散模型** | PR #34893 引入 FlexAttention-based cube sparse attention，支持 3D 视觉流的 packed-sequence 与 denoise-step 稀疏调度。 | [#34893](https://github.com/sgl-project/sglang/pull/34893) |
| **Mamba SSD 严格后端** | PR #35444 新增 `flashinfer_ssd` 与 `cake` 两种 Mamba SSD prefill 后端，Nemotron-H 的 C256 逻辑元数据映射至物理 C128 域。 | [#35444](https://github.com/sgl-project/sglang/pull/35444) |

---

## 性能与优化

| 条目 | 详情 | 链接 |
|:---|:---|:---|
| **Weight Cache Daemon Phase 1 落地** | Issue #33522 更新：Phase 1（#27139）已实现 per-rank daemon 通过 CUDA IPC 提供 post-quantized weights，Qwen3-235B FP8 的权重加载从 **306–327s 降至 <1s**。Phase 2 路线图讨论中。 | [#33522](https://github.com/sgl-project/sglang/issues/33522) |
| **Gated Launch 延迟内存分配** | PR #35927 支持启动时延迟内存分配，降低冷启动峰值显存压力，对多租户调度场景有意义。 | [#35927](https://github.com/sgl-project/sglang/pull/35927) |
| **PD 角色运行时切换** | PR #28403（mori backend）允许 prefill/decode 角色在运行时动态切换，无需重启即可重平衡 P:D 比例。 | [#28403](https://github.com/sgl-project/sglang/pull/28403) |
| **LoRA MoE 执行引擎** | PR #34803 引入 `sgl_lora` MoE 引擎替代实验性 TRT-LLM 路径，解除 Blackwell-only 限制，支持 JSON 配置解析。 | [#34803](https://github.com/sgl-project/sglang/pull/34803) |
| **I-quant GGUF 性能缺口** | Issue #35019 报告 `IQ1/2/3/4_*` 量化类型缺失 MMQ kernel，prefill 较 llama.cpp **慢 4-6x**，为已知优化机会。 | [#35019](https://github.com/sgl-project/sglang/issues/35019) |

---

## 稳定性与回归

### 🔴 严重（崩溃/静默错误）

| 条目 | 详情 | 状态 |
|:---|:---|:---|
| **SM10x 架构 Kernel 兼容性** | Issue #34340：B300（sm_103）上两个 SM100-gated kernel 失败——cutedsl TGV BF16 GEMM 触发 Xid 13 "CTA Not Present"，trtllm-gen MoE finalize 静默挂起。根因：`is_sm100_supported()` 为 family check（major==10），sm_103 误入 SM100 路径。 | **待修复** [#34340](https://github.com/sgl-project/sglang/issues/34340) |
| **NVFP4 KV + 投机解码崩溃** | Issue #36010：`--kv-cache-dtype nvfp4` 与投机解码（MTP/EAGLE）不兼容，`extend_prefix_lens_cpu` 为 None 导致 FlashInfer dequant workspace TypeError。SM120/RTX 5090 复现。 | **待修复** [#36010](https://github.com/sgl-project/sglang/issues/36010) |
| **AMD DSA dense graph 全 NaN** | Issue #36071：#31324 后 HIP DSA decode runner 的 dense (k-only) 图在 DP attention 下输出全 NaN，sampler 持续输出 token 0。 | **待修复** [#36071](https://github.com/sgl-project/sglang/issues/36071) |
| **HiCache prefetch all_reduce 死锁** | Issue #30760：TP=4 无 PP 时 `check_prefetch_progress` 调用计数不匹配导致死锁。 | **待修复** [#30760](https://github.com/sgl-project/sglang/issues/30760) |
| **Qwen3.5 MTP 权重静默丢弃** | Issue #35797：compressed-tensors 检查点下 MTP 权重被静默忽略，accept length 钉死在 1.0，用户无感知降级。 | **待修复** [#35797](https://github.com/sgl-project/sglang/issues/35797) |

### 🟡 中等（功能缺陷/性能退化）

| 条目 | 详情 | 状态 |
|:---|:---|:---|
| **PD 失败通知不一致** | Issue #36033：NIXL 后端缺失 prefill→decode 失败通知（Mooncake/mori 均有），decode 端仅能通过超时发现故障。 | **待修复** [#36033](https://github.com/sgl-project/sglang/issues/36033) |
| **prefill 失败时 staging watermark 订阅者残留** | Issue #34737：heterogeneous-TP disagg 场景下 prefill 失败导致 stale subscriber，影响后续调度。 | **待修复** [#34737](https://github.com/sgl-project/sglang/issues/34737) |
| **pause_generation retract 未释放 RadixCache KV** | Issue #33783：`mode="retract"` 仅释放 request-owned suffix，RadixCache 持有的 prefix 未解锁。 | **待修复** [#33783](https://github.com/sgl-project/sglang/issues/33783) |
| **Qwen-VL 视觉标记误解析** | Issue #35582：请求含图片时，text 中的 literal `<|vision_start|>...` 被当作额外图像，HTTP 500。 | **待修复** [#35582](https://github.com/sgl-project/sglang/issues/35582) |

### 🟢 已关闭（供参考）

| 条目 | 详情 | 链接 |
|:---|:---|:---|
| PP=0 + HiCache 内存泄漏 | 今日关闭：PP 与 HiCache 同开时 server memory leak。 | [#28902](https://github.com/sgl-project/sglang/issues/28902) |
| qwen3.6 MTP crash (0.5.13) | 今日关闭：版本特定崩溃。 | [#28431](https://github.com/sgl-project/sglang/issues/28431) |
| LGPL-3.0 许可证合规 | 今日关闭：`easydict` 依赖的许可证冲突。 | [#29177](https://github.com/sgl-project/sglang/issues/29177) |

---

## 对应用开发者的意义

| 维度 | 影响 |
|:---|:---|
| **可靠性风险** | 若使用 NVFP4 KV cache（SM120/B200）+ 投机解码，**当前必崩**，需等待 #36010 修复；B300 用户需警惕 SM10x kernel 兼容性问题。 |
| **PD 分离生产化** | NIXL 后端缺失失败通知（#36033）意味着生产环境若选 NIXL，故障检测依赖超时而非主动通知，建议暂用 Mooncake/mori 或自建健康探测。 |
| **多模态应用** | Qwen-VL 的 literal vision marker 误解析（#35582）影响任意含 `<\|vision_start\|>` 文本的用户输入，需前端过滤或等待修复。 |
| **动态 LoRA/权重热更新** | Weight Cache Daemon（#33522）的 <1s 加载与 scheduler 版本追踪（PR #35925, #35926）为 **在线 A/B 测试、增量更新** 铺路，Agent 平台可关注其 API 定型。 |
| **跨平台部署** | AMD MI355 + Intel XPU 的 DSA 支持持续完善，但 AMD dense graph NaN（#36071）阻塞部分场景，建议验证后上线。 |

---

*日报基于 sgl-project/sglang 2026-08-24 的公开 GitHub 数据生成。*

</details>

<details>
<summary><strong>llama.cpp</strong> — <a href="https://github.com/ggml-org/llama.cpp">ggml-org/llama.cpp</a></summary>

# llama.cpp 动态日报 | 2026-08-24

## 今日速览

今日 llama.cpp 密集发布 7 个版本（b10589-b10603），核心围绕 **GLM-4.5-Air 的 MTP 多 token 预测支持**、**DeepSeek-V4 多序列回滚修复** 以及 **mtmd 视觉模块的 Pillow 精确缩放算法**；同时 OpenCL 后端迎来 5 个并行优化 PR，聚焦 Adreno GPU 的 speculative decoding 与 MoE 性能。社区侧 Blackwell/SM120 的 NVFP4/MXFP4 CUTLASS 路径与 DFlash2 推测解码进入实验阶段。

---

## 版本发布与破坏性变更

| 版本 | 核心变更 | 影响面 |
|:---|:---|:---|
| **[b10603](https://github.com/ggml-org/llama.cpp/releases/tag/b10603)** | `model: support MTP in GLM-4.5-Air` ([#26534](https://github.com/ggml-org/llama.cpp/pull/26534)) | **模型支持**：GLM-4.5-Air 现可启用 MTP 推测解码，降低首 token 延迟 |
| **[b10599](https://github.com/ggml-org/llama.cpp/releases/tag/b10599)** | `test: move tools/parser to tests` ([#27548](https://github.com/ggml-org/llama.cpp/pull/27548)) | 测试结构重构，无运行时影响 |
| **[b10598](https://github.com/ggml-org/llama.cpp/releases/tag/b10598)** | `mtmd: use pillow-accurate algo, correct resize_algo for all models` ([#27594](https://github.com/ggml-org/llama.cpp/pull/27594)) | **视觉精度修复**：多模态模型图像预处理与 Pillow 对齐，避免 resize 方式错配导致精度损失 |
| **[b10595](https://github.com/ggml-org/llama.cpp/releases/tag/b10595)** | `server: add LLAMA_SERVER_SLOTS_N_DIFF` ([#27600](https://github.com/ggml-org/llama.cpp/pull/27600)) | **API 新增**：服务端暴露 slot 数量差异指标，便于监控负载不均衡 |
| **[b10594](https://github.com/ggml-org/llama.cpp/releases/tag/b10594)** | `common: skip device_info loop if not printed` ([#26692](https://github.com/ggml-org/llama.cpp/pull/26692)) | **启动优化**：CUDA 等后端避免无意义的 GPU context 创建，缩短冷启动 ~100-500ms |
| **[b10593](https://github.com/ggml-org/llama.cpp/releases/tag/b10593)** | `DeepseekV4: fix rollback with multi-seq` ([#26756](https://github.com/ggml-org/llama.cpp/pull/26756)) | **关键修复**：DeepSeek-V4 多序列并行 + speculative decoding 时回滚损坏 KV cache 的问题 |
| **[b10589](https://github.com/ggml-org/llama.cpp/releases/tag/b10589)** | `cuda: add POOL_1D support` ([#27573](https://github.com/ggml-org/llama.cpp/pull/27573)) | 算子补齐：1D 池化支持，为音频/时序模型铺路 |

> **迁移注意**：b10598 的 `resize_algo` 修正为全模型默认值，若此前依赖特定 resize 行为的下游视觉 pipeline 需验证输出一致性。

---

## 新模型与硬件支持

| 类别 | 详情 | 链接 |
|:---|:---|:---|
| **新架构** | **Zamba2** 支持进入长期 PR 阶段，已完成架构映射与张量布局 ([#21412](https://github.com/ggml-org/llama.cpp/pull/21412)) | [PR #21412](https://github.com/ggml-org/llama.cpp/pull/21412) |
| **MTP 扩展** | GLM-4.5-Air MTP 落地；dots3note 模型的 draft-mtp 支持在审 ([#27618](https://github.com/ggml-org/llama.cpp/pull/27618)) | [PR #26534](https://github.com/ggml-org/llama.cpp/pull/26534), [PR #27618](https://github.com/ggml-org/llama.cpp/pull/27618) |
| **音频** | **Fun-ASR-Nano** 单二进制 ASR CLI（SAN-M encoder + Qwen3-0.6B LLM），CPU + CUDA 双后端 ([#27607](https://github.com/ggml-org/llama.cpp/pull/27607)) | [PR #27607](https://github.com/ggml-org/llama.cpp/pull/27607) |
| **Blackwell/SM120** | 实验性 CUTLASS MoE prefill 支持 **NVFP4/MXFP4**，W4A4 kernel 用于 prefill，MMVQ 用于 M=1 decode ([#26704](https://github.com/ggml-org/llama.cpp/pull/26704)) | [PR #26704](https://github.com/ggml-org/llama.cpp/pull/26704) |
| **Metal** | Mamba-2 prefill 分块 SSD MMA 优化，64 token chunk 内并行 simdgroup matmul ([#26647](https://github.com/ggml-org/llama.cpp/pull/26647)) | [PR #26647](https://github.com/ggml-org/llama.cpp/pull/26647) |

---

## 性能与优化

| 领域 | 优化内容 | 数据/范围 | 链接 |
|:---|:---|:---|:---|
| **OpenCL/Adreno** | 为 speculative decoding/MTP 的验证步骤选用 **right-sized matmul kernel**（ skinny GEMM 优化） | 解决 batch=1~4 时大 kernel 调度开销 | [PR #27635](https://github.com/ggml-org/llama.cpp/pull/27635) |
| **OpenCL/Adreno** | MoE expert matmul **按 batch size 动态选型**，替代静态可用性判断 | speculative decoding 小 batch 场景显著收益 | [PR #27637](https://github.com/ggml-org/llama.cpp/pull/27637) |
| **OpenCL/Adreno** | X2E 代 GPU 启用 `kernel_mul_mm_f16_f32_l4_lm`，修复 F16xF32 GEMM 路径选择 | 避免最慢 kernel 被误选 | [PR #27640](https://github.com/ggml-org/llama.cpp/pull/27640) |
| **SYCL/BMG** | 量化 KV decode（q4_0/q8_0）**TILE kernel 替代 VEC**，+42% ~ +169% | Qwen3.6-35B / Gemma 4 26B/12B @ 32K/118K context，零回归 | [PR #26689](https://github.com/ggml-org/llama.cpp/pull/26689) |
| **SYCL** | `--fit` 参数正确感知 `--fit-target`，VRAM 预留计算修正 | 多设备场景 `--fit-target 1` 可用 | [PR #27629](https://github.com/ggml-org/llama.cpp/pull/27629) |
| **推测解码** | **DFlash2** 引入：局部动态深度卷积 + candidate selector，比 DFlash 更深度的动态 kernel ([#27342](https://github.com/ggml-org/llama.cpp/pull/27342)) | 公式：`out[i,c] = Σ_t (base[t,c] + δ[i,t,g(c)]) · x[i−t,c]` | [PR #27342](https://github.com/ggml-org/llama.cpp/pull/27342) |
| **测试覆盖** | `test-backend-ops` 补充真实模型 emit 的 mul_mat、flash-attention、copy shape | 避免后端 kernel 选型无正确性 gate | [PR #27641](https://github.com/ggml-org/llama.cpp/pull/27641) |

---

## 稳定性与回归

| 严重程度 | 问题 | 状态 | 关键信息 |
|:---|:---|:---|:---|
| 🔴 **高** | **CUDA kernel stall / watchdog kill** (RTX Pro 6000 Blackwell, Qwen3.8-27B UD-Q8_K_XL) | **OPEN** [#27102](https://github.com/ggml-org/llama.cpp/issues/27102) | 执行中 GPU 挂死，26 评论持续跟踪，疑似与 Blackwell 新硬件相关 |
| 🔴 **高** | **Blackwell SM100 Qwen3.8-27B-NVFP4 decode hang**，CPU 空转无 GPU 工作 | **OPEN** [#27329](https://github.com/ggml-org/llama.cpp/issues/27329) | RTX 5090D，MTP 层已移除仍复现，构建 169e4a7 |
| 🔴 **高** | **HIP/ROCm gfx1151 (Strix Halo) 输出损坏**，Vulkan 同权重同参数正确 | **OPEN** [#27579](https://github.com/ggml-org/llama.cpp/issues/27579) | RDNA3.5 APU，ROCm 7.2.4，两 dense 架构均失败，Vulkan byte-identical 对照 |
| 🟡 **中** | **MTP native 独立 CUDA compute arena OOM**，共享 gallocr 可修复 | **OPEN** [#27282](https://github.com/ggml-org/llama.cpp/issues/27282) | RTX 3090 24GB，MTP 预留独立 arena 导致 OOM，社区有 workaround |
| 🟡 **中** | **Vulkan + MTP + `--no-mmap` GGML_ASSERT(addr)** | **CLOSED** [#24280](https://github.com/ggml-org/llama.cpp/issues/24280) | 已关闭，12 评论，涉及 Vulkan 内存映射路径 |
| 🟡 **中** | **mtmd video: MOOV atom 在 MP4 末尾时静默失败 / Windows hang** | **FIX PR 在审** [#27596](https://github.com/ggml-org/llama.cpp/pull/27596) | ngxson 提交修复，含 SIGPIPE 处理；关联 [#24394](https://github.com/ggml-org/llama.cpp/issues/24394), [#24429](https://github.com/ggml-org/llama.cpp/issues/24429) |
| 🟡 **中** | **MTP draft acceptance 1/633**（极低接受率） | **OPEN** [#27151](https://github.com/ggml-org/llama.cpp/issues/27151) | FreeBSD/Clang，Qwen3.8-27B，需确认是否与 BSD 特定或模型配置相关 |
| 🟡 **中** | **Vulkan Flash Attention 性能暴跌** (AMD Strix Halo) | **CLOSED** [#25207](https://github.com/ggml-org/llama.cpp/issues/25207) | 18 评论后关闭，可能为驱动/后端交互问题 |
| 🟢 **低** | **SYCL CoopMat2 shader 编译失败** (glslc -O, SPIR-V cap 5432) | **OPEN** [#25985](https://github.com/ggml-org/llama.cpp/issues/25985) | Polaris RX 580，老硬件新特性不匹配 |
| 🟢 **低** | **Windows Vulkan b10587 无法启动 llama-server** | **OPEN** [#27576](https://github.com/ggml-org/llama.cpp/issues/27576) | 5 评论，待复现确认 |

> **今日已修复**：DeepSeek-V4 多序列回滚（b10593）、mtmd resize 算法（b10598）、device_info 冷启动（b10594）、Vulkan 混合 GPU split-mode row（[#25884](https://github.com/ggml-org/llama.cpp/issues/25884) 关联）、`--repeat-last-n -1` 禁用重复惩罚（[#25388](https://github.com/ggml-org/llama.cpp/issues/25388)）。

---

## 对应用开发者的意义

| 场景 | 影响 | 行动建议 |
|:---|:---|:---|
| **Agent/多模态应用** | mtmd 视频输入的 MOOV 问题即将修复，图像 resize 与 Pillow 对齐 | 升级至 b10598+ 验证视觉 pipeline 一致性；视频场景可跟踪 [#27596](https://github.com/ggml-org/llama.cpp/pull/27596) |
| **推理服务部署** | `LLAMA_SERVER_SLOTS_N_DIFF` 新指标 + 冷启动优化 | 监控 slot 不均衡；CUDA 后端启动延迟降低，适合 Serverless 冷启动场景 |
| **Speculative Decoding 集成** | GLM-4.5-Air / dots3note MTP 扩展 + DFlash2 动态卷积 | 新模型可接入 draft 加速；DFlash2 的 candidate selector 可能提升接受率，关注 [#27342](https://github.com/ggml-org/llama.cpp/pull/27342) |
| **移动端/边缘 (Adreno)** | OpenCL 5 个并行优化，speculative decoding 性能大幅提升 | 高优先级跟进 wanghqc 的 Adreno 系列 PR，特别是 [#27635](https://github.com/ggml-org/llama.cpp/pull/27635), [#27637](https://github.com/ggml-org/llama.cpp/pull/27637), [#27640](https://github.com/ggml-org/llama.cpp/pull/27640) |
| **Blackwell 新硬件** | NVFP4/MXFP4 CUTLASS 路径实验性可用，但存在 decode hang 风险 | 生产环境暂缓 NVFP4；若测试，同步跟踪 [#27329](https://github.com/ggml-org/llama.cpp/issues/27329) 的 root cause |
| **ASR/语音 Agent** | Fun-ASR-Nano 单二进制方案进入 llama.cpp 生态 | 可评估替代 whisper.cpp 路径，SAN-M + Qwen3 组合适合中文场景 |
| **Agent Skills 标准化** | WebUI 新增 [agentskills.io](https://agentskills.io/specification) 支持 | 构建 Tool-use Agent 时可遵循该规范，提升跨框架互操作性 | [PR #27312](https://github.com/ggml-org/llama.cpp/pull/27312) |

---

*日报基于 ggml-org/llama.cpp 公开 GitHub 数据生成，版本号与 issue/PR 编号可直接用于追踪。*

</details>

<details>
<summary><strong>Ollama</strong> — <a href="https://github.com/ollama/ollama">ollama/ollama</a></summary>

# Ollama 动态日报 | 2026-08-24

## 今日速览

今日 Ollama 社区活跃度极高，**17 个 PR 和 12 个 Issue 在 24 小时内更新**，核心焦点集中在 **KV Cache 持久化预填充**（2 个 competing PR）、**MLX 后端版本升级与 CUDA 负载去重**，以及 **Qwen3 系列模型的解析器与推理稳定性修复**。无新版本发布。

---

## 版本发布与破坏性变更

**无** — 过去 24 小时无新 Release。

---

## 新模型与硬件支持

| 动态 | 说明 | 链接 |
|:---|:---|:---|
| **MLX 版本升级** | PR #17955 将 MLX 从 `27fec90` 升级至 `d9077d8`，PR #17850 已关闭（曾临时携带 mlx-c#127 补丁） | [PR #17955](https://github.com/ollama/ollama/pull/17955), [PR #17850](https://github.com/ollama/ollama/pull/17850) |
| **MLX CUDA 负载去重** | PR #17956 将 Windows/Linux MLX CUDA 构建中共享的 CUDA runtime 库从 `mlx_cuda_v13` 子目录移至同级 `cuda_v13` payload，减少安装包体积 | [PR #17956](https://github.com/ollama/ollama/pull/17956) |
| **AMD Strix Halo (gfx1151) 功耗异常** | Issue #17958：ROCm 后端下 ≥2 个常驻模型导致 GPU 无法 power-gate，空载功耗锁定 ~25W；单模型常驻正常。疑似调度器/ROCm runtime 问题，**尚无 fix** | [Issue #17958](https://github.com/ollama/ollama/issues/17958) |

---

## 性能与优化

| 动态 | 说明 | 链接 |
|:---|:---|:---|
| **实验性预填充缓存持久化** | PR #17953（CHIPMUNK-T0T）：跨 runner 重载的 KV Cache 持久化，解决 keep-alive 过期或模型切换后的全量 prefill 重计算问题。基于 slot save/restore，需显式启用 | [PR #17953](https://github.com/ollama/ollama/pull/17953) |
| **预填充缓存预热（替代方案）** | PR #17278（rohanpatel2002，7 月 21 日创建，今日更新）：`OLLAMA_PREFILL_CACHE` 环境变量，调度器管理的 save/restore，8 GiB LRU 上限，fail-open。覆盖 llama-server + MLX 双后端。**与 #17953 形成 competing implementation** | [PR #17278](https://github.com/ollama/ollama/pull/17278) |
| **Embed 并发上限** | PR #17954：限制 `/api/embed` 的 fan-out 并发，防止大批量请求打开数千个 runner 导致 OOM | [PR #17954](https://github.com/ollama/ollama/pull/17954) |
| **DRAFT 模型内存测量修复** | Issue #17951（已关闭）：Gemma 4 MTP 草稿模型内存测量失败，`/api/ps` 误报 315 MB（实际 4.4 GB），推测已修复 | [Issue #17951](https://github.com/ollama/ollama/issues/17951) |

> **基础设施视角**：两个 prefill cache PR 的并存值得关注。#17278 更成熟（有测试计划、双后端支持、LRU 上限），#17953 更轻量（纯实验性）。建议跟踪哪个会被合并，这直接影响长上下文服务的 TTFT（Time-To-First-Token）稳定性。

---

## 稳定性与回归

| 严重程度 | 问题 | 状态 | 链接 |
|:---|:---|:---|:---|
| 🔴 **高** | **Qwen3.8 流式推理崩溃**：`no user query found in messages`（500 错误），工具调用循环中触发，2050K 上下文 | **Open**，17 评论，8 👍 | [Issue #17778](https://github.com/ollama/ollama/issues/17778) |
| 🔴 **高** | **Qwen3.8:27B 响应截断**：Claude Code 集成场景下 20-30 秒后输出中断 | **Open**，可能与 #17778 同源 | [Issue #17961](https://github.com/ollama/ollama/issues/17961) |
| 🟡 **中** | **ornith-1.5:35b grammar 解析失败**：同时指定 `response format` + `tools` 时 `Failed to initialize samplers`，Qwen3.6:35b 正常 | **Open**，7 评论 | [Issue #17957](https://github.com/ollama/ollama/issues/17957) |
| 🟡 **中** | **HF 导入 stop 参数推导错误**：Muse-Glimmer-30B GGUF 被截断至 ~3 tokens | **Open**，2 评论 | [Issue #17939](https://github.com/ollama/ollama/issues/17939) |
| 🟡 **中** | **分片 GGUF 导入失败时机过晚**：无效分片在复制到 blob storage 后才报错，浪费磁盘/时间 | **Open**，PR #17949 提交修复 | [Issue #17946](https://github.com/ollama/ollama/issues/17946), [PR #17949](https://github.com/ollama/ollama/pull/17949) |
| 🟡 **中** | **context deadline exceeded**：多个 HF 下载模型触发，可能与量化格式或超时配置相关 | **Open**，13 评论 | [Issue #17484](https://github.com/ollama/ollama/issues/17484) |
| 🟢 **低** | **Qwen3 parser 不分离 thinking/content**：HF 导入的 RuadaptQwen3-8B-Hybrid 模型 `<think>` 标签未进入 `message.thinking` | **已关闭**，PR #17945 修复 | [Issue #17937](https://github.com/ollama/ollama/issues/17937), [PR #17945](https://github.com/ollama/ollama/pull/17945) |
| 🟢 **低** | **低比特量化输出乱码**：q4_0 KV cache 导致 "AI AI AI..." 重复垃圾输出 | **已关闭**，q8_0→q4_0 降级触发 | [Issue #17614](https://github.com/ollama/ollama/issues/17614) |

> **关键模式**：Qwen3.8 系列存在**系统性稳定性问题**（#17778、#17961），涉及工具调用 + 长上下文组合场景。生产环境建议暂缓升级 Qwen3.8 或限制工具使用。

---

## 对应用开发者的意义

| 影响域 | 具体说明 |
|:---|:---|
| **Agent/工具调用开发者** | Qwen3.8 的 `no user query found` 崩溃（#17778）和响应截断（#17961）是**阻塞性问题**。若你的 Agent 依赖 Qwen3.8 + function calling，建议：① 降级至 Qwen3.6；② 监控 #17778 修复；③ 实现客户端重试+降级逻辑。 |
| **长上下文服务** | prefill cache 持久化（#17278/#17953）一旦合并，将显著降低多租户场景下的 TTFT 抖动。建议提前设计 `OLLAMA_PREFILL_CACHE` 的运维策略（8 GiB LRU 需评估命中率）。 |
| **桌面应用集成** | 3 个新的 Community Integration PR（Grux、Locally Uncensored、Ontheia），Ollama 作为本地后端的选择持续扩大。 |
| **GGUF 自定义模型导入** | 分片 GGUF 验证前置（PR #17949）和 stop 参数推导修复（#17939）将减少"模型加载成功但输出异常"的调试成本。 |
| **ROCm/AMD 用户** | Strix Halo 的功耗异常（#17958）影响边缘/笔记本部署的多模型并发策略，当前 workaround 是限制单模型常驻。 |

---

*日报基于 ollama/ollama 公开 GitHub 数据生成，时间范围：2026-08-23 至 2026-08-24。*

</details>

<details>
<summary><strong>LiteLLM</strong> — <a href="https://github.com/BerriAI/litellm">BerriAI/litellm</a></summary>

# LiteLLM 动态日报 | 2026-08-24

## 今日速览

今日 LiteLLM 无新版本发布，但代码活跃度极高：**Anthropic 生态成为焦点**——Workload Identity Federation 联邦认证正式落地，同时修复了 thinking 块空文本、mid-conversation system 消息缓存失效等 3 个生产级缺陷。Bedrock 侧同步修复 rerank 文档回传、tool_choice="any" 适配等 4 项问题。稳定性方面，共享健康检查数据库写入协调机制进入 PR 阶段，有望缓解高并发场景下的模型状态漂移。

---

## 版本发布与破坏性变更

**无新版本发布**（过去 24 小时无 Release）

---

## 新模型与硬件支持

| 变动 | 详情 | 链接 |
|:---|:---|:---|
| **新增 Bee by HEOSSI 提供商** | 开源兼容（openai_like）的多模态智能平台，接入 6 个模型的 cost map，认证方式 `BEE_API_KEY` | [PR #38042](https://github.com/BerriAI/litellm/pull/38042) |

---

## 性能与优化

| 变动 | 详情 | 链接 |
|:---|:---|:---|
| **共享健康检查写入协调** | 解决缓存消费者重复读取 PostgreSQL 健康历史、慢模型写入逃逸持久化任务的问题；通过"仅本地执行检查持久化结果 + 等待选定模型写入"降低 DB 负载 | [PR #38058](https://github.com/BerriAI/litellm/pull/38058) |
| **OTel v2 指标导出间隔可配置** | 移除硬编码 5 秒间隔，支持标准 `OTEL_METRIC_EXPORT_INTERVAL` 环境变量，减少累积指标序列的冗余导出 | [PR #38055](https://github.com/BerriAI/litellm/pull/38055) |
| **token_counter 支持 OpenAI 'file' 内容块** | 补全文件类型内容的 token 计数能力，避免此类请求计数为 0 导致成本失真 | [PR #33659](https://github.com/BerriAI/litellm/pull/33659) |

---

## 稳定性与回归

### 🔴 严重（生产影响明确）

| 问题 | 状态 | 详情 | 链接 |
|:---|:---|:---|:---|
| **Anthropic thinking 块空文本导致 400** | **PR 待合并** | `thinking` 块有签名但 `thinking` 文本为空时未被丢弃，Anthropic 直接拒绝；影响 `anthropic/*` 与 `vertex_ai/claude-*` | [PR #38049](https://github.com/BerriAI/litellm/pull/38049) |
| **Anthropic chat completions 中 system 消息破坏缓存前缀** | **PR 待合并** | 每轮 mid-conversation system 消息被提升到顶层 system prompt，导致缓存前缀重写、全量重新计费；`/v1/messages` 已修，chat completions 路径遗漏 | [PR #38053](https://github.com/BerriAI/litellm/pull/38053) |
| **Bedrock rerank 忽略 `return_documents`** | **PR 待合并** | Bedrock 原生不返回文档文本，LiteLLM 未按 Cohere 兼容语义回填 `document.text`，导致下游 RAG 链路拿不到内容 | [PR #38044](https://github.com/BerriAI/litellm/pull/38044) · [Issue #38006](https://github.com/BerriAI/litellm/issues/38006) |
| **OTel v2 GenAI 异常事件整批丢失** | **PR 待合并** | `Event(body=None)` 被 OTLP encoder 丢弃，所有异常事件从未导出；修复后设置非空 body | [PR #38041](https://github.com/BerriAI/litellm/pull/38041) · [Issue #36863](https://github.com/BerriAI/litellm/issues/36863) |

### 🟡 中等（特定路径触发）

| 问题 | 状态 | 详情 | 链接 |
|:---|:---|:---|:---|
| **Azure 图像生成与 OCR 未传递 Entra ID Token** | **PR 待合并** | `enable_azure_ad_token_refresh` 下，chat/audio/embeddings 等路径可无 key 运行，但 image generation 与 `azure_ai/` OCR 失败 | [PR #38043](https://github.com/BerriAI/litellm/pull/38043) · [Issue #37727](https://github.com/BerriAI/litellm/issues/37727) |
| **Bedrock Converse 不支持 `tool_choice="any"`** | **PR 待合并** | `"required"` 已映射为 `{any: {}}`，但字面量 `"any"` 落入 else 分支抛异常 | [PR #38047](https://github.com/BerriAI/litellm/pull/38047) · [Issue #37980](https://github.com/BerriAI/litellm/issues/37980) |
| **拦截降级 stream:true 时返回非可迭代对象** | **PR 待合并** | `_wrap_response_as_fake_stream` 返回裸 chunk 而非迭代器，下游消费方直接崩溃 | [PR #38051](https://github.com/BerriAI/litellm/pull/38051) · [Issue #37652](https://github.com/BerriAI/litellm/issues/37652) |
| **assistant tool-call turn 的 `content: null` 被误删** | **PR 待合并** | `cleanup_none_field_in_message` 暴力过滤所有 `None` 值，破坏 OpenAI 消息格式契约 | [PR #38048](https://github.com/BerriAI/litellm/pull/38048) · [Issue #37711](https://github.com/BerriAI/litellm/issues/37711) |
| **OCRResponse Pydantic 模型无法构建** | **PR 待合并** | `tables`/`keyValuePairs` 类型注解导致循环引用，clean checkout 即报错 | [PR #38045](https://github.com/BerriAI/litellm/pull/38045) |

### 🟢 低 / 已关闭

| 问题 | 状态 | 详情 | 链接 |
|:---|:---|:---|:---|
| **OOM 内存持续增长** | 已关闭 | v1.82.0-stable Docker 镜像持续 OOM，社区反馈后关闭，建议跟进最新版本 | [Issue #25219](https://github.com/BerriAI/litellm/issues/25219) |
| **Redis cache `ssl_check_hostname` 异常** | 开放 | v1.93.0 引入，Redis 缓存与预算计数器完全失效 | [Issue #34614](https://github.com/BerriAI/litellm/issues/34614) |
| **预算超限误报（无 Redis 场景）** | 开放 | 高并发下 `Current cost = max_budget + recent spend`，约 2 分钟自愈 | [Issue #36926](https://github.com/BerriAI/litellm/issues/36926) |

---

## 对应用开发者的意义

| 场景 | 影响与行动 |
|:---|:---|
| **使用 Anthropic Claude 的 Agent 应用** | 立即关注 [PR #38053](https://github.com/BerriAI/litellm/pull/38053)：若你的 Agent 在多轮对话中动态注入 system 消息（如记忆总结、工具结果反馈），当前版本会导致**缓存前缀逐轮失效，token 成本指数级膨胀**。建议临时避免 mid-conversation system 消息，或跟踪该 PR 合并进度。 |
| **企业级 SSO / 联邦认证部署** | Anthropic Workload Identity Federation ([PR #38013](https://github.com/BerriAI/litellm/pull/38013)) 与 Azure Entra ID 全路径覆盖 ([PR #38043](https://github.com/BerriAI/litellm/pull/38043)) 双落地，**可开始淘汰长期静态 key** 的认证模式，转向零信任架构。 |
| **RAG / rerank 流水线** | Bedrock rerank 文档回传修复前，下游无法直接拿到 `document.text`，需从请求侧自行维护索引-文档映射表作为 workaround。 |
| **可观测性建设** | OTel v2 异常事件整批丢失是**静默故障**——你的监控大盘可能显示"零异常"，实际服务已在抛错。建议检查是否启用 `LITELLM_OTEL_V2=true` 且版本 < 修复版，或临时切回 v1 路径。 |
| **成本精细化运营** | 时序定价（off-peak/peak）PR [31725](https://github.com/BerriAI/litellm/pull/31725) 持续迭代中，DeepSeek 等提供商的分时计费场景可提前评估接入。 |

---

*日报基于 GitHub 公开数据生成，PR 评论数为 `undefined` 表示未获取到具体数值。*

</details>

<details>
<summary><strong>Unsloth</strong> — <a href="https://github.com/unslothai/unsloth">unslothai/unsloth</a></summary>

# Unsloth 动态日报 | 2026-08-24

## 今日速览

今日 Unsloth 无新版本发布，但社区活跃度极高：**121 个 PR 在过去 24 小时有更新**，核心团队密集修复 Studio 桌面端的稳定性问题（IndexedDB、WebKitGTK 崩溃、SQLite 死锁），同时推进多项基础设施改进：HTTPS 默认下载、Hub 模型缓存生命周期重构、以及 AMD APU VRAM 检测修正。训练侧修复了 `embed_tokens`/`lm_head` 被静默丢弃的 PEFT 回归问题。

---

## 新模型与硬件支持

| 变动 | 说明 | 链接 |
|:---|:---|:---|
| **新增音频模型支持** | PR #8794 为 Studio 添加 Higgs TTS、MOSS-TTS（Local/Nano）、MiniMax-Music3 原生音频支持，覆盖语音合成与音乐生成 | [PR #8794](https://github.com/unslothai/unsloth/pull/8794) |
| **AMD APU VRAM 检测修正** | PR #9314 修复 Windows 下 ROCm 将 APU 统一内存的 carve-out 误读为可用总量的问题（如 128GB Strix Halo 被识别为 ~8GB） | [PR #9314](https://github.com/unslothai/unsloth/pull/9314) |
| **AMD Docker 镜像需求** | Issue #9581 请求官方 AMD Docker 镜像，目前仅 CUDA 镜像可用 | [Issue #9581](https://github.com/unslothai/unsloth/issues/9581) |

---

## 性能与优化

| 变动 | 说明 | 链接 |
|:---|:---|:---|
| **GPU 显存报告改为驱动级** | PR #9362 将 `get_gpu_memory_info` 的 `free_gb` 从 `total - torch.allocated` 改为系统级驱动遥测，解决多进程/缓存未计入导致的显存误判 | [PR #9362](https://github.com/unslothai/unsloth/pull/9362) |
| **Kaggle T4 GPU 调度优化** | PR #9596 让 Studio GPU payload 与训练任务共享 GPU 而非等待完全释放，降低 T4 环境启动延迟 | [PR #9596](https://github.com/unslothai/unsloth/pull/9596) |
| **模型加载预估显存** | PR #9525 在 Load Model 面板新增"Estimated Memory Usage"行，展示权重、KV Cache、上下文长度对显存的细分影响 | [PR #9525](https://github.com/unslothai/unsloth/pull/9525) |
| **HTTPS 默认下载 + 显式传输切换** | PR #9272 将新安装默认从 Xet 改为 HTTPS，并在 Settings 添加传输协议显式切换，避免 Xet 在部分网络环境下的下载失败 | [PR #9272](https://github.com/unslothai/unsloth/pull/9272) |
| **HF Hub 断点续传恢复** | PR #9602 修复 huggingface_hub 1.18+ 取消下载后重新拉取整个文件的问题，800MB GGUF 可 Range 续传 | [PR #9602](https://github.com/unslothai/unsloth/pull/9602) |

---

## 稳定性与回归

### 🔴 严重（崩溃/数据丢失）

| 问题 | 状态 | 说明 | 链接 |
|:---|:---|:---|:---|
| **Studio SQLite 死锁致服务无响应** | ✅ **已修复** | `sqlite3.connect()/close()` 全线程阻塞，socket 存活但无连接接受；影响 `unsloth studio` 长时间运行 | [Issue #9008](https://github.com/unslothai/unsloth/issues/9008) |
| **WebKitWebProcess SIGABRT（Skia COLRv1 字体断言）** | 🟡 待修复 | Fedora 44 Wayland 下 Model Hub 崩溃，与 #9393 不同根因 | [Issue #9480](https://github.com/unslothai/unsloth/issues/9480) |
| **IndexedDB 无法建立数据库文件** | 🟡 待修复 | 聊天窗口发送消息时 IndexedDB 写入失败，导致消息无法持久化 | [Issue #9600](https://github.com/unslothai/unsloth/issues/9600) |

### 🟡 中等（功能异常）

| 问题 | 状态 | 说明 | 链接 |
|:---|:---|:---|:---|
| **聊天窗口 IndexedDB 持久化失败** | 🟡 待修复 | AppImage 桌面端每发一条消息必现，后端 API 健康但前端存储层异常 | [Issue #9518](https://github.com/unslothai/unsloth/issues/9518) |
| **PEFT `embed_tokens`/`lm_head` 被静默丢弃** | 🟡 **PR 修复中** | `target_modules` 显式列出时因正则匹配范围问题未注入 LoRA，影响继续预训练 | [PR #9331](https://github.com/unslothai/unsloth/pull/9331) |
| **Linux 语音输入无音频（WebKitGTK MediaRecorder 0 bytes）** | 🟡 待修复 | STT 录制产生空文件 | [Issue #9543](https://github.com/unslothai/unsloth/issues/9543) |
| **Windows 用户名含特殊字符导致模型加载失败** | 🟡 待修复 | 路径解析问题 | [Issue #9588](https://github.com/unslothai/unsloth/issues/9588) |

### 🟢 轻微/已修复

| 问题 | 状态 | 说明 | 链接 |
|:---|:---|:---|:---|
| **取消提示词产生连续 user 消息** | ✅ 已修复 | 违反部分 Jinja 模板约束 | [Issue #9484](https://github.com/unslothai/unsloth/issues/9484) |
| **Markdown chunk 加载失败拖垮整个应用** | ✅ 已修复 | `React.lazy` + `Suspense` 单点故障 | [Issue #9235](https://github.com/unslothai/unsloth/issues/9235) |
| **CI 测试写共享状态致 flaky failure** | ✅ 已修复 | `pytest-xdist` 多 worker 目录/环境变量冲突 | [Issue #9585](https://github.com/unslothai/unsloth/issues/9585) |

---

## 对应用开发者的意义

| 维度 | 影响 |
|:---|:---|
| **API 服务化** | Issue #9522 请求开放 LAN/WSL 访问（当前绑定 127.0.0.1），若合并将允许跨机器/容器调用 Unsloth 本地模型；PR #9288 修复 MLX 推理模型的 `reasoning_content` 与 `content` 字段兼容问题，对 OpenAI 兼容客户端更友好 |
| **MCP 工具链** | Issue #9584 请求 MCP 工具输出原生内联图片渲染（如 ComfyUI 工作流），当前仅文本输出，多模态 Agent 需自行处理外链 |
| **量化与部署** | Issue #9583 指出 Docker 镜像 llama.cpp 版本滞后（5 月 26 日），生产部署需自行更新或等待官方重建 |
| **训练可靠性** | PR #9331 修复的 `embed_tokens`/`lm_head` 静默丢弃问题直接影响继续预训练场景，使用自定义 `target_modules` 的开发者应尽快验证 |
| **Studio 作为宿主** | 今日 6 个 Studio 相关崩溃/存储问题表明桌面端仍处于快速迭代期，生产环境建议优先使用 Python 包/CLI 而非 AppImage |

---

*数据截止：2026-08-24 | 来源：github.com/unslothai/unsloth*

</details>

---
*本日报由 [agents-radar](https://github.com/brysoh435-dev/agents-radar) 自动生成。*