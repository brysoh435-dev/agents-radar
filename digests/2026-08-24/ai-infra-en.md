# AI Infrastructure Digest 2026-08-24

> Generated: 2026-08-24 03:32 UTC | Projects covered: 6

- [vLLM](https://github.com/vllm-project/vllm)
- [SGLang](https://github.com/sgl-project/sglang)
- [llama.cpp](https://github.com/ggml-org/llama.cpp)
- [Ollama](https://github.com/ollama/ollama)
- [LiteLLM](https://github.com/BerriAI/litellm)
- [Unsloth](https://github.com/unslothai/unsloth)

---

## Cross-Project Comparison

# AI Infrastructure Ecosystem Cross-Project Report
## 2026-08-24 Digest

---

## 1. Ecosystem Overview

The AI inference infrastructure landscape is experiencing intense convergence around three pressure points: **speculative decoding reliability** (MTP/EAGLE/DFlash across all engines), **quantized precision at scale** (NVFP4/MXFP4/FP8 block formats on Blackwell and beyond), and **stateful serving primitives** (KV cache persistence, sleep-mode weight reload, and PD disaggregation). All five projects show heavy investment in these areas, but stability lags behind feature velocity—critical bugs in new silicon paths (SM100/SM103/SM120) and speculative decoding correctness dominate risk profiles. The boundary between training/fine-tuning and serving runtimes continues to blur as Unsloth ships Studio serving features and vLLM deepens quantization-aware fine-tuning integrations.

---

## 2. Activity Comparison

| Project | Issues (Active/Critical) | PRs (Active/Landed 24h) | Release Status | Development Velocity Signal |
|---------|-------------------------|------------------------|----------------|----------------------------|
| **vLLM** | 8 open (2 critical, 3 high) | 5 landed (sleep-mode series), 5+ in progress | No release | Very high—core kernel and scheduling churn |
| **SGLang** | 7 open (2 critical, 3 high) | 7 in review (weight cache, LoRA infra, backends) | No release | Very high—infrastructure for hot-swap serving |
| **llama.cpp** | 10 open (2 critical, 4 high) | 6 landed (builds b10595–b10603), 6+ in progress | **b10603** shipped | High—rapid build cadence, broad backend coverage |
| **Ollama** | 6 open (2 high, 3 medium) | 4 in review (cache persistence, MLX, embed bounding) | No release | Moderate—focused on reliability hardening |
| **LiteLLM** | 5 open (2 high, 2 medium) | 6+ in progress (auth, OTel, Bedrock fixes) | No release | High—133 active PRs on staging branch |
| **Unsloth** | 9 open (4 high, 5 medium) | 4 landed (studiobench, downloads, VRAM), 5+ open | No release | Moderate—Studio-centric, not core library |

*Note: Issue/PR counts reflect 24h digest scope, not total repository state.*

---

## 3. Model Support Race

| Model/Architecture | vLLM | SGLang | llama.cpp | Ollama | Unsloth |
|-------------------|:----:|:------:|:---------:|:------:|:-------:|
| **Qwen3.6** | ✅ Kernel fusion (PR #52676) | ⚠️ MTP weights dropped (#35797) | ✅ SYCL optimizations | ⚠️ Tool-use bugs (#17778) | — |
| **Qwen3.8** | — | — | ✅ CUDA/SYCL | ⚠️ Streaming truncation (#17961) | ⚠️ RAM fallback (#9549) |
| **DeepSeek V4/Flash** | ✅ FP8 compat (PR #53503), ⚠️ GB10/SM120 crashes | — | ✅ Fixed (parallel + speculation) | — | — |
| **GLM-5.x** | ✅ Sparse index reuse (+3.76%, PR #49678) | ✅ DSA attention (MI355/XPU) | ✅ GLM-4.5-Air MTP | — | — |
| **Gemma-4** | ⚠️ Trailing token regression (#49955) | — | ✅ Pillow-accurate resize | ✅ Draft mem fix (#17951) | — |
| **MiniMax H3** | ✅ Sleep-mode fixes (MiniMax layers) | ✅ Diffusion/cube sparse attention | — | — | ✅ Audio models |
| **Mamba-2/SSD/Nemotron-H** | — | ✅ Strict backends (flashinfer_ssd/cake) | ✅ Metal chunked MMA | — | — |
| **Zamba2** | — | — | ✅ Conversion pipeline | — | — |
| **dots3note** | — | — | ✅ Draft-MTP | — | — |

**Leader Assessment:** **vLLM** leads on frontier model kernel integration (Qwen3.6 fusion, DeepSeek V4 fixes, GLM-5.2 optimization), but with highest defect density. **llama.cpp** leads on breadth—new architectures (Zamba2, dots3note, Fun-ASR-Nano) and backend diversity (Metal, SYCL, OpenCL, Vulkan, CUDA). **SGLang** is selective but deep on MoE/attention variants. **Ollama** and **Unsloth** are followers on model support, focusing on packaging and UX rather than native kernel work.

---

## 4. Performance Frontier

| Optimization Domain | Primary Projects | Key Developments | Maturity |
|--------------------|------------------|------------------|----------|
| **KV Cache / Stateful Serving** | vLLM, SGLang, Ollama | Sleep-mode weight reload (vLLM #53344 series, <1s via SGLang weight cache daemon); Ollama competing PRs for cross-reload persistence (#17953, #17278); PD disaggregation timeout tuning (vLLM #53128, SGLang NIXL failure notification #36033) | **Emerging**—foundational for serverless/spot inference, but sleep-mode correctness and PD reliability still flaky |
| **Speculative Decoding** | All except LiteLLM | MTP for GLM-4.5-Air (llama.cpp), Qwen3.6 prefill-lookahead (vLLM), DFlash2/AMD DSA graphs (SGLang), dots3note draft-MTP; **critical bugs pervasive** | **Volatile**—throughput potential proven, production deployment risky across all engines |
| **Quantization Kernels** | vLLM, llama.cpp, SGLang | NVFP4/MXFP4/FP8 block formats on Blackwell (SM100/SM120/SM103); XPU W4A4/W8A8 sequence parallelism (vLLM); CUTLASS paths (llama.cpp); I-quant GGUF gap (SGLang 4–6× slower than llama.cpp #35019) | **Hardware-constrained**—new silicon paths unstable, established formats (Q4_K_M, Q8_0) remain safe harbor |
| **Batching / Scheduling** | vLLM, SGLang | Speculative token budget separation (vLLM #53080); gated launch/deferred allocation (SGLang #35927); embed concurrency bounding (Ollama #17954) | **Maturing**—production-grade for standard paths, speculative and disaggregated paths need tuning |
| **Distributed / EP / TP** | vLLM, SGLang | Elastic EP initialization (vLLM #53483); HiCache all_reduce deadlock (SGLang #30760); heterogeneous-TP zombie subscribers (SGLang #34737) | **Stressed**—expert parallelism scaling reveals collective communication edge cases |

---

## 5. Layer Positioning

| Layer | Projects | Role | Differentiation |
|-------|----------|------|-----------------|
| **Serving Engine (datacenter)** | **vLLM**, **SGLang** | High-throughput, multi-GPU, batched inference for production APIs | vLLM: broadest hardware/model coverage, deepest quantization integration; SGLang: fastest weight loading, hot-swap LoRA infrastructure, structured generation |
| **Local / Edge Runtime** | **llama.cpp**, **Ollama** | Single-node, consumer-GPU, CPU, and mobile inference | llama.cpp: maximum backend diversity (Vulkan, Metal, SYCL, OpenCL, CUDA), single-binary deployment; Ollama: developer UX, model packaging, runner lifecycle management |
| **Gateway / Router** | **LiteLLM** | Unified API across providers, load balancing, budget/observability | Provider abstraction, enterprise auth (SSO, workload identity), spend tracking—not an inference engine itself |
| **Training / Fine-tuning** | **Unsloth** | Parameter-efficient fine-tuning, quantization-aware training | Studio desktop application (Tauri-based), 2–5× faster training claims; serving features secondary |

**Integration Pattern:** Production stacks typically compose LiteLLM → (vLLM|SGLang) for cloud, or Ollama → llama.cpp for local. Unsloth feeds into both paths via exported GGUF/checkpoints.

---

## 6. Trend Signals

| Signal | Evidence | Implication for Agent/Application Developers |
|--------|----------|---------------------------------------------|
| **Speculative decoding is a "production hazard" zone** | Critical open issues in vLLM (#40756 MTP crash, #51039 Kimi-K3 NaN), SGLang (#36010 NVFP4+spec crash), llama.cpp (#27151 zero acceptance, #27282 OOM) | **Disable `num_spec_tokens` / `--speculative-algorithm` for production deployments** until model+backend combinations are explicitly validated. The throughput gains are real but correctness failures are silent or catastrophic. |
| **Blackwell (SM100/SM103/SM120) is not yet production-stable** | vLLM #50720, #52732; SGLang #34340 (B300 kernel failures); llama.cpp #27102, #27329 (RTX Pro 6000/5090 hangs) | **Prefer B200, A100, or Ada (SM89) for Q4 2026 production launches.** NVFP4/MXFP4 paths are explicitly experimental. Budget 2–3 months additional validation for Blackwell-specific deployments. |
| **Stateful serving primitives are the next platform battleground** | Weight cache daemon (<1s load, SGLang), sleep-mode five-part fix (vLLM), KV persistence PRs (Ollama), PD disaggregation benchmarks | **Design for spot/preemptible inference now.** The 300× weight load improvement and cross-reload KV survival will enable serverless LLM pricing models. Monitor which project's semantics (slot-based vs. scheduler-fingerprinted) win. |
| **Intel XPU and AMD ROCm are viable but second-class** | XPU SP+quantized MoE (vLLM), SYCL +42–169% (llama.cpp), AMD DSA graphs (SGLang), but persistent bugs (ROCm DFlash collapse vLLM #53323, AMD NaN SGLang #36071, Strix Halo power-gating Ollama #17958) | **Use for cost optimization, not primary production paths.** Validate exact model+quantization+backend combinations; expect 1.5–2× more debugging than CUDA. |
| **Observability gaps mask production failures** | OTel exception events black-holed (LiteLLM #36863), studiobench wrong numbers withdrawn (Unsloth #9542), false budget errors (LiteLLM #36926) | **Do not trust single-source metrics.** Implement cross-validation between engine-level, gateway-level, and application-level telemetry. Audit benchmark provenance before making capacity decisions. |
| **Agentic workflow reliability lags model capability** | Qwen3.8 tool-use loops crash (Ollama #17778), template/parser edge cases (Ollama), thinking block handling (LiteLLM #38049), whitespace system messages rejected (LiteLLM #28706) | **Add defensive client-side logic:** explicit user message injection in tool loops, retry with backoff on 500/429, validate streaming completion rather than assuming truncation safety. Pin to proven model versions for agent deployments. |

---

*Report generated from 2026-08-24 digest data across six active projects. Recommend re-evaluation on next digest cycle for speculative decoding and Blackwell stability status.*

---

## Per-Project Reports

<details>
<summary><strong>vLLM</strong> — <a href="https://github.com/vllm-project/vllm">vllm-project/vllm</a></summary>

# vLLM Digest — 2026-08-24

## Today's Highlights

The vLLM project is actively stabilizing sleep-mode and weight-reload infrastructure across multiple quantization backends, with Ronald1995 landing a five-part bugfix series for tensor ownership recovery after wake-up. Meanwhile, speculative decoding remains a hot zone: MTP prefill-lookahead chunking gains regression test coverage, and multiple open issues track crashes and silent correctness failures with draft models on both NVIDIA and AMD hardware.

---

## Releases & Breaking Changes

*No releases in the last 24 hours.*

---

## New Model & Hardware Support

| Item | Status | Details |
|------|--------|---------|
| **Qwen3.6 kernel fusion** | In Progress | PR [#52676](https://github.com/vllm-project/vllm/pull/52676) adds fused QK-norm + partial MRoPE + gate extraction for Qwen3.6-35B-A3B, unifying 10 full-attention layer ops. |
| **DeepSeek V4 FP8 weight compatibility** | In Progress | PR [#53503](https://github.com/vllm-project/vllm/pull/53503) fixes `wo_a` weight layer handling for `sgl-project/DeepSeek-V4-Flash-FP8` variant. |
| **XPU: MXFP4 sequence parallelism** | In Progress | PR [#49943](https://github.com/vllm-project/vllm/pull/49943) enables SP for W4A4 MXFP4 MoE (`openai/gpt-oss-120b`) on Intel GPU, forcing SP via `sp_min_token_num=1` for sub-threshold hidden sizes. |
| **XPU: Block FP8 sequence parallelism** | In Progress | PR [#49009](https://github.com/vllm-project/vllm/pull/49009) extends SP to block-quant W8A8 FP8 on XPU (`RedHatAI/Qwen3-32B-FP8-block`). |
| **GLM-5.2 sparse index reuse** | In Progress | PR [#49678](https://github.com/vllm-project/vllm/pull/49678) caches sparse physical indices in attention metadata, claiming **+3.76% throughput**. |

---

## Performance & Optimization

| PR/Issue | Impact | Notes |
|----------|--------|-------|
| [#52676](https://github.com/vllm-project/vllm/pull/52676) | Latency reduction | Fused QK-norm/MRoPE/gate for Qwen3.6 eliminates separate kernel launches. |
| [#49678](https://github.com/vllm-project/vllm/pull/49678) | **+3.76% throughput** | Sparse physical index caching in GLM-5.2 attention metadata avoids repeated `triton_convert_req_index_to_global_index` calls. |
| [#53080](https://github.com/vllm-project/vllm/pull/53080) | Scheduling efficiency | Separates target/draft token budgets in speculative decoding to prevent under-utilization when draft token counts vary. |
| [#53483](https://github.com/vllm-project/vllm/pull/53483) | Elastic EP initialization | Removes warmup collectives from new group initialization, reducing startup latency and ROCm overlap hazards. |

---

## Stability & Regressions

| Severity | Issue | Summary | Fix Status |
|----------|-------|---------|------------|
| **Critical** | [#40756](https://github.com/vllm-project/vllm/issues/40756) | MTP speculative decoding crashes with illegal memory access on long sequences (Qwen3.6-27B-FP8, v0.19.1); 42 comments, active investigation. | **Open** |
| **Critical** | [#51039](https://github.com/vllm-project/vllm/issues/51039) | Kimi-K3 degenerates to repeated token after ~240K prefill (NaN logits, packed KDA prefill suspected); total deployment failure. | **Open** |
| **High** | [#51744](https://github.com/vllm-project/vllm/issues/51744) | `vllm/vllm-openai:latest` fails to start Gemma4 with Transformers 5.15.0 (NVFP4, TP=2); image compatibility broken. | **Open** |
| **High** | [#52897](https://github.com/vllm-project/vllm/issues/52897) | Align-mode prefix caching hits 0/996k queries with `--scheduling-policy priority` on hybrid GDN model; post-#51113 regression. | **Open**; PR [#52244](https://github.com/vllm-project/vllm/pull/52244) in progress |
| **High** | [#50720](https://github.com/vllm-project/vllm/issues/50720) | DeepSeek-V4-Flash-0731 + DSpark fails on RTX PRO 6000 (SM120) with FlashInfer sparse MLA decode kernel routing. | **Open** |
| **High** | [#52732](https://github.com/vllm-project/vllm/issues/52732) | DeepSeekV4 cannot start on GB10 (sm_121): DeepGEMM assumed available, no fallback path. | **Open** |
| **Medium** | [#49955](https://github.com/vllm-project/vllm/issues/49955) | Regression: trailing `<turn\|>` token in Gemma-4-31B-it output after v0.26.0 upgrade. | **Open** |
| **Medium** | [#53323](https://github.com/vllm-project/vllm/issues/53323) | ROCm: DFlash2 acceptance collapses with `ROCM_ATTN` draft backend, recovers with `TRITON_ATTN`. | **Open** |
| **Medium** | [#52735](https://github.com/vllm-project/vllm/issues/52735) | XPU: OffloadingConnector stores but never serves with MTP/EAGLE speculative decoding enabled. | **Open** |
| **Medium** | [#51581](https://github.com/vllm-project/vllm/issues/51581) | DFlash fused-KV projection calls `F.linear` on sliced `qkv_proj` weight, breaks quantized drafters silently. | **Open** |
| **Medium** | [#52404](https://github.com/vllm-project/vllm/issues/52404) | **Closed** — B200 DP4+EP/TP4+EP: DeepSeek V4 Flash 0731 outputs garbled characters under heavy concurrent load. | **Closed** (resolution unclear from data) |

**Sleep-mode / reload stability** (landed fixes):
- PR series [#53344](https://github.com/vllm-project/vllm/pull/53344), [#53507](https://github.com/vllm-project/vllm/pull/53507)-[#53511](https://github.com/vllm-project/vllm/pull/53511) fixes tensor ownership, Lamport workspace reset, lock recovery, derived buffer restoration, and KV allocation isolation for sleep-mode wake-up.

---

## What This Means for Application Developers

1. **Avoid v0.26.0 for Gemma-4 deployments** — the trailing `<turn|>` token regression ([#49955](https://github.com/vllm-project/vllm/issues/49955)) will break chat formatting; pin to v0.25.1 or await patch.

2. **Speculative decoding is risky on latest models** — MTP with Qwen3.6, Kimi-K3, and DeepSeek V4 variants has multiple open crash/correctness issues. Production deployments should disable `num_spec_tokens` or validate extensively against exact model/checkpoint combinations.

3. **Sleep-mode users should upgrade once #53343 series lands** — the five-part fix resolves weight-reload corruption in MiniMax, Humming, and MoE layers. If using level-1/level-2 sleep with quantization, monitor these PRs.

4. **Intel XPU path is maturing for SP+quantized MoE** — MXFP4 and block-FP8 sequence parallelism PRs are active, but still require `torch.compile` and manual SP enabling; not yet zero-config.

5. **PD disaggregation needs tuning** — [#53128](https://github.com/vllm-project/vllm/issues/53128) shows parked KV discarded after 60s, causing recompute for queued consumers. Adjust `TieringOffloadingSpec` timeouts or queue depths if running prefill-decode separation.

</details>

<details>
<summary><strong>SGLang</strong> — <a href="https://github.com/sgl-project/sglang">sgl-project/sglang</a></summary>

# SGLang Digest — 2026-08-24

## 1. Today's Highlights

The **Fast Engine Recovery: Weight Cache Daemon** landed Phase 1, cutting Qwen3-235B FP8 weight load from ~306–327s to **<1s** via per-rank CUDA IPC daemons ([sgl-project/sglang#33522](https://github.com/sgl-project/sglang/issues/33522)). A cluster of **fzyzcjy** PRs ([sgl-project/sglang#35923](https://github.com/sgl-project/sglang/pull/35923)–[#35929](https://github.com/sgl-project/sglang/pull/35929)) refactors scheduler internals for weight-version tracking and gated launch, signaling deeper infrastructure for **hot-swappable LoRA/model weights** in production. Meanwhile, **B300 (sm_103) Blackwell compatibility issues** emerged with two SM10x-gated kernels failing on the new silicon ([sgl-project/sglang#34340](https://github.com/sgl-project/sglang/issues/34340)).

---

## 2. Releases & Breaking Changes

**None today.** No releases in the last 24h.

---

## 3. New Model & Hardware Support

| Item | Status | Details |
|------|--------|---------|
| **MiniMax H3 diffusion** | In Review ([sgl-project/sglang#34893](https://github.com/sgl-project/sglang/pull/34893)) | FlexAttention-based **cube sparse attention** for 3D visual streams; dense attention fallback for text/audio/ref images |
| **Intel XPU: GLM5.1 DSA Attention** | In Review ([sgl-project/sglang#24959](https://github.com/sgl-project/sglang/pull/24959)) | Enables `GlmMoeDsaForCausalLM` on Intel GPUs with JIT kernel support |
| **AMD HIP DSA draft_extend CUDA graph** | In Review ([sgl-project/sglang#35672](https://github.com/sgl-project/sglang/pull/35672)) | GLM MI355 MTP can now capture `draft_extend` graph instead of eager launches |
| **Mamba SSD backends** | In Review ([sgl-project/sglang#35444](https://github.com/sgl-project/sglang/pull/35444)) | Strict `flashinfer_ssd` and `cake` prefill backends for Nemotron-H (C256→C128 metadata projection) |
| **LoRA MoE execution engine** | In Review ([sgl-project/sglang#34803](https://github.com/sgl-project/sglang/pull/34803)) | `sgl_lora` MoE engine with JSON config resolution; replaces Blackwell-only TRT-LLM path |

---

## 4. Performance & Optimization

| Item | Metric | Status |
|------|--------|--------|
| **Weight Cache Daemon** | **306–327s → <1s** weight load for Qwen3-235B FP8 | Landed Phase 1 ([sgl-project/sglang#33522](https://github.com/sgl-project/sglang/issues/33522)) |
| **Gated launch / deferred memory allocation** | Reduces startup-time OOM pressure | In Review ([sgl-project/sglang#35927](https://github.com/sgl-project/sglang/pull/35927)) |
| **I-quant GGUF MMQ kernel gap** | **4–6x slower prefill than llama.cpp** | Open issue, no assignee ([sgl-project/sglang#35019](https://github.com/sgl-project/sglang/issues/35019)) |
| **Benchmark steady-state window** | Isolates ramp-up/drain in PD disaggregated benchmarks | In Review ([sgl-project/sglang#30918](https://github.com/sgl-project/sglang/pull/30918)) |
| **KV events: per-component placement tracking** | Enables hybrid-model (attention + sliding window + mamba) aware routing | In Review ([sgl-project/sglang#32514](https://github.com/sgl-project/sglang/pull/32514)) |

---

## 5. Stability & Regressions

| Severity | Issue | Details | Fix Status |
|----------|-------|---------|------------|
| **🔴 Critical** | **B300 kernel failures** ([sgl-project/sglang#34340](https://github.com/sgl-project/sglang/issues/34340)) | `is_sm100_supported()` family check incorrectly routes sm_103 to SM100 paths; **cutedsl TGV BF16 GEMM** raises Xid 13 CGA, **trtllm-gen MoE finalize** hangs silently | No fix PR; diagnostic via CUDA coredump |
| **🔴 Critical** | **NVFP4 KV + speculative decoding crash** ([sgl-project/sglang#36010](https://github.com/sgl-project/sglang/issues/36010)) | `extend_prefix_lens_cpu` is None in FlashInfer dequant workspace; SM120/RTX 5090 with `--speculative-algorithm NEXTN` | No fix PR |
| **🟠 High** | **HiCache all_reduce deadlock** ([sgl-project/sglang#30760](https://github.com/sgl-project/sglang/issues/30760)) | `check_prefetch_progress` mismatched call count with TP=4, no PP | No fix PR |
| **🟠 High** | **AMD DSA dense graph all-NaN logits** ([sgl-project/sglang#36071](https://github.com/sgl-project/sglang/issues/36071)) | Post-#31324, HIP DSA dense (k-only) decode graph produces NaN with DP attention on GLM-5.2 | No fix PR |
| **🟠 High** | **Qwen3.5 MTP weights dropped on compressed-tensors** ([sgl-project/sglang#35797](https://github.com/sgl-project/sglang/issues/35797)) | Accept length pinned at 1.0; silent failure | No fix PR |
| **🟡 Medium** | **Stale staging watermark subscribers** ([sgl-project/sglang#34737](https://github.com/sgl-project/sglang/issues/34737)) | Heterogeneous-TP disaggregated prefill failure leaves zombie subscribers | No fix PR |
| **🟡 Medium** | **NPU OOM in deep_ep dispatch** ([sgl-project/sglang#27351](https://github.com/sgl-project/sglang/issues/27351)) | Attempted 5503 GiB allocation; closed as inactive but unaddressed | Closed inactive |
| **🟢 Low** | **PP=0 + HiCache memory leak** ([sgl-project/sglang#28902](https://github.com/sgl-project/sglang/issues/28902)) | Closed inactive; fix status unclear | Closed inactive |

**CI Health:** 3 broken, 11 flaky, 672 recently fixed per auto-update ([sgl-project/sglang#17050](https://github.com/sgl-project/sglang/issues/17050)).

---

## 6. What This Means for Application Developers

- **Production LoRA serving:** The weight-version tracking PRs ([sgl-project/sglang#35925](https://github.com/sgl-project/sglang/pull/35925), [#35926](https://github.com/sgl-project/sglang/pull/35926)) and gated launch ([sgl-project/sglang#35927](https://github.com/sgl-project/sglang/pull/35927)) are foundational for **zero-downtime model/LoRA hot-swapping**—expect this to land in v0.6.x. Plan architecture for dynamic adapter loading.
- **Blackwell/B300 caution:** sm_103 support is incomplete. Avoid SM100-gated kernels (NVFP4 KV, certain MoE paths) on B300 until [sgl-project/sglang#34340](https://github.com/sgl-project/sglang/issues/34340) is resolved. Use B200/A100 for stable production.
- **Speculative decoding + quantization:** Do not combine `--kv-cache-dtype nvfp4` with any speculative algorithm today ([sgl-project/sglang#36010](https://github.com/sgl-project/sglang/issues/36010)).
- **PD disaggregation:** NIXL backend lacks failure notification from prefill→decode ([sgl-project/sglang#36033](https://github.com/sgl-project/sglang/issues/36033)); prefer Mooncake or mori for reliability, or implement client-side timeout fallback.
- **GGUF I-quant models:** Performance is severely degraded vs. llama.cpp ([sgl-project/sglang#35019](https://github.com/sgl-project/sglang/issues/35019)); use K-quant or native checkpoints for SGLang serving.

</details>

<details>
<summary><strong>llama.cpp</strong> — <a href="https://github.com/ggml-org/llama.cpp">ggml-org/llama.cpp</a></summary>

# llama.cpp Digest — 2026-08-24

## Today's Highlights

The project landed **GLM-4.5-Air MTP (Multi-Token Prediction) support** in [b10603](https://github.com/ggml-org/llama.cpp/releases/tag/b10603), expanding speculative decoding capabilities to another major model family. The **mtmd multimodal pipeline** received critical fixes for video input handling, including proper MP4/MOOV atom placement and Windows deadlock resolution. Meanwhile, **OpenCL backend work accelerated dramatically** with 6 PRs targeting Adreno GPU matmul paths, speculative decoding kernels, and MoE routing optimizations.

---

## Releases & Breaking Changes

| Build | Change | Impact |
|-------|--------|--------|
| [b10603](https://github.com/ggml-org/llama.cpp/releases/tag/b10603) | GLM-4.5-Air MTP support ([#26534](https://github.com/ggml-org/llama.cpp/pull/26534)) | New speculative decoding target; check MTP layer compatibility |
| [b10599](https://github.com/ggml-org/llama.cpp/releases/tag/b10599) | Test infrastructure: `tools/parser` moved to `tests/` ([#27548](https://github.com/ggml-org/llama.cpp/pull/27548)) | May break custom build scripts referencing old path |
| [b10598](https://github.com/ggml-org/llama.cpp/releases/tag/b10598) | mtmd: Pillow-accurate resize algorithm, corrected `resize_algo` for all vision models ([#27594](https://github.com/ggml-org/llama.cpp/pull/27594)) | **Behavior change**: image preprocessing now matches reference implementation; review vision model outputs |
| [b10595](https://github.com/ggml-org/llama.cpp/releases/tag/b10595) | Server: `LLAMA_SERVER_SLOTS_N_DIFF` env var added ([#27600](https://github.com/ggml-org/llama.cpp/pull/27600)) | New tuning knob for slot divergence handling |

---

## New Model & Hardware Support

| Item | Details | PR/Issue |
|------|---------|----------|
| **GLM-4.5-Air** | Full MTP (Multi-Token Prediction) support for speculative decoding | [#26534](https://github.com/ggml-org/llama.cpp/pull/26534) |
| **Zamba2** | New architecture support in conversion pipeline | [#21412](https://github.com/ggml-org/llama.cpp/pull/21412) |
| **dots3note** | draft-mtp speculative decoding support (text-only, vision TODO) | [#27618](https://github.com/ggml-org/llama.cpp/pull/27618) |
| **Fun-ASR-Nano** | New single-binary ASR CLI (SAN-M encoder + Qwen3-0.6B), CPU + CUDA | [#27607](https://github.com/ggml-org/llama.cpp/pull/27607) |
| **CUDA SM120/121** | Experimental CUTLASS MoE prefill for MXFP4/NVFP4 (Blackwell) | [#26704](https://github.com/ggml-org/llama.cpp/pull/26704) |
| **Metal (Apple)** | Chunked SSD MMA for Mamba-2 prefill optimization | [#26647](https://github.com/ggml-org/llama.cpp/pull/26647) |

---

## Performance & Optimization

| Area | Change | Metrics | PR |
|------|--------|---------|-----|
| **SYCL (Intel BMG)** | TILE kernel gating for quantized KV decode (q4_0/q8_0) | **+42% to +169%** on Qwen3.6-35B, Gemma 4 26B/12B at 32K/118K context; zero regressions | [#26689](https://github.com/ggml-org/llama.cpp/pull/26689) |
| **OpenCL/Adreno** | Better matmul path selection (X2E, Y2E GPUs) | Eliminates slowest GEMM fallback; F16xF32 now properly selected | [#27640](https://github.com/ggml-org/llama.cpp/pull/27640) |
| **OpenCL speculative decoding** | Right-sized kernels for verification step | Targets skinny GEMM bottleneck in MTP/DFlash | [#27635](https://github.com/ggml-org/llama.cpp/pull/27635) |
| **OpenCL MoE** | Batch-size-aware expert matmul selection | Critical for small-batch speculative decode vs. large prefill | [#27637](https://github.com/ggml-org/llama.cpp/pull/27637) |
| **CUDA (Blackwell)** | CUTLASS W4A4 for NVFP4/MXFP4 prefill; MMVQ decode with repacked layout | Optional path, falls back to existing CUDA | [#26704](https://github.com/ggml-org/llama.cpp/pull/26704) |
| **Server** | `LLAMA_SERVER_SLOTS_N_DIFF` | Allows tuning slot divergence threshold for multi-turn conversations | [#27600](https://github.com/ggml-org/llama.cpp/pull/27600) |

---

## Stability & Regressions

| Severity | Issue | Status | Fix PR |
|----------|-------|--------|--------|
| 🔴 **Critical** | **CUDA kernel stall → watchdog kill** on RTX Pro 6000 Blackwell (Qwen3.8-27B UD-Q8_K_XL) | Open ([#27102](https://github.com/ggml-org/llama.cpp/issues/27102)) | None |
| 🔴 **Critical** | **Qwen3.8-27B-NVFP4 decode hang** on RTX 5090D (sm_100): CPU spin, zero GPU work | Open ([#27329](https://github.com/ggml-org/llama.cpp/issues/27329)) | None |
| 🟡 **High** | **HIP/ROCm corrupted output on gfx1151** (Strix Halo/RDNA3.5): Vulkan correct with identical flags | Open ([#27579](https://github.com/ggml-org/llama.cpp/issues/27579)) | None |
| 🟡 **High** | **MTP draft acceptance 1/633** on FreeBSD (near-zero speculation efficiency) | Open ([#27151](https://github.com/ggml-org/llama.cpp/issues/27151)) | None |
| 🟡 **High** | **MTP + Vulkan + `--no-mmap`**: `GGML_ASSERT(addr)` fail on Qwen 3.6 27B q8 | Closed ([#24280](https://github.com/ggml-org/llama.cpp/issues/24280)) | Fixed in recent build |
| 🟡 **High** | **DeepSeekV4 garbled output** with parallel processing + speculation | Closed ([#26741](https://github.com/ggml-org/llama.cpp/issues/26741)) | Fixed |
| 🟡 **High** | **Native MTP OOM** — separate CUDA compute arena; shared `gallocr` resolves | Open ([#27282](https://github.com/ggml-org/llama.cpp/issues/27282)) | Community workaround identified |
| 🟢 **Medium** | **mtmd video hangs on Windows** (probe deadlock, 0-frame decode) | Open ([#24429](https://github.com/ggml-org/llama.cpp/issues/24429)) | [#27500](https://github.com/ggml-org/llama.cpp/pull/27500) merged, [#27596](https://github.com/ggml-org/llama.cpp/pull/27596) pending |
| 🟢 **Medium** | **llama-server.exe won't start** in b10587 (Windows Vulkan, Qwen3.8-27B) | Open ([#27576](https://github.com/ggml-org/llama.cpp/issues/27576)) | None |
| 🟢 **Medium** | **Vulkan Mali-G925**: all-NaN logits on Qwen3.5-0.8B multimodal prefill | Open ([#26921](https://github.com/ggml-org/llama.cpp/issues/26921)) | None |
| 🟢 **Medium** | **SYCL CoopMat2 shader build failure** on Polaris RX 580 | Open ([#25985](https://github.com/ggml-org/llama.cpp/issues/25985)) | None |
| 🟢 **Medium** | **SYCL segfault** with `GGML_SYCL_DEVICE_ARCH=xe2` | Open ([#25808](https://github.com/ggml-org/llama.cpp/issues/25808)) | None |

---

## What This Means for Application Developers

1. **Speculative decoding is expanding rapidly** — MTP support for GLM-4.5-Air and dots3note, plus DFlash2 in development, means more models can achieve 2-3x token throughput. However, **MTP remains fragile**: acceptance rates near zero ([#27151](https://github.com/ggml-org/llama.cpp/issues/27151)), OOM with separate CUDA arenas ([#27282](https://github.com/ggml-org/llama.cpp/issues/27282)), and Vulkan-specific crashes ([#24280](https://github.com/ggml-org/llama.cpp/issues/24280)) suggest production deployments need careful A/B testing per backend.

2. **Blackwell (SM100/SM120) users should tread carefully** — The RTX 50-series/Pro 6000 has multiple open critical bugs: NVFP4 decode hangs ([#27329](https://github.com/ggml-org/llama.cpp/issues/27329)), generic CUDA stalls ([#27102](https://github.com/ggml-org/llama.cpp/issues/27102)), and the CUTLASS path is explicitly experimental. Consider staying on Q4_K_M or Q8_0 rather than NVFP4 until stability improves.

3. **Multimodal video pipelines are stabilizing** — The MOOV-atom and Windows deadlock fixes ([#27596](https://github.com/ggml-org/llama.cpp/pull/27596), [#27500](https://github.com/ggml-org/llama.cpp/pull/27500)) resolve silent failures that plagued mtmd video input. If you built workarounds (e.g., ffmpeg pre-processing), you can likely remove them after [b10598](https://github.com/ggml-org/llama.cpp/releases/tag/b10598)+.

4. **OpenCL/Adreno is becoming viable for edge deployment** — The concentration of Adreno-specific optimizations (matmul routing, speculative decode kernels, image kernel safety) suggests this backend is maturing for mobile/embedded use. The Fun-ASR-Nano port ([#27607](https://github.com/ggml-org/llama.cpp/pull/27607)) further validates single-binary edge ASR.

5. **Server operators**: The new `LLAMA_SERVER_SLOTS_N_DIFF` env var ([#27600](https://github.com/ggml-org/llama.cpp/pull/27600)) and checkpoint invalidation fix ([#24891](https://github.com/ggml-org/llama.cpp/pull/24891)) address long-standing multi-turn conversation bugs. If you saw degraded coherence after tool calls or excessive slot divergence, upgrade and tune this parameter.

</details>

<details>
<summary><strong>Ollama</strong> — <a href="https://github.com/ollama/ollama">ollama/ollama</a></summary>

# Ollama Digest — 2026-08-24

## Today's Highlights

The Ollama team is actively landing infrastructure hardening PRs around runner lifecycle management and KV cache persistence, with two competing experimental approaches for prefill cache survival across model reloads ([#17953](https://github.com/ollama/ollama/pull/17953), [#17278](https://github.com/ollama/ollama/pull/17278)). Meanwhile, Qwen3.8 continues to surface integration pain points—particularly around tool-use loops triggering "no user query found" errors ([#17778](https://github.com/ollama/ollama/issues/17778)) and incomplete streaming responses in Claude Code setups ([#17961](https://github.com/ollama/ollama/issues/17961)). No new release was cut in the last 24h.

---

## Releases & Breaking Changes

**None today.**

---

## New Model & Hardware Support

| Item | Status | Details |
|------|--------|---------|
| **MLX version bump** | In review | [#17955](https://github.com/ollama/ollama/pull/17955) bumps MLX to `d9077d83`; [#17956](https://github.com/ollama/ollama/pull/17956) deduplicates CUDA runtime payloads for Windows/Linux MLX builds by moving shared libs into the `cuda_v13` payload rather than duplicating under `mlx_cuda_v13`. Reduces installer bloat. |
| **AMD Strix Halo (gfx1151) power-gating** | Reported | [#17958](https://github.com/ollama/ollama/issues/17958): ROCm backend fails to power-gate GPU when ≥2 models are resident, pinning ~25W idle. Single-model residency idles correctly. Root cause suspected in runner process refcounting or ROCm context retention. |

---

## Performance & Optimization

| Item | Status | Details |
|------|--------|---------|
| **Prefill cache persistence (experimental)** | Two competing PRs | [#17953](https://github.com/ollama/ollama/pull/17953) by CHIPMUNK-T0T: opt-in KV cache persistence across runner reloads via slot save/restore in llama-server; [#17278](https://github.com/ollama/ollama/pull/17278) by rohanpatel2002: scheduler-owned save/restore with fingerprinting, 8 GiB LRU cap, fail-open, plus MLX prefix-cache frontier persistence. Both address the same core problem: keep-alive expiry or model switching currently forces full prefill recomputation. |
| **Embed concurrency bounding** | In review | [#17954](https://github.com/ollama/ollama/pull/17954) bounds `/api/embed` fan-out to prevent large batches from opening thousands of goroutines and exhausting runner pools. |
| **Draft model memory measurement** | Fixed | [#17951](https://github.com/ollama/ollama/issues/17951) closed: Gemma 4 MTP draft models were incorrectly reported as ~315 MB in `/api/ps` vs. actual ~4.4 GB; fix landed in associated PR. |

---

## Stability & Regressions

| Severity | Issue | Fix PR |
|----------|-------|--------|
| **High** | [#17778](https://github.com/ollama/ollama/issues/17778): Qwen3.8 tool-use loops hit `ResponseError: no user query found in messages (500)` during streaming. 17 comments, 8 👍. Appears to be template/parser interaction when model emits tool calls without interleaving user turns. | None identified |
| **High** | [#17484](https://github.com/ollama/ollama/issues/17484): `context deadline exceeded` on HF-imported models, particularly Qwen3.6 variants. Suggests timeout mismatch between Ollama's scheduler and llama-server startup for large GGUFs. | None |
| **Medium** | [#17957](https://github.com/ollama/ollama/issues/17957): `ornith-1.5:35b` fails with `failed to parse grammar` when **both** `response_format` and `tools` are specified. Grammar composition bug in sampler initialization; works for `qwen3.6:35b`. | None |
| **Medium** | [#17946](https://github.com/ollama/ollama/issues/17946): Sharded GGUF import fails only *after* blobs are copied to storage, wasting disk I/O and time. | [#17949](https://github.com/ollama/ollama/pull/17949) validates shard filenames/metadata before upload |
| **Medium** | [#17961](https://github.com/ollama/ollama/issues/17961): Qwen3.8:27B produces incomplete responses (~20-30s then truncation) in Claude Code 2.1.241 + Ollama 0.32.15. Context deadline or client-side timeout suspected. | None |
| **Low** | [#17939](https://github.com/ollama/ollama/issues/17939): HF-imported `Muse-Glimmer-30B` gets incorrect `stop` parameters, truncating output to ~3 tokens. Metadata derivation bug from GGUF tokenizer config. | None |
| **Low** | [#17614](https://github.com/ollama/ollama/issues/17614) **CLOSED**: q4_0 KV quantization caused garbage output; resolved. | Closed |
| **Low** | [#17937](https://github.com/ollama/ollama/issues/17937) **CLOSED**: Qwen3 parser failed to split `<think>` blocks for HF-imported RuadaptQwen3-8B-Hybrid. | [#17945](https://github.com/ollama/ollama/pull/17945) adds explicit thinking support, disabled by default |

---

## What This Means for Application Developers

1. **Qwen3.8 is not yet production-stable for agentic workflows.** If you're building tool-calling agents, the "no user query found" crash ([#17778](https://github.com/ollama/ollama/issues/17778)) and streaming truncation ([#17961](https://github.com/ollama/ollama/issues/17961)) suggest template/parser edge cases in multi-turn tool loops. Consider pinning to Qwen3.6 or adding client-side retry logic with explicit user message injection.

2. **Monitor the prefill cache PRs closely.** If your workload involves frequent model switching or short keep-alives, either [#17953](https://github.com/ollama/ollama/pull/17953) or [#17278](https://github.com/ollama/ollama/pull/17278) could dramatically reduce TTFT (time-to-first-token) latency. The scheduler-fingerprinting approach in #17278 appears more comprehensive but is also more complex; evaluate against your reliability requirements.

3. **Validate sharded GGUFs before import.** Until [#17949](https://github.com/ollama/ollama/pull/17949) lands, large sharded uploads can silently fail after expensive blob copies. Pre-validate shard naming (`*-00001-of-00002.gguf` convention) and metadata locally.

4. **HF-imported models need stop-token auditing.** The Muse-Glimmer case ([#17939](https://github.com/ollama/ollama/issues/17939)) shows Ollama's automatic `stop` derivation from GGUF tokenizer metadata can be wrong. For new HF imports, verify output length behavior and be prepared to override `stop` in your Modelfile.

</details>

<details>
<summary><strong>LiteLLM</strong> — <a href="https://github.com/BerriAI/litellm">BerriAI/litellm</a></summary>

# LiteLLM Digest — 2026-08-24

## Today's Highlights

LiteLLM's stability sprint continues with a flurry of targeted fixes across authentication, streaming, and provider-specific edge cases. Anthropic and Bedrock integrations see particular attention, with PRs landing for workload identity federation, thinking block handling, and rerank document text back-filling. No new release was cut in the last 24h, but 133 active PRs indicate heavy development velocity on `litellm_internal_staging`.

---

## Releases & Breaking Changes

**None** — No releases published in the last 24h.

---

## New Model & Hardware Support

| Item | Status | Details |
|------|--------|---------|
| **Bee by HEOSSI provider** | PR #38042 | New `openai_like` provider with 6 model cost entries. OpenAI-compatible API at `https://api.bee.heossi.com/bee`, auth via `BEE_API_KEY`. |
| **Anthropic Workload Identity Federation** | PR #38013 | OIDC JWT-bearer token exchange for Anthropic-direct auth, eliminating long-lived `sk-ant` keys. Pluggable identity sources with provider-level setup. Addresses #28607. |

---

## Performance & Optimization

| Item | Impact | Details |
|------|--------|---------|
| **Shared health database writes** | PR #38058 | Eliminates redundant PostgreSQL health-history reads by cache consumers; coordinates persistence so slow model writes don't escape their tasks. |
| **Import speed** | Issue #7605 (closed) | 1-second import time remains a known pain point; issue closed as stale after 42 👍 and extensive discussion. No active PR referenced. |
| **OTel metric export interval** | PR #38055 | Removes hardcoded 5-second export interval; honors `OTEL_METRIC_EXPORT_INTERVAL` env var, reducing unnecessary re-exports of cumulative series. |

---

## Stability & Regressions

| Severity | Item | Fix Status |
|----------|------|------------|
| 🔴 **High** | **OOM Killed pods — continuous memory increase** (#25219, closed) | Closed without clear resolution; affects `v1.82.0-stable`. Operators should monitor and report if reproducible on newer versions. |
| 🔴 **High** | **False BudgetExceededError under sustained load** (#36926) | **Open**, no PR. Self-heals in ~2 min; `Current cost` calculation incorrectly adds `max_budget + recent_spend`. No Redis in reported environment. |
| 🔴 **High** | **Redis cache fails with `ssl_check_hostname` TypeError** (#34614) | **Open**, no PR. Regression in `v1.93.0`; blocks caching and budget counters entirely. |
| 🟡 **Medium** | **OTel v2 GenAI exception events never exported** (#36863) | **PR #38041 open** — `Event(body=None)` dropped by OTLP encoder. |
| 🟡 **Medium** | **Bedrock rerank ignores `return_documents`** (#38006) | **PR #38044 open** — document text never back-filled in response. |
| 🟡 **Medium** | **Anthropic streaming KeyError: 'text'** (#28067, closed) | Fixed in prior release; upstream omitting optional `text` field on `content_block_start`. |
| 🟡 **Medium** | **Whitespace-only system messages rejected by Anthropic** (#28706) | **Open**, no PR. Should be dropped before passthrough. |
| 🟢 **Low** | **SSO role name inconsistency** (`internal_user_view_only` vs `internal_user_viewer`) (#33690) | **Open**, docs/code drift. |
| 🟢 **Low** | **Token counter crashes on `image_url: None`** (#28119, closed) | Fixed. |
| 🟢 **Low** | **Timeout silently dropped on `responses()` completion path** (#28132, closed) | Fixed for Anthropic, Bedrock, Vertex. |

---

## What This Means for Application Developers

**If you're running LiteLLM Proxy in production:**

- **Upgrade caution for v1.93.0**: The Redis `ssl_check_hostname` regression (#34614) breaks caching and budget enforcement. Pin to v1.92.x or earlier until fixed, or disable Redis caching.
- **Anthropic users**: The workload identity federation PR (#38013) will enable keyless auth patterns matching Azure/Vertex. If you're managing `sk-ant` keys in vaults today, plan migration.
- **Bedrock rerank users**: `return_documents=True` currently returns indices without text. Apply PR #38044 patch or wait for next release if your downstream logic depends on document content.
- **Budget-sensitive workloads**: The false `BudgetExceededError` (#36926) under sustained load means you may see 429s that self-resolve. Consider Redis-backed spend tracking as a mitigation, or implement client-side retry with backoff.
- **Observability**: OTel v2 users should watch for #36863/#38041 — exception events are currently black-holed, masking error signals in your tracing backend.

**If you're building agents with the SDK:**

- Anthropic `thinking` blocks with empty text now get properly dropped (#38049), preventing 400s on valid signatures.
- Assistant tool-call turns preserve `content: null` (#38048), fixing potential round-trip issues with strict schema validators.
- The `responses()` timeout fix (#28132) ensures non-streaming timeout parameters actually propagate for Anthropic/Bedrock/Vertex.

---

*Generated from BerriAI/litellm activity on 2026-08-24. All links: `https://github.com/BerriAI/litellm/issues/` or `/pull/`*

</details>

<details>
<summary><strong>Unsloth</strong> — <a href="https://github.com/unslothai/unsloth">unslothai/unsloth</a></summary>

# Unsloth Digest — 2026-08-24

## Today's Highlights

The Studio desktop application dominates today's activity with multiple critical fixes in flight: a [readiness gate fix for studiobench](https://github.com/unslothai/unsloth/pull/9439) unblocks the 100K+ context slowdown measurement that had been scoring UNSCORED, while [resumable HuggingFace downloads](https://github.com/unslothai/unsloth/pull/9602) finally address the 800MB re-download pain point for interrupted GGUF transfers. Meanwhile, [four measurement instrument defects](https://github.com/unslothai/unsloth/pull/9542) that had already produced acted-upon wrong numbers are being withdrawn and corrected.

---

## Releases & Breaking Changes

**None today.** No releases in the last 24h.

---

## New Model & Hardware Support

| Item | Status | Details |
|------|--------|---------|
| [Higgs, MOSS, and MiniMax audio models](https://github.com/unslothai/unsloth/pull/8794) | OPEN | Native Studio Audio support for `bosonai/higgs-tts-2-3b-base`, `OpenMOSS-Team/MOSS-TTS-Local-Transformer-v1.5`, `OpenMOSS-Team/MOSS-TTS-Nano-100M`, `multimodalart/higgs-audio-v3-tts-4b-transformers`, `MiniMaxAI/MiniMax-Music3` |
| [AMD APU VRAM detection on Windows](https://github.com/unslothai/unsloth/pull/9314) | OPEN | Fixes 128 GiB Strix Halo being budgeted as ~8 GiB by reading driver total instead of carve-out |
| [AMD Docker image request](https://github.com/unslothai/unsloth/issues/9581) | OPEN | Community request for official AMD ROCm container, currently only bare-metal install documented |
| [MultiGPU Training in Studio](https://github.com/unslothai/unsloth/issues/9576) | OPEN | Feature request for automatic model splitting across multiple GPUs (e.g., dual RTX 3060s) |

---

## Performance & Optimization

| PR/Issue | Impact | Notes |
|----------|--------|-------|
| [#9439: studiobench readiness gate with windowed threads](https://github.com/unslothai/unsloth/pull/9439) | **Critical** — unblocks 100K+ context measurement | Virtualized threads never mount every message by design; previous gate timed out at 9/18 messages in 180s, causing UNSCORED results for the exact slowdown users report |
| [#9542: studiobench instrument defect fixes](https://github.com/unslothai/unsloth/pull/9542) | **High** — corrects four published wrong numbers | Nine defects catalogued; four had already produced actionable numbers that are now withdrawn |
| [#9602: Resumable HF Hub downloads](https://github.com/unslothai/unsloth/pull/9602) | **High UX** — saves 800MB re-downloads | Restores `huggingface_hub` 1.18+ HTTP partial resume; cancels/no longer restart entire GGUF from byte 0 |
| [#9362: Driver-level free VRAM reporting](https://github.com/unslothai/unsloth/pull/9362) | **Correctness** | `get_gpu_memory_info` previously used `total - torch.allocated`, missing reserved cache and other processes; now uses system-wide driver telemetry |
| [#9596: Kaggle T4 GPU payload sharing](https://github.com/unslothai/unsloth/pull/9596) | **Latency** | Lets Studio GPU half start before training leg fully releases card, vs. current drain-wait |

---

## Stability & Regressions

| Severity | Item | Fix Status |
|----------|------|------------|
| **Critical — Fixed** | [#9008: Studio server deadlocks, all threads blocked in `sqlite3.connect()/close()`](https://github.com/unslothai/unsloth/issues/9008) | **CLOSED** — Process stays alive, socket open, accepts no connections; full hang after minutes of normal operation |
| **High — Fixed** | [#9585: Tests write shared state causing CI flakes](https://github.com/unslothai/unsloth/issues/9585) | **CLOSED** — Five channels of shared state; one already caused day-long flake investigation |
| **High — Open** | [#9518: Chat window IndexedDB persist fails on every message](https://github.com/unslothai/unsloth/issues/9518) | OPEN — Backend healthy, Tauri/WebKitGTK desktop build 0.1.800-beta; repros across all models/providers |
| **High — Open** | [#9480: Model Hub crashes WebKitWebProcess (SIGABRT), Skia COLRv1 font assert](https://github.com/unslothai/unsloth/issues/9480) | OPEN — Fedora 44 Wayland; distinct from #9393 |
| **High — Open** | [#9551: AMD RDNA3 W7900 VRAM detection incorrect post-management patch](https://github.com/unslothai/unsloth/issues/9551) | OPEN — 45 GiB card misdetected; regression from recent VRAM management changes |
| **Medium — Open** | [#9549: Models loading into RAM despite VRAM-only checks](https://github.com/unslothai/unsloth/issues/9549) | OPEN — Qwen3.8 on AMD W7900/W7500; ROCm 7.13.99004 |
| **Medium — Open** | [#9550: MTP dropped to fit context, but loads take extremely long](https://github.com/unslothai/unsloth/issues/9550) | OPEN — Choosing MTP keeps same context window, contradicting UI implication |
| **Medium — Open** | [#9543: Linux voice dictation records 0 bytes](https://github.com/unslothai/unsloth/issues/9543) | OPEN — WebKitGTK MediaRecorder produces empty audio |
| **Medium — Open** | [#9588: Special characters in Windows user home break Desktop](https://github.com/unslothai/unsloth/issues/9588) | OPEN — Path handling bug with non-ASCII home directories |
| **Medium — Open** | [#9600: IDB database file establishment failure](https://github.com/unslothai/unsloth/issues/9600) | OPEN — UnknownError on IndexedDB init |
| **Low — Open** | [#9522: API bound to 127.0.0.1, no LAN/WSL access](https://github.com/unslothai/unsloth/issues/9522) | OPEN — Feature request for configurable bind address |

**CI/Infrastructure fixes:**
- [#9586: Four additional shared-state test channels](https://github.com/unslothai/unsloth/issues/9586) — OPEN, follow-up to #9585
- [#9601: Import lint misses quoted forward references](https://github.com/unslothai/unsloth/pull/9601) — CLOSED, CI safety net fix

---

## What This Means for Application Developers

1. **Studio reliability is the current focus, not the core training library.** The majority of issues and PRs target the Tauri-based desktop application (IndexedDB, WebKitGTK, WebKitWebProcess crashes, sandbox paths). If you're using Unsloth via Python package/CLI/Colab, today's churn largely doesn't affect you.

2. **Download resumption finally lands.** For agent builders distributing GGUFs to end users, [#9602](https://github.com/unslothai/unsloth/pull/9602) eliminates the "user cancelled at 799MB, restart from 0" failure mode. This is particularly impactful for large vision or MoE models where 800MB is a single shard.

3. **VRAM reporting is becoming trustworthy.** [#9362](https://github.com/unslothai/unsloth/pull/9362) fixes a long-standing issue where `torch.allocated` lied about actual availability. For multi-tenant or multi-model serving setups, this corrects the data feeding into load-balancing and OOM-prevention logic.

4. **AMD support is maturing but fragmented.** Multiple open items ([W7900 detection bug](https://github.com/unslothai/unsloth/issues/9551), [APU carve-out fix](https://github.com/unslothai/unsloth/pull/9314), [Docker gap](https://github.com/unslothai/unsloth/issues/9581)) suggest ROCm path is actively used but still has rough edges compared to CUDA. The [RAM fallback bug](https://github.com/unslothai/unsloth/issues/9549) is particularly concerning for production AMD deployments.

5. **studiobench instrumentation is being hardened.** If you're relying on published benchmark numbers for context-window performance claims, verify whether your source data came from the pre- or post-[#9542](https://github.com/unslothai/unsloth/pull/9542) instrument set. Four previously published numbers were withdrawn.

</details>

---
*This digest is auto-generated by [agents-radar](https://github.com/brysoh435-dev/agents-radar).*