# AI Infrastructure Digest 2026-08-23

> Generated: 2026-08-23 03:30 UTC | Projects covered: 6

- [vLLM](https://github.com/vllm-project/vllm)
- [SGLang](https://github.com/sgl-project/sglang)
- [llama.cpp](https://github.com/ggml-org/llama.cpp)
- [Ollama](https://github.com/ollama/ollama)
- [LiteLLM](https://github.com/BerriAI/litellm)
- [Unsloth](https://github.com/unslothai/unsloth)

---

## Cross-Project Comparison

# AI Infrastructure Ecosystem Cross-Project Report — 2026-08-23

---

## 1. Ecosystem Overview

The AI inference infrastructure landscape is experiencing intense convergence around **DeepSeek-V4 ecosystem readiness**, with vLLM and SGLang racing to harden SM120/SM121 Blackwell support while llama.cpp expands its multi-modal and speculative decoding capabilities. **Production stability concerns dominate** — scheduler deadlocks, KV cache corruption, and health-check timeouts are forcing operators to pin older versions rather than chase latest releases. Meanwhile, **observability and cost accounting are maturing** (LiteLLM's cosigned images and cache-read pricing, vLLM's batch-invariant tuning), reflecting infrastructure's shift from "make it work" to "make it operable." The boundary between local runtimes and distributed serving engines continues to blur as projects like SGLang's Weight Cache Daemon and vLLM's PCP tensor parallelism push single-node optimizations into multi-tenant architectures.

---

## 2. Activity Comparison

| Project | Issues (Active/Critical) | PRs (24h/Recent) | Release Status | Maintenance Mode |
|---------|--------------------------|------------------|----------------|------------------|
| **vLLM** | 5 critical, 7 high open | Heavy: #41834, #53247, #53422, #53426 landed or active | No release 24h; v0.26.0 has showstoppers | Active development, production-hardening for Blackwell |
| **SGLang** | 2 critical, 4 high open | 7-PR config refactor series (#35904–#35917) | No release 24h; v0.5.18 has NVFP4+specdec crash | CI maintenance mode just closed; config system in flux |
| **llama.cpp** | 2 critical, 5 high open | b10585–b10588 (JSON refactor), #27524, #27535 | b10588 hotfix shipped | Steady cadence; Ubuntu ROCm CI restored |
| **Ollama** | 2 high, 2 medium open | #17943, #17929 (structured output), #17944 | No release 24h; 0.32.x series regressed | Regression triage mode |
| **LiteLLM** | 2 high, 5 medium open | #37926–#37997 (cost fixes), #37067 (new provider) | **v1.99.0-rc.1 + v1.98.0 shipped** | Release-active; supply-chain hardening |
| **Unsloth** | 3 high, 3 medium open | 128 active PRs; #9489, #9425, #9477, #9490 | No release 24h; accumulating for version bump | Studio stability sprint |

---

## 3. Model Support Race

| Model/Architecture | vLLM | SGLang | llama.cpp | Ollama | Unsloth |
|-------------------|:----:|:------:|:---------:|:------:|:-------:|
| **DeepSeek-V4 / V4-Flash** | ✅ SM12x full stack (#41834) | ✅ Verified, DSPARK fix closed | ✅ DSpark variant (#27508) | ⬜ Not mentioned | ⬜ Not mentioned |
| **DeepSeek-V3-style (GigaChat 3.5)** | ⬜ Not mentioned | ⬜ Not mentioned | ✅ MLA + hybrid attention (#25342) | ⬜ Not mentioned | ⬜ Not mentioned |
| **Qwen3 family (1.7B–235B)** | ✅ Tuned kernels (#53247), FP8 issue tracked | ✅ 235B weight cache, 27B NVFP4 verified | ✅ Qwen3.8-27B, Qwen3.6-27B | ✅ MLX structured output (#17929); cloud lag (#17926) | ⬜ Not mentioned |
| **Kimi-K3** | ⬜ Not mentioned | ❌ Crashing (#36018) | ⬜ Not mentioned | ⬜ Not mentioned | ⬜ Not mentioned |
| **Gemma 4 vision** | ⬜ Not mentioned | ⬜ Not mentioned | ⬜ Not mentioned | ❌ Bug: images not processed (#17931) | ⬜ Not mentioned |
| **Mistral3 multimodal** | ✅ Image placeholder fix (#52874) | ⬜ Not mentioned | ⬜ Not mentioned | ⬜ Not mentioned | ⬜ Not mentioned |
| **dots3-note (vision+audio)** | ⬜ Not mentioned | ⬜ Not mentioned | ✅ (#27524) | ⬜ Not mentioned | ⬜ Not mentioned |
| **Ling 3.0** | ⬜ Not mentioned | ⬜ Not mentioned | ⬜ Not mentioned | ⬜ Not mentioned | ⏳ Requested (#8532) |
| **BailingMoE3 / DSpark** | ⬜ Not mentioned | ⬜ Not mentioned | ✅ (#27508) | ⬜ Not mentioned | ⬜ Not mentioned |

**Ahead:** **vLLM** leads on DeepSeek-V4 production readiness with full SM12x kernel stack; **SGLang** leads on weight-cache elasticity for large-model serving; **llama.cpp** leads on multi-modal breadth and mobile/edge backends (Hexagon NPU, Vulkan, OpenCL).

---

## 4. Performance Frontier

| Optimization Domain | vLLM | SGLang | llama.cpp | Ollama | LiteLLM | Unsloth |
|---------------------|------|--------|-----------|--------|---------|---------|
| **KV Cache** | MLA single-KV-head + FP4 experts (#51454); PCP O-Proj TP (#53422) | NVFP4 KV + specdec ❌ broken; HiCache L3 + PP (#27010) | KV cache re-creation on FA disable (#26460) | `prompt_eval_cached_count` (#17943) | Cache-read pricing fix (#37996) | ⬜ Not mentioned |
| **Batching / Scheduling** | Batch-invariant persistent matmul (~3x decode, #53247); **scheduler deadlock** (#53130) | `/health` timeout orphans requests (#35884) | ⬜ Not mentioned | CPU fallback despite VRAM fit (#17833) | ⬜ Not mentioned | ⬜ Not mentioned |
| **Quantization** | FP8 checkpoint regression (#48035); INT32 overflow fix (#53409) | NVFP4 KV broken; ROCm AITER MXFP4 blocked | Q4_0 KV garbage output (closed); UD-Q8_K_XL | q4_0 KV fixed (closed); gemma4:e2b memory layout | ⬜ Not mentioned | ⬜ Not mentioned |
| **Speculative Decoding** | Ngram global cache (#44597); K=0 skip (#53426); MTP | KDA fused-accept (#33722); **NVFP4+specdec crash** | Adaptive MTP depth (#27210); DFlash2 dynamic conv (#27342) | ⬜ Not mentioned | ⬜ Not mentioned | ⬜ Not mentioned |
| **Distributed / Multi-Node** | MNNVL allreduce hang fix (#53377); Ray TP8 wedge (#53157) | MORI CP deduplication (#36025); NIXL P/D restart required | 3-GPU tensor split crash; dual-GPU garbled | ⬜ Not mentioned | ⬜ Not mentioned | ⬜ Not mentioned |
| **Kernel / Backend** | SM120/SM121 Blackwell hardening | AMD gfx1250 in progress; FlashInfer dequant | Vulkan fusion, OpenCL big.LITTLE, Metal ops | MLX backend maturing | ⬜ Not mentioned | ⬜ Not mentioned |

**Concentration:** **vLLM** and **SGLang** are both heavily invested in **KV cache efficiency for long-context, high-concurrency serving** (MLA, FP4/FP8, cache-aware scheduling), while **llama.cpp** pursues **edge-backend diversity** and **speculative decoding algorithmics**. **LiteLLM** uniquely focuses on **cost-accounting accuracy** as a performance-adjacent concern.

---

## 5. Layer Positioning

| Layer | Projects | Differentiation |
|-------|----------|---------------|
| **Distributed Serving Engine** | **vLLM**, **SGLang** | vLLM: production-proven at scale, broad model coverage, scheduler sophistication. SGLang: faster weight loading, structured generation focus, diffusion pipelines, but younger and less stable. |
| **Local / Edge Runtime** | **llama.cpp**, **Ollama** | llama.cpp: maximum backend portability (CUDA, Vulkan, Metal, OpenCL, SYCL, Hexagon), quantization flexibility, server + library. Ollama: opinionated UX wrapper, MLX-native on Apple, but constrained by its model-packaging ecosystem. |
| **LLM Gateway / Router** | **LiteLLM** | Universal API translation, multi-provider failover, cost tracking, enterprise features (SSO, rate limiting). Not a serving engine — sits above them. |
| **Training / Fine-tuning + Local Inference** | **Unsloth** | Studio-centric workflow (chat, fine-tune, export), qLoRA optimization, GGUF ecosystem integration. Inferior to dedicated engines for serving; competes on training efficiency and ease-of-use. |

**Tension:** vLLM and SGLang are converging on similar distributed-serving capabilities, but vLLM's maturity advantage is eroded by its **critical stability issues** (#53130, #53157). SGLang's config refactor and weight-cache daemon suggest it's **architecting for faster iteration** at the cost of present stability. llama.cpp remains **irreplaceable for edge deployment** but lacks the scheduler and batching sophistication for datacenter scale. Ollama occupies a **comfortable middle ground** that risks commoditization as MLX and llama.cpp improve their standalone server capabilities.

---

## 6. Trend Signals

| Signal | Evidence | Action for Developers |
|--------|----------|----------------------|
| **Blackwell/GB10 is the new optimization target** | vLLM SM12x stack (#41834), SGLang NVFP4 issues on RTX 5090, llama.cpp RTX Pro 6000 stall (#27102) | Validate on Ada/Hopper before migrating; expect 3–6 months of kernel stabilization |
| **Speculative decoding is becoming table stakes, but interactions are fragile** | vLLM K=0 skip + ngram cache + MTP; SGLang NVFP4+specdec crash; llama.cpp adaptive MTP + DFlash2 | Test greedy-sampling correctness on quantized targets; isolate draft-model failures from base model |
| **Prompt caching observability drives cost optimization** | vLLM prefix caching (with Mamba caveat), Ollama `prompt_eval_cached_count`, LiteLLM cache-read pricing | Instrument cache hit rates; negotiate pricing with providers; expect SLAs to emerge |
| **Config system complexity is a new failure mode** | SGLang 7-PR ServerArgs refactor, vLLM scheduler deadlock from state management | Audit your configuration pipelines; immutable config bags > mutation-at-construction |
| **Supply-chain security enters operational checklist** | LiteLLM cosign verification | Add image verification now; expect other projects to follow |
| **Agent-to-agent protocols (A2A) require casing normalization** | LiteLLM #37917 | Defensively normalize protocol fields; spec ambiguity will persist |
| **Multi-modal is expanding beyond "image in, text out"** | llama.cpp dots3-note vision+audio, vLLM Mistral3 fixes, SGLang diffusion pipelines | Plan for audio/video input in your architectures; format preprocessing (MOOV-at-end, WebP) matters |
| **"Fast recovery" beats "fast load" for elasticity** | SGLang 306s→1s weight cache daemon | Architect for model-swapping, not just model-loading; daemon-based weight residency |

**Bottom line:** The ecosystem is **capability-rich but stability-poor** at the bleeding edge. For production deployments, **pin proven versions** (vLLM v0.25.0, Ollama 0.32.13, LiteLLM v1.98.0+), **isolate speculative decoding combinations**, and **invest in observability** before chasing the last 20% of throughput optimization.

---

## Per-Project Reports

<details>
<summary><strong>vLLM</strong> — <a href="https://github.com/vllm-project/vllm">vllm-project/vllm</a></summary>

# vLLM Digest — 2026-08-23

## Today's Highlights

The vLLM community is heavily focused on **DeepSeek-V4 ecosystem hardening**, with multiple PRs targeting SM120/SM121 (Blackwell/GB10) kernel correctness, sparse MLA page sizes, and speculative decoding shape fixes. Separately, **batch-invariant inference** continues to mature with per-architecture tuned persistent matmul configs showing ~3x decode kernel improvements on RTX 4090D/H20, while a scheduler deadlock bug (#53130) demands attention from production operators.

---

## Releases & Breaking Changes

*None in the last 24h.*

---

## New Model & Hardware Support

| Item | Description | Link |
|------|-------------|------|
| **DeepSeek-V4 Flash on SM12x** | PR #41834 carries the full SM120/SM121 fallback and tuning stack for V1 path, targeting RTX PRO 6000 Blackwell, RTX 5090-class, and GB10/DGX Spark. Includes structured output, tool-calling, speculative decoding, and KV connector integration. | [#41834](https://github.com/vllm-project/vllm/pull/41834) |
| **Holo2 reasoning parser** | Rust frontend parity: adds `holo2` reasoning parser matching Python's `DeepSeekV3ReasoningWithThinkingParser` default behavior. | [#50763](https://github.com/vllm-project/vllm/pull/50763) |
| **Mistral3 image placeholder fix** | Processor now respects `mm_processor_kwargs.size.longest_edge` overrides instead of hardcoding `vision_config.image_size`. | [#52874](https://github.com/vllm-project/vllm/pull/52874) |

---

## Performance & Optimization

| Item | Details | Link |
|------|---------|------|
| **Batch-invariant persistent matmul tuning** | Per-architecture configs for Hopper (H20) and Ada (RTX 4090D) on Qwen3-1.7B shapes: ~3x decode kernel speedup via tuned `BLOCK_SIZE_M/N`, `num_warps`, `num_stages`. Falls back to generic config for untuned shapes. | [#53247](https://github.com/vllm-project/vllm/pull/53247) |
| **DeepSeek-V4 DP8 vs TP8** | Issue #51454 documents 7.7x KV cache efficiency and 3.4x faster 1M-context TTFT at concurrency=8 on 8×B200 with `v0.25.0`, using MLA single-KV-head with FP4 experts + FP8 KV. | [#51454](https://github.com/vllm-project/vllm/issues/51454) |
| **PCP O-Proj tensor parallelism** | Experimental `--enable-pcp-o-proj-tp` shards MLA output-projection over `TP × PCP`, preserving token-parallel PCP prefill. Decode path slices TP-local attention output; prefill uses fused local matmul. | [#53422](https://github.com/vllm-project/vllm/pull/53422) |
| **Ngram prompt lookup global cache** | Opt-in `prompt_lookup_cache_scope="global"` for speculative decoding: bounded process-local cache records ngram continuations across requests, improving hit rates for shared prompt prefixes. | [#44597](https://github.com/vllm-project/vllm/pull/44597) |
| **Dynamic spec-decode K=0 skip** | Opt-in to skip draft-model forward when Dynamic Speculative Decoding resolves K=0, reducing wasted compute. Stacked on #51575. | [#53426](https://github.com/vllm-project/vllm/pull/53426) |
| **FP8 checkpoint regression** | Official FP8 checkpoint ~40% slower at bs=1 than runtime FP8 quant of same model (Qwen3-4B, RTX 4090, v0.24.0); root cause under investigation. | [#48035](https://github.com/vllm-project/vllm/issues/48035) |

---

## Stability & Regressions

| Severity | Item | Status | Link |
|----------|------|--------|------|
| **Critical** | **Scheduler deadlock**: engine stops admitting requests when `running + skipped_waiting ("deferred")` reaches `max_num_seqs`; KV cache mostly free, only restart recovers. No fix PR yet. | Open | [#53130](https://github.com/vllm-project/vllm/issues/53130) |
| **High** | **Ray TP8 intermittent wedge**: `shm_broadcast` block starvation for ~60s → `RayWorkerProc rank=[0] died unexpectedly`; API stays up but generation dead. GLM-5.2-FP8, v0.26.0. | Open | [#53157](https://github.com/vllm-project/vllm/issues/53157) |
| **High** | **Hybrid Mamba illegal memory access**: prefix-cache resume with explicit `--block-size` seeds state column with wrong block size (W4A16, Qwen3.8-27B, SM86/RTX 3090). | Open | [#53142](https://github.com/vllm-project/vllm/issues/53142) |
| **High** | **MNNVL allreduce hang + memory leak**: 30s hang on IB-only multi-node during workspace init; fix gates workspace creation on fabric support. | **Fix PR #53377** | [#51986](https://github.com/vllm-project/vllm/issues/51986) |
| **Medium** | **INT32 overflow in fused SiLU block quant**: `token_idx * (2 * hidden_size)` wraps for large batches; causes illegal CUDA memory access. | **Fix PR #53409** | [#53390](https://github.com/vllm-project/vllm/issues/53390) |
| **Medium** | **Streaming input quadratic cost**: per-chunk arrival cost is O(cumulative prompt), adding ~150ms per turn in long multimodal sessions. | Open | [#52471](https://github.com/vllm-project/vllm/issues/52471) |
| **Medium** | **BF16 NVFP4 Marlin garbled output**: on GPUs without native FP4 support. | Open | [#34694](https://github.com/vllm-project/vllm/issues/34694) |
| **Low** | **Qwen3-VL-Embedding divergence**: pooling runner produces different embeddings than official `qwen_vl_utils`. | Open | [#33204](https://github.com/vllm-project/vllm/issues/33204) |

---

## What This Means for Application Developers

1. **DeepSeek-V4 on consumer Blackwell is approaching viable** — if you're targeting RTX 5090 or GB10 edge deployments, monitor PR #41834 and its dependencies (#53425, #52499, #52357, #53055). The SM12x path is still stabilization-heavy; production use should wait for the kernel shape and page-size fixes to land.

2. **Avoid v0.26.0 with Ray TP8 on large models** — the `shm_broadcast` wedge (#53157) and scheduler deadlock (#53130) are showstoppers for high-availability services. Consider v0.25.0 or pinned earlier versions until fixes merge.

3. **Prefix caching users with hybrid Mamba models** — explicit `--block-size` is unsafe until #53142 resolves; use default block sizing or disable prefix caching for Qwen3.5-122B-A10B and similar hybrid architectures.

4. **Speculative decoding gains are stackable** — combine ngram global cache (#44597), K=0 skip (#53426), and MTP (#52244) for compound latency reduction, but verify interaction with your target model's attention backend.

5. **AMD ROCm CI improvements** (#53355, #34994) suggest growing production readiness for MI325/MI355; no immediate action unless you're already on AMD, but worth tracking for TCO-sensitive deployments.

</details>

<details>
<summary><strong>SGLang</strong> — <a href="https://github.com/sgl-project/sglang">sgl-project/sglang</a></summary>

# SGLang Digest — 2026-08-23

---

## 1. Today's Highlights

The project is in active **CI maintenance mode** with a massive config-system refactor underway: ch-wan's 7-PR series ([#35904](https://github.com/sgl-project/sglang/pull/35904)–[#35917](https://github.com/sgl-project/sglang/pull/35917)) moves `ServerArgs` from mutation-at-construction to "resolve once, publish into config bags," touching quant, DeepSeek, speculative decoding, HiCache, NPU, and diffusion paths. Meanwhile, **NVFP4 KV cache + speculative decoding remains fundamentally broken** with two independent crash reports ([#36001](https://github.com/sgl-project/sglang/issues/36001), [#36010](https://github.com/sgl-project/sglang/issues/36010)) confirming the same FlashInfer dequant workspace `NoneType` error.

---

## 2. Releases & Breaking Changes

**No releases in the last 24h.**

**CI Maintenance Mode** ([#21065](https://github.com/sgl-project/sglang/issues/21065)) — **CLOSED** yesterday after being opened 2026-03-21. Normal PR CI runs have resumed, but the tracking issue shows 3 broken, 11 flaky tests as of this morning's auto-update ([#17050](https://github.com/sgl-project/sglang/issues/17050)).

**Config system refactor (breaking for downstream tooling):** The `ServerArgs` refactor changes how configuration is resolved and published. Downstream tools that depend on construction-time mutation or read partially-resolved state will need updates. The series lands bottom-up; [#35904](https://github.com/sgl-project/sglang/pull/35904) ("defensive publish must not re-project over a live process") is marked `ready-to-merge`.

---

## 3. New Model & Hardware Support

| Item | Status | Details |
|------|--------|---------|
| **AMD gfx1250** | In Progress | [#32754](https://github.com/sgl-project/sglang/pull/32754) — New GPU architecture with no MFMA/WMMA FP4 scale instructions, requiring custom paths in `sgl-kernel` JIT kernels. High priority, `bypass-fastfail`. |
| **ROCm AITER + FlyDSL MXFP4 MoE** | Blocked | [#35591](https://github.com/sgl-project/sglang/issues/35591) — ROCm images pin AITER below FlyDSL MXFP4 MoE kernels; tuned `AITER_CONFIG_FMOE` tables unusable on gfx950. Open with corrected analysis. |
| **Qwen3.8-27B NVFP4 / DGX Spark** | Verified | [#35860](https://github.com/sgl-project/sglang/issues/35860) — Playground cell verified for DGX-Spark DFlash2 single-GPU deployment. Cookbook page still shows "Final Verification In Progress" badge. |
| **Kimi-K3** | Crashing | [#36018](https://github.com/sgl-project/sglang/issues/36018) — New report in v0.5.18, no details yet. |

**Diffusion model improvements:**
- **LTX-2.5 decoder parallel tiling** ([#36026](https://github.com/sgl-project/sglang/pull/36026))
- **LingBot VAE decode optimization** ([#36024](https://github.com/sgl-project/sglang/pull/36024)) — FP32 resident for lossless, BF16 only for `quality=high`; 1x H200, 832×480×9, 4 steps
- **ComfyUI integrated mode refactor** ([#35960](https://github.com/sgl-project/sglang/pull/35960)) — DiT-only runtime profile on native pipelines, drops ~1450 lines of forked pipeline code

---

## 4. Performance & Optimization

| PR/Issue | Improvement | Numbers |
|----------|-------------|---------|
| **Weight Cache Daemon Phase 1** ([#33522](https://github.com/sgl-project/sglang/issues/33522), landed in [#27139](https://github.com/sgl-project/sglang/pull/27139)) | Fast engine recovery | Qwen3-235B FP8: **306–327s → <1s** weight load via per-rank daemon + CUDA IPC |
| **MORI CP deduplication** ([#36025](https://github.com/sgl-project/sglang/pull/36025)) | Reduce state transfer traffic | Eliminates `cp_size` multiplier on replicated state transfers |
| **HiCache L3 + PP consistency** ([#27010](https://github.com/sgl-project/sglang/pull/27010)) | Fix pipeline parallelism divergence | Two synchronization mechanisms for PP rank alignment |
| **Diffusion auto residency** ([#35335](https://github.com/sgl-project/sglang/pull/35335)) | Warmup-calibrated promotion | Eliminates needless layerwise offload on 80–140 GB GPUs |
| **KDA fused-accept state advance** ([#33722](https://github.com/sgl-project/sglang/pull/33722)) | FlashInfer KDA MTP verify | Fuses verify + commit scatter for `recurrent_kda` on SM100 |

---

## 5. Stability & Regressions

| Severity | Issue | Status | Fix PR |
|----------|-------|--------|--------|
| **🔴 Critical** | NVFP4 KV + speculative decoding: `extend_prefix_lens_cpu is None` in dequant workspace | **Two open reports** ([#36001](https://github.com/sgl-project/sglang/issues/36001), [#36010](https://github.com/sgl-project/sglang/issues/36010)) | None yet; affects SM120 (RTX 5090) with native MTP/NEXTN |
| **🔴 Critical** | NIXL P/D stops serving after one role replaced | Open ([#33789](https://github.com/sgl-project/sglang/issues/33789)) | None; requires full restart |
| **🟡 High** | `/health` timeout orphans scheduler requests, crashes paged-prefill batching | Open ([#35884](https://github.com/sgl-project/sglang/issues/35884)) | None |
| **🟡 High** | Qwen 3.8 27B NVFP4 incorrect coordinate detection on images | Open ([#35949](https://github.com/sgl-project/sglang/issues/35949)) | None |
| **🟡 High** | Kimi-K3 crash in v0.5.18 | Open ([#36018](https://github.com/sgl-project/sglang/issues/36018)), zero details | None |
| **🟡 High** | Diffusion LoRA auto mode static merge with post-load FP8 | Open ([#35970](https://github.com/sgl-project/sglang/issues/35970)) | None |
| **🟢 Medium** | DSPARK silently corrupts identifiers on DeepSeek-V4-Flash | **Fixed** ([#34959](https://github.com/sgl-project/sglang/issues/34959)) | Closed |
| **🟢 Medium** | DeepSeek-V4 slight accuracy issue | Closed stale ([#28414](https://github.com/sgl-project/sglang/issues/28414)) | None |
| **🟢 Medium** | DeepSeek V4 Pro hang on 8×H20-3e | Closed stale ([#26783](https://github.com/sgl-project/sglang/issues/26783)) | None |
| **🟢 Medium** | EAGLE3 CUDA graph illegal memory access on gpt-oss-120b | Closed stale ([#28569](https://github.com/sgl-project/sglang/issues/28569)) | None |

**CI infrastructure:** CUDA coredump tracker ([#26340](https://github.com/sgl-project/sglang/issues/26340)) auto-collecting from `pr-test.yml`; 238 comments. Unit test coverage drive ([#20865](https://github.com/sgl-project/sglang/issues/20865)) still open for core modules.

---

## 6. What This Means for Application Developers

1. **Avoid NVFP4 + speculative decoding** — This combination is confirmed broken across multiple hardware generations (SM120 RTX 5090 reported, likely broader). Use BF16/FP8 KV cache if you need draft-model speculative decoding. Watch [#36010](https://github.com/sgl-project/sglang/issues/36010) for resolution.

2. **NIXL P/D deployments need planned restarts** — Rolling replacement of prefill or decode roles breaks the pair; budget for full restarts in production ([#33789](https://github.com/sgl-project/sglang/issues/33789)).

3. **Health-check timeouts are dangerous** — The `/health` handler's timeout path leaks scheduler requests and can crash paged-prefill batching. Consider external health probes with shorter timeouts than internal scheduler deadlines ([#35884](https://github.com/sgl-project/sglang/issues/35884)).

4. **Config system changes affect custom integrations** — If you parse `ServerArgs` or depend on its construction behavior, audit against ch-wan's refactor series. The "config bags" abstraction is cleaner but changes the public surface.

5. **Diffusion pipeline is rapidly evolving** — ComfyUI mode is being refactored onto native pipelines ([#35960](https://github.com/sgl-project/sglang/pull/35960)); quality/BCG controls are being standardized ([#36016](https://github.com/sgl-project/sglang/pull/36016)). Expect breaking changes in diffusion APIs through v0.5.x.

6. **Weight Cache Daemon enables fast model swapping** — For multi-tenant or router-based deployments, the <1s recovery for Qwen3-235B-class models ([#33522](https://github.com/sgl-project/sglang/issues/33522)) dramatically improves elasticity. Consider architecture changes to exploit this once Phase 2+ lands.

</details>

<details>
<summary><strong>llama.cpp</strong> — <a href="https://github.com/ggml-org/llama.cpp">ggml-org/llama.cpp</a></summary>

# llama.cpp Digest — 2026-08-23

## Today's Highlights

The project landed a major JSON abstraction refactor (`common/json.h`) to reduce compile times and enable future backend swaps, alongside restoration of Ubuntu ROCm CI after a months-long outage. Multi-modal work accelerated with dots3-note vision+audio support and WebP via ffmpeg, while speculative decoding saw two new draft variants: adaptive MTP depth and DFlash2 with dynamic convolution.

---

## Releases & Breaking Changes

| Build | Change | Impact |
|-------|--------|--------|
| [b10588](https://github.com/ggml-org/llama.cpp/releases/tag/b10588) | `common: json.h: fix clang lto` (#27575) | Hotfix for LTO builds after b10585 refactor; no API change |
| [b10585](https://github.com/ggml-org/llama.cpp/releases/tag/b10585) | `common: add json.h abstraction` (#27511) | **Breaking for downstream forks**: `nlohmann::json` now pimpled via `common/json.h`; rename `json` → `common::json` required in custom code |

**Migration note**: Projects embedding llama.cpp server or common utilities must update JSON type references. The refactor reduces recompilation of `json.hpp` across translation units.

---

## New Model & Hardware Support

| Item | Details | PR/Issue |
|------|---------|----------|
| **DSpark for BailingMoE3** | Model architecture support | [b10581](https://github.com/ggml-org/llama.cpp/releases/tag/b10581), #27508 |
| **dots3-note vision+audio** | Multi-modal: image + audio input pipeline | [b10580](https://github.com/ggml-org/llama.cpp/releases/tag/b10580), #27524 |
| **WebP images via ffmpeg** | Replaces vendored decoder; no binary bloat | [#27520](https://github.com/ggml-org/llama.cpp/pull/27520) |
| **GigaChat 3.5 (432B MoE)** | DeepSeek-V3-style backbone, MLA + hybrid attention | [#25342](https://github.com/ggml-org/llama.cpp/pull/25342) |
| **Hexagon NPU (FastRPC)** | Alternative Qualcomm backend coexisting with dspqueue | [#27535](https://github.com/ggml-org/llama.cpp/pull/27535) |
| **Vulkan: PAD_REFLECT_1D** | New GGML op for Vulkan backend | [b10587](https://github.com/ggml-org/llama.cpp/releases/tag/b10587), #26586 |

---

## Performance & Optimization

| Work | Backend | Gain | Status |
|------|---------|------|--------|
| **Concat op: row-level memcpy** | CPU/GGML | Replaces per-element copy; reduces overhead for large tensor concatenation | [b10578](https://github.com/ggml-org/llama.cpp/releases/tag/b10578), #24575 |
| **Vulkan fuse UNARY+MUL** | Vulkan | Matches CUDA fusion for SiLU/Sigmoid/Softplus gates | [#27220](https://github.com/ggml-org/llama.cpp/pull/27220) |
| **CUDA: row-contiguous SUM_ROWS** | CUDA | Extends SUM_ROWS to strided tensors | [#26308](https://github.com/ggml-org/llama.cpp/pull/26308) |
| **CUDA: POOL_1D** | CUDA | 216/216 tests pass (avg/max), new op coverage | [#27573](https://github.com/ggml-org/llama.cpp/pull/27573) |
| **OpenCL: quant lm_head/decode GEMV** | OpenCL/Adreno | Skinny GEMM for speculative decoding/MTP | [#26477](https://github.com/ggml-org/llama.cpp/pull/26477) |
| **OpenCL: Intel Xe-LP tuning** | OpenCL/Intel | **+11-12% TG and PP** on NEO iGPUs | [#26438](https://github.com/ggml-org/llama.cpp/pull/26438) |
| **OpenCL: big.LITTLE thread affinity** | OpenCL/Adreno | Driver submission threads pinned to big cores (A6x/A7x) | [#26437](https://github.com/ggml-org/llama.cpp/pull/26437) |
| **Metal: DIAG_MASK_INF** | Metal | New op support, test-backend-ops clean | [#27197](https://github.com/ggml-org/llama.cpp/pull/27197) |
| **Adaptive MTP draft depth** | Server/SpecDec | Counting-based state machine, `--spec-draft-n-max 12` recommended | [#27210](https://github.com/ggml-org/llama.cpp/pull/27210) |
| **DFlash2: dynamic depthwise conv + candidate selector** | Server/SpecDec | Local convolution for improved draft quality | [#27342](https://github.com/ggml-org/llama.cpp/pull/27342) |
| **`--n-cpu-ffn` dense layer offloading** | CPU/GGML | Per-layer FFN CPU fallback analogous to `--n-cpu-moe` | [#26622](https://github.com/ggml-org/llama.cpp/pull/26622) |
| **KV cache re-creation on FA disable** | CUDA/All | Fixes layout mismatch when flash attention auto-resolves to disabled | [#26460](https://github.com/ggml-org/llama.cpp/pull/26460) |

---

## Stability & Regressions

| Severity | Issue | Backend | Status |
|----------|-------|---------|--------|
| 🔴 **Critical** | [#27102](https://github.com/ggml-org/llama.cpp/issues/27102): CUDA kernel stall → watchdog kill on RTX Pro 6000 Blackwell | CUDA | Open, 25 comments, repro on Qwen3.8-27B UD-Q8_K_XL |
| 🔴 **Critical** | [#27148](https://github.com/ggml-org/llama.cpp/issues/27148): RAM-backed prompt cache restores wrong conversation under concurrency | Server | Open, cross-slot cache contamination |
| 🟡 **High** | [#27579](https://github.com/ggml-org/llama.cpp/issues/27579): HIP/ROCm corrupted output on gfx1151 (Strix Halo); Vulkan correct with identical flags | HIP | Open, 3 comments, dual dense arch failure |
| 🟡 **High** | [#25692](https://github.com/ggml-org/llama.cpp/issues/25692): SYCL GPU hang (xe ccs reset) with FA + quantized KV under sustained load | SYCL | Open, Arc Pro B70 |
| 🟡 **High** | [#27063](https://github.com/ggml-org/llama.cpp/issues/27063): SYCL completely broken on A770 (works on B60) | SYCL | Open, 17 comments |
| 🟡 **High** | [#26257](https://github.com/ggml-org/llama.cpp/issues/26257): Qwen3.6-27B garbled on dual-GPU CUDA (5060 Ti + 3060), single GPU fine | CUDA | Open |
| 🟡 **High** | [#26837](https://github.com/ggml-org/llama.cpp/issues/26837): 3-GPU tensor split instant crash, 2-GPU works | CUDA | Open |
| 🟠 **Medium** | [#25618](https://github.com/ggml-org/llama.cpp/issues/25618): Speculative decoding diverges from vanilla on quantized targets (greedy) | SpecDec | Open, draft-MTP/draft-DSpark affected; ngram unaffected |
| 🟠 **Medium** | [#26088](https://github.com/ggml-org/llama.cpp/issues/26088), [#26239](https://github.com/ggml-org/llama.cpp/issues/26239): Gemma 4 `<unused49>` infinite loop after long idle/prompts | CUDA/HIP | Open, multiple quant sources affected |
| 🟠 **Medium** | [#25835](https://github.com/ggml-org/llama.cpp/issues/25835): VRAM leak with CUDA Graphs on V100 | CUDA | Open |
| 🟠 **Medium** | [#26435](https://github.com/ggml-org/llama.cpp/issues/26435): HIP prompt processing regression on MI210 | HIP | Open |
| 🟠 **Medium** | [#27479](https://github.com/ggml-org/llama.cpp/issues/27479): SYCL build failure (10556, ff14356e0) Fedora 44/Win11 | SYCL | Open, compiler errors |
| 🟢 **Low** | [#24394](https://github.com/ggml-org/llama.cpp/issues/24394), [#24429](https://github.com/ggml-org/llama.cpp/issues/24429): MP4 video input fails when MOOV atom at end; Windows deadlock | mtmd | Open, ffmpeg probe issue |

**CI/Build**: Ubuntu ROCm job restored ([#27399](https://github.com/ggml-org/llama.cpp/pull/27399)) with `ccache compiler_check=content` workaround for pip-installed toolchain freshness.

---

## What This Means for Application Developers

1. **Speculative decoding is maturing fast** — Adaptive MTP (#27210) and DFlash2 (#27342) give production deployments more knobs to trade draft quality vs. latency. If you're running quantized targets, monitor [#25618](https://github.com/ggml-org/llama.cpp/issues/25618) for greedy-sampling correctness; bf16 targets are unaffected.

2. **JSON refactor requires attention** — The `common/json.h` abstraction (#27511) is a compile-time win but will break custom server forks. Pin to b10588+ for the LTO fix if you build with Clang LTO.

3. **Multi-modal pipelines expanding** — dots3-note vision+audio (#27524) and WebP via ffmpeg (#27520) reduce format friction. Note the MP4 MOOV-at-end issue [#24394](https://github.com/ggml-org/llama.cpp/issues/24394) for video workflows; pre-process with `ffmpeg -movflags +faststart` as workaround.

4. **SYCL remains volatile** — Multiple open issues on Arc A770/B70/B60 across hangs, garbage output, and build failures. For Intel discrete GPU production, Vulkan may be more stable per [#27579](https://github.com/ggml-org/llama.cpp/issues/27579) (gfx1151). Test `--split-mode layer` vs. `tensor` carefully; P2P multi-GPU crashes reported [#27198](https://github.com/ggml-org/llama.cpp/issues/27198).

5. **CUDA Graphs + quantized KV on V100** — Memory leak [#25835](https://github.com/ggml-org/llama.cpp/issues/25835) suggests avoiding this combination for long-running services until fixed.

6. **New CPU offloading granularity** — `--n-cpu-ffn` (#26622) lets you offload dense FFN layers without full `--ngl` slowdown, useful for memory-constrained deployments with fast CPU RAM.

</details>

<details>
<summary><strong>Ollama</strong> — <a href="https://github.com/ollama/ollama">ollama/ollama</a></summary>

# Ollama Digest — 2026-08-23

## 1. Today's Highlights

The Ollama project saw significant activity around **prompt caching infrastructure** and **MLX backend maturity**: a new PR (#17943) adds cached prompt token reporting across native, OpenAI-compatible, and Anthropic-compatible APIs, while structured output support finally lands for the MLX runner (#17929) after a previous attempt (#17697) was closed. Multiple regressions in the 0.32.x series remain under active investigation, including non-stopping generation (#17910) and unexpected CPU fallback when models fit in VRAM (#17833).

---

## 2. Releases & Breaking Changes

**No releases in the last 24 hours.** No breaking API changes landed.

---

## 3. New Model & Hardware Support

| Item | Status | Details |
|------|--------|---------|
| **Qwen3.8-27B Cloud** | Requested | Community request (#17926, #17796) for cloud-hosted variant; no official tag yet |
| **Muse-Glimmer-30B** | Bug reported | HF-imported GGUFs derive incorrect `stop` parameters, truncating output to ~3 tokens (#17939) |
| **Gemma 4 vision** | Bug reported | Image attachments not being processed (#17931) |
| **RuadaptQwen3-8B-Hybrid** | Parser gap | HF-imported GGUF fails to split `<think>` blocks via `PARSER qwen3` (#17937) |
| **gfx1151 (Strix Halo) Vulkan** | Workaround known | `num_batch=128` avoids amdgpu compute-ring timeout on long-prompt prefill (#17870) |

---

## 4. Performance & Optimization

| PR/Issue | Description | Impact |
|----------|-------------|--------|
| **#17943** — Report cached prompt tokens | Adds `prompt_eval_cached_count` to responses; excludes cache hits from CLI/benchmark prefill rates | Enables accurate latency measurement; prerequisite for prompt cache pricing/optimization |
| **#17929** — MLX structured output support | Enforces `format` via xgrammar logit masking | Eliminates silent failures; brings MLX to parity with GGUF for constrained decoding |
| **#17833** — v0.32.14 CPU regression | 50-80% CPU spike despite 100% GPU-bound model in `ollama ps` | **Regression** — reverted to 0.32.13 as workaround; root cause under investigation |
| **#17386** — gemma4:e2b memory layout | Allocates disproportionately in CUDA_Host vs. VRAM on Windows | Memory pressure on 16GB RAM systems; no fix PR yet |

---

## 5. Stability & Regressions

| Severity | Issue | Status | Fix PR |
|----------|-------|--------|--------|
| **High** | **#17910** — Non-stopping generation (0.32.11–0.32.15) | Open | None; bisected to 0.32.9→0.32.11 window |
| **High** | **#17833** — CPU fallback despite VRAM fit (0.32.14) | Open | None; downgrade to 0.32.13 confirmed workaround |
| **Medium** | **#17484** — "context deadline exceeded" on HF models | Open | None; affects Qwen3.6-27B and others |
| **Medium** | **#17870** — amdgpu compute-ring timeout (gfx1151 Vulkan) | Open | Workaround: `num_batch=128` |
| **Medium** | **#17754** — `500 system message must be at the beginning` (qwen3.8:27b) | **Closed** | Fixed in release |
| **Medium** | **#17614** — Garbage output with q4_0 KV quantization | **Closed** | Fixed in release |
| **Medium** | **#17270** — `token repeat limit reached` abort (0.32.1) | **Closed** | Fixed in release |
| **Low** | **#17889** — Inconsistent HTTP 400/200 on over-length prompts | **Closed** | Behavior documented; #17944 adds docs |
| **Low** | **#17933** — Structured outputs silently ignored on MLX | **Closed** | Superseded by #17929 |

**Notable closed today:** #17822 (local API returning "Invalid API Key" with no cloud config), #17459 (Gemma 4 `<unused49>` tokens with `think=false`), #17932 (CloudFront pull reliability — redirected to registry mirror discussion).

---

## 6. What This Means for Application Developers

**Prompt caching is becoming observable.** PR #17943 adds `prompt_eval_cached_count` — if you're building cost-tracking or latency-budgeting logic, prepare to consume this field. The OpenAI-compatible `usage.prompt_tokens` and Anthropic-equivalent fields will expose cache hits separately from total tokens.

**MLX backend is approaching production parity for constrained decoding.** If you've been avoiding `format`/`response_format` on Apple Silicon, #17929 (xgrammar-based) resolves the silent-ignore behavior. Validate against your schemas — this was previously a no-op.

**Avoid 0.32.11–0.32.15 for long-generation workloads.** The non-stopping completion bug (#17910) has no fix yet and affects agentic use cases where you rely on natural stop conditions. Pin to 0.32.9 or 0.32.13 until resolved.

**HF-imported GGUFs need validation.** Two distinct issues (#17939 for stop parameters, #17937 for thinking parser) show that Ollama's auto-derived metadata for third-party GGUFs can be incorrect. Test with a known-good prompt before deploying new models.

**Cloud roadmap signal:** Multiple requests for Qwen3.8-27B and Muse-Glimmer cloud tags (#17796, #17926) suggest Ollama Cloud's model freshness is lagging community demand; plan fallback to local or alternative providers for latest open-weight releases.

---

*Digest generated from ollama/ollama activity on 2026-08-23. All links reference `https://github.com/ollama/ollama/issues/` or `/pull/` with the numbered suffix.*

</details>

<details>
<summary><strong>LiteLLM</strong> — <a href="https://github.com/BerriAI/litellm">BerriAI/litellm</a></summary>

# LiteLLM Digest — 2026-08-23

## Today's Highlights

LiteLLM v1.99.0-rc.1 and v1.98.0 shipped with **cosigned Docker images** for supply-chain verification, while the project saw heavy activity on **cost accounting fixes** (Azure cache-read pricing, compression/caching savings, unpriced request settlement) and **Bedrock/Anthropic translation layer** hardening for the Responses API. A critical security fix for UI JWT cookie replayability landed in the release cycle, and the A2A (Agent-to-Agent) protocol integration received a casing normalization fix for broader ecosystem compatibility.

---

## Releases & Breaking Changes

| Version | Notes |
|---------|-------|
| **[v1.99.0-rc.1](https://github.com/BerriAI/litellm/releases/tag/v1.99.0-rc.1)** | Release candidate; Docker images now signed with cosign (key introduced in [0112e53](https://github.com/BerriAI/litellm/commit/0112e53046018d726492c814b3644b7d376029d0)). Verify via `cosign verify`. |
| **[v1.98.0](https://github.com/BerriAI/litellm/releases/tag/v1.98.0)** | Stable release with same cosign infrastructure. No explicit breaking changes in release notes. |

**Action required for ops teams:** Add cosign verification to your image pull pipeline; the public key is consistent across releases starting 0112e53.

---

## New Model & Hardware Support

| PR/Issue | Description |
|----------|-------------|
| **[#37067](https://github.com/BerriAI/litellm/pull/37067)** | **RunInfra provider**: New `runinfra` OpenAI-compatible provider registration with cost map entries for four hosted models. Enables routing + spend tracking for this inference platform. |
| **[#37995](https://github.com/BerriAI/litellm/pull/37995)** | **Bedrock native `web_search` tool**: Forwards Bedrock's built-in web search tool on Responses API calls; previously dropped silently, causing ungrounded answers and broken Codex 0.148 web search. |

---

## Performance & Optimization

| PR/Issue | Description |
|----------|-------------|
| **[#37926](https://github.com/BerriAI/litellm/pull/37926)** | **Cost-based routing savings fix**: `compression_savings_spend` and `prompt_caching_savings_spend` now correctly read deployment-level cost maps instead of returning `$0.00` for custom/self-hosted models or multi-deployment `model_name` groups. |
| **[#37927](https://github.com/BerriAI/litellm/pull/37927)** | **Unpriced request budget settlement**: Successful calls with missing `response_cost` now settle at `reserved_cost` rather than zero, preventing budget overruns when pricing is unavailable. |
| **[#37996](https://github.com/BerriAI/litellm/pull/37996)** | **Azure cache-read pricing**: Adds `cache_read_input_token_cost` (`1.375e-06`) for `azure/us/gpt-4o-2024-11-20` and `azure/eu/gpt-4o-2024-11-20`, matching the 2024-08-06 data-zone entries. Previously billed at $0. |
| **[#37728](https://github.com/BerriAI/litellm/pull/37728)** | **DeepSeek log noise reduction**: Aggregates `reasoning_content` placeholder warnings from one-per-message to one-per-request, reducing log volume by 6-10x on long conversations. |

---

## Stability & Regressions

| Severity | Issue/PR | Description | Fix Status |
|----------|----------|-------------|------------|
| **High** | **[#35664](https://github.com/BerriAI/litellm/issues/35664)** (closed) | **Security: UI JWT cookie contained reusable API key material** (`key` claim). Copied cookies could authenticate arbitrary sessions. | **Fixed in v1.94.0+** |
| **High** | **[#36922](https://github.com/BerriAI/litellm/issues/36922)** | **Proxy fails to start** after `uv tool update` due to FastAPI `get_flat_dependant` incompatibility (v1.96.2). Blocks clean installs. | **Open** — no PR linked |
| **High** | **[#34614](https://github.com/BerriAI/litellm/issues/34614)** | **Redis cache broken in v1.93.0**: `ssl_check_hostname` TypeError aborts caching and budget counters. | **Open** — affects Docker `v1.93.0` |
| **Medium** | **[#27954](https://github.com/BerriAI/litellm/issues/27954)** | **Memory leak**: Heavy RAM growth over time on Kubernetes, eventual OOM crash. Reported May, still active with 11 comments. | **Open** — under investigation |
| **Medium** | **[#37997](https://github.com/BerriAI/litellm/pull/37997)** | **S3 v2 logging regression (v1.97.0)**: Object keys with `=`, `+`, `&`, `#`, `?` either 403 or collide due to double encoding. Session log keys hit this frequently. | **PR open** |
| **Medium** | **[#37917](https://github.com/BerriAI/litellm/pull/37917)** | **A2A protocol breakage (v1.97)**: Lowercase `protocolBinding` (e.g., LangGraph Platform) fails transport match with "no compatible transports found." | **PR open** |
| **Medium** | **[#37140](https://github.com/BerriAI/litellm/issues/37140)** | **Non-streaming requests never cancel upstream work** on client disconnect. Wasted compute and spend. Streaming fixed in #30244/#30245; non-streaming path still broken. | **Open** |
| **Medium** | **[#37877](https://github.com/BerriAI/litellm/issues/37877)** | **Provider budget ignores embeddings/rerank**: `RouterBudgetLimiting` raises `custom_llm_provider is required`, causing all `/v1/embeddings` and `/v1/rerank` calls to fail budget enforcement. | **Open** |
| **Low** | **[#28706](https://github.com/BerriAI/litellm/issues/28706)** | **Anthropic rejects whitespace-only system messages**: Should be dropped in translation layer; instead forwarded and rejected. | **Open** |
| **Low** | **[#28553](https://github.com/BerriAI/litellm/issues/28553)** | **Azure Responses API streaming fails**: `stream_options.include_usage` rejected as unknown parameter, blocking Codex usage. | **Open** |

---

## What This Means for Application Developers

1. **Verify your Docker images now.** The cosign infrastructure is live; add verification to your CI/CD to prevent supply-chain compromises. The key is stable starting v1.98.0.

2. **Audit cost tracking if you use Azure data-zone models or custom deployments.** The cache-read pricing fix for `gpt-4o-2024-11-20` and the compression/caching savings fixes correct undercharging bugs that could mask true spend. Conversely, the unpriced-request settlement fix means calls without pricing data now consume budget reserves—monitor for unexpected budget exhaustion.

3. **Avoid v1.93.0-v1.96.2 for new deployments.** Redis caching is broken in v1.93.0; FastAPI startup crashes hit v1.96.2 post-uv-update. Prefer v1.98.0+ or pin dependencies carefully.

4. **Bedrock + Codex users:** The native `web_search` tool forwarding fix (#37995) resolves silent failures where Codex 0.148 web searches returned ungrounded answers. Upgrade to get citations and grounded responses.

5. **A2A/Agent developers:** If you hit "no compatible transports found" with LangGraph or other lowercase `protocolBinding` agents, the fix is in PR #37917—apply or wait for next patch.

6. **Non-streaming workloads:** Client disconnects do not abort upstream LLM work. Implement client-side timeouts and idempotency keys; do not rely on connection drop to save tokens.

---

*Generated from github.com/BerriAI/litellm activity on 2026-08-23.*

</details>

<details>
<summary><strong>Unsloth</strong> — <a href="https://github.com/unslothai/unsloth">unslothai/unsloth</a></summary>

# Unsloth Digest — 2026-08-23

## Today's Highlights

The Unsloth team is pushing hard on Studio stability ahead of a broader release, with 128 active PRs and heavy focus on chat reliability. Key fixes landed for conversation state corruption—consecutive user messages after cancelled prompts ([#9489](https://github.com/unslothai/unsloth/pull/9489)), duplicate auto-continuations across tabs ([#9425](https://github.com/unslothai/unsloth/pull/9425)), and long-thinking stream performance degradation ([#9477](https://github.com/unslothai/unsloth/pull/9477)). Meanwhile, new feature work includes inline web search images ([#9490](https://github.com/unslothai/unsloth/pull/9490)) and expanded API monitoring for multimodal endpoints ([#9217](https://github.com/unslothai/unsloth/pull/9217)).

---

## Releases & Breaking Changes

**None today.** No new releases in the last 24h. The project appears to be accumulating changes for a larger version bump given the PR volume.

---

## New Model & Hardware Support

| Item | Status | Details |
|------|--------|---------|
| **Ling 3.0** | Requested ([#8532](https://github.com/unslothai/unsloth/issues/8532)) | Community feature request for full Studio support (download, load, configure, serve). 5 upvotes, 6 comments. No implementation yet. |
| **Intel XPU** | Broken ([#9524](https://github.com/unslothai/unsloth/issues/9524)) | qLoRA training fails with cross-device tensor error (`xpu:0` vs `cpu`). No fix PR. |
| **Speech GGUFs** | Filtered out ([#9433](https://github.com/unslothai/unsloth/pull/9433)) | Unrunnable TTS GGUFs now excluded from model pickers to prevent user confusion. |

---

## Performance & Optimization

| PR | Impact | Details |
|----|--------|---------|
| [#9477](https://github.com/unslothai/unsloth/pull/9477) | **Critical UX fix** | Long thinking traces caused single-digit FPS and multi-second click latency in Tauri/WebKitGTK builds. Hard bounds now placed on live reasoning DOM and Markdown re-rendering. |
| [#9251](https://github.com/unslothai/unsloth/pull/9251) | Visual polish | Prevents Studio reloads from flashing blank between document swap. |
| [#9325](https://github.com/unslothai/unsloth/pull/9325) | Startup efficiency | Skips unnecessary llama-server binary discovery when no launch revision exists. |

---

## Stability & Regressions

| Severity | Issue / PR | Description | Fix Status |
|----------|-----------|-------------|------------|
| **High** | [#9033](https://github.com/unslothai/unsloth/issues/9033) | System RAM not released after GGUF load with full VRAM offloading (`-ngl -1`). Windows Desktop. | Open, 3 comments |
| **High** | [#9518](https://github.com/unslothai/unsloth/issues/9518) | Chat IndexedDB persistence fails on every message send (AppImage, all models). Backend healthy. | Open, 1 comment |
| **High** | [#9500](https://github.com/unslothai/unsloth/issues/9500) | Presets fail to save with 400 on `PUT /api/chat/settings`. | Open, 1 comment |
| **Medium** | [#9516](https://github.com/unslothai/unsloth/issues/9516) | GGUF export to HF fails with Cloudflare 524 (timeout); LoRA export works. Suggests large payload handling issue. | Open, 1 comment |
| **Medium** | [#9537](https://github.com/unslothai/unsloth/issues/9537) | Desktop app reads/downloads all model files from synced drives on startup. Windows 11. | Open, 0 comments |
| **Medium** | [#9484](https://github.com/unslothai/unsloth/issues/9484) / [#9489](https://github.com/unslothai/unsloth/pull/9489) | Cancelled prompts produce consecutive user messages, breaking strict Jinja templates. | **Fix PR open** |
| **Medium** | [#9453](https://github.com/unslothai/unsloth/issues/9453) | Linux AppImage freeze on Model Hub tab (0.1.801-beta). | Closed—regression in 801, likely fixed in 802+ |
| **Low** | [#9519](https://github.com/unslothai/unsloth/issues/9519) | Duplicate LAN access settings in UI (settings + API sections). | Open, cosmetic |
| **Low** | [#9510](https://github.com/unslothai/unsloth/issues/9510) | Agent tasks cancel when switching to API/settings page; unnecessary model reload on task page. | Open |

**Closed today:** [#9521](https://github.com/unslothai/unsloth/issues/9521) (missing exports causing Vite build error), [#9032](https://github.com/unslothai/unsloth/issues/9032) (RAM spike during model download), [#6329](https://github.com/unslothai/unsloth/issues/6329) (privacy questions on web search), [#9453](https://github.com/unslothai/unsloth/issues/9453) (AppImage freeze).

---

## What This Means for Application Developers

1. **Build on the API, not the Studio UI, for stability.** The chat persistence layer (IndexedDB) and settings API have active breakage. If you're integrating Unsloth as a backend, use the OpenAI-compatible `/v1/chat/completions` endpoint directly—confirmed working in [#9518](https://github.com/unslothai/unsloth/issues/9518)—rather than relying on Studio's chat chrome.

2. **Watch the conversation state fixes closely.** [#9489](https://github.com/unslothai/unsloth/pull/9489) and [#9425](https://github.com/unslothai/unsloth/pull/9425) resolve subtle but critical bugs in chat history management. If you're building agents that manipulate conversation threads programmatically, these edge cases (empty assistant turns, cross-tab continuation races) may have affected your integrations.

3. **LAN/API binding is improving but incomplete.** [#9522](https://github.com/unslothai/unsloth/issues/9522) requests configurable bind address (currently hardcoded to `127.0.0.1`); [#9519](https://github.com/unslothai/unsloth/issues/9519) notes duplicate UI controls. For multi-machine or WSL setups, expect this to unblock soon—monitor [#9522](https://github.com/unslothai/unsloth/issues/9522).

4. **Multimodal API monitoring is expanding.** [#9217](https://github.com/unslothai/unsloth/pull/9217) adds visibility into `/v1/images/generations`, `/v1/audio/speech`, and `/v1/audio/transcriptions` in the API monitor. If you're building apps using these endpoints, you now have better observability into traffic and latency.

5. **Intel XPU remains unsupported for training.** The qLoRA tensor offloading bug ([#9524](https://github.com/unslothai/unsloth/issues/9524)) is unassigned—stick to CUDA or ROCm for production training workloads.

</details>

---
*This digest is auto-generated by [agents-radar](https://github.com/brysoh435-dev/agents-radar).*