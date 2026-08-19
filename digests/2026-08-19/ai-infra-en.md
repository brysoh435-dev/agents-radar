# AI Infrastructure Digest 2026-08-19

> Generated: 2026-08-19 05:56 UTC | Projects covered: 6

- [vLLM](https://github.com/vllm-project/vllm)
- [SGLang](https://github.com/sgl-project/sglang)
- [llama.cpp](https://github.com/ggml-org/llama.cpp)
- [Ollama](https://github.com/ollama/ollama)
- [LiteLLM](https://github.com/BerriAI/litellm)
- [Unsloth](https://github.com/unslothai/unsloth)

---

## Cross-Project Comparison

# Cross-Project AI Infrastructure Comparison Report
**Date: 2026-08-19**

---

## 1. Ecosystem Overview

The AI inference infrastructure landscape is experiencing intense bifurcation between **datacenter-scale serving** (vLLM, SGLang) and **edge/local runtimes** (llama.cpp, Ollama), with **gateway abstraction** (LiteLLM) and **training tooling** (Unsloth) filling critical adjacent layers. Today's activity reveals three dominant themes: **ROCm/AMD parity is now an explicit competitive battleground** across all serving engines, **speculative decoding architectures are fragmenting into model-specific implementations** (DFlash2, PARD-2, EAGLE3, adaptive MTP), and **disaggregated prefill/decode (PD) remains pre-production** despite heavy investment. The vLLM-SGLang rivalry is particularly acute, with both projects landing identical-day optimizations for Kimi-K3 and DeepSeek-V4 on competing backends. Meanwhile, Ollama's critical sm_86 regression and llama.cpp's semantic versioning debut signal maturation pressures on consumer-facing tools.

---

## 2. Activity Comparison

| Project | Issues (Active/Open Today) | PRs (Active/Merged Today) | Release Status | Velocity Signal |
|---------|---------------------------|---------------------------|----------------|---------------|
| **vLLM** | 6 critical/high open, 3 critical open | 8+ performance/model PRs, 2 reverts | No release; active `main` | 🔥 Very high — frontier model enablement at pace |
| **SGLang** | 5 critical/high open, 2 resolved | 6+ PRs (NIXL, multimodal, quantization) | No release | 🔥 Very high — PD disaggregation focus |
| **llama.cpp** | 6 critical/high open, 1 closed | 7+ PRs (kernels, speculative decode, backends) | v0.1.2 (WIP) + b10485 nightly | High — formal versioning begins |
| **Ollama** | 7 critical/high open, 2 closed | 3 PRs (metadata cache, MLX, memory) | No release | Moderate — stability crisis on CUDA/MLX |
| **LiteLLM** | 3 critical/high open, 4 resolved | 6+ PRs (UI, security, routing fixes) | v1.99.0-dev.1 | Moderate — enterprise/security polish |
| **Unsloth** | 6 critical/high open, 3 fixed | 9 PRs (Studio fixes, ROCm, certificates) | No release (v0.1.800-beta regressed) | Moderate — Studio stability triage |

---

## 3. Model Support Race

| Model/Architecture | First Mover | Lagging | Notable |
|-------------------|-------------|---------|---------|
| **Kimi-K3 (full)** | **SGLang** — NVFP4/FP8 mixed on Blackwell (#35077) | vLLM — ROCm AITER paths active but CUDA graph corruption (#52531) open | vLLM has ROCm parity effort; SGLang has production-ready quantization |
| **Kimi-K3 (ROCm)** | **vLLM** — AITER topk decode, sparse MLA (#52878, #52882) | SGLang — no explicit K3 ROCm mention today | vLLM explicit gfx950 targeting |
| **DeepSeek-V4 / V3.2** | **Tie** — vLLM DSA routing (#52861); SGLang MXFP8×BF16 MegaMOE (#35459), HiCache | llama.cpp — ROCm TOP_K crash blocks >128K (#27021) | SGLang has DSPARK+HiCache integration; vLLM has prefix cache bug (#42948) |
| **DeepSeek-V2-Lite (CPU)** | **vLLM** — full MLA prefill+decode (#51471) | Others — no CPU MLA equivalent | Unique vLLM capability |
| **GLM-5 / GLM-5.2** | **vLLM** — DSA routing, MTP (#52861, #48568 hang) | SGLang — no mention | vLLM has MTP hang on MI300X |
| **MiniMax-M3 / MiniMax-H3** | **SGLang** — ComfyUI video+audio (#35352) | vLLM — ROCm EAGLE3 decode (#52849) | SGLang leads multimodal integration |
| **Qwen3.6-35B MoE** | **llama.cpp** — SYCL +169% (#26689), but CUDA crash (#26609) | vLLM — tool calls + MTP bug (#46249); Ollama — layer overflow (#17856 fixed) | Fragmented: llama.cpp fast on Intel, broken on CUDA partial offload |
| **Qwen3.8** | **Ollama** — system message fix in review (#17855) | vLLM/SGLang — no explicit mention | Ollama has MLX-specific regression (#17829) |
| **Nemotron-H (Mamba-Transformer)** | **llama.cpp** — LoRA GGUF + Metal diag_mask (#27356, #27197) | vLLM/SGLang — SSM async scheduling restricted (#37285) | llama.cpp leads hybrid architecture support |
| **Gemma 4** | **llama.cpp** — MTP regression bisected (#24795) | Others — no mention | Actually a regression, not progress |

**Leaderboard:** SGLang edges ahead on **production-ready frontier model quantization** (Kimi-K3 NVFP4, MegaMOE); vLLM leads on **hardware breadth** (ROCm parity, CPU MLA); llama.cpp dominates **edge heterogeneity** (Metal, SYCL, Vulkan, OpenCL) but with critical CUDA/ROCm gaps.

---

## 4. Performance Frontier

| Optimization Domain | Leading Project | Key Innovation | Maturity |
|--------------------|-----------------|--------------|----------|
| **KV Cache Management** | **SGLang** — HiCache L3, deferred NIXL KV release (#35360) | Elastic PD with runtime role switching (#28403) | ⚠️ Pre-GA — hangs, cache miss storms (#34235, #35129) |
| | **vLLM** — extensible growable KV cache (#50779) | Eliminate pre-allocation overhead | 🔬 Draft stage |
| | **llama.cpp** — Vulkan Q8_0 dequant once (#25494) | Cooperative matrix prefill bandwidth reduction | ✅ Merged |
| **Speculative Decoding** | **vLLM** — DFlash2, PARD-2, EAGLE3 parallel drafts | Model-specific architectures, 3D split-KV attention (#52879) | 🔬 Active review |
| | **SGLang** — Ngram v2 overlap (#22332), AITER packed GQA (#35457) | Precompute/target overlap, AMD-optimized verify | 🔬 Draft/review |
| | **llama.cpp** — Adaptive MTP depth (#27210), DFlash2 (#27342) | Counting-based state machine, local convolution | 🔬 Active review |
| **Kernel Backends** | **vLLM** — AITER integration (gfx950) | Topk decode, batched GEMM, sparse indexer | 🚀 Landing daily |
| | **SGLang** — AITER direct-write BMM (#34498) | Eliminate transpose copy on MI355X | ✅ Merged |
| | **llama.cpp** — SYCL TILE kernel (#26689) | +42-169% on Battlemage; Metal i-quant decode | ✅ Merged |
| **Quantization** | **SGLang** — NVFP4/FP8 mixed, MXFP8×BF16, SiTU | Production checkpoint loading | ✅ Merged |
| | **vLLM** — MXFP4 variant (MiniMax-M3) | EAGLE3 speculative decode compatible | ✅ Merged |
| | **Ollama** — GGUF metadata caching (#17858) | ~300ms/request overhead elimination | 🚀 In review |
| **Batching/Scheduling** | **vLLM** — LoRA+MoE warp-shuffle (#52880) | Register-pressure tradeoff, single warp vs. K-loop | ✅ Merged |
| | **SGLang** — MM preprocessing worker pool (#35349) | Default 2 workers, tokenizer loop unblocking | ✅ Merged |

**Concentration:** The optimization frontier is **deeply split between NVIDIA-specific kernel fusion** (vLLM/SGLang competing on AITER/Cutlass) and **cross-platform backend hardening** (llama.cpp's SYCL/Vulkan/Metal). Notably, **no project has solved PD disaggregation reliability** — all have critical open bugs.

---

## 5. Layer Positioning

| Layer | Project | Role | Target User | Deployment Pattern |
|-------|---------|------|-------------|------------------|
| **Datacenter Serving Engine** | vLLM, SGLang | High-throughput, low-latency LLM inference with advanced scheduling (continuous batching, PD disaggregation, speculative decoding) | MLOps engineers, cloud providers | Kubernetes, bare-metal GPU clusters, multi-node TP/PP |
| **Local/Edge Runtime** | llama.cpp, Ollama | Consumer hardware inference, quantization-optimized, single-node | Application developers, end users | Desktop, mobile, embedded, Apple Silicon |
| **Gateway/Router** | LiteLLM | Unified API across providers, load balancing, cost tracking, enterprise governance | Platform engineers, AI application teams | Sidecar, proxy, multi-tenant control plane |
| **Training/Fine-tuning** | Unsloth | Memory-efficient LoRA/QLoRA, GGUF export, consumer GPU training | ML engineers, researchers | Desktop (Studio), notebooks, small clusters |

**Critical Distinction:** vLLM and SGLang are **converging on feature parity** (both do PD, both do speculative decode, both target AMD), but vLLM retains **ecosystem breadth** (CPU MLA, Rust frontend, multimodal) while SGLang pushes **quantization integration depth** (ModelOpt, mixed precision). Ollama's value is **distribution and UX**, not performance frontier — its sm_86 regression shows architectural risk from upstream dependency management. LiteLLM is the **only vendor-agnostic abstraction**, making it essential for multi-provider resilience but vulnerable to provider API drift.

---

## 6. Trend Signals

| Trend | Evidence | Implication for Developers |
|-------|----------|---------------------------|
| **AMD ROCm is now a first-class target, not trailing** | vLLM: 4 AITER PRs today; SGLang: direct-write BMM, packed GQA; llama.cpp: Strix Halo bugs being triaged | **Action:** Evaluate MI300X/MI355X for cost-sensitive inference; monitor gfx950/gfx1151 support matrices. NVIDIA lock-in is weakening at the serving layer. |
| **Speculative decoding is fragmenting, not standardizing** | DFlash2 (vLLM, llama.cpp), PARD-2 (vLLM), EAGLE3 (vLLM, SGLang), adaptive MTP (llama.cpp), Ngram v2 (SGLang) | **Action:** Abstract draft model selection behind configuration; do not hardcode checkpoint formats. Expect breaking changes in speculative decode pipelines quarterly. |
| **PD disaggregation is "almost ready" for 18+ months** | vLLM: Kimi-K3 corruption in NIXL (#52627); SGLang: deferred KV release (#35360) but hangs remain (#34235); both have active PD investment | **Action:** Treat PD as experimental for non-standard architectures (Mamba, reasoning models). Standard transformers with TP=1 may be viable; validate heavily. |
| **Quantization complexity is exploding** | NVFP4+FP8 mixed (SGLang), MXFP8×BF16 (SGLang), SiTU beta scaling, block scaling 128×128 | **Action:** Prefer frameworks with automatic mixed-precision loading (SGLang's #35077 pattern). Manual quantization config is becoming error-prone. |
| **Consumer/edge tools are maturing under pressure** | llama.cpp semantic versioning; Ollama metadata caching; Unsloth certificate handling | **Action:** Pin to explicit versions, not "latest." Expect stability regressions during rapid feature expansion. |
| **Security is moving from checkbox to infrastructure** | LiteLLM cosign signing; Unsloth certificate trust; LiteLLM credential leak (#36898) | **Action:** Verify supply chain (cosign, SBOMs); audit health endpoints and debug logs for credential exposure. |
| **Multimodal is a stability hazard** | vLLM: sampler regression from MM metadata placement (#52870/#52881); SGLang: 33/37 call sites bypassing worker pool (#35342) | **Action:** Pin to known-good commits for multimodal workloads; avoid `main` bleeding edge for production MM deployment. |

---

**Bottom Line:** The serving engine race (vLLM vs. SGLang) is the most dynamic segment, with AMD parity and speculative decoding as the current battlegrounds. For production deployments, **SGLang offers more mature quantization integration** while **vLLM offers broader hardware and model coverage**. The edge remains llama.cpp's domain, but its CUDA/ROCm critical bugs demand careful version pinning. LiteLLM's value increases as provider fragmentation grows, though its router bugs require defensive configuration. Unsloth remains a development tool, not a serving infrastructure component.

---

## Per-Project Reports

<details>
<summary><strong>vLLM</strong> — <a href="https://github.com/vllm-project/vllm">vllm-project/vllm</a></summary>

# vLLM Digest — 2026-08-19

## Today's Highlights

The vLLM project is experiencing intense activity around **Kimi-K3 and DeepSeek-V4 enablement on ROCm**, with multiple performance PRs landing today targeting AITER kernel integration for top-k, sparse MLA indexing, and attention decode paths. Simultaneously, **speculative decoding continues to expand** with DFlash2 and PARD-2 architectures entering review, while the **Rust frontend** gains post-thinking sampling parameters—signaling maturation beyond experimental status. A critical revert of multimodal metadata device placement (#52870/#52881) fixes H200 CI regressions, highlighting ongoing tension between feature velocity and stability in the multimodal stack.

---

## Releases & Breaking Changes

*No releases in the last 24h.*

---

## New Model & Hardware Support

| Item | Status | Details |
|------|--------|---------|
| **DeepSeek-V3.2 / GLM-5 DSA routing** | PR [#52861](https://github.com/vllm-project/vllm/pull/52861) | Routes `DeepseekV32ForCausalLM`, `GlmMoeDsaForCausalLM` and MTP drafts to CUDA non-compiled path on all NVIDIA GPUs; fixes unreachable optimized classes |
| **PARD-2 parallel draft models** | PR [#49406](https://github.com/vllm-project/vllm/pull/49406) | AMD-AGI/PARD target-aligned parallel speculative decoding; target-dependent draft architecture |
| **DFlash2 speculative decoder** | PR [#52816](https://github.com/vllm-project/vllm/pull/52816) | Adds grouped dynamic depthwise convolution + candidate selector; backward-compatible with DFlash checkpoints |
| **MiniMax-M3 on ROCm** | PR [#52849](https://github.com/vllm-project/vllm/pull/52849) | Enables AITER PA gluon decode for EAGLE3 speculative decoding on MXFP4 variant |
| **CPU MLA end-to-end** | PR [#51471](https://github.com/vllm-project/vllm/pull/51471) | DeepSeek-V2-Lite now runs complete prefill+decode on CPU with MLA, including LMCache external KV reload path |

---

## Performance & Optimization

| PR | Target | Improvement | Notes |
|----|--------|-------------|-------|
| [#52878](https://github.com/vllm-project/vllm/pull/52878) | DSv4 on ROCm | AITER topk decode replaces `torch.ops._C.top_k_per_row_decode`; AITER `batched_gemm_bf16` for small decode batches (T ≤ 32) | 3-part optimization, gfx950-focused |
| [#52882](https://github.com/vllm-project/vllm/pull/52882) | DSv4 C4A top-k | AITER v0.1.19 for short/medium contexts; tuned native fallback for long contexts | Graph-safe, correctness-adapted |
| [#50470](https://github.com/vllm-project/vllm/pull/50470) | Sparse indexer decode top-k | Replaces HIP radix kernel with AITER dispatcher | ~15× context-width scaling penalty vs. B200 eliminated |
| [#46172](https://github.com/vllm-project/vllm/pull/46172) | Sparse MLA indexer | Speed-of-light ATOM implementation for topk-token=2048 | Validated on aiter v0.1.16.post2 |
| [#52880](https://github.com/vllm-project/vllm/pull/52880) | LoRA + MoE | Hoist fused shrink reduction out of K loop | Single warp-shuffle vs. per-iteration; register-pressure tradeoff |
| [#52879](https://github.com/vllm-project/vllm/pull/52879) | Spec decode attention | Enables 3D split-KV path for `max_seqlen_q > 1` | Fixes [#48076](https://github.com/vllm-project/vllm/issues/48076); speculative decode latency win |

**In-progress:** Extensible growable KV cache ([#50779](https://github.com/vllm-project/vllm/pull/50779)) — draft stacked on #51718, would eliminate pre-allocated KV reservation overhead.

---

## Stability & Regressions

| Severity | Issue/PR | Description | Fix Status |
|----------|----------|-------------|------------|
| **Critical** | [#52531](https://github.com/vllm-project/vllm/issues/52531) | Kimi-K3 CUDA graph capture silently corrupts output at batch=1; three failure modes across cudagraph modes | **Open**, 7 comments, active investigation |
| **Critical** | [#52627](https://github.com/vllm-project/vllm/issues/52627) | Kimi-K3 silent output corruption in 1P1D NIXL Direct-PD disaggregated deployment | **Open**, 6 comments; NIXL-only PD clean, MultiConnector dirty |
| **High** | [#52870](https://github.com/vllm-project/vllm/pull/52870) / [#52881](https://github.com/vllm-project/vllm/pull/52881) | H200 sampler CI failure after #52827 multimodal metadata device placement | **Fix merged today** — full revert |
| **High** | [#42948](https://github.com/vllm-project/vllm/issues/42948) | Prefix-cache 0% hit on DeepSeek-V4-Flash reassignment; hybrid groups lose first-block cache keys | **Open**, 17 comments; DSv4 variant of [#32802](https://github.com/vllm-project/vllm/issues/32802) |
| **High** | [#41306](https://github.com/vllm-project/vllm/issues/41306) | v0.20 latency/throughput regression on MoE vs. v0.19; 8× H200 | **Open**, 13 comments; stale-tagged but active |
| **High** | [#48568](https://github.com/vllm-project/vllm/issues/48568) | GLM-5.2 MTP hangs on MI300X at first spec-decode step (RCCL peer transport) | **Open**, 8 comments |
| **Medium** | [#46249](https://github.com/vllm-project/vllm/issues/46249) | Qwen3.6-27B tool calls fail on Responses API when MTP enabled | **Open**, 13 comments; structured output + speculative decode interaction |
| **Medium** | [#37035](https://github.com/vllm-project/vllm/issues/37035) | `cudaErrorIllegalAddress` in `gdn_attn.py` with qwen3_next_mtp, num_speculative_tokens=5 | **Open**, 7 comments |
| **Medium** | [#52663](https://github.com/vllm-project/vllm/issues/52663) | FP8 on RDNA3 exceeds 600s engine-ready timeout; no tuned configs shipped | **Open**, 5 comments; community-verified functional but unoptimized |
| **Low** | [#52741](https://github.com/vllm-project/vllm/issues/52741) | OpenAI `strict` flag leaks into model-visible chat template | **Open**, 6 comments; behavioral not crash |

---

## What This Means for Application Developers

1. **ROCm is now a first-class target for frontier models** — Kimi-K3 and DeepSeek-V4 optimizations are landing in parallel with CUDA, not trailing by months. If you're on AMD MI300X/gfx950, monitor [#50682](https://github.com/vllm-project/vllm/issues/50682) and [#41820](https://github.com/vllm-project/vllm/issues/41820) for production readiness; the AITER integration velocity suggests ROCm parity is an explicit Q3/Q4 goal.

2. **Speculative decoding is fragmenting into model-specific architectures** — DFlash2, PARD-2, EAGLE3 each target different model families with incompatible checkpoint formats. If you're building generic serving infrastructure, abstract the draft model selection behind config; hardcoding DFlash for Qwen will break when DFlash2 checkpoints arrive.

3. **Disaggregated prefill/decode (PD) remains hazardous for production** — Two independent Kimi-K3 corruption issues in NIXL-based PD deployments this week, plus the SSM async-scheduling restriction ([#37285](https://github.com/vllm-project/vllm/issues/37285)), indicate PD is still pre-GA for non-standard transformer architectures. Validate heavily if your use case requires TP>1 or reasoning-trace stripping.

4. **Multimodal metadata placement is unstable** — Today's revert (#52870/#52881) shows the MM device-placement refactor broke samplers. If you're on `main` with multimodal workloads, pin to pre-#52827 or post-revert commits; avoid the intermediate range.

5. **Rust frontend gaining API surface** — Post-thinking sampling parameters (#52876) imply the Rust path is accumulating OpenAI-compatible behavioral nuances. Still experimental, but viable for latency-sensitive deployments where Python GIL matters; track [#44280](https://github.com/vllm-project/vllm/issues/44280) for feature parity gaps.

</details>

<details>
<summary><strong>SGLang</strong> — <a href="https://github.com/sgl-project/sglang">sgl-project/sglang</a></summary>

# SGLang Digest — 2026-08-19

## Today's Highlights

The community is pushing hard on **PD disaggregation reliability** and **multimodal infrastructure**: deferred KV release for NIXL landed to prevent abort-time corruption ([#35360](https://github.com/sgl-project/sglang/pull/35360)), while multimodal preprocessing finally gets proper worker pool routing after 33 of 37 call sites bypassed it ([#35342](https://github.com/sgl-project/sglang/pull/35342)). On the model front, **Kimi-K3 mixed NVFP4/FP8 checkpoint support** arrived for Blackwell ([#35077](https://github.com/sgl-project/sglang/pull/35077)), and **MXFP8×BF16 MegaMOE** is now supported for DeepSeek variants ([#35459](https://github.com/sgl-project/sglang/pull/35459)).

---

## Releases & Breaking Changes

**None today** — no releases in the last 24h.

---

## New Model & Hardware Support

| Item | PR/Issue | Details |
|------|----------|---------|
| **Kimi-K3 ModelOpt mixed NVFP4/FP8** | [#35077](https://github.com/sgl-project/sglang/pull/35077) | Official `nvidia/Kimi-K3-NVFP4` checkpoint now loads on Blackwell; routed MoE experts use NVFP4 with SiTU (`beta=4`, `linear_beta=25`), attention projections use `FP8_PB_WO` 128×128 block scaling. |
| **MXFP8 × BF16 MegaMOE** | [#35459](https://github.com/sgl-project/sglang/pull/35459) | New quantization path for DeepSeek-family MegaMOE architectures. |
| **Intel XPU: encoder embeddings + InternVL3_5** | [#35304](https://github.com/sgl-project/sglang/pull/35304) | `bge-base-en-v1.5`, `nomic-embed-text-v1.5`, `granite-embedding-english-r2`, and `InternVL3_5-30B-A3B` now supported on XPU. |
| **MiniMax-H3 in ComfyUI** | [#35352](https://github.com/sgl-project/sglang/pull/35352) | Video+audio generation node added to ComfyUI plugin with task-routed conditioning (`t2va`/`fl2va`/`ref2va`). |
| **PaddleOCR-VL docs clarified** | [#35458](https://github.com/sgl-project/sglang/pull/35458) | Documentation now explicitly states this serves only the VLM stage, not full OCR pipeline. |

---

## Performance & Optimization

| Item | PR/Issue | Details |
|------|----------|---------|
| **AMD: direct-write a8w8 BMM** | [#34498](https://github.com/sgl-project/sglang/pull/34498) | Eliminates `o_proj` transpose copy on gfx95/MI355X by emitting BMM output in correct layout; validated on `Kimi-K2.7-Code-MXFP4`. |
| **AMD: pack AITER target-verify GQA** | [#35457](https://github.com/sgl-project/sglang/pull/35457) | Follow-up to #34517: EAGLE target-verify on AITER backend loads shared KV head once per TP-local block, matching Triton optimization. |
| **Ngram spec v2: precompute + overlap** | [#22332](https://github.com/sgl-project/sglang/pull/22332) | Draft-stage PR to fully overlap draft precomputation with target forward pass. |
| **Multimodal preprocessing: default 2 workers** | [#35349](https://github.com/sgl-project/sglang/pull/35349) | `--mm-processor-workers` now defaults to 2, preventing tokenizer event loop blocking on image-heavy workloads. |
| **Mamba: FlashInfer SSD + Cake backends** | [#35444](https://github.com/sgl-project/sglang/pull/35444) | Strict opt-in `flashinfer_ssd` and `cake` prefill backends for Mamba SSD; Nemotron-H C256→C128 metadata projection. |

---

## Stability & Regressions

| Severity | Item | Issue/PR | Status |
|----------|------|----------|--------|
| **Critical** | **MoRI EP: silent output corruption at low concurrency** | [#27194](https://github.com/sgl-project/sglang/issues/27194) | Small `SGLANG_MORI_NUM_MAX_DISPATCH_TOKENS_PER_RANK` causes gsm8k=0 despite high speculative acceptance; **no fix PR yet**. |
| **Critical** | **DSV4 + HiCache + chunked prefill: scheduler hang** | [#34235](https://github.com/sgl-project/sglang/issues/34235) | Watchdog abort on DeepSeek-V4 FP8/H20; sampling device-side assert on 0.5.16+. **Under investigation.** |
| **High** | **HiCache L3 + PP inconsistency crash** | [#27010](https://github.com/sgl-project/sglang/pull/27010) | Fix PR open: adds cross-rank synchronization to prevent divergent PP ranks. |
| **High** | **DeepSeek-V4-Flash + DSPARK + HiCache: cache miss storm** | [#35129](https://github.com/sgl-project/sglang/issues/35129) | Long agentic sessions drop to 0% cache hit despite 50%+ prefix overlap; short requests fine. **No fix PR.** |
| **High** | **FlashAttention CUDA graph bounds overflow** | [#35454](https://github.com/sgl-project/sglang/pull/35454) | Fix PR open: aggregate batch tokens can exceed single-request `max_context_len`, causing sliding-window buffer overflow. |
| **Medium** | **SM10x kernels fail on B300 (sm_103)** | [#34340](https://github.com/sgl-project/sglang/issues/34340) | `is_sm100_supported()` family check incorrectly gates SM100-specific paths for sm_103; CutDSL TGV BF16 GEMM Xid 13, TRTLLM-gen MoE finalize hang. |
| **Medium** | **Mamba radix cache: prefix hit degradation** | [#22935](https://github.com/sgl-project/sglang/issues/22935) | Fresh prefill splits can turn valid prefix hits into 0-hit; **open, 6 upvotes.** |
| **Medium** | **Kimi-K3: [PAD] token storms + NaN logits** | [#32968](https://github.com/sgl-project/sglang/issues/32968) | Released image predates #32477 fix; `allowed_special="all"` injects [PAD]. **Workaround: use newer build.** |
| **Resolved** | EAGLE/NEXTN TP=2 hang on Intel XPU | [#35144](https://github.com/sgl-project/sglang/issues/35144) | Closed; caused by #34238 moving verify-decision broadcast. |
| **Resolved** | DeepGEMM shared memory limit on H20-3e | [#25484](https://github.com/sgl-project/sglang/issues/25484) | Closed; `paged_mqa_logits_metadata` JIT kernel exceeded limit. |

---

## What This Means for Application Developers

1. **If you're serving Kimi-K3 on Blackwell**: The official NVFP4 checkpoint now works out-of-the-box with SGLang — no manual quantization config needed. Mixed precision is handled automatically ([#35077](https://github.com/sgl-project/sglang/pull/35077)).

2. **If you're running agentic workloads with HiCache**: Two live risks to monitor — long sessions may hit cache miss storms with DSPARK ([#35129](https://github.com/sgl-project/sglang/issues/35129)), and DSV4 sparse prefill can hang with chunked prefill enabled ([#34235](https://github.com/sgl-project/sglang/issues/34235)). Consider disabling chunked prefill or pinning to known-good versions until fixes land.

3. **If you're using multimodal APIs**: The preprocessing worker pool is now actually usable — default 2 workers should eliminate tokenizer-loop blocking that caused TTFT spikes on image batches ([#35349](https://github.com/sgl-project/sglang/pull/35349), [#35342](https://github.com/sgl-project/sglang/pull/35342)).

4. **If you're on AMD MI355X**: AITER path optimizations continue (direct-write BMM, packed GQA verify), but verify you're not hitting the MoRI silent corruption bug at low batch sizes ([#27194](https://github.com/sgl-project/sglang/issues/27194)) — this manifests as plausible-looking but semantically wrong outputs.

5. **If you're doing PD disaggregation**: The NIXL backend now has deferred KV release to prevent abort-time page corruption ([#35360](https://github.com/sgl-project/sglang/pull/35360)), and runtime P:D role switching is in progress ([#28403](https://github.com/sgl-project/sglang/pull/28403)) for elastic rebalancing without server restart.

</details>

<details>
<summary><strong>llama.cpp</strong> — <a href="https://github.com/ggml-org/llama.cpp">ggml-org/llama.cpp</a></summary>

# llama.cpp Digest — 2026-08-19

## Today's Highlights

The project has begun formal semantic versioning with **v0.1.2** (though marked WIP), while build **b10485** is the current nightly. The most significant technical work today centers on **speculative decoding expansion** — adaptive MTP depth and DFlash2 support are both in active PR review — alongside continued backend hardening for Vulkan, SYCL, and Metal. Several critical correctness bugs remain open, particularly around ROCm TOP_K crashes on long-context DeepSeek V4 and CUDA illegal memory access with Qwen3.6 MoE.

---

## Releases & Breaking Changes

| Item | Details |
|------|---------|
| **v0.1.2** | First formal semantic version release, but explicitly marked work-in-progress. Discussion at [ggml#1579](https://github.com/ggml-org/ggml/discussions/1579). No breaking API changes noted. |
| **Nightly: b10485** | Standard ggml sync release with macOS Apple Silicon binaries. [Release](https://github.com/ggml-org/llama.cpp/releases/tag/b10485) |

---

## New Model & Hardware Support

| PR/Issue | Description |
|----------|-------------|
| [#27356](https://github.com/ggml-org/llama.cpp/pull/27356) | **Nemotron-H LoRA GGUF conversion fix** — resolves two sequential defects in `convert_lora_to_gguf.py` (hparams resolution and SSM architecture handling). Enables LoRA adapter workflows for NVIDIA's hybrid Mamba-Transformer architecture. |
| [#27342](https://github.com/ggml-org/llama.cpp/pull/27342) | **DFlash2 speculative decoding** — adds grouped dynamic depthwise convolution + candidate selector modules. Expands draft model capabilities beyond DFlash with local convolution-based candidate generation. |
| [#27197](https://github.com/ggml-org/llama.cpp/pull/27197) | **Metal: `GGML_OP_DIAG_MASK_INF` support** — closes gap in Metal backend for attention masking ops, tested via `test-backend-ops`. |
| [#27350](https://github.com/ggml-org/llama.cpp/pull/27350) | **Metal: i-quant support in `mul_mv_ext`** — enables IQ1/IQ2/IQ3/IQ4_XS in small-batch decode (BS 2–8), critical for speculative decoding where draft models use narrow batches. |
| [#26439](https://github.com/ggml-org/llama.cpp/pull/26439) | **OpenCL: fused SSM_SCAN kernel** — ports Mamba-2 (d_state 128/256, all-f32) to GPU; previously fell back to CPU. Mamba-1 and other shapes still CPU-bound. |

---

## Performance & Optimization

| PR | Change | Impact |
|----|--------|--------|
| [#26705](https://github.com/ggml-org/llama.cpp/pull/26705) | **CUDA: Branchless Q4_K/Q5_K scale unpack** | Eliminates nvcc predication overhead in `mmvq`; improves batch > 1 performance by avoiding per-column re-execution of scale unpack. |
| [#27341](https://github.com/ggml-org/llama.cpp/pull/27341) | **CUDA: Fuse FFN gate + GLU into MMQ epilogue** | Matches existing decode (`mul_mat_vec_q`) optimization for MMQ path; gate tensor avoids memory round-trip. Expected throughput gain on FFN-bound models. |
| [#26689](https://github.com/ggml-org/llama.cpp/pull/26689) | **SYCL: TILE kernel for quantized KV decode** | **+42% to +169%** on Qwen3.6-35B, Gemma 4 26B/12B at 32K/118K context on Battlemage (BMG); zero regressions. Replaces VEC kernel for q4_0/q8_0 KV. |
| [#25494](https://github.com/ggml-org/llama.cpp/pull/25494) | **Vulkan: Dequant Q8_0 KV once in coopmat1** | Removes 32× redundant dequantization in prefill FA; reorganizes F16 KV to per-head-contiguous scratch. Memory-bandwidth reduction for FA-bound prefill. |
| [#26585](https://github.com/ggml-org/llama.cpp/pull/26585) | **Vulkan: Tiled transpose for 0↔2 permute** | Optimizes `ggml_cont(ggml_permute(x, 2, 1, 0, 3))` via shared-memory shader; previously fell back to generic strided copy. |
| [#27210](https://github.com/ggml-org/llama.cpp/pull/27210) | **Adaptive MTP draft depth** | Counting-based state machine with climb counter + weighted drop-pressure accumulator. Config: `--spec-type draft-mtp-adaptive --spec-draft-n-max 12`. Reduces wasted draft tokens vs. fixed-depth MTP. |

---

## Stability & Regressions

| Severity | Issue | Summary | Status |
|----------|-------|---------|--------|
| **🔴 Critical** | [#27021](https://github.com/ggml-org/llama.cpp/issues/27021) | **ROCm TOP_K crash**: bitonic kernel block-size overflow when `ncols > 1024`; blocks DeepSeek V4 context > 128K on gfx1151 (Strix Halo). Invalid configuration argument. | **Open, no fix PR** |
| **🔴 Critical** | [#26609](https://github.com/ggml-org/llama.cpp/issues/26609) | **CUDA illegal memory access** in `cudaStreamSynchronize` (flash-attn path) with Qwen3.6-35B MoE + partial expert offload. Deterministic on second request of sequence. **Disappears with `-fa off`**. | **Open, no fix PR** |
| **🟠 High** | [#27102](https://github.com/ggml-org/llama.cpp/issues/27102) | **CUDA kernel stall / watchdog kill** on RTX Pro 6000 Blackwell MAX-Q during model execution. Qwen3.8-27B UD-Q8_K_XL. | **Open, help wanted** |
| **🟠 High** | [#25593](https://github.com/ggml-org/llama.cpp/issues/25593) | **SM_60 (Tesla P100) quality loss**: FP32 math silently done in FP16. Fix merged in two forks, not upstream. | **Open, fix available externally** |
| **🟠 High** | [#24795](https://github.com/ggml-org/llama.cpp/issues/24795) | **Gemma 4 MTP draft model regression**: "invalid vector subscript" on b9702+, works on b9553. Windows CUDA 13.3. | **Open, bisected** |
| **🟡 Medium** | [#24492](https://github.com/ggml-org/llama.cpp/issues/24492) | **Gemma 4 31B MTP crash on Vulkan**: pre-allocated tensor cannot run operation NONE. RX 7900 XTX. | **Open** |
| **🟡 Medium** | [#26257](https://github.com/ggml-org/llama.cpp/issues/26257) | **Qwen3.6-27B garbled output on dual-GPU CUDA** (RTX 5060 Ti + RTX 3060); single GPU works. | **Open** |
| **🟡 Medium** | [#25713](https://github.com/ggml-org/llama.cpp/issues/25713) | **MTP decoding crash on pre-Ampere** (SM_35/older); working patch provided by reporter. | **Open, patch available** |
| **🟢 Low/Tracked** | [#27253](https://github.com/ggml-org/llama.cpp/issues/27253) | SYCL backend model loading regression from b10451 (Ministral-3-14B). **Closed** — resolved or superseded. | **Closed** |

---

## What This Means for Application Developers

1. **Speculative decoding is maturing rapidly** — If you're running inference services, evaluate [#27210](https://github.com/ggml-org/llama.cpp/pull/27210) (adaptive MTP) and [#27342](https://github.com/ggml-org/llama.cpp/pull/27342) (DFlash2) in staging. Adaptive depth should reduce wasted compute from over-aggressive drafting, while DFlash2's local convolution may improve draft quality for code/math workloads.

2. **Avoid flash-attention with Qwen3.6 MoE + partial offload** — The [#26609](https://github.com/ggml-org/llama.cpp/issues/26609) crash is deterministic and severe. If you're serving Qwen3.6-35B-A3B or similar MoE architectures with CUDA, use `-fa off` until resolved. This particularly affects multi-turn chat applications where request sequences trigger the second-request pathology.

3. **ROCm long-context DeepSeek V4 is currently broken** — The TOP_K bitonic overflow ([#27021](https://github.com/ggml-org/llama.cpp/issues/27021)) hard-caps context at ~128K on Strix Halo/gfx1151. If your product advertises 200K+ context on AMD, you'll need to gate features or wait for a kernel fix.

4. **Metal gains parity for speculative decoding** — i-quant support in `mul_mv_ext` ([#27350](https://github.com/ggml-org/llama.cpp/pull/27350)) means Apple Silicon deployments can now use IQ-level quantized draft models without falling back to generic kernels. This directly improves tokens/second for speculative decode on Macs.

5. **Semantic versioning is coming** — Start pinning to explicit versions rather than build numbers. The v0.1.x series signals API stability efforts; monitor [ggml#1579](https://github.com/ggml-org/ggml/discussions/1579) for migration guidance.

</details>

<details>
<summary><strong>Ollama</strong> — <a href="https://github.com/ollama/ollama">ollama/ollama</a></summary>

# Ollama Digest — 2026-08-19

## Today's Highlights

Ollama's MLX backend is receiving urgent attention with a PR to update the engine and a critical bug report exposing zero prompt caching between agent steps, causing linear TTFT degradation. The project also faces a significant CUDA architecture regression where sm_86 GPUs (RTX 30/A40/A6000) silently fall back to CPU due to CUDA 13 builds omitting compute capability 8.6. Meanwhile, a major infrastructure PR landed to extract and cache GGUF metadata, unifying capability detection and eliminating per-request ~300ms overhead.

---

## Releases & Breaking Changes

*None today.*

---

## New Model & Hardware Support

| Item | Status | Details |
|------|--------|---------|
| **Qwen 3.8 support normalization** | In review | PR [#17855](https://github.com/ollama/ollama/pull/17855) fixes system message rendering for Qwen3.8 when conversation history contains system messages after model/base messages. Merges multiple system/developer instructions into one leading message. |
| **Intel GPU support** | Long-standing request | Issue [#3113](https://github.com/ollama/ollama/issues/3113) (75 👍, active since Mar 2024) remains open; no new movement. |
| **Legacy macOS support** | Declined | Issue [#17842](https://github.com/ollama/ollama/issues/17842) — macOS 12.7.2 user blocked by 14.0 minimum; no plans announced to support older versions. |

---

## Performance & Optimization

| PR/Issue | Impact | Details |
|----------|--------|---------|
| **PR [#17858](https://github.com/ollama/ollama/pull/17858)** — GGUF metadata extraction & unified capabilities | Eliminates ~300ms per-request overhead, fixes inconsistent capability detection | Extracts metadata once per blob to `<OLLAMA_MODELS>/metadata/sha256-<hex>.json`; replaces dual caches that produced divergent results. |
| **PR [#17752](https://github.com/ollama/ollama/pull/17752)** — Model metadata cache (closed, superseded by #17858) | Validated ~300ms per-request overhead reduction | Confirmed the performance problem; implementation absorbed into #17858. |
| **PR [#17857](https://github.com/ollama/ollama/pull/17857)** — llama-server memory accounting fix for multi-model loads | Fixes 10x+ VRAM under-reporting (30B model showed ~2.3GB instead of ~23GB) | Buffer line deduplication logic incorrectly replaced target model entries with speculative draft model entries. Critical for speculative decoding and hybrid-attention models. |
| **Issue [#17829](https://github.com/ollama/ollama/issues/17829)** — MLX engine: no prompt/prefix caching | **Severe regression**: full re-prefill every agent step, linear TTFT growth | 20-30K token prompts reprocessed from scratch on each multi-step agent request. Apple Silicon M1 Ultra, 128GB unified memory. No fix PR yet. |

---

## Stability & Regressions

| Severity | Item | Fix Status |
|----------|------|------------|
| **Critical** | **Issue [#17841](https://github.com/ollama/ollama/issues/17841)** — CUDA 13 builds omit sm_86; RTX 30/A40/A6000 silently CPU-fallback | No fix PR. Workaround: downgrade to CUDA 12 build or override arch flags. |
| **Critical** | **Issue [#17829](https://github.com/ollama/ollama/issues/17829)** — MLX prompt caching completely broken for agent workflows | No fix PR. PR [#17850](https://github.com/ollama/ollama/pull/17850) (MLX update) may address. |
| **High** | **Issue [#17856](https://github.com/ollama/ollama/issues/17856)** (closed) — qwen35moe layer count overflow (`n_layer=4294967274`, overflow_type=4) | **Fixed**: Affects qwen3.5/3.6 35B variants, Laguna XS 35B, Ornith 35B. Present since 0.31.x. |
| **High** | **Issue [#17833](https://github.com/ollama/ollama/issues/17833)** — v0.32.14 50-80% CPU spike despite 100% GPU-bound model | No fix PR. Regression from 0.32.13. |
| **High** | **Issue [#17847](https://github.com/ollama/ollama/issues/17847)** — ROCm KV state bleed across requests on Strix Halo (gfx1151) | No fix PR. Responses contaminated by previous request content. |
| **Medium** | **Issue [#17839](https://github.com/ollama/ollama/issues/17839)** — Agent integrations hang with local Qwen models on macOS; direct API works | No fix PR. Suggests client library/protocol mismatch in agent bridge. |
| **Medium** | **Issue [#17778](https://github.com/ollama/ollama/issues/17778)** — Qwen 3.8: "no user query found in messages" (500 error) at 205K context | No fix PR. Appears during tool-calling loops. |
| **Medium** | **Issue [#17782](https://github.com/ollama/ollama/issues/17782)** — ROCm TensileLibrary missing for gfx1200 (RX 9060 XT) | No fix PR. Missing AMD GPU architecture support in ROCm backend. |
| **Medium** | **Issue [#17837](https://github.com/ollama/ollama/issues/17837)** (closed) / **[#17859](https://github.com/ollama/ollama/issues/17859)** — CLI `/set think true\|false` accepted but backend rejects with 400 | **Partial fix**: Backend validation inconsistent with CLI; string "true"/"false" vs boolean mismatch. |
| **Medium** | **Issue [#17816](https://github.com/ollama/ollama/issues/17816)** — qwen3.8 download defunct (EOF on manifest pull) | No fix PR. Registry-side issue suspected. |
| **Low** | **Issue [#17860](https://github.com/ollama/ollama/issues/17860)** — install.sh fails silently on Ubuntu 26.04 without zstd | No fix PR. Empty `/usr/local/lib/ollama` directory created; poor error visibility. |
| **Low** | **Issue [#17251](https://github.com/ollama/ollama/issues/17251)** — `ollama ps` VRAM under-reports vs `rocm-smi` with MTP accelerator | No fix PR. 23GB reported vs actual higher allocation. |

---

## What This Means for Application Developers

1. **Verify your GPU architecture before upgrading to 0.32.14** — If you're on RTX 30-series, A40, or A6000 (sm_86), you may be silently downgraded to CPU inference. Check `ollama ps` PROCESSOR column against actual CUDA memory allocation via `nvidia-smi`. [Issue #17841](https://github.com/ollama/ollama/issues/17841)

2. **Avoid MLX for multi-step agent workloads until prompt caching is restored** — The MLX backend reprocesses full context on every request, causing linear latency growth. Use the llama.cpp backend on Apple Silicon as a workaround, or pin to an earlier version if MLX-specific optimizations are required. [Issue #17829](https://github.com/ollama/ollama/issues/17829)

3. **Audit Qwen 3.5/3.6/3.8 integrations for think-mode handling** — The `/set think` CLI/backend mismatch and format-ignored-when-think-disabled bugs suggest fragile state management around reasoning modes. Explicitly set think mode via API parameters rather than CLI commands, and validate JSON schema enforcement independently. [Issue #17837](https://github.com/ollama/ollama/issues/17837), [Issue #14645](https://github.com/ollama/ollama/issues/14645)

4. **Monitor for KV cache contamination with AMD ROCm** — If deploying on AMD GPUs (especially Strix Halo/gfx1151 or RX 9060 XT/gfx1200), implement response validation or force single-request serialization until state isolation is confirmed. [Issue #17847](https://github.com/ollama/ollama/issues/17847), [Issue #17782](https://github.com/ollama/ollama/issues/17782)

5. **Expect ~300ms latency improvement from metadata caching once #17858 lands** — For high-frequency inference workloads, this eliminates repeated GGUF parsing. No application changes required. [PR #17858](https://github.com/ollama/ollama/pull/17858)

</details>

<details>
<summary><strong>LiteLLM</strong> — <a href="https://github.com/BerriAI/litellm">BerriAI/litellm</a></summary>

# LiteLLM Digest — 2026-08-19

## 1. Today's Highlights

The v1.99.0-dev.1 release introduces Docker image signature verification via cosign, addressing supply-chain security. A major UI modernization effort is underway with **six active PRs** porting dashboard forms from antd to react-hook-form to fix mounted-field submission bugs and enable theming. Meanwhile, critical fixes are landing for the Responses API bridge (decrypting `previous_response_id` for stateful sessions) and Vertex AI batch cost calculations that were blocking CI.

---

## 2. Releases & Breaking Changes

| Version | Change | Link |
|---------|--------|------|
| **v1.99.0-dev.1** | Docker images now signed with cosign; verify with key from commit `0112e53` | [Release](https://github.com/BerriAI/litellm/releases/tag/v1.99.0-dev.1) |

**Security note for operators:** Existing verification workflows should update to use the documented cosign public key. No breaking API changes in this dev release.

---

## 3. New Model & Hardware Support

| Model/Provider | Status | Details | Link |
|----------------|--------|---------|------|
| **Z.AI** | Added to provider catalog | 6 new models added; provider search now matches `zai` slug | [PR #37433](https://github.com/BerriAI/litellm/pull/37433) |
| **GPT-5.6 family (Luna/Sol/Terra)** | Cost map fix landed for Bedrock Converse | Previously only `bedrock_mantle/` route had entries; regular Converse route now supported | [PR #37307](https://github.com/BerriAI/litellm/pull/37307) |
| **GPT-5.4 family (mini, mini Fast, Fast)** | Pending | Feature request open for ChatGPT Subscription provider entries | [Issue #25954](https://github.com/BerriAI/litellm/issues/25954) |
| **Llama-4-Scout** | Pending | Awaiting cost map update via OpenRouter | [Issue #28307](https://github.com/BerriAI/litellm/issues/28307) |

---

## 4. Performance & Optimization

| Topic | Status | Details | Link |
|-------|--------|---------|------|
| **Vertex AI batch pricing** | Fixed | Batch rates now correctly halved (gemini-3.6-flash); 4 PRs resolve test/CI regressions from the rate change | [PR #37444](https://github.com/BerriAI/litellm/pull/37444), [PR #37447](https://github.com/BerriAI/litellm/pull/37447), [PR #37448](https://github.com/BerriAI/litellm/pull/37448), [PR #37443](https://github.com/BerriAI/litellm/pull/37443) |
| **Streaming usage accuracy** | Fix in review | `prompt_tokens_details` and `completion_tokens_details` (cache-read, reasoning tokens) now preserved in stream chunk builder | [PR #36370](https://github.com/BerriAI/litellm/pull/36370) |
| **Proxy memory consumption** | Closed (won't fix / env tuning) | 16GB RAM maxed with 10 models loaded; typical for K8s deployment with large model registry cache | [Issue #31073](https://github.com/BerriAI/litellm/issues/31073) |

---

## 5. Stability & Regressions

| Severity | Issue | Status | Fix PR | Link |
|----------|-------|--------|--------|------|
| **🔴 Critical** | Adaptive router permanently bricks with `gammavariate: alpha and beta must be > 0.0` (persisted zero cell) | **Open** — no fix yet | None | [Issue #35590](https://github.com/BerriAI/litellm/issues/35590) |
| **🔴 Critical** | `GET /health` leaks `extra_headers` and `aws_session_token` in plaintext | **Open** | None | [Issue #36898](https://github.com/BerriAI/litellm/issues/36898) |
| **🟡 High** | Responses API bridge: empty output + `completion()` fails with "Unknown items in responses API response: []" for `chatgpt/gpt-5.4` | **Open** | None | [Issue #25429](https://github.com/BerriAI/litellm/issues/25429) |
| **🟡 High** | Virtual key `BudgetExceededError` uses stale spend while `/key/info` shows spend below budget | **Open** | None | [Issue #27735](https://github.com/BerriAI/litellm/issues/27735) |
| **🟡 High** | MCP auto-execute (`require_approval: "never"`) hijacks client-side `tool_use` from Claude Code | **Open** | None | [Issue #37031](https://github.com/BerriAI/litellm/issues/37031) |
| **🟡 High** | `/v1/messages` streaming emits duplicate `content_block_stop` → tools execute twice | **Closed** | ✅ Fixed | [Issue #37273](https://github.com/BerriAI/litellm/issues/37273) |
| **🟡 High** | Azure GPT-5 `reasoning_effort='none'` rejected due to `base_model`-ignorant capability gate | **Closed** | ✅ Fixed | [Issue #31243](https://github.com/BerriAI/litellm/issues/31243) |
| **🟡 High** | Bedrock OpenAI GPT-5.6 (us.openai.*) — Responses routing hang + missing transforms | **Closed** | ✅ Fixed | [Issue #37132](https://github.com/BerriAI/litellm/issues/37132) |
| **🟡 High** | Stateful `/v1/responses` chaining loses context when Responses ID security enabled | **Fix in review** | [PR #36360](https://github.com/BerriAI/litellm/pull/36360) | [PR #36360](https://github.com/BerriAI/litellm/pull/36360) |
| **🟡 High** | `use_chat_completions_api: true` drops `content` when provider returns `reasoning_content` | **Open** | None | [Issue #27492](https://github.com/BerriAI/litellm/issues/27492) |
| **🟢 Medium** | `auto_router/complexity_router` throws "Unmapped LLM provider" 400 | **Open** | None | [Issue #27473](https://github.com/BerriAI/litellm/issues/27473) |
| **🟢 Medium** | Salt key rotation unsupported (leaks in debug logs) | **Open** | None | [Issue #12448](https://github.com/BerriAI/litellm/issues/12448) |
| **🟢 Medium** | `gpt-5.1-mini/nano` reject `temperature` when `reasoning_effort` omitted (registry gap) | **Open** | None | [Issue #27351](https://github.com/BerriAI/litellm/issues/27351) |
| **🟢 Medium** | Bedrock `CountTokens` unsupported → LiteLLM silently returns understated token counts | **Open** | None | [Issue #37102](https://github.com/BerriAI/litellm/issues/37102) |
| **🟢 Medium** | Failed requests not counted toward RPM limits | **Open** | None | [Issue #21312](https://github.com/BerriAI/litellm/issues/21312) |
| **🟢 Low** | `disable_global_guardrails` key name mismatch (singular vs plural) | **Open** | None | [Issue #25487](https://github.com/BerriAI/litellm/issues/25487) |
| **🟢 Low** | `config.yaml` missing from OpenAPI spec/Swagger | **Open** | None | [Issue #16623](https://github.com/BerriAI/litellm/issues/16623) |

---

## 6. What This Means for Application Developers

| Action Item | Rationale |
|-------------|-----------|
| **Verify Docker image signatures** if pulling `v1.99.0-dev.1+` | New cosign signing is live; validate in CI/CD pipelines to prevent supply-chain compromise |
| **Avoid `adaptive_router` in production** until [Issue #35590](https://github.com/BerriAI/litellm/issues/35590) is fixed | A single bad state cell permanently 500s all requests with no recovery path |
| **Audit `GET /health` responses** for credential leakage | `extra_headers` and `aws_session_token` exposed in plaintext ([Issue #36898](https://github.com/BerriAI/litellm/issues/36898)) |
| **Use `bedrock_mantle/` route for GPT-5.6 on Bedrock cautiously** | Regular Converse route now supported ([PR #37307](https://github.com/BerriAI/litellm/pull/37307)), but verify tool-calling behavior matches expectations |
| **Test Claude Code + MCP auto-execute combinations carefully** | Server-side auto-execution conflicts with client-side tools ([Issue #37031](https://github.com/BerriAI/litellm/issues/37031)); consider `require_approval: "always"` for mixed-tool deployments |
| **Monitor streaming token accounting** if using cache-dependent pricing | Fix for detail preservation in-flight ([PR #36370](https://github.com/BerriAI/litellm/pull/36370)) |
| **Expect UI form behavior changes** in upcoming releases | antd → react-hook-form migration changes field submission semantics (mounted vs. full store); test any custom dashboard integrations |

</details>

<details>
<summary><strong>Unsloth</strong> — <a href="https://github.com/unslothai/unsloth">unslothai/unsloth</a></summary>

# Unsloth Digest — 2026-08-19

## Today's Highlights

Unsloth's Studio desktop and web UI saw intense bug-fixing activity with **9 PRs merged or opened** targeting focus management, certificate handling, and GPU memory reporting. A critical fix landed for Linux desktop users where self-signed certificates trusted by the OS were incorrectly rejected ([PR #9240](https://github.com/unslothai/unsloth/pull/9240)), while AMD ROCm users gained improved attention kernel support through AOTriton gating fixes ([PR #8821](https://github.com/unslothai/unsloth/pull/8821)). The issue tracker remains heavily weighted toward Studio stability, with 57 active issues and several new deadlock/crash reports requiring attention.

---

## Releases & Breaking Changes

**None.** No releases in the last 24 hours. The latest desktop version remains **v0.1.800-beta**, which introduced a regression in image transforms ([Issue #9241](https://github.com/unslothai/unsloth/issues/9241)) and an over-aggressive security scanner blocking all training models ([Issue #9239](https://github.com/unslothai/unsloth/issues/9239)).

---

## New Model & Hardware Support

| Item | Status | Details |
|------|--------|---------|
| **Intel XPU support documentation** | In review | [PR #9250](https://github.com/unslothai/unsloth/pull/9250) adds Intel GPU (XPU) support description to README; depends on [PR #9084](https://github.com/unslothai/unsloth/pull/9084) for XPU auto-detection and llama.cpp SYCL compilation |
| **ROCm AOTriton attention** | Merged | [PR #8821](https://github.com/unslothai/unsloth/pull/8821) opens the `TORCH_ROCM_AOTRITON_ENABLE_EXPERIMENTAL` gate for library users, enabling flash/mem-efficient SDPA on ROCm instead of falling back to MATH |
| **ROCm split Debian stack detection** | In review | [PR #8886](https://github.com/unslothai/unsloth/pull/8886) fixes ROCm version detection when Debian 13/LMDE exposes mismatched `hipconfig` and HSA runtime versions |

---

## Performance & Optimization

| Item | Status | Details |
|------|--------|---------|
| **Context length guard for unified memory** | In review | [PR #9172](https://github.com/unslothai/unsloth/pull/9172) refuses hand-set context lengths that exceed unified memory capacity, preventing kernel panics on Apple Silicon (reported on M1 Max 32GB with Qwen3.8-27B-UD-Q4_K_XL.gguf) |
| **Dedicated vs. shared VRAM distinction** | In review | [PR #9247](https://github.com/unslothai/unsloth/pull/9247) fixes Windows ROCm systems aggregating discrete VRAM + iGPU shared memory into one misleading "VRAM" figure; Vulkan path already flagged this, torch fallback did not |
| **CI apt timeout hardening** | Merged | [PR #9256](https://github.com/unslothai/unsloth/pull/9256) bounds every apt step with retry logic after three jobs lost their full budget to unbounded apt (30m+ each) |

---

## Stability & Regressions

| Severity | Issue / PR | Description | Fix Available |
|----------|-----------|-------------|---------------|
| **Critical** | [Issue #9008](https://github.com/unslothai/unsloth/issues/9008) | Studio server deadlock: all threads block in `sqlite3.connect()`/`close()`, stops accepting connections entirely; process stays alive but hangs | ❌ No fix yet |
| **Critical** | [Issue #9239](https://github.com/unslothai/unsloth/issues/9239) | Desktop v0.1.800-beta security scanner blocks **all** training models with "Custom code blocked / CRITICAL", including models with no custom code | ❌ No fix yet |
| **High** | [Issue #8610](https://github.com/unslothai/unsloth/issues/8610) | macOS app fails on second launch (Apple M4 Pro) | ❌ No fix yet |
| **High** | [Issue #9255](https://github.com/unslothai/unsloth/issues/9255) | Failed llama.cpp prebuilt install **silently** falls back to CPU-only build on CUDA hosts; GPU inference lost with no warning | ❌ No fix yet |
| **High** | [Issue #9254](https://github.com/unslothai/unsloth/issues/9254) | Installer rolls back venv after successful install when shell profile isn't writable; trap fires after success banner | ❌ No fix yet |
| **Medium** | [Issue #9241](https://github.com/unslothai/unsloth/issues/9241) | Image transforms broken in v0.1.800-beta: "Casting a quantized model to a new dtype is unsupported" | ❌ No fix yet |
| **Medium** | [Issue #9128](https://github.com/unslothai/unsloth/issues/9128) | `/embeddings` API rarely works in Studio | ❌ No fix yet |
| **Medium** | [Issue #9108](https://github.com/unslothai/unsloth/issues/9108) | Cannot run two consecutive web searches in same chat | ❌ No fix yet |
| **Low** | [Issue #9245](https://github.com/unslothai/unsloth/issues/9245), [Issue #9156](https://github.com/unslothai/unsloth/issues/9156) | Focus management bugs: menu dismissal drops focus to `<body>` instead of trigger | ✅ [PR #9243](https://github.com/unslothai/unsloth/pull/9243) addresses related bug |
| **Low** | [Issue #9244](https://github.com/unslothai/unsloth/issues/9244) | Sidebar "More" menu opens over modal dialogs | ✅ [PR #9248](https://github.com/unslothai/unsloth/pull/9248) in review |

**Fixed today:**
- [PR #9240](https://github.com/unslothai/unsloth/pull/9240): Linux desktop now trusts OS certificate store for self-signed certs
- [PR #9253](https://github.com/unslothai/unsloth/pull/9253): Test fix for overlay stack height counting
- [PR #9252](https://github.com/unslothai/unsloth/pull/9252): Security audit allowlist for `huggingface-hub` backoff loop

---

## What This Means for Application Developers

1. **Certificate handling on Linux desktop is now usable for internal gateways** — if you run Unsloth Studio against corporate or self-hosted endpoints with custom TLS, [PR #9240](https://github.com/unslothai/unsloth/pull/9240) removes the need for fragile `SSL_CERT_FILE` workarounds. Upgrade when this lands in the next beta.

2. **Avoid v0.1.800-beta for production training workflows** — the security scanner regression ([Issue #9239](https://github.com/unslothai/unsloth/issues/9239)) and image transform breakage ([Issue #9241](https://github.com/unslothai/unsloth/issues/9241)) make this release unsuitable for automated pipelines. Pin to v0.1.702-beta or earlier if stability is required.

3. **ROCm users should watch AOTriton + Debian detection PRs** — [PR #8821](https://github.com/unslothai/unsloth/pull/8821) and [PR #8886](https://github.com/unslothai/unsloth/pull/8886) together address the two most common causes of silent CPU fallback on AMD: disabled flash attention and misdetected ROCm version. If you're seeing unexpectedly slow inference on RX 7000-series or newer, these fixes are relevant.

4. **Studio as a service is not production-ready** — the SQLite deadlock ([Issue #9008](https://github.com/unslothai/unsloth/issues/9008)) and lack of systemd support ([Issue #9258](https://github.com/unslothai/unsloth/issues/9258)) mean Unsloth Studio should be treated as a development tool, not a headless inference server. For multi-model serving, [Issue #9257](https://github.com/unslothai/unsloth/issues/9257) requesting concurrent model loading with device pinning is still open with no timeline.

5. **Context length auto-detection is safer than manual tuning** — the M1 Max kernel panic ([PR #9172](https://github.com/unslothai/unsloth/pull/9172)) shows that "Auto" context sizing has guardrails manual entry lacks. If your application exposes context configuration to users, enforce memory-bounded validation or default to Auto.

</details>

---
*This digest is auto-generated by [agents-radar](https://github.com/brysoh435-dev/agents-radar).*