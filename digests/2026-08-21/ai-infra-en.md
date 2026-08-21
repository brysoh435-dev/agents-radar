# AI Infrastructure Digest 2026-08-21

> Generated: 2026-08-21 03:30 UTC | Projects covered: 6

- [vLLM](https://github.com/vllm-project/vllm)
- [SGLang](https://github.com/sgl-project/sglang)
- [llama.cpp](https://github.com/ggml-org/llama.cpp)
- [Ollama](https://github.com/ollama/ollama)
- [LiteLLM](https://github.com/BerriAI/litellm)
- [Unsloth](https://github.com/unslothai/unsloth)

---

## Cross-Project Comparison

# AI Infrastructure Ecosystem Cross-Project Report
## 2026-08-21

---

## 1. Ecosystem Overview

The AI inference infrastructure landscape is experiencing acute pressure from model proliferation—particularly DeepSeek-V4, Kimi K3, and the Qwen 3.x family—while hardware heterogeneity (Blackwell SM120, AMD MI355X/gfx950, Apple Silicon, Intel XPU) fragments optimization targets. Production stability is deteriorating: speculative decoding (MTP/DFLASH) shows regressions across vLLM, SGLang, and llama.cpp, and prefix caching—critical for agent workloads—has zero-hit-rate bugs in multiple engines. The industry is bifurcating between cloud-scale disaggregation (PD separation, KV cache distribution) and edge/local deployment (quantized KV, breakable CUDA graphs, MLX optimization), with no project cleanly spanning both.

---

## 2. Activity Comparison

| Project | Issues (Active) | PRs (24h / Open) | Release Status | Build Velocity |
|---------|-----------------|------------------|----------------|----------------|
| **vLLM** | 15+ tracked (5 critical/high) | 2 merged (optimization), 1 open fix | No release; nightly `cu129` misresolves to CUDA 13 | — |
| **SGLang** | 9 tracked (2 critical, 5 high) | 5+ merged (models, graphs, infra), 1 open recipe fix | No release | — |
| **llama.cpp** | 10+ tracked (2 critical, 4 high) | 4 merged (b10532–b10541), 4 open model support | **10 builds shipped** (b10532–b10541) | Highest |
| **Ollama** | 10 tracked (2 critical, 3 high) | 3 in review (CORS, truncation, parallel requests) | Stable at **v0.32.14** | — |
| **LiteLLM** | 10 tracked (3 high, 4 medium) | 6+ merged (pricing, RBAC, adapters), 2 open fixes | No release | — |
| **Unsloth** | 10+ tracked (2 critical, 3 high) | 200+ merged (v0.1.801-beta), 5 open fixes | **v0.1.801-beta** shipped | High (batch) |

*Note: Issue/PR counts are digest-derived approximations, not exhaustive repository totals.*

---

## 3. Model Support Race

| Model/Architecture | vLLM | SGLang | llama.cpp | Ollama | Unsloth |
|-------------------|:----:|:------:|:---------:|:------:|:-------:|
| **DeepSeek-V4** | ⚠️ Partial (sm_80 blocked, ROCm PD disagg in progress) | ✅ Tool-call parser fix | ✅ CUDA/Metal complete; Vulkan closing gaps | ⚠️ Cloud loops/corruption | — |
| **Kimi K3** | ✅ Active optimization (2 kernel fusion PRs, ~5% latency) | ⚠️ DFLASH speculative decoding (review, not prod) | — | ⏳ Cloud requested, not deployed | — |
| **Qwen 3.x family** | ✅ Qwen3-Omni LoRA, ModernBERT FP8; MTP regressions | ✅ Weight cache daemon, AITER GDN decode | — | ⚠️ Dominates bug volume (reasoning leak, truncation, loading) | ✅ Qwen3.8-27B official; M3 corruption |
| **GLM-4.5/MTP** | — | — | ⚠️ MTP review | — | — |
| **LFM2 + DSpark** | — | — | ✅ Merged | — | — |
| **SANA-Video/LongCat (diffusion)** | — | ✅ Breakable CUDA graphs | — | — | ⚠️ Image gen hangs |
| **ERNIE (CPU)** | — | ✅ `topk_softmax_cpu` BF16 bias | — | — | — |
| **TeleChat4** | — | — | ⚠️ Review | — | — |
| **Gemma4 tool calls** | — | — | — | ⚠️ In review (`=` separator) | — |

**Leaders by category:**
- **Broadest model coverage**: vLLM (multimodal + MoE + encoder-decoder)
- **Fastest to new architectures**: llama.cpp (LFM2, GLM-4.5-Air MTP shipped same-week)
- **Diffusion/video**: SGLang exclusive with SANA-Video/LongCat breakable graphs
- **Most model-bug burden**: Ollama (Qwen 3.x family accounts for majority of open issues)

---

## 4. Performance Frontier

| Optimization Domain | Primary Projects | Key Developments | Status |
|---------------------|------------------|------------------|--------|
| **KV Cache** | vLLM, SGLang, Ollama, llama.cpp | Prefix caching: vLLM has zero-hit-rate bugs under MTP (#52244, #52897); SGLang builds distributed KV roadmap (#21846); Ollama adds cancellation recovery (#17901) but lacks inter-request caching; llama.cpp enables quantized KV+Flash Attention on Metal (b10532) | ⚠️ Unstable, actively repaired |
| **Speculative Decoding** | vLLM, SGLang, llama.cpp | vLLM: 76% MTP latency regression on Qwen3-Next; SGLang: DFLASH for Kimi-K3 in review, zero-probability draft bug (#35771); llama.cpp: adaptive MTP depth in review (#27210), multiple performance cliffs | 🔴 Regressing |
| **Kernel Fusion / GEMM** | vLLM, SGLang | vLLM: MXFP4 top-k fusion (Kimi K3), MoonViT RoPE fusion, FlashInfer BF16 default; SGLang: AITER packed GDN decode (AMD 13–23% improvement), SANA-Video linear attention fast path | ✅ Advancing |
| **Quantization (FP8/NVFP4/MXFP4)** | vLLM, SGLang, llama.cpp, Unsloth | vLLM: FP8 block-scale fails sm120; SGLang: NVFP4 cookbook OOM on RTX 5090; llama.cpp: FP32 Q-quant numerical fix (Vulkan); Unsloth: auto-installs llm-compressor without consent | ⚠️ Immature on new hardware |
| **Distributed Serving / PD Disaggregation** | vLLM, SGLang | vLLM: DeepSeek-V4-Pro PD via MORI IO on MI355X; SGLang: session-aware router (#25760), push-based load reporting | ⚠️ Early, API evolving |
| **Weight Loading / Cold Start** | SGLang, llama.cpp | SGLang: daemon reduces Qwen3-235B FP8 load 306s→<1s; llama.cpp: lazy-load startup_models (b10536) | ✅ Solid gains |
| **Batching / Scheduling** | vLLM, SGLang | vLLM: TurboQuant KV cache crash on chunked prefill; SGLang: `PrefillDelayer` mixed-state feedback loop under DP attention | 🔴 Critical bugs |

---

## 5. Layer Positioning

| Layer | Projects | Differentiation | Current Focus |
|-------|----------|-----------------|---------------|
| **Cloud Serving Engine** | **vLLM**, **SGLang** | vLLM: broadest backend coverage (CUDA/ROCm/XPU), production default; SGLang: PD disaggregation, agent-oriented routing, faster feature velocity | vLLM: stability repair; SGLang: distributed KV, speculative decoding recipes |
| **Local / Edge Runtime** | **llama.cpp**, **Ollama** | llama.cpp: maximum portability (Vulkan/Metal/CUDA/CPU), quantization leader; Ollama: UX abstraction, MLX integration, desktop market | llama.cpp: speculative decoding correctness; Ollama: agent workload reliability, CORS/web integration |
| **Gateway / Proxy** | **LiteLLM** | Universal routing, cost tracking, enterprise RBAC, multi-provider abstraction | Cost accuracy, rate-limiting correctness, China-region adapters |
| **Training / Fine-tuning** | **Unsloth** | 2–5× faster fine-tuning, Studio GUI, consumer GPU optimization | Desktop stability, long-context agents, multi-modal skills |

**Integration patterns:**
- Ollama embeds llama.cpp; bugs in upstream (Qwen3.5 parallel requests) propagate downstream
- LiteLLM proxies to vLLM/SGLang/Ollama; its cost-tracking bugs affect all backends uniformly
- Unsloth increasingly overlaps with serving (LAN remote access, model serving) but remains training-centric

---

## 6. Trend Signals

| Signal | Evidence | Implication for Agents/Apps |
|--------|----------|----------------------------|
| **Speculative decoding is not production-ready** | 76% regression (vLLM), zero-probability draft acceptance (SGLang), MTP performance cliffs (llama.cpp), prefix cache collapse under MTP (vLLM #52244) | **Disable MTP/DFLASH** for latency-sensitive production; use greedy or low-temperature sampling until 2026 Q4 |
| **Agent workloads stress infrastructure assumptions** | Prefix caching broken (vLLM, Ollama MLX), KV bleed across requests (Ollama ROCm), chat truncation drops user messages (Ollama), `PrefillDelayer` state corruption (SGLang) | Implement **defensive retries**, **response deduplication**, **strict output validation**; assume conversation state is fragile |
| **Blackwell/SM120 is a beta platform** | FP8 block-scale failure (vLLM), FlashInfer OOM (vLLM), decode degradation (llama.cpp), NVFP4 cookbook broken (SGLang), `torch.compile` 6× regression (SGLang) | **Avoid RTX 5090/5070 Ti for production inference** until CUDA 13.x + driver stabilization; validate all quantization paths |
| **AMD ROCm is closing gaps but remains secondary** | AITER kernels replacing Triton (SGLang), MI355X PD disaggregation (vLLM), gfx950 3D split-KV, but Strix Halo KV bleed (Ollama), GLM-5.2 MTP 0% acceptance (vLLM) | **Test exhaustively on AMD**; do not assume CUDA-optimized paths port cleanly; Vulkan fallback often more stable |
| **PD disaggregation is the next architecture battleground** | vLLM MORI IO connector, SGLang distributed KV roadmap + session router, both early and API-unstable | **Early adopters only**; expect breaking changes; weight cache daemon (SGLang) is the most immediately useful sub-feature |
| **Cost tracking is systematically underestimated** | Azure cache writes billed at zero (LiteLLM), OpenAI `cache_write_tokens` dropped, per-model budgets stuck at zero | **Audit spend logs against provider invoices**; do not trust proxy-reported costs for cached workloads |
| **Long-context = new failure mode** | Auto Compaction needed (Unsloth), full re-prefill per agent step (Ollama MLX), TurboQuant crash on chunked prefill (vLLM) | **Cap context windows aggressively** for agent loops; design for checkpoint/resume, not persistent long context |

---

**Bottom line for technical decision-makers:** The ecosystem is optimizing for capability breadth (new models, new hardware) at the expense of production stability. Agent developers should prioritize **vLLM for cloud scale** (with MTP disabled), **llama.cpp for edge portability** (pinned to pre-MTP builds), and **LiteLLM for multi-provider routing** (with manual cost validation). Avoid bleeding-edge features—speculative decoding, PD disaggregation, Blackwell deployment—until Q4 2026 stability milestones are demonstrated.

---

## Per-Project Reports

<details>
<summary><strong>vLLM</strong> — <a href="https://github.com/vllm-project/vllm">vllm-project/vllm</a></summary>

# vLLM Digest — 2026-08-21

---

## 1. Today's Highlights

The vLLM project is actively pushing **Kimi K3 inference optimizations** with two new kernel fusion PRs targeting ~5% E2E latency reduction, while **ROCm/AMD support gaps for DeepSeek-V4 and Kimi-K3** remain a major focus area with dedicated tracking issues and CI enablement work. On the stability front, **prefix cache regressions under MTP speculative decoding** and **cross-node CUDA graph capture failures on GB10 clusters** are emerging as critical infrastructure bugs affecting production deployments.

---

## 2. Releases & Breaking Changes

**No releases in the last 24 hours.**

Notable ongoing: Issue [#42338](https://github.com/vllm-project/vllm/issues/42338) documents that `cu129` nightly installation resolves to CUDA 13 wheels due to PEP 440 pre-release sorting — users pinning to `cu129` nightlies may silently get CUDA 13 builds.

---

## 3. New Model & Hardware Support

| Item | Status | Details |
|------|--------|---------|
| **Qwen3-Omni multimodal LoRA** | PR [#52786](https://github.com/vllm-project/vllm/pull/52786) | Adds LoRA support for `Qwen3OmniMoeThinkerForConditionalGeneration` with 3D MoE LoRA loading and audio attention `qkv` → `qkv_proj` rename |
| **ModernBERT FP8** | PR [#53101](https://github.com/vllm-project/vllm/pull/53101) | Fixes missing `quant_config` propagation to encoder linear layers for `llmcompressor` FP8_DYNAMIC checkpoints |
| **DeepSeek-V4 on sm_80 (A100/A800)** | Issue [#40851](https://github.com/vllm-project/vllm/issues/40851) | **Blocked**: `deepgemm-src` assertion failure; 45 comments, 22 👍 — major community demand |
| **DeepSeek-V4 on L20** | Issue [#42949](https://github.com/vllm-project/vllm/issues/42949) | `auto_functionalized was not removed` error — unassigned, stale |
| **Kimi-K3 on ROCm** | Issue [#50682](https://github.com/vllm-project/vllm/issues/50682) | Day-0 AITER fused-MoE baselines landed; gaps remain for full feature parity |

**ROCm/AMD specific**: PR [#48989](https://github.com/vllm-project/vllm/pull/48989) enables DeepSeek-V4-Pro PD disaggregation via MORI IO KV connector on AMD MI355X; PR [#53195](https://github.com/vllm-project/vllm/pull/53195) enables WideEP intranode DI CI tests.

---

## 4. Performance & Optimization

| PR/Issue | Improvement | Numbers |
|----------|-------------|---------|
| **PR [#53152](https://github.com/vllm-project/vllm/pull/53152)** — Fuse MXFP4 top-k finalization into Kimi K3 latent-tail | ~5% E2E latency reduction on K3 | Eliminates separate `finalize` kernel (unpermute × router weight × top-k reduction) |
| **PR [#53168](https://github.com/vllm-project/vllm/pull/53168)** — Fuse MoonViT Q/K complex RoPE | SM90+ Triton path, reduces memory traffic | Fuses real/imaginary RoPE into packed-QKV read |
| **PR [#53196](https://github.com/vllm-project/vllm/pull/53196)** — Packed FlashKDA checkpoint export | Reduces kernel launch overhead | Replaces per-request split execution from #52789 |
| **PR [#53202](https://github.com/vllm-project/vllm/pull/53202)** — Default BF16 linear to FlashInfer `mm_bf16` | Removes CuTe skinny GEMM fallback | `backend="auto"` now selects FlashInfer for unquantized BF16 |
| **PR [#52275](https://github.com/vllm-project/vllm/pull/52275)** — Adaptive layouts for TRTLLM MXFP8 | `128x4` activation-scale layout above configurable row threshold | Opt-in, preserves `8x4` default |
| **Issue [#43295](https://github.com/vllm-project/vllm/issues/43295)** / [#35387](https://github.com/vllm-project/vllm/issues/35387) | **MTP latency regressions** | 76% regression on Qwen3-Next-80B-A3B-FP8 (TP=4, 2 speculative tokens); MTP "clearly slower" per user reports |

---

## 5. Stability & Regressions

| Severity | Item | Fix Status |
|----------|------|------------|
| **🔴 Critical** | **Issue [#46253](https://github.com/vllm-project/vllm/issues/46253)**: Cross-node CUDA graph capture fails on GB10 (DGX Spark) TP=2 — `vllm::all_reduce` captured inside `breakable_cudagraph`, `splitting_ops` ignored | No fix PR; workaround: disable CUDA graphs or GPUDirect |
| **🔴 Critical** | **Issue [#52023](https://github.com/vllm-project/vllm/issues/52023)**: `draft_model` speculative decoding crashes at init when draft `hidden_size` > target under TP>1 — TRT-LLM fused allreduce+RMSNorm workspace sized from target only | No fix PR |
| **🟡 High** | **Issue [#41726](https://github.com/vllm-project/vllm/issues/41726)**: TurboQuant KV cache crashes on large chunked continuation prefill after workspace lock (PR #39931 on Qwen3.5-9B) | Testing PR #39931; nightly `0.20.2rc1.dev35` |
| **🟡 High** | **Issue [#52897](https://github.com/vllm-project/vllm/issues/52897)**: Align-mode prefix caching 0% hit rate (0/996k queries) with `--scheduling-policy priority` on hybrid GDN model post-#51113 | No fix PR |
| **🟡 High** | **Issue [#52244](https://github.com/vllm-project/vllm/pull/52244)**: Hybrid GDN prefix-cache hits broken under MTP spec decoding — prompts length multiple of hash unit get zero hits | **Fix PR open**: #52244 |
| **🟡 High** | **Issue [#52833](https://github.com/vllm-project/vllm/issues/52833)**: GLM-5.2 MTP 0% draft acceptance on MI355X; disabling EP hits `hipErrorIllegalAddress` | No fix PR |
| **🟡 High** | **Issue [#51884](https://github.com/vllm-project/vllm/issues/51884)**: FP8 block-scaled weights fail on sm120 (RTX 5090) — DeepGEMM "Unknown SF transformation" | No fix PR |
| **🟡 High** | **Issue [#49476](https://github.com/vllm-project/vllm/issues/49476)**: FlashInfer b12x SM120 MoE workspace OOM on 16GB Blackwell (regressed v0.25.1) | No fix PR |
| **🟠 Medium** | **Issue [#42769](https://github.com/vllm-project/vllm/issues/42769)**: DeepSeek-V4 `load_weights` `UnboundLocalError: 'name_mapped'` when expert mapping empty | No fix PR |
| **🟠 Medium** | **Issue [#51914](https://github.com/vllm-project/vllm/issues/51914)**: DeepSeek-V4-Flash-0731 intermittent malformed DSML tool-call wrapper with DSpark | No fix PR |
| **🟠 Medium** | **Issue [#38182](https://github.com/vllm-project/vllm/issues/38182)**: MTP speculative decoding reduces prefix cache hit rate on Qwen3.5-35B-A3B | No fix PR |

---

## 6. What This Means for Application Developers

**If you're running production inference:**

- **Avoid MTP speculative decoding** on Qwen3.5/3.6 MoE models until [#52244](https://github.com/vllm-project/vllm/pull/52244) merges — prefix cache hit rates collapse to zero for certain prompt lengths, and latency regressions of 76% are reported ([[#35387](https://github.com/vllm-project/vllm/issues/35387)]).
- **GB10/DGX Spark clusters**: Disable piecewise CUDA graphs for cross-node TP deployments until [#46253](https://github.com/vllm-project/vllm/issues/46253) is resolved; the NCCL all-reduce capture bug causes deterministic `illegal memory access` crashes.
- **RTX 50-series (sm120)**: FP8 block-scaled and NVFP4 models may hit DeepGEMM/FlashInfer workspace OOMs — test thoroughly before deploying on 16GB cards like 5070 Ti/5080 ([[#51884](https://github.com/vllm-project/vllm/issues/51884)], [[#49476](https://github.com/vllm-project/vllm/issues/49476)]).

**If you're building agents with tool-calling:**

- DeepSeek-V4-Flash with DSpark shows intermittent DSML wrapper corruption ([[#51914](https://github.com/vllm-project/vllm/issues/51914)]) — implement defensive parsing and fallback retry logic.

**If you're optimizing for latency:**

- Kimi K3 on NVIDIA is the most actively optimized path; monitor [#53152](https://github.com/vllm-project/vllm/pull/53152) and [#53168](https://github.com/vllm-project/vllm/pull/53168) for ~5% E2E gains. AMD K3 support lags significantly — track [#50682](https://github.com/vllm-project/vllm/issues/50682) for roadmap updates.

---

</details>

<details>
<summary><strong>SGLang</strong> — <a href="https://github.com/sgl-project/sglang">sgl-project/sglang</a></summary>

# SGLang Digest — 2026-08-21

## Today's Highlights

The project is actively pushing forward **PD disaggregation for agentic workloads** with a new distributed KV cache roadmap and lightweight session-aware router, while **diffusion model support expands** with breakable CUDA graphs for SANA-Video and LongCat. Multiple critical bugs surfaced in speculative decoding, scheduler state management, and XPU/NVFP4 paths, with several receiving same-day PRs. CI infrastructure remains under pressure with 3 broken and 11 flaky tracked tests.

---

## Releases & Breaking Changes

*No releases in the last 24h.*

---

## New Model & Hardware Support

| Item | Status | Details |
|------|--------|---------|
| **Kimi-K3 + DFLASH speculative decoding** | PR #35784 | Adds `set_dflash_layers_to_capture` to enable DFLASH draft-path aux hidden-state capture; production integration tracked in new roadmap issue #35783 |
| **ERNIE models on CPU** | PR #35222 | Enables via `topk_softmax_cpu` kernel with BF16 `correction_bias` support |
| **SANA-Video breakable CUDA graphs** | PR #35729 | Native breakable graph support with fixed 300-token prompt shape, avoiding generic bucket padding |
| **LongCat breakable CUDA graphs** | PR #35724 | Enables existing BCG runner for `meituan-longcat/LongCat-Image` with fixed 512-token DiT prompt shape |
| **AMD ROCm 7.14 (gfx942/gfx950)** | PR #35319 | New release channel support via pip wheels on Ubuntu 24.04; no apt repo available |
| **AMD AITER HIP packed GDN decode** | PR #33113 | 13–23% decode latency reduction for Qwen3.5 on MI355X vs. Triton GDN |

---

## Performance & Optimization

| Item | Metric / Change | Reference |
|------|-----------------|-----------|
| **SANA-Video linear attention fast path** (`quality=high`) | BF16 input GEMM with FP32 accumulation/output; second GEMM in FP32; preserves bit-exact default path | PR #35728 |
| **Weight Cache Daemon Phase 1** | Qwen3-235B FP8 load time: **306–327s → <1s** via per-rank daemon + CUDA IPC | Issue #33522 |
| **DFLASH2 recipe fix for RTX 5090** | Corrects mem-fraction pins that OOMed on `main` | PR #35786 |
| **AMD AITER target-verify GQA packing** | Avoids MHA inflation, enables 3D split-KV kernel on gfx950 | PR #35457 |
| **NPU DSpark compact verify graph** | Shortens verify window to SPS-selected length, folds epilogue into single graph | PR #35670 |
| **Push-based engine load reporting** | Replaces Router→Worker polling with scheduler-side push channel for consistent DP-rank load views | PR #32523 |

---

## Stability & Regressions

| Severity | Issue | Description | Fix Status |
|----------|-------|-------------|------------|
| **🔴 Critical** | #35189 | NIXL/UCX prefill segfault (`nixlUcxSharedThread → cuEventQuery`) on v0.5.17 / CUDA 13.0 / B200; previously closed issues #23489/#23499 lacked root cause | **Unfixed** — reopened |
| **🔴 Critical** | #35241 | `PrefillDelayer` persistent mixed-state feedback loop under DP Attention + chunked prefill, collapsing prefill progress | Open, under investigation |
| **🟡 High** | #35777 | Qwen3.8-27B NVFP4 on RTX 5090: cookbook `mem-fraction` OOMs at decode-graph capture (~5GB); `torch.compile` + decode graph = **6x regression** | Open, recipe fix in PR #35786 |
| **🟡 High** | #35771 | Target-only speculative sampler accepts zero-probability draft at RNG boundary | Open |
| **🟡 High** | #34112 | Chunked-prefill tail cancellation leaks visible token, leaves negative scheduler output IDs | Open |
| **🟡 High** | #34720 | XPU: Qwen3.5 GDN + speculative decode fails with `causal_conv1d_update_xpu()` unexpected `intermediate_conv_window` | Open |
| **🟡 High** | #35345 | Qwen3.6/3.8 multimodal mRoPE passed to 1D fused QK RMSNorm+RoPE kernel (shape mismatch) | Open |
| **🟢 Medium** | #35779 | MiniMax-M2 CPU inference failure | Open |
| **🟢 Medium** | #35772 | Qwen3-VL vision features diverge from Transformers/vLLM on fine-grained grounding | Open |
| **🟢 Medium** | #35785 | Triton version mismatch: AITER gluon kernels require ≥3.6.0, Docker has 3.4.0 | Open |
| **✅ Fixed** | #35563 | DeepSeek-V3/V4 tool-call parsers dropped calls on two-chunk streamed output | Closed |
| **✅ Fixed** | #28179 | `repetition_penalty` ignored due to missing `BatchedRepetitionPenalizer` | Closed as inactive |
| **✅ Fixed** | #26454 | Non-DP multi-node TP=8 hang in `event_loop_overlap` | Closed as inactive |

**CI Health:** 3 broken, 11 flaky tests tracked in #17050. CI Maintenance Mode (#21065) was previously activated for infrastructure stabilization.

---

## What This Means for Application Developers

1. **Speculative decoding users:** Verify your deployment isn't hitting the zero-probability draft bug (#35771) or NIXL/UCX segfault (#35189) before upgrading to v0.5.17. The DFLASH path for Kimi-K3 is landing but not yet production-ready (#35783).

2. **Diffusion workloads:** SANA-Video and LongCat now support breakable CUDA graphs — you can enable these for lower latency without sacrificing dynamic shape handling. The `quality=high` path for SANA-Video trades some throughput for numerical precision.

3. **RTX 5090 / NVFP4 deployments:** The published Qwen3.8-27B cookbook recipe is currently broken on `main`; use PR #35786's corrected mem-fraction values or expect OOMs and severe `torch.compile` regressions.

4. **PD disaggregation / agentic builders:** The distributed KV cache roadmap (#21846) and session-aware router (#25760) are active but early; expect API evolution. The weight cache daemon dramatically improves cold-start for large FP8 models if you're running Qwen3-class weights.

5. **AMD ROCm users:** 7.14 is now supported via pip-only install; AITER kernels are progressively replacing Triton for GDN decode with measurable latency wins.

</details>

<details>
<summary><strong>llama.cpp</strong> — <a href="https://github.com/ggml-org/llama.cpp">ggml-org/llama.cpp</a></summary>

# llama.cpp Digest — 2026-08-21

## Today's Highlights

The project shipped **10 builds in 24 hours** (b10532–b10541), with critical fixes for **Metal KV cache dequantization before Flash Attention** and **Vulkan numerical stability in FP32 Q-quantization paths**. A **revert of tensor-split meta backend fixes** (#26502) signals ongoing instability in multi-GPU tensor parallelism, while **adaptive MTP draft depth** enters review as the project races to stabilize speculative decoding across backends.

---

## Releases & Breaking Changes

| Build | Change | Impact |
|-------|--------|--------|
| **b10541** | [`mtmd: add --mmproj-device argument`](https://github.com/ggml-org/llama.cpp/releases/tag/b10541) | **Breaking for multi-modal deployments**: New `--mmproj-device` / `-mmdev` flag and `MTMD_BACKEND_DEVICE` env var control where vision projection layers run. Previously defaulted to same device as text model; now requires explicit configuration for split setups. |
| **b10531** | [`Revert "tensor-split meta backend fixes"`](https://github.com/ggml-org/llama.cpp/releases/tag/b10531) | **Regression risk**: Reverts #26502 due to unreported breakage. Users on `--split-mode tensor` with quantized KV cache should **stay on b10530 or earlier** until replacement fix lands. |

---

## New Model & Hardware Support

| PR/Issue | Description | Status |
|----------|-------------|--------|
| [#27435](https://github.com/ggml-org/llama.cpp/pull/27435) | **TeleChat4 model support** — MHC module, CUDA ops, weight conversion | Open, under review |
| [#26534](https://github.com/ggml-org/llama.cpp/pull/26534) | **GLM-4.5-Air MTP** — multi-token prediction for GLM MoE architecture | Open, tested on GLM 4.5 Air |
| [#27383](https://github.com/ggml-org/llama.cpp/pull/27383) | **LFM2 models + DSpark speculative decoding** — with partial recurrent state rollback | **Merged** |
| [#27453](https://github.com/ggml-org/llama.cpp/pull/27453) | **DeepSeek V4 `LIGHTNING_INDEXER`** on Vulkan — F32/F16/BF16 + quants | Open, completes backend parity |
| [#26578](https://github.com/ggml-org/llama.cpp/pull/26578) | **DeepSeek-V4 hyper-connection fused ops** (`DSV4_HC_COMB/PRE/POST`) on Vulkan | Open, last major backend gap |

**Backend parity note**: DeepSeek-V4 inference now complete on CUDA/Metal, with Vulkan closing remaining gaps. ROCm gains radix TOP-K for long rows ([#27466](https://github.com/ggml-org/llama.cpp/pull/27466)).

---

## Performance & Optimization

| Change | Backend | Details |
|--------|---------|---------|
| [`metal: dequant kv cache only for large batches`](https://github.com/ggml-org/llama.cpp/releases/tag/b10538) (#27438) | **Metal** | Conditional KV dequantization — skips overhead for small batches, triggers at threshold TBD |
| [`metal: dequantize q8_0 KV to f16 before flash attention`](https://github.com/ggml-org/llama.cpp/releases/tag/b10532) (#27390) | **Metal** | Preprocessing pass for `GGML_OP_FLASH_ATTN_EXT`; enables quantized KV + FA on Apple Silicon |
| [`CUDA: switch points per HW/quant type for mvq→MMQ crossover`](https://github.com/ggml-org/llama.cpp/releases/tag/b10534) (#26079) | **CUDA** | Runtime `GGML_CUDA_MMVQ_MAX` tuning; per-architecture batch size thresholds for vec-q vs. MMQ decode |
| [`AVX2: speed up IQ models at large batch`](https://github.com/ggml-org/llama.cpp/pull/27402) (#27402) | **CPU** | Reduces redundant weight decoding during 512-token batches; targets imatrix/perplexity workloads |
| [`server: lazy-load startup_models after main setup`](https://github.com/ggml-org/llama.cpp/releases/tag/b10536) (#27424) | **Server** | Router mode defers model loading until first request; reduces startup latency, prevents failed models from blocking healthy ones |

---

## Stability & Regressions

| Severity | Issue | Symptoms | Fix Status |
|----------|-------|----------|------------|
| **🔴 Critical** | [#27102](https://github.com/ggml-org/llama.cpp/issues/27102) — CUDA kernel stall, watchdog kill | RTX Pro 6000 Blackwell, Qwen3.8-27B UD-Q8_K_XL | Open, 18 comments, no PR |
| **🔴 Critical** | [#27447](https://github.com/ggml-org/llama.cpp/issues/27447) — Step 3.7 fails b10509+ | Vulkan, Strix Halo, regression from b10507 | **Closed** (build-specific, likely fixed in later release) |
| **🟠 High** | [#27444](https://github.com/ggml-org/llama.cpp/issues/27444) — Qwen3.8-27B decode degrades ~30% mid-generation | RTX 5090, CUDA, b10536 | Open, 5 comments |
| **🟠 High** | [#25489](https://github.com/ggml-org/llama.cpp/issues/25489) — MTP performance drop since b9935 | Windows, `draft-mtp` speculative | Open, 13 comments, suspected scheduler interaction |
| **🟠 High** | [#25618](https://github.com/ggml-org/llama.cpp/issues/25618) — Speculative decoding diverges on quantized targets | Greedy sampling mismatch Q4_K_M vs bf16 | Open, quantization + draft interaction |
| **🟡 Medium** | [#25304](https://github.com/ggml-org/llama.cpp/issues/25304) — `cublasCreate_v2` resource failure | Regression b9553→b9870, first inference crash | Open, 5 comments |
| **🟡 Medium** | [#26031](https://github.com/ggml-org/llama.cpp/issues/26031) — Garbled output with concurrent clients, Qwen3.6-35B | CPU, `-np > 1`, b9922+ | Open, bisected to b9922 |
| **🟡 Medium** | [#23774](https://github.com/ggml-org/llama.cpp/issues/23774) — MTP huge perf degradation Vulkan | Linux, llama-server | Open, 20 comments |
| **🟢 Low** | [#25060](https://github.com/ggml-org/llama.cpp/issues/25060) — Blackwell `SOFT_MAX` crash | RTX 5090, SM 12.0, community patch available | Open, DeepSeek-generated fix in comments |

**Numerical correctness**: [#25593](https://github.com/ggml-org/llama.cpp/issues/25593) — SM_60 (Tesla P100) FP32 math silently done in FP16; fix merged in two community forks, not upstream.

---

## What This Means for Application Developers

1. **Multi-modal deployments**: Plan for `--mmproj-device` configuration in b10541+. Vision pipelines now require explicit device placement; defaults may break existing split-GPU setups.

2. **Speculative decoding is still bleeding edge**: MTP shows **multiple open regressions** — performance cliffs (#25489, #23774), correctness divergence on quantized targets (#25618), and model-specific load failures (#24795). For production use, **pin to b9918 or earlier** if stable, or monitor [#27210](https://github.com/ggml-org/llama.cpp/pull/27210) (adaptive MTP) which may address depth-tuning issues.

3. **Blackwell (SM 12.0) remains immature**: Three open issues (#27102, #27444, #25060) on RTX 50-series suggest driver/CUDA 13.x interactions still being ironed out. Avoid for production inference until stabilization.

4. **Server/router mode improvements**: b10536's lazy-loading reduces cold-start penalties for multi-model serving; pair with [#24822](https://github.com/ggml-org/llama.cpp/issues/24822) progress reporting (in flight) for better orchestration visibility.

5. **Quantized KV + Flash Attention now viable on Metal**: b10532/b10538 unlock memory-efficient serving on Apple Silicon — significant for edge deployments previously blocked by KV cache size.

</details>

<details>
<summary><strong>Ollama</strong> — <a href="https://github.com/ollama/ollama">ollama/ollama</a></summary>

# Ollama Digest — 2026-08-21

## Today's Highlights

The Ollama team shipped multiple critical fixes for CORS preflight handling, chat truncation logic, and installation reliability on Ubuntu 26.04, while MLX prefix caching for agent workloads received a major architectural improvement. On the stability front, ROCm on AMD's new Strix Halo (gfx1151) emerged as a problematic backend with KV state bleeding and output corruption on long contexts, and Qwen 3.x family models continue to dominate the bug report volume with reasoning serialization regressions and loading failures.

---

## Releases & Breaking Changes

**None today** — no new release published in the last 24h. Current stable remains **v0.32.14**.

**Notable API behavior change (fix incoming):** `OPTIONS` requests to `/api/generate` on loopback/private hosts were returning `405 Method Not Allowed`, breaking browser `fetch()` CORS preflights. PR [#17890](https://github.com/ollama/ollama/pull/17890) restores `204` responses with proper CORS headers for these hosts, fixing a regression introduced in the v0.32.x series. ([#17887](https://github.com/ollama/ollama/issues/17887))

---

## New Model & Hardware Support

| Item | Status | Link |
|------|--------|------|
| **Server-side MLX imports** — Safetensors → MLX pipeline, remote upload/staging, draft layers, cancellation propagation. Drops GGUF conversion for MLX path. | In Review | [PR #14969](https://github.com/ollama/ollama/pull/14969) |
| **Qwen3.5 parallel request unlock** — `qwen35`/`qwen35moe` forced to `numParallel=1` due to fixed llama.cpp crash (2026-03-08 upstream). | In Review | [PR #17144](https://github.com/ollama/ollama/pull/17144) |
| **Gemma4 tool call parsing** — Accepts `=` separator (`save_as='report.docx'`) in addition to canonical `:<\|"\|>` format. | In Review | [PR #17888](https://github.com/ollama/ollama/pull/17888) |

**Cloud model availability:** Kimi K3 Cloud and Qwen3.8 Cloud remain requested but not yet deployed to Pro/Max tiers. ([#17235](https://github.com/ollama/ollama/issues/17235), [#17715](https://github.com/ollama/ollama/issues/17715), [#17720](https://github.com/ollama/ollama/issues/17720))

---

## Performance & Optimization

| Improvement | Details | Link |
|-------------|---------|------|
| **MLX prefix cache survivability** | Cancelled prefills (common with agent timeouts < multi-minute 40K-token prefill times) now preserve computed KV cache state. Retries resume from checkpoint instead of restarting at zero, eliminating the "hangs forever" symptom in agent loops. | [PR #17901](https://github.com/ollama/ollama/pull/17901) |
| **GGUF metadata extraction caching** | Unifies two divergent caches into single `<OLLAMA_MODELS>/metadata/sha256-<hex>.json` per blob. Eliminates duplicate expensive metadata reads; fixes capability detection inconsistencies. | [PR #17858](https://github.com/ollama/ollama/pull/17858) |

**Critical gap identified:** MLX engine currently performs **full re-prefill on every agent step** — no prompt/prefix caching between requests. A 20-30K token context sees linearly growing TTFT per step. PR [#17901](https://github.com/ollama/ollama/pull/17901) addresses cancellation recovery but not inter-request caching. ([#17829](https://github.com/ollama/ollama/issues/17829))

---

## Stability & Regressions

| Severity | Issue | Symptoms | Fix Status |
|----------|-------|----------|------------|
| **🔴 Critical** | **ROCm gfx1151 (Strix Halo) KV bleed** | Sequential requests contaminate each other's outputs — response N describes content from request N-1. Reproducible with alternating prompts. | **Open** — [#17847](https://github.com/ollama/ollama/issues/17847) |
| **🔴 Critical** | **ROCm gfx1151 long-context corruption** | >4K prompts produce wrong outputs (instruction ignoring, not crashing). Vulkan/CPU correct on same hardware. | **Open** — [#17895](https://github.com/ollama/ollama/issues/17895) |
| **🟠 High** | **Qwen3.6 reasoning JSON leak** | `think: false` + `format: "json"` returns `{"thought": "..."}` instead of user schema. Regression v0.31.2 → v0.32.x. | **Open** — [#17871](https://github.com/ollama/ollama/issues/17871) |
| **🟠 High** | **Chat truncation drops user message** | Multi-step tool loops overflowing `num_ctx` trigger `500: no user query found in messages`. qwen3.8 affected. | **Fix in review** — [PR #17894](https://github.com/ollama/ollama/pull/17894) ([#17778](https://github.com/ollama/ollama/issues/17778)) |
| **🟠 High** | **Windows UI thread infinite loop** | `ollama app.exe` spins on `GET↔POST /api/v1/settings`, blocks server readiness and all UI requests. | **Open** — [#17876](https://github.com/ollama/ollama/issues/17876) |
| **🟡 Medium** | **Qwen3.5 Vulkan loading failure** | `qwen3.5:0.8b` fails to load on Vulkan backend with `llama-server` crash during initialization. | **Open** — [#17903](https://github.com/ollama/ollama/issues/17903) |
| **🟡 Medium** | **Qwen3.6 memory loading regression** | RTX 5070Ti 12GB + 32GB DDR5: models that loaded previously now hit ceiling without filling GPU. 4K context insufficient. | **Open, needs info** — [#17517](https://github.com/ollama/ollama/issues/17517) |
| **🟡 Medium** | **deepseek-v4-flash cloud loops** | Thinking block repeats 221× over ~105s; or literal `</think>` leaked into history causing 193 identical tool calls (~31M tokens). | **Open** — [#17892](https://github.com/ollama/ollama/issues/17892), [#17617](https://github.com/ollama/ollama/issues/17617) |
| **🟢 Low** | **Qwen3.x vision chroma loss** | Solid red→"gray", green/blue→"black". Luminance preserved. Same weights correct under `mlx_vlm`. | **Closed** — [#17872](https://github.com/ollama/ollama/issues/17872) |
| **🟢 Low** | **Image generation rejection inconsistency** | `/api/generate` rejects image models despite `/api/tags` listing `"capabilities": ["image"]`. | **Closed** — [#17893](https://github.com/ollama/ollama/issues/17893) |

**Installation reliability:** Ubuntu 26.04 clean installs failed silently due to missing `zstd` CLI. Two complementary fixes in review: auto-install `zstd` ([PR #17891](https://github.com/ollama/ollama/pull/17891)) and `.tgz` fallback ([PR #17877](https://github.com/ollama/ollama/pull/17877)). ([#17860](https://github.com/ollama/ollama/issues/17860))

---

## What This Means for Application Developers

**For agent builders:**
- **Avoid ROCm on Strix Halo (gfx1151)** for production agent workloads until KV isolation is fixed. The cross-request contamination breaks determinism and security boundaries. Use Vulkan or CPU fallback on this hardware.
- **MLX on Apple Silicon:** Agent loops with long contexts will see growing latency per step due to missing prefix caching. PR [#17901](https://github.com/ollama/ollama/pull/17901) fixes cancellation recovery but not the root cause — consider shorter context windows or batching tool results until [#17829](https://github.com/ollama/ollama/issues/17829) is resolved.
- **Qwen 3.x reasoning models:** Do not rely on `think: false` + structured JSON output — reasoning content may leak into the JSON envelope. Validate output schema strictly or pin to v0.31.2.

**For web/SPA developers:**
- The CORS preflight fix ([PR #17890](https://github.com/ollama/ollama/pull/17890)) unblocks browser-based UIs calling Ollama on `localhost` or LAN addresses. Previously broken `fetch()` integrations will work again once this lands.

**For cloud API consumers:**
- `deepseek-v4-flash:cloud` has two distinct failure modes (infinite thinking loops, `</think>` marker leaks). Implement response deduplication and token burn limits as circuit breakers. The `</think>` leak specifically poisons conversation history — sanitize or truncate assistant messages before continuation.

**For Windows desktop users:**
- If `ollama app.exe` fails to start or appears hung, check for infinite `/api/v1/settings` polling in browser dev tools or Process Monitor. No workaround known — restart service or use CLI-only mode until [#17876](https://github.com/ollama/ollama/issues/17876) is fixed.

</details>

<details>
<summary><strong>LiteLLM</strong> — <a href="https://github.com/BerriAI/litellm">BerriAI/litellm</a></summary>

# LiteLLM Digest — 2026-08-21

## Today's Highlights

The project saw heavy activity across proxy reliability and cost accuracy fixes. A critical **max_parallel_requests slot leak** that caused false 429s for hours after streaming logging failures now has a fix with configurable TTL (PR [#37768](https://github.com/BerriAI/litellm/pull/37768)). Multiple **Azure and OpenAI cache pricing regressions** were patched, including missing `cache_creation_input_token_cost` for `azure/gpt-5.6*` models and dropped `cache_write_tokens` from cost calculations. A new **custom RBAC system with route allow-lists** (PR [#37771](https://github.com/BerriAI/litellm/pull/37771)) and **DashScope (Alibaba Bailian) Anthropic Messages API adapter** (PR [#37619](https://github.com/BerriAI/litellm/pull/37619)) expand enterprise and China-region deployment options.

---

## Releases & Breaking Changes

**No releases in the last 24h.**

---

## New Model & Hardware Support

| Item | Description | Link |
|------|-------------|------|
| **DashScope/Bailian Anthropic adapter** | New provider adapter routing supported Bailian models to native `/apps/anthropic/v1/messages` instead of OpenAI fallback | [PR #37619](https://github.com/BerriAI/litellm/pull/37619) |
| **Cognition SWE-1.7 pricing fix + Lightning tier** | Corrected `cognition/swe-1.7` from Lightning to Standard tier ($0.50/$2.50 per M); added `swe-1.7-lightning` entry | [PR #37763](https://github.com/BerriAI/litellm/pull/37763) |
| **SageMaker inference components** | `sagemaker_chat` now sends `X-Amzn-SageMaker-Inference-Component` header and honors `hf_model_name` in request body | [PR #37766](https://github.com/BerriAI/litellm/pull/37766) |

---

## Performance & Optimization

| Item | Details | Link |
|------|---------|------|
| **Spend-log write bounding** | Added row-count limit alongside byte limit; prevents 1000-row statements from accumulating ~110 MB per worker indefinitely | [PR #37758](https://github.com/BerriAI/litellm/pull/37758) |
| **Bedrock response header forwarding** | Surfaces `x-amzn-requestid` on `/chat/completions` for request correlation; covers converse (streaming + non-streaming) and invoke | [PR #37003](https://github.com/BerriAI/litellm/pull/37003) |
| **Anthropic 429 header passthrough** | Forwards `retry-after` and `anthropic-ratelimit-unified-status` on `/v1/messages` to prevent infinite client retries | [PR #37767](https://github.com/BerriAI/litellm/pull/37767) |

---

## Stability & Regressions

| Severity | Issue | Status | Details | Link |
|----------|-------|--------|---------|------|
| **High** | `max_parallel_requests` slot leak on stream logging failure | **Fix open** | Streaming cost-calc or logging failures leaked slots for fixed 3600s TTL; keys hit 429s with 1-5 actual in-flight requests. Fix makes slot release guaranteed and TTL configurable. | [PR #37768](https://github.com/BerriAI/litellm/pull/37768), [PR #37535](https://github.com/BerriAI/litellm/pull/37535) |
| **High** | `azure/gpt-5.6*` cache writes billed at zero | **Fix open** | Missing `cache_creation_input_token_cost` in Azure price map since v1.97.0; non-Azure entries correct. | [Issue #37631](https://github.com/BerriAI/litellm/issues/37631) |
| **High** | Per-model budgets track wrong spend counter | **Fix open** | Budget usage stays at zero while spend accrues; Bedrock model names fail budget matching. | [PR #37736](https://github.com/BerriAI/litellm/pull/37736) |
| **Medium** | `GET /health` exposes `extra_headers` and `aws_session_token` | **Open** | Health endpoint sanitizer incomplete; `api_key` masked but other secrets plaintext. | [Issue #36898](https://github.com/BerriAI/litellm/issues/36898) |
| **Medium** | MCP auto-execute hijacks client-side tool calls | **Open** | `require_approval: "never"` proxy-side execution breaks agentic clients (Claude Code) sending their own tools. | [Issue #37031](https://github.com/BerriAI/litellm/issues/37031) |
| **Medium** | Virtual key `BudgetExceededError` uses stale spend | **Open** | Race between spend logging and budget enforcement; `/key/info` shows spend below limit. | [Issue #27735](https://github.com/BerriAI/litellm/issues/27735) |
| **Medium** | Proxy fails to start after `uv tool update` | **Open** | FastAPI `get_flat_dependant` incompatibility in v1.96.2. | [Issue #36922](https://github.com/BerriAI/litellm/issues/36922) |
| **Medium** | DeepSeek thinking-mode log spam | **Open** | Warnings scale with replayed history, obscuring useful logs in multi-turn tool calls. | [Issue #37629](https://github.com/BerriAI/litellm/issues/37629) |
| **Low** | `provider_budget_config` reset at +57 years without Redis | **Open** | Monthly budgets never reset; missing TTL logic for non-Redis deployments. | [Issue #37261](https://github.com/BerriAI/litellm/issues/37261) |
| **Low** | `token_counter` crashes on `video_url` blocks | **Open** | `ValueError` on OpenAI-style video content; missing handler in content-type dispatch. | [Issue #28071](https://github.com/BerriAI/litellm/issues/28071) |

**Fixed today:**
- OpenAI `cache_write_tokens` dropped from cost calc ([Issue #33772](https://github.com/BerriAI/litellm/issues/33772))
- OpenAI Responses API `cache_read_cost` / `cache_creation_cost` always null ([Issue #34309](https://github.com/BerriAI/litellm/issues/34309))
- Anthropic system-role hoist invalidating prompt-cache prefix ([Issue #36559](https://github.com/BerriAI/litellm/issues/36559))
- `batches.create` fallback returning wrong provider error ([Issue #35359](https://github.com/BerriAI/litellm/issues/35359))
- Managed files `client.files.list()` missing API key ([Issue #35362](https://github.com/BerriAI/litellm/issues/35362))

---

## What This Means for Application Developers

1. **Upgrade urgency for Azure GPT-5.6 users**: If you're on v1.97.0+ with Azure-hosted GPT-5.6 models, cache writes are under-billed (zero cost). Monitor [Issue #37631](https://github.com/BerriAI/litellm/issues/37631) and [PR #37736](https://github.com/BerriAI/litellm/pull/37736) before next billing cycle.

2. **Streaming production safety**: The `max_parallel_requests` leak fix ([PR #37768](https://github.com/BerriAI/litellm/pull/37768)) is critical for high-throughput deployments. If you've seen unexplained 429s with low concurrency, this is likely the cause—set `LITELLM_MAX_PARALLEL_REQUESTS_TTL_SECONDS` after merge.

3. **MCP + agentic clients don't mix yet**: Using `require_approval: "never"` with Claude Code or similar agents will break non-MCP tools. Use explicit approval flows or wait for [Issue #37031](https://github.com/BerriAI/litellm/issues/37031).

4. **New RBAC enables least-privilege**: Custom roles with route allow-lists ([PR #37771](https://github.com/BerriAI/litellm/pull/37771)) let you scope internal users to specific endpoints (e.g., batch-only, embeddings-only) without forking the proxy.

5. **China-region Anthropic workloads**: The DashScope adapter ([PR #37619](https://github.com/BerriAI/litellm/pull/37619)) provides a native Messages API path for Alibaba Bailian, avoiding the lossy OpenAI compatibility shim for tool-calling and streaming.

</details>

<details>
<summary><strong>Unsloth</strong> — <a href="https://github.com/unslothai/unsloth">unslothai/unsloth</a></summary>

# Unsloth Digest — 2026-08-21

## Today's Highlights

Unsloth shipped **v0.1.801-beta** with experimental **Auto Compaction** for long-chat context management and **LAN Remote Access** preview, alongside 200+ merged PRs. The release follows last week's Qwen3.8-27B and Unsloth Desktop launches, but significant stability work remains: multiple critical PRs address Windows install failures, AMD GPU misreporting, and IndexedDB stalls on Linux AppImage. The team is also converging on portable agent skills and external TTS/STT endpoint support, signaling a push toward multi-modal, distributed Studio deployments.

---

## Releases & Breaking Changes

| Item | Details | Link |
|------|---------|------|
| **v0.1.801-beta** | Auto Compaction (experimental) for conversations beyond context limits; Remote & LAN Access (preview) for network access. Follows Qwen3.8-27B and Desktop support from prior week. | [Release](https://github.com/unslothai/unsloth/releases) |

---

## New Model & Hardware Support

| Item | Details | Link |
|------|---------|------|
| **Ling 3.0 request** | Community request for full Studio support (download, load, configure, serve). Not yet landed. | [#8532](https://github.com/unslothai/unsloth/issues/8532) |
| **Qwen3.8-27B follow-up** | Official quants supported, but active compatibility issues on Apple Silicon (see Stability). | [#9279](https://github.com/unslothai/unsloth/issues/9279) |
| **NVFP4 on 5060 Ti 16GB** | Reported failure to load; issue open with repro steps. | [#8246](https://github.com/unslothai/unsloth/issues/8246) |
| **ROCm AOTriton gate** | PR opens experimental flash/mem-efficient SDPA kernels for library users via `TORCH_ROCM_AOTRITON_ENABLE_EXPERIMENTAL`. Previously gated only for Studio internals. | [#8821](https://github.com/unslothai/unsloth/pull/8821) |
| **MLX runtime amp helpers** | Fixes `AttributeError: module 'unsloth.models._utils' has no attribute 'torch_amp_custom_fwd'` on MLX backend. | [#9447](https://github.com/unslothai/unsloth/pull/9447) |

---

## Performance & Optimization

| Item | Details | Link |
|------|---------|------|
| **Auto Compaction** | Reduces long conversations to fit context windows; one internal test showed 10,583 → 1,265 tokens (1,536-token window). Related PR fixes compaction over-counting token budget. | [v0.1.801-beta](https://github.com/unslothai/unsloth/releases), [#9442](https://github.com/unslothai/unsloth/pull/9442) |
| **Quantized KV cache + tensor parallelism** | PR fixes silent dropping of quantized KV cache types (Q4_0, Q8_0, etc.) when tensor split is enabled; previously forced to f16, inflating memory. | [#8939](https://github.com/unslothai/unsloth/pull/8939) |
| **LLM-compressor consent** | Feature request to require explicit user consent before auto-installing `llm-compressor` for FP8/FP4 exports. Currently auto-pip/uv installs without prompt. | [#8904](https://github.com/unslothai/unsloth/issues/8904) |

---

## Stability & Regressions

| Severity | Item | Status | Link |
|----------|------|--------|------|
| **🔴 Critical** | **Windows installation fails** with `WinError 2` during studio setup; fresh reports today. | Open, no fix PR | [#9440](https://github.com/unslothai/unsloth/issues/9440) |
| **🔴 Critical** | **Qwen3.8-27B on M3 Mac**: GUI turns purple, screen flickers, system-wide rendering corruption. LM Studio unaffected. | Open | [#9279](https://github.com/unslothai/unsloth/issues/9279) |
| **🟠 High** | **AMD GPU misreporting**: Studio reports healthy ROCm GPU while venv contains CPU/CUDA PyTorch, permanently breaking training via dependency fast-path. | PR open | [#8606](https://github.com/unslothai/unsloth/pull/8606) |
| **🟠 High** | **Linux AppImage IndexedDB stall**: WebKitGTK 2.50.4 vs 2.52.3 profile incompatibility leaves legacy chat DB permanently pending; server chats invisible. | PR open (#9446 supersedes closed #9444) | [#9446](https://github.com/unslothai/unsloth/pull/9446) |
| **🟠 High** | **Studio port fallback broken on Windows**: `SO_REUSEADDR` probe socket collides with existing listener, preventing fallback to free port. | PR open | [#9449](https://github.com/unslothai/unsloth/pull/9449) |
| **🟡 Medium** | **Image generation hangs at 0%** on "Preparing (text encoding + warmup)..." | Open | [#9404](https://github.com/unslothai/unsloth/issues/9404) |
| **🟡 Medium** | **Two consecutive web searches fail** with cloud models (e.g., ollama cloud). | Open | [#9108](https://github.com/unslothai/unsloth/issues/9108) |
| **🟡 Medium** | **Local model loads but returns HTTP 400** on every message (Qwen3.5-4B Safetensors). | Open | [#9398](https://github.com/unslothai/unsloth/issues/9398) |
| **🟡 Medium** | **Stats refresh broken**: Daily/monthly toggles update graph but not summary numbers. | Open | [#9337](https://github.com/unslothai/unsloth/issues/9337) |
| **🟡 Medium** | **macOS text encoding errors** in Desktop app (v0.1.701-beta). | Open | [#8594](https://github.com/unslothai/unsloth/issues/8594) |
| **🟢 Low/Fixed** | **Tool calling failed** with NVIDIA Nemotron API (invalid JSON object string). | Closed | [#9338](https://github.com/unslothai/unsloth/issues/9338) |
| **🟢 Low/Fixed** | **Setup lock acquisition failure** on first launch ("Can not acquire lock"). | Closed | [#9140](https://github.com/unslothai/unsloth/issues/9140) |
| **🟢 Low/Fixed** | **MLX Train/Export greyed out** due to startup thread race on first `transformers` import. | Closed | [#9120](https://github.com/unslothai/unsloth/issues/9120) |

---

## What This Means for Application Developers

1. **Long-context agents**: Auto Compaction in v0.1.801-beta is worth testing for persistent agent sessions, but monitor for the token accounting bug fixed in [#9442](https://github.com/unslothai/unsloth/pull/9442)—compacted turns may over-consume budget otherwise.

2. **Windows deployments remain risky**: Fresh install failures ([#9440](https://github.com/unslothai/unsloth/issues/9440)) and port-binding issues ([#9449](https://github.com/unslothai/unsloth/pull/9449)) suggest holding production Windows rollups until next patch. Linux AppImage users should also watch [#9446](https://github.com/unslothai/unsloth/pull/9446) for IndexedDB chat visibility fixes.

3. **Apple Silicon + large models**: Avoid Qwen3.8-27B on M-series Macs until [#9279](https://github.com/unslothai/unsloth/issues/9279) is resolved; the system-level rendering corruption suggests a Metal/graphics interop bug, not just a model load failure.

4. **API consumers**: The model list API currently omits quantization variants ([#9340](https://github.com/unslothai/unsloth/issues/9340))—if you rely on programmatic Q4_K_M vs Q8 selection, you'll need to work around this with manual path construction or wait for the fix.

5. **Agent builders**: Portable agent skills ([#9355](https://github.com/unslothai/unsloth/pull/9355)) and external TTS/STT endpoints ([#9214](https://github.com/unslothai/unsloth/pull/9214), [#9349](https://github.com/unslothai/unsloth/pull/9349)) are converging—good time to prototype multi-modal agents, but expect churn in the skills metadata format.

</details>

---
*This digest is auto-generated by [agents-radar](https://github.com/brysoh435-dev/agents-radar).*