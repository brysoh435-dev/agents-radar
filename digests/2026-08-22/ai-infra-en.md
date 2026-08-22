# AI Infrastructure Digest 2026-08-22

> Generated: 2026-08-22 03:08 UTC | Projects covered: 6

- [vLLM](https://github.com/vllm-project/vllm)
- [SGLang](https://github.com/sgl-project/sglang)
- [llama.cpp](https://github.com/ggml-org/llama.cpp)
- [Ollama](https://github.com/ollama/ollama)
- [LiteLLM](https://github.com/BerriAI/litellm)
- [Unsloth](https://github.com/unslothai/unsloth)

---

## Cross-Project Comparison

# AI Infrastructure Ecosystem Cross-Project Report — 2026-08-22

---

## 1. Ecosystem Overview

The AI inference infrastructure landscape is consolidating around **disaggregated prefill/decode architectures** as the next production frontier, with vLLM and SGLang both investing heavily in KV cache connectors, router safety, and tier-aware cache events. **Multimodal autoregressive models** (vision+audio towers, MoE vision backbones) are becoming first-class citizens across all runtimes, while **speculative decoding** continues to fragment into competing approaches—DFlash2 dynamic convolution, EAGLE3, adaptive MTP draft depth—with no clear standard emerging. Hardware support remains uneven: Blackwell (sm_100/sm_120) and AMD MI355X see active kernel development but suffer from critical stability issues, while Ampere (sm_80/86) faces growing deprecation pressure as newer model architectures assume Hopper+ capabilities. The gateway layer (LiteLLM) is maturing on security and cost attribution, but routing correctness bugs persist at scale.

---

## 2. Activity Comparison

| Project | Issues (Active/Critical) | PRs (24h / Recent) | Release Status | Notes |
|---------|-------------------------|-------------------|----------------|-------|
| **vLLM** | 7 critical/high open, 3 medium with PRs ready | 6+ performance/stability PRs referenced; active daily | No release in 24h; v0.6.x implied | Highest issue severity density; production blockers on V1 engine + MTP |
| **SGLang** | 5 high, 4 medium open; 7 closed today | 710 PRs in v0.5.18 from 212 contributors | **v0.5.18 shipped** | Largest release volume; packaging regression (uv <0.12 silent downgrade) |
| **llama.cpp** | 2 critical, 2 high, 3 medium open | 6+ in review/landed (b10549–b10569) | **v0.2.0 cut** (major version milestone) | Nightly build infrastructure now attested; multimodal momentum |
| **Ollama** | 1 critical fixed, 1 critical open, 3 high open | 8 PRs in v0.33.0-rc1 | **v0.33.0-rc1** | Desktop-focused; MLX and Claude integration priorities |
| **LiteLLM** | 3 high, 4 medium open | 20 active PRs | **v1.99.0-dev.2** | Security/cost tracking focus; cosign signatures new |
| **Unsloth** | 2 critical (1 fixed), 4 high open | 6 PRs open | No release; v0.1.801-beta current | Desktop beta regressions dominate; library users less affected |

---

## 3. Model Support Race

| Model / Architecture | vLLM | SGLang | llama.cpp | Ollama | LiteLLM | Unsloth |
|---------------------|:----:|:------:|:---------:|:------:|:-------:|:-------:|
| **DeepSeek-V4-Flash** | 🔴 SM8x blocked | 🟢 Supported | 🟡 IQ3_XXS issues (ROCm) | — | 🟡 `reasoning_effort` stripped | — |
| **Qwen3.8-27B** | 🟢 | 🟢 Verified configs (RTX 5090, DGX Spark) | 🔴 Blackwell hang (NVFP4) | 🟢 MLX fixed | — | 🟡 API guide broken |
| **GLM-5.1 / 5.2** | 🔴 V1+MTP hangs / 0% draft acceptance | — | 🔴 RPC multi-node crash | — | — | — |
| **Kimi-K3** | — | 🟡 NaN logits (DSPARK) | — | — | — | 🟢 PR #9506 (API) |
| **Muse Glimmer** (multimodal AR) | — | 🟢 v0.5.18 | — | — | — | — |
| **dots3-note** (vision+audio+MoE) | — | — | 🟢 b10569 + follow-up | — | — | — |
| **MiniMax-M3 MTP** | 🟡 ROCm in progress | — | — | — | — | — |
| **Qwen3.5-VL** | 🟢 CPU offload fix | 🟡 Grounding divergence | — | 🟢 MLX vision fixed | — | — |

**Leader assessment:** SGLang leads on verified hardware configs and multimodal model breadth; vLLM has widest model coverage but critical gaps on newer architectures for older hardware; llama.cpp is fastest to novel multimodal cache types (DSA-ISWA) but lags on production stability; Ollama and Unsloth are downstream consumers with integration lag.

---

## 4. Performance Frontier

| Domain | vLLM | SGLang | llama.cpp | Ollama | LiteLLM | Unsloth |
|--------|------|--------|-----------|--------|---------|---------|
| **KV Cache / Disaggregated Serving** | PCP KV bypass (#52863), hybrid backfill (#52774), NIXL config hashing (#52999) | Weight Cache Daemon (306s→<1s recovery), HiCache write-back/load-back (#34515) | `llama_kv_cache_dsa_iswa` new type | Prefix cache survives cancelled prefills (#17901) | — | — |
| **Batching / Scheduling** | GDN metadata vectorization (#51327), SWA backend selection (#53007) | Scheduler sync removal (#34515), breakable CUDA graph investigation (~50% idle) | — | `OLLAMA_NUM_THREAD` cgroup escape (#17920) | Complexity router retuned (#37910) | Auto context + RAM offload (#9492) |
| **Quantization** | TurboQuant cache dtype (#50248), MXFP4 LoRA stale | NVFP4 per-token activation scales (#35943) | IQ2_NL/IQ3_NL block types (#27322–27325), Mamba2 GEMV→GEMM (#27513) | — | — | — |
| **Distributed / Parallelism** | PCP direct KV, Mooncake topology | DCP for `trtllm_mla` (#33926), Lean Attention (#33576) | Tensor split LFM2/LFM2MOE | — | — | — |
| **Speculative Decoding** | EAGLE3 misdispatch fix, DSpark cache hit (#52804) | Kimi-K3 NaN (DSPARK) | DFlash2 (#27342), adaptive MTP draft depth (#27210) | DFlash2 MLX draft (#17865) | — | — |
| **Kernel Optimization** | TRITON_MLA_SPARSE backend | AMD Lean Attention (persistent CTA) | RWKV7 CUDA fusion, Vulkan `MUL_MAT_ID` hoisting | — | — | ROCm AOTriton (#8821) |

**Concentration:** KV cache disaggregation is the shared priority across vLLM and SGLang; speculative decoding is fragmenting into hardware-specific paths; quantization innovation is concentrated in llama.cpp for edge deployment.

---

## 5. Layer Positioning

| Layer | Projects | Characteristics |
|-------|----------|---------------|
| **Production Serving Engine** | **vLLM**, **SGLang** | Full distributed serving (TP/PP/DP), disaggregated prefill/decode, production SLIs. vLLM has broader ecosystem adoption; SGLang is optimizing for recovery time and scheduler correctness. |
| **Local / Edge Runtime** | **llama.cpp**, **Ollama** | Single-node or small-cluster inference, quantization-first, desktop/edge deployment. llama.cpp is the engine; Ollama is the consumer-facing wrapper with model management. |
| **LLM Gateway / Router** | **LiteLLM** | Multi-provider abstraction, cost tracking, fallback routing, enterprise SSO/budgeting. Not an inference engine—sits above them. |
| **Training / Fine-tuning Framework** | **Unsloth** | Desktop-centric fine-tuning with inference capability. Overlaps with local runtime but optimizes for training throughput (GRPO, LoRA) rather than serving efficiency. |

**Key tension:** vLLM and SGLang are converging on similar disaggregated architectures from different starting points—vLLM from dense transformer scale-out, SGLang from scheduler-centric design. Both now compete for the same production deployments, while llama.cpp/Ollama capture the long tail of local and edge use cases.

---

## 6. Trend Signals

| Trend | Evidence | Implication for Developers |
|-------|----------|---------------------------|
| **Disaggregated prefill/decode is production-adjacent** | vLLM NIXL hashing, SGLang Weight Cache Daemon, both with router safety PRs | Start evaluating PD separation for cost optimization; monitor prefill throughput stability under DP attention |
| **Speculative decoding is becoming model-specific** | DFlash2, EAGLE3, adaptive MTP, DSPARK—each with incompatible assumptions | Avoid generic "enable spec decode" flags; validate acceptance rates and silent corruption paths per model |
| **Ampere deprecation accelerates** | DeepSeek-V4-Flash SM8x blocked, TRITON_MLA_SPARSE as workaround, FlashInfer JIT failures on sm_120 with older CUDA | Hardware planning: prioritize Hopper/Blackwell for new deployments; budget for A100/A800 retirement |
| **Multimodal cache types proliferate** | `llama_kv_cache_dsa_iswa`, Qwen3.5 vision tower offloading, Muse Glimmer | Prompt caching logic must be cache-type-aware; vision encoder placement now a tunable (`--mmproj-device`) |
| **Supply-chain security hardening** | LiteLLM cosign signatures, llama.cpp attestations, vLLM release job `ccache-clear` | Add signature verification to CI/CD; expect attestation as baseline procurement requirement |
| **Agent workload optimization** | vLLM agentic API RFC (#52567), SGLang scheduler sync removal for agents, Ollama Claude Code KV fixes | First-class session identity coming to inference engines; external conversation state management may become unnecessary |
| **Silent failure modes dominate risk** | SGLang uv silent downgrade, Ollama all-zero embeddings, vLLM prefill misdispatch → garbage output | Add output validation (embedding magnitude checks, deterministic test prompts) beyond HTTP 200 monitoring |

**Watch list for agent/application developers:**
- vLLM [#52567](https://github.com/vllm-project/vllm/issues/52567) agentic API RFC for session identity primitives
- SGLang [#35241](https://github.com/sgl-project/sglang/issues/35241) PrefillDelayer feedback loop if running DP attention with chunked prefill
- Ollama [#17910](https://github.com/ollama/ollama/issues/17910) unbounded generation regression before upgrading past 0.32.9
- LiteLLM [Issue #37611](https://github.com/BerriAI/litellm/issues/37611) health check memory storm at worker scale

---

## Per-Project Reports

<details>
<summary><strong>vLLM</strong> — <a href="https://github.com/vllm-project/vllm">vllm-project/vllm</a></summary>

# vLLM Digest — 2026-08-22

## 1. Today's Highlights

The vLLM project is actively pushing toward **disaggregated prefill/decode production readiness** with multiple KV connector fixes for Mooncake topology alignment and NIXL config hashing for rolling upgrades. Meanwhile, **DeepSeek-V4-Flash Ampere support remains the top community pain point**, with 101 comments on a fresh SM8x tracking issue (#50576) as users hit DeepGEMM assertion failures on A100/A800 hardware. The Rust frontend is marching toward feature parity (#44280) while speculative decoding sees critical bugfixes for DSpark and EAGLE3 misdispatch paths.

---

## 2. Releases & Breaking Changes

**None** — No releases in the last 24h.

---

## 3. New Model & Hardware Support

| Item | Status | Details |
|------|--------|---------|
| **DeepSeek-V4-Flash / V4-Flash-0731 on SM8x (Ampere)** | 🔴 Blocked | [#50576](https://github.com/vllm-project/vllm/issues/50576), [#40851](https://github.com/vllm-project/vllm/issues/40851) — DeepGEMM hyper-rectangular tile assertions fail on sm_80. No ETA; community workaround is Hopper/Blackwell migration. |
| **TRITON_MLA_SPARSE backend for sm80/120/121** | 🟡 In Progress | [#38006](https://github.com/vllm-project/vllm/issues/38006) — Would unblock Sparse MLA on Ampere through Blackwell. PRs #37968, #35271 need rebase. |
| **Qwen3.5 vision tower CPU offloading** | 🟢 Fix Ready | [#53120](https://github.com/vllm-project/vllm/pull/53120) — `--cpu-offload-params visual` was silently no-op; now routes through UVA offloader. |
| **MiniMax-M3 MTP on ROCm (AITER PA gluon decode)** | 🟡 In Progress | [#52849](https://github.com/vllm-project/vllm/pull/52849) — Enables EAGLE3 speculative decoding without falling back to native unified_attention on AMD. |
| **MXFP4 LoRA on MI355X (CDNA4)** | 🟡 Stale/Needs Rebase | [#37268](https://github.com/vllm-project/vllm/pull/37268) — Triton backend extension for ROCm. Stale since March. |

---

## 4. Performance & Optimization

| PR/Issue | Impact | Details |
|----------|--------|---------|
| [#52863](https://github.com/vllm-project/vllm/pull/52863) | **PCP KV bypass** | Replicated KV updates via PyTorch SymmetricMemory, skipping AllGather. Opt-in (`VLLM_USE_PCP_DIRECT_KV=1`). Target: multi-replica prefill clusters. |
| [#52774](https://github.com/vllm-project/vllm/pull/52774) | **Hybrid model backfill** | Offloading connector backfills divergent local hits for Mamba+attention hybrids, reducing recompute when recurrent state and KV cache diverge under pressure. |
| [#51327](https://github.com/vllm-project/vllm/pull/51327) | **GDN metadata vectorization** | NumPy-vectorized causal conv1d indices; ~batch-size-proportional engine-thread overhead reduction on B200. |
| [#51217](https://github.com/vllm-project/vllm/pull/51217) | **MoE activation generalization** | Single masked activation path for flat `[T,D]` and padded `[E,T,D]` layouts. Fail-closed for unsupported activations. |
| [#53007](https://github.com/vllm-project/vllm/pull/53007) | **SWA backend selection** | `--attention-config` allows alternate backends for sliding-window layers, with LCM block size readjustment. |
| [#52804](https://github.com/vllm-project/vllm/pull/52804) | **KIMI + RHAI DSpark cache hit rate** | Unifies KV cache group block sizes across hybrid targets and draft models with mismatched per-token KV footprints. |

---

## 5. Stability & Regressions

| Severity | Issue | Details | Fix Status |
|----------|-------|---------|------------|
| 🔴 **Critical** | [#40926](https://github.com/vllm-project/vllm/issues/40926) | V1 engine + MTP + GLM-5.1: workers hang under sustained traffic, `sample_tokens` RPC timeout → `EngineDeadError`. TP=8, production traffic. | **No fix PR identified** |
| 🔴 **Critical** | [#53051](https://github.com/vllm-project/vllm/issues/53051) | Prefill misdispatched into spec-decode FULL cudagraph when `prompt_len == 1 + num_speculative_tokens` → silent GDN state loss, garbage output. Hybrid/Qwen3-Next models. | **No fix PR identified** |
| 🟡 **High** | [#50851](https://github.com/vllm-project/vllm/issues/50851) | DSpark speculative decoding broken on H200 nightly: code paths assume `"dflash"` only, `DSparkSpeculator` extends wrong base. | **No fix PR identified** |
| 🟡 **High** | [#52833](https://github.com/vllm-project/vllm/issues/52833) | GLM-5.2 MTP: 0% draft acceptance on MI355X; disabling EP hits `hipErrorIllegalAddress`. | **No fix PR identified** |
| 🟡 **High** | [#52735](https://github.com/vllm-project/vllm/issues/52735) | `OffloadingConnector` stores but never serves when MTP/EAGLE enabled on XPU (hybrid GDN). | **No fix PR identified** |
| 🟡 **High** | [#48953](https://github.com/vllm-project/vllm/issues/48953) | Intel Arc B50 TP=2: `zeMemOpenIpcHandle INVALID_ARGUMENT`. Battlemage Xe2 IPC handle breakage. | **No fix PR identified**; extends [#41663](https://github.com/vllm-project/vllm/issues/41663) |
| 🟡 **High** | [#50705](https://github.com/vllm-project/vllm/issues/50705) | sm_120 + CUDA < 12.9: FlashInfer JIT failures kill engine init in three default paths (sampler, fused-MoE, FP8 KV) instead of graceful fallback. | **No fix PR identified** |
| 🟢 **Medium** | [#48435](https://github.com/vllm-project/vllm/issues/48435) | hybrid-SWA prefix caching collapses to zero at ~25% pool occupancy for Gemma-4-31B; eager-freed SWA tails recycled tail-first. | **No fix PR identified** |
| 🟢 **Medium** | [#49717](https://github.com/vllm-project/vllm/issues/49717) | Gemma4 streaming: `content` empty, `reasoning` holds full output when reasoning channel left open. | **No fix PR identified** |
| 🟢 **Medium** | [#50248](https://github.com/vllm-project/vllm/pull/50248) | TurboQuant cache dtype propagation + FP8 store failure on Ampere. | **PR ready** [#50248](https://github.com/vllm-project/vllm/pull/50248) |
| 🟢 **Medium** | [#50247](https://github.com/vllm-project/vllm/pull/50247) | ROCm: mixed KV-cache layout assertion bypass in spec-decode. | **PR ready** [#50247](https://github.com/vllm-project/vllm/pull/50247) |

---

## 6. What This Means for Application Developers

**If you're running production inference:**

- **Avoid V1 + MTP + hybrid models (GLM-5.1/5.2, Qwen3-Next)** until [#40926](https://github.com/vllm-project/vllm/issues/40926) and [#53051](https://github.com/vllm-project/vllm/issues/53051) are resolved — worker hangs and silent corruption are both in play. V0 engine or disabling MTP speculative decoding is the safer path.
- **Disaggregated prefill/decode is approaching deployable** with [#52999](https://github.com/vllm-project/vllm/pull/52999) (NIXL config hashing for router safety) and [#52731](https://github.com/vllm-project/vllm/pull/52731) (tier-aware cache events). If you're building a router, start consuming `vllm:nixl_config_info`.
- **Ampere users (A100/A800/RTX 30xx):** DeepSeek-V4-Flash is still **not viable** — the SM8x support gap is architectural (DeepGEMM tile constraints), not a quick fix. Plan hardware accordingly or watch [#50576](https://github.com/vllm-project/vllm/issues/50576) for any community workaround.

**If you're building agents or multi-turn apps:**

- The **agentic API RFC** [#52567](https://github.com/vllm-project/vllm/issues/52567) is worth tracking — first-class session identity would replace ad-hoc conversation state management in external frameworks. No implementation timeline yet.
- **Batch invariance for DP+EP** [#30321](https://github.com/vllm-project/vllm/issues/30321) is still inconsistent — if you need deterministic outputs across parallelism strategies, stick to TP-only for now.

**If you're on Intel/AMD:**

- Intel Arc Battlemage: TP>1 is broken for XPU ([#48953](https://github.com/vllm-project/vllm/issues/48953)). AMD MI355X: GLM-5.2 MTP is unusable ([#52833](https://github.com/vllm-project/vllm/issues/52833)). Both need kernel/driver-level fixes outside vLLM core.

</details>

<details>
<summary><strong>SGLang</strong> — <a href="https://github.com/sgl-project/sglang">sgl-project/sglang</a></summary>

# SGLang Digest — 2026-08-22

## Today's Highlights

SGLang **v0.5.18** shipped with **710 PRs from 212 contributors**, adding Muse Glimmer (multimodal autoregressive) to the model zoo. The release cycle is dominated by **disaggregated prefill/hi-cache hardening** and **Blackwell/AMD ROCm 7.14** platform expansion, while a flurry of new issues exposes scheduler-edge crashes and packaging regressions that production deployers should watch.

---

## Releases & Breaking Changes

| Item | Details |
|------|---------|
| **v0.5.18** | New release with 710 PRs. No explicit breaking changes noted in truncated release notes; full changelog at [v0.5.18](https://github.com/sgl-project/sglang/releases/tag/v0.5.18). |
| **Packaging regression** | `uv pip install sglang` with `uv < 0.12` silently installs **v0.5.9** (6 months old), causing silent correctness failures on FP8 inference. Workaround: upgrade `uv` or pin explicit version. [#35912](https://github.com/sgl-project/sglang/issues/35912) |

---

## New Model & Hardware Support

| Model / Hardware | Type | PR / Issue | Notes |
|-----------------|------|-----------|-------|
| **Muse Glimmer** | Autoregressive (Multimodal) | v0.5.18 release | New in model zoo; cookbook available |
| **ROCm 7.14** (gfx942 / gfx950) | AMD backend | [#35319](https://github.com/sgl-project/sglang/pull/35319) | Pip-wheel-based assembly (no apt repo); `site-packages` layout requires build system changes |
| **NVFP4 per-token activation scales** | Quantization | [#35943](https://github.com/sgl-project/sglang/pull/35943) | Inkling model support; enables higher dynamic range than block-wise scaling |
| **Qwen3.8-27B** verified configs | RTX 5090, RTX PRO 6000, DGX Spark + DFLASH2 | [#35825](https://github.com/sgl-project/sglang/pull/35825) | Unified on commit `1cf2b8c` and single container image |

---

## Performance & Optimization

| Work | Status | Numbers | Reference |
|------|--------|---------|-----------|
| **Weight Cache Daemon (Phase 1)** | Landed in v0.5.18 | Qwen3-235B FP8 weight load: **306–327s → <1s** via CUDA IPC | [#33522](https://github.com/sgl-project/sglang/issues/33522), [blog](https://www.lmsys.org/blog/2026-08-21-sglang-fast-recovery) |
| **AMD Work-Centric (Lean) Attention** | In review | Persistent-CTA decode kernel for long-context; avoids SplitK overhead at long sequence lengths | [#33576](https://github.com/sgl-project/sglang/pull/33576) |
| **Breakable CUDA graph: per-layer break cost** | Under investigation | ~**50% GPU idle time** on hybrid models (30 linear + 10 full attention layers) due to graph breaks between every layer | [#35851](https://github.com/sgl-project/sglang/issues/35851) |
| **Decode context parallelism (DCP) for `trtllm_mla`** | Draft PR | Unassigned roadmap item; needed for Blackwell scale-out | [#33926](https://github.com/sgl-project/sglang/pull/33926) |
| **Scheduler synchronization removal** | In review | Enables overlap scheduler on agent workloads; targets allocator cleanup, HiCache write-back/load-back, speculative-MoE stalls | [#34515](https://github.com/sgl-project/sglang/pull/34515) |
| **Input logprob compute without full-vocab materialization** | Landed | Avoids `[rows, vocab]` `log_softmax`; NPU path | [#31958](https://github.com/sgl-project/sglang/pull/31958) |
| **Diffusion: mapped-weight prefetch + layerwise placement** | Multiple PRs | Eliminates major-page-fault stalls on checkpoint-mapped weights; `--layerwise-resident-layers video_vae=all` now supported | [#35749](https://github.com/sgl-project/sglang/pull/35749), [#35940](https://github.com/sgl-project/sglang/pull/35940) |

---

## Stability & Regressions

| Severity | Issue | Summary | Fix Status |
|----------|-------|---------|------------|
| **High** | [#35241](https://github.com/sgl-project/sglang/issues/35241) | **PrefillDelayer mixed-state feedback loop** under DP Attention + chunked prefill collapses prefill progress; scheduler/performance stability | Open, 8 comments |
| **High** | [#32968](https://github.com/sgl-project/sglang/issues/32968) | **Kimi-K3 [PAD] token storms + NaN logits** with DSPARK speculative decode; root cause likely #32477 (not in any release yet) | Open, needs release with #32477 |
| **High** | [#34720](https://github.com/sgl-project/sglang/issues/34720) | **XPU: Qwen3.5 GDN + speculative decode crash** — `causal_conv1d_update_xpu()` kwarg mismatch | Open |
| **Medium** | [#35705](https://github.com/sgl-project/sglang/issues/35705) | **`move_logprobs_to_cpu` AttributeError** — `'list' object has no attribute 'tolist'` | Open, 1 comment |
| **Medium** | [#35772](https://github.com/sgl-project/sglang/issues/35772) | **Qwen3-VL vision feature divergence** from Transformers/vLLM on fine-grained grounding (v0.5.17) | Open |
| **Medium** | [#35884](https://github.com/sgl-project/sglang/issues/35884) | **`/health` timeout path orphans scheduler requests**, crashes paged-prefill batching | Open |
| **Medium** | [#35891](https://github.com/sgl-project/sglang/issues/35891) | **EncoderScheduler dispatches after request timeout** — request still broadcast to TP workers | Open |
| **Medium** | [#25790](https://github.com/sgl-project/sglang/issues/25790) | **FP8 KV Cache logprob mismatch** at exactly index 96 (Prefill vs Decode) on H100 | Open, 4 comments |
| **Low** | [#35692](https://github.com/sgl-project/sglang/issues/35692) | Anthropic endpoint: `tool_reference` in `tool_result` breaks chat templates without deferred-reference support → HTTP 500 | Open |
| **Closed today** | [#26324](https://github.com/sgl-project/sglang/issues/26324), [#28628](https://github.com/sgl-project/sglang/issues/28628), [#27109](https://github.com/sgl-project/sglang/issues/27109), [#28915](https://github.com/sgl-project/sglang/issues/28915), [#28971](https://github.com/sgl-project/sglang/issues/28971), [#28873](https://github.com/sgl-project/sglang/issues/28873), [#35743](https://github.com/sgl-project/sglang/issues/35743) | Various: flashinfer_trtllm MoE corruption, CANN ABI mismatch, DeepSeek-V4-Pro PP/TP hangs, AMD kv_canary, EAGLE+Mooncake regression, global `--attention-backend` crash | Resolved or stale-closed |

---

## What This Means for Application Developers

1. **Verify your installer**: If you use `uv`, ensure `uv >= 0.12` or pin `sglang==0.5.18` explicitly. Silent downgrades to v0.5.9 produce **correct but wrong outputs** on modern FP8 models — the worst failure mode for production. [#35912](https://github.com/sgl-project/sglang/issues/35912)

2. **Disaggregated prefill / HiCache is production-ready but watch the edges**: The Weight Cache Daemon delivers sub-second engine recovery (was 5+ minutes), but new issues expose scheduler feedback loops [#35241](https://github.com/sgl-project/sglang/issues/35241) and health-check orphaning [#35884](https://github.com/sgl-project/sglang/issues/35884). If you run PD separation with DP attention or chunked prefill, monitor prefill throughput stability closely.

3. **AMD ROCm 7.14 and Blackwell are active investment areas**: If you're on MI300X or B200, expect rapid kernel churn. The Lean Attention kernel [#33576](https://github.com/sgl-project/sglang/pull/33576) and DCP decode path [#33926](https://github.com/sgl-project/sglang/pull/33926) are not yet merged — plan validation cycles before production deployment.

4. **Multimodal + speculative decode has known landmines**: Qwen3-VL grounding divergence [#35772](https://github.com/sgl-project/sglang/issues/35772) and Kimi-K3 NaN contamination [#32968](https://github.com/sgl-project/sglang/issues/32968) suggest keeping speculative decoding disabled for vision-language workloads until fixes land in a release.

5. **Unit test coverage drive is ongoing**: The project acknowledges 600+ test files are mostly E2E. If you're contributing or relying on edge-case scheduler behavior, expect some instability as unit test gaps are filled [#20865](https://github.com/sgl-project/sglang/issues/20865).

</details>

<details>
<summary><strong>llama.cpp</strong> — <a href="https://github.com/ggml-org/llama.cpp">ggml-org/llama.cpp</a></summary>

# llama.cpp Digest — 2026-08-22

## Today's Highlights

llama.cpp officially cuts **v0.2.0** (build b10566), marking a major version milestone with nightly build infrastructure and attestation support. The release coincides with significant momentum in **multimodal support**—dots3-note vision+audio landed in b10569 and is already getting MoE vision tower follow-ups—while **speculative decoding** sees both new architectures (DFlash2) and adaptive MTP draft depth in active PR review.

---

## Releases & Breaking Changes

| Item | Details |
|------|---------|
| **v0.2.0 released** | First major version bump since 0.x series. Nightly builds now tagged via `nightly-tag.txt` asset; release jobs conclude with `ccache-clear` to prevent stale cache poisoning ([#27503](https://github.com/ggml-org/llama.cpp/pull/27503)). Attestations available at [42207505](https://github.com/ggml-org/llama.cpp/attestations/42207505). |
| **CI: WebUI build logic cleanup** | `release.yml` being refactored to remove HF bucket UI hosting dependencies; some jobs pass `-DHF_UI_VERSION=...` while others don't—watch for build script changes if you fork release workflows ([#27316](https://github.com/ggml-org/llama.cpp/pull/27316)). |

---

## New Model & Hardware Support

| Feature | Status | Notes |
|---------|--------|-------|
| **dots3-note** (vision + audio) | **Landed** b10569 | New `llama_kv_cache_dsa_iswa` cache type; text conversion, RoPE fixes. Follow-up PR [#27524](https://github.com/ggml-org/llama.cpp/pull/27524) adds MoE support for vision tower and Whisper-based audio. NMSE 1.6e-4 vs HF on 448×336 image test. |
| **DFlash2 speculative decoding** | **In review** [#27342](https://github.com/ggml-org/llama.cpp/pull/27342) | Adds grouped dynamic depthwise convolution + candidate selector to DFlash; formula uses static `base` kernel + dynamic `δ[i,t,g(c)]` predicted from input. |
| **Mamba2 GEMM dispatch** | **In review** [#27513](https://github.com/ggml-org/llama.cpp/pull/27513) | Flattens in/out projections to dispatch GEMM instead of GEMV; targets Nemotron 3 Nano (30B A3B NVFP4) where `mul_mat_vec_*` dominated ~40% GPU time at npl=32. |
| **IQ2_NL / IQ3_NL quant types** | **In review** (stacked PRs: [#27322](https://github.com/ggml-org/llama.cpp/pull/27322) CPU → [#27324](https://github.com/ggml-org/llama.cpp/pull/27324) Metal → [#27325](https://github.com/ggml-org/llama.cpp/pull/27325) CUDA/SYCL) | 32-element block types for tensors where `ncols % 256 != 0`, avoiding sub-optimal fallback from 256-block K-quants/I-quants. |
| **Tensor split for LFM2/LFM2MOE** | **Landed** b10549 | TP sharding enabled for Liquid Foundation Model 2 variants ([#26993](https://github.com/ggml-org/llama.cpp/pull/26993)). |
| **WebP via FFmpeg** | **Landed** [#27520](https://github.com/ggml-org/llama.cpp/pull/27520) | Replaces single-header library approach; shares video code path, no binary bloat. Fixes [#27443](https://github.com/ggml-org/llama.cpp/issues/27443), [#12410](https://github.com/ggml-org/llama.cpp/issues/12410). |

---

## Performance & Optimization

| Work | Impact | PR/Issue |
|------|--------|----------|
| **AVX2 IQ model prompt processing** | Large batch size (512 token) speedup for imatrix/perplexity workloads by decoding each weight once per batch instead of 512× | [#27402](https://github.com/ggml-org/llama.cpp/pull/27402) |
| **Vulkan `MUL_MAT_ID` hoisting** | MoE prompt processing: row IDs and expert counts collected once, reused across workgroups vs. independent routing table searches per workgroup | [#26686](https://github.com/ggml-org/llama.cpp/pull/26686) |
| **RWKV7 CUDA kernel fusion** | Fuses recurrent input + state paths | [#27523](https://github.com/ggml-org/llama.cpp/pull/27523) |
| **Metal K-extent clamping** | Fixes mat-mat kernel tile overflow when `ne00 % 32 != 0` in Tensor API path (`GGML_METAL_HAS_TENSOR`) | [#27450](https://github.com/ggml-org/llama.cpp/pull/27450) |
| **RoPE offset API adoption** | `ggml_rope_set_offset()` replaces manual offset handling; partially applied to DeepSeek2, now extended to MTMD vision ([#27521](https://github.com/ggml-org/llama.cpp/pull/27521)) | [#27382](https://github.com/ggml-org/llama.cpp/pull/27382) |
| **Adaptive MTP draft depth** | `--spec-type draft-mtp-adaptive` with climb counter + weighted drop-pressure accumulator; targets `--spec-draft-n-max 12` | [#27210](https://github.com/ggml-org/llama.cpp/pull/27210) |

---

## Stability & Regressions

| Severity | Issue | Status | Details |
|----------|-------|--------|---------|
| **🔴 Critical** | CUDA kernel stall / watchdog kill (RTX Pro 6000 Blackwell) | **Open** [#27102](https://github.com/ggml-org/llama.cpp/issues/27102) | Model execution hangs, killed by watchdog. Qwen3.8-27B UD-Q8_K_XL. 20 comments, no fix PR yet. |
| **🔴 Critical** | Qwen3.8-27B-NVFP4 decode hang (Blackwell sm_100) | **Open** [#27329](https://github.com/ggml-org/llama.cpp/issues/27329) | CPU spin, zero GPU work. MTP layers removed. 4 comments. |
| **🟠 High** | `cublasSgemm INVALID_VALUE` crash with `--spec-type draft-mtp` under KV saturation | **Open** [#26558](https://github.com/ggml-org/llama.cpp/issues/26558) | Hard crash in llama-server. 9 comments. |
| **🟠 High** | GLM-5.2 RPC multi-node CUDA crash | **Open** [#26583](https://github.com/ggml-org/llama.cpp/issues/26583) | `invalid data ptr` worker-side, `ggml_backend_rpc_buffer_get_tensor` abort orchestrator-side. DGX Spark nodes. |
| **🟡 Medium** | Vulkan performance drop (RX 6600, b9484) | **Open** [#24066](https://github.com/ggml-org/llama.cpp/issues/24066) | 41 comments, ongoing bisect. |
| **🟡 Medium** | DeepSeek V4 garbled output (Strix Halo, ROCm) | **Open** [#25436](https://github.com/ggml-org/llama.cpp/issues/25436) | IQ3_XXS quants. 28 comments. |
| **🟡 Medium** | SYCL garbage on second prompt (Intel Arc Pro B60) | **Open** [#26845](https://github.com/ggml-org/llama.cpp/issues/26845) | 9 comments. |
| **🟢 Low/Fixed** | Qwen3.8-27B 30% throughput degradation on CUDA | **Closed** [#27444](https://github.com/ggml-org/llama.cpp/issues/27444) | RTX 5090, single generation decay. Fixed in 24h. |
| **🟢 Low** | `-ngl` ignored in Vulkan, full VRAM load | **Open** [#27264](https://github.com/ggml-org/llama.cpp/issues/27264) | Windows, builds ≥10369. 3 comments. |

---

## What This Means for Application Developers

1. **Upgrade path for v0.2.0**: The version bump signals API stability commitments. If you pin to nightly builds, migrate to the new `nightly-tag.txt` asset for deterministic CI/CD. Verify attestation signatures if you distribute binaries.

2. **Multimodal apps**: dots3-note support with MoE vision towers means you can now serve vision+audio models with sparse expert routing—test your prompt caching logic against the new `llama_kv_cache_dsa_iswa` type. The `--mmproj-device` flag (from b10541) lets you offload vision encoders to different backends than text models.

3. **Speculative decoding operators**: Two competing approaches in flight—DFlash2's dynamic convolution (PR [#27342](https://github.com/ggml-org/llama.cpp/pull/27342)) and adaptive MTP (PR [#27210](https://github.com/ggml-org/llama.cpp/pull/27210)). For production, adaptive MTP is closer to merge; DFlash2 may offer better acceptance rates for code generation workloads but needs more bake time.

4. **Blackwell caution**: Multiple open issues on RTX 5090/Pro 6000 with NVFP4 and Qwen3.8-27B—avoid NVFP4 for now if you target Blackwell, or test thoroughly with MTP layers removed. The `ggml_rope_set_offset` changes suggest ongoing kernel churn.

5. **Quantization tooling**: The IQ2_NL/IQ3_NL PR stack will let you hit tighter file size targets (e.g., `--target-size 1.5g` from PR [#15550](https://github.com/ggml-org/llama.cpp/pull/15550)) without falling back to legacy Q4_0 on odd-width tensors—relevant for edge deployment.

</details>

<details>
<summary><strong>Ollama</strong> — <a href="https://github.com/ollama/ollama">ollama/ollama</a></summary>

# Ollama Digest — 2026-08-22

## Today's Highlights

Ollama cut the **v0.33.0-rc1** release candidate with critical MLX cross-platform fixes and began rolling out **Claude Desktop integration** in the macOS app. The team closed a severe MLX memory leak leaking ~147 MiB per request and fixed a KV-cache-busting bug in Claude Code compatibility. Meanwhile, container operators gained a new `OLLAMA_NUM_THREAD` escape hatch for cgroup-constrained deployments.

---

## Releases & Breaking Changes

| Item | Detail | Link |
|------|--------|------|
| **v0.33.0-rc1** | Release candidate; fixes MLX macOS assumptions on Linux/Windows, MLX backend update, lint fixes, and adds Claude Desktop app integration | [Release](https://github.com/ollama/ollama/releases/tag/v0.33.0-rc1) |

---

## New Model & Hardware Support

| Item | Detail | Link |
|------|--------|------|
| **MLX: DFlash2 draft model support** | Native MLX loading/inference for `DFlash2DraftModel` checkpoints with dynamic short convolution, parallel path selector, and rotating draft KV caches | [PR #17865](https://github.com/ollama/ollama/pull/17865) |
| **Cloud model requests** | Community requests for **Kimi K3 Cloud** ([#17235](https://github.com/ollama/ollama/issues/17235), closed) and **Qwen3.8-27B Cloud** ([#17926](https://github.com/ollama/ollama/issues/17926), closed) — no upstream availability yet |

---

## Performance & Optimization

| Item | Detail | Link |
|------|--------|------|
| **MLX prefix cache survives cancelled prefills** | Previously, agent timeouts on long prefills caused full recomputation; retries now resume from restore points instead of restarting from zero | [PR #17901](https://github.com/ollama/ollama/pull/17901) |
| **Claude Code token countdown disabled** | Removes per-turn "tokens left" system message that was breaking KV cache continuity and forcing full re-prompting | [PR #17918](https://github.com/ollama/ollama/pull/17918) |
| **OLLAMA_NUM_THREAD for cgroup-aware CPU config** | New env var to override auto-detected thread count; fixes ~45× throughput collapse in CPU-quota-limited containers where `n_threads` exceeded CFS budget | [PR #17920](https://github.com/ollama/ollama/pull/17920), [Issue #17916](https://github.com/ollama/ollama/issues/17916) |
| **HumanEval bench prompts** | Benchmark switched from synthetic word-list to packed Python code prompts for realistic draft-model evaluation | [PR #17480](https://github.com/ollama/ollama/pull/17480) |

---

## Stability & Regressions

| Severity | Item | Status | Link |
|----------|------|--------|------|
| **🔴 Critical — Fixed** | **MLX runner memory leak**: ~0.147 GiB resident growth per request at fixed context, plateauing at ~28.5 GiB (0.32.15, M4 Pro/48GB). Independent of context size. | **Fixed in PR** | [Issue #17924](https://github.com/ollama/ollama/issues/17924) |
| **🔴 Critical — Open** | **Long completions never stop** (regression 0.32.11–0.32.15, 0.32.9 clean). Generation runs past natural end until killed. M1 Max/macOS 26.6.1. | **Bisected, no fix yet** | [Issue #17910](https://github.com/ollama/ollama/issues/17910) |
| **🟡 High — Fixed** | **MLX vision runner crash**: ~125GB Metal buffer request on 24.5MP image with Qwen3.8-27B (M5 Pro/48GB). | **Fixed** | [Issue #17804](https://github.com/ollama/ollama/issues/17804) |
| **🟡 High — Open** | **Embeddings return all-zero vectors** under sustained load: HTTP 200, correct dimensionality, no error indicator. Silent failure mode. | **No fix PR** | [Issue #17878](https://github.com/ollama/ollama/issues/17878) |
| **🟡 High — Open** | **CPU spike when model fits in VRAM** (0.32.14): 50-80% CPU despite 100% GPU-bound report; 0.32.13 clean. | **No fix PR** | [Issue #17833](https://github.com/ollama/ollama/issues/17833) |
| **🟡 High — Open** | **NUMA multi-socket CPU underutilization**: Only half of available cores active on VMware. Long-running (since Mar 2024). | **No fix PR** | [Issue #2929](https://github.com/ollama/ollama/issues/2929) |
| **🟢 Medium — Fixed** | **Vulkan backend fails to load Qwen3.5 models** on 0.32.14 (`llama-server` crash). | **Fixed** | [Issue #17903](https://github.com/ollama/ollama/issues/17903) |
| **🟢 Medium — Fixed** | **`think:false` ignored on Qwen3.8-MLX**: reasoning still streamed; occasionally full answer in reasoning block. | **Fixed** | [Issue #17911](https://github.com/ollama/ollama/issues/17911) |
| **🟢 Medium — Fixed** | **Anthropic `xhigh` mapped to `high`**, breaking Qwen3.8 chat templates expecting `xhigh`/`medium`/`low`. | **Fix in PR #17917** | [Issue #17906](https://github.com/ollama/ollama/issues/17906) |
| **🟢 Medium — Open** | **`tool_choice` ignored** (0.32.15): `required` returns plain text, `none` still calls tools. Both OpenAI and Anthropic compat affected. | **No fix PR** | [Issue #17921](https://github.com/ollama/ollama/issues/17921) |
| **🟢 Medium — Open** | **Tool calling not streaming on MLX macOS**: timeouts on large `write` outputs. | **No fix PR** | [Issue #16279](https://github.com/ollama/ollama/issues/16279) |

---

## What This Means for Application Developers

| Takeaway | Action |
|----------|--------|
| **Claude Code users: upgrade to ≥0.33.0-rc1** | The KV-cache fix ([PR #17918](https://github.com/ollama/ollama/pull/17918)) and context-window suffix support ([PR #17908](https://github.com/ollama/ollama/pull/17908)) eliminate the "hangs indefinitely" behavior with local Qwen models. Claude Desktop can now be connected via the Apps UI ([PR #17900](https://github.com/ollama/ollama/pull/17900)). |
| **Container/K8s deployments: set `OLLAMA_NUM_THREAD`** | If you run CPU-only inference with CFS quotas or cpuset limits, pin this env var to your actual CPU budget to avoid the 45× throughput collapse from oversubscribed threads spinning on barriers. |
| **MLX macOS agents: monitor for memory leak fix** | The per-request 147 MiB leak is closed; if you saw hangs that resolved on retry, the prefix-cache survival fix ([PR #17901](https://github.com/ollama/ollama/pull/17901)) addresses the root cause. |
| **Avoid 0.32.11–0.32.15 for long-generation workloads** | The unbounded generation regression ([Issue #17910](https://github.com/ollama/ollama/issues/17910)) has no fix yet; pin to 0.32.9 or test 0.33.0-rc1 if your use case involves open-ended completions. |
| **Embedding pipelines: add vector validation** | The all-zero silent failure ([Issue #17878](https://github.com/ollama/ollama/issues/17878)) means you should sanity-check embedding magnitudes until a fix lands—cosine similarity will not catch this. |
| **OpenAI-compat migration: `max_completion_tokens`** | The deprecated `max_tokens` field still works, but prepare for `max_completion_tokens` support ([Issue #7125](https://github.com/ollama/ollama/issues/7125)) to avoid future breakage. |

</details>

<details>
<summary><strong>LiteLLM</strong> — <a href="https://github.com/BerriAI/litellm">BerriAI/litellm</a></summary>

# LiteLLM Digest — 2026-08-22

## Today's Highlights

LiteLLM v1.99.0-dev.2 ships with cosign-signed Docker images, strengthening supply-chain security for production deployments. The day's 20 active PRs show heavy focus on routing correctness—fixing Anthropic OAuth token leakage to Bedrock/Vertex, honoring chat-completions opt-ins for Messages API, and retuning complexity-router tier boundaries. Several long-standing issues closed around Vertex AI credential skip logic, metrics authentication, and tag budget resets.

---

## Releases & Breaking Changes

| Item | Detail | Link |
|------|--------|------|
| **v1.99.0-dev.2** | Docker images now signed with cosign; verify with key from commit `0112e53` | [Release](https://github.com/BerriAI/litellm/releases/tag/v1.99.0-dev.2) |
| **Terraform provider** | Now publishes in lockstep with LiteLLM version (dev/rc/stable) from same commit | [PR #37912](https://github.com/BerriAI/litellm/pull/37912) |
| **CI: Migration safety** | New gate bans row-rewriting DML (`UPDATE`/`DELETE`/`MERGE`) from Prisma migrations to prevent boot-time downtime | [PR #37899](https://github.com/BerriAI/litellm/pull/37899) |

---

## New Model & Hardware Support

| Item | Detail | Link |
|------|--------|------|
| **Databricks Claude pricing** | Adds Opus 4.8, Opus 5, Sonnet 5 with DBU-based pricing and cached-token cost tracking | [PR #37902](https://github.com/BerriAI/litellm/pull/37902) |
| **Reasoning effort levels** | Router now exposes `supported_reasoning_efforts` per model group with `max`/`ultra` tiers; chat→Responses bridge no longer silently drops `max` | [PR #37897](https://github.com/BerriAI/litellm/pull/37897) |
| **DeepSeek V4** | `reasoning_effort` ("high"/"max") still **not** passed through—stripped to `thinking: {"type": "enabled"}`; fix pending | [Issue #27439](https://github.com/BerriAI/litellm/issues/27439) |

---

## Performance & Optimization

| Item | Detail | Link |
|------|--------|------|
| **Health check memory storm** | Background health checks load unbounded `LiteLLM_HealthCheckTable` into every worker each 15-min cycle; near-OOM at scale with `use_shared_health_check: true`. Fix PR **not yet linked** | [Issue #37611](https://github.com/BerriAI/litellm/issues/37611) |
| **Budget Redis sync** | Await pipeline before sync reads to prevent live spend overwrite with stale Redis values | [PR #32618](https://github.com/BerriAI/litellm/pull/32618) |
| **Complexity router retuned** | Default tier boundaries shifted to 0.10/0.25/0.50 (was higher); prevents technical prompts parking in cheap tiers, makes `COMPLEX` tier reachable | [PR #37910](https://github.com/BerriAI/litellm/pull/37910) |

---

## Stability & Regressions

| Severity | Item | Fix Status | Link |
|----------|------|------------|------|
| **High** | **Anthropic OAuth token leaks to Bedrock/Vertex** — client token displaces deployment credentials, fallbacks fail, token surfaces in AWS/Google logs | **PR open** | [PR #37905](https://github.com/BerriAI/litellm/pull/37905) |
| **High** | **UI extremely slow in v1.82.x** — loading and interaction latency regressed | Open, no PR linked | [Issue #23005](https://github.com/BerriAI/litellm/issues/23005) |
| **High** | **MCP context state leakage** — `_mcp_active_toolset_id` persists across async stream interruptions | Open | [Issue #30416](https://github.com/BerriAI/litellm/issues/30416) |
| **Medium** | **Azure GPT-5.6 terra/luna pricing wrong** — carries OpenAI's July 30 cuts, Azure never matched; 20%/80% underreporting | Open | [Issue #36192](https://github.com/BerriAI/litellm/issues/36192) |
| **Medium** | **Anthropic batch costs always $0** — `transform_file_content_request` routes `msgbatch_*` IDs to wrong endpoint, `CheckBatchCost` drops data | Open | [Issue #27944](https://github.com/BerriAI/litellm/issues/27944) |
| **Medium** | **Prisma migrations fail in `litellm-non_root`** — `@prisma/engines` not writable, blocks upgrade from 1.84.0→1.92.1 | Open | [Issue #34236](https://github.com/BerriAI/litellm/issues/34236) |
| **Medium** | **Tool call JSON parse aborts turn** — `split_concatenated_json_objects` raises `JSONDecodeError` on malformed remainder | Open | [Issue #37699](https://github.com/BerriAI/litellm/issues/37699) |
| **Low** | **Azure `gpt-4o-2024-11-20` missing `cache_read_input_token_cost`** — cache reads billed at zero | Open | [Issue #37823](https://github.com/BerriAI/litellm/issues/37823) |
| **Fixed** | Vertex AI custom `api_base` credential skip logic restored | **Merged** | [Issue #19138](https://github.com/BerriAI/litellm/issues/19138) |
| **Fixed** | `/metrics` unauthenticated access restored behind reverse proxy | **Merged** | [Issue #27926](https://github.com/BerriAI/litellm/issues/27926) |
| **Fixed** | Tag budgets permanently blocked after first overage — `ResetBudgetJob` now handles tags | **Merged** | [Issue #27481](https://github.com/BerriAI/litellm/issues/27481) |

---

## What This Means for Application Developers

1. **Security hardening**: Verify new Docker image signatures before deploying v1.99.0-dev.2; the Terraform provider version now matches LiteLLM exactly, simplifying drift management.

2. **Routing reliability improvements**: The Anthropic OAuth fix ([PR #37905](https://github.com/BerriAI/litellm/pull/37905)) unblocks multi-provider fallbacks for Claude Code and similar tools. Messages API now properly respects `use_chat_completions_api: true` ([PR #37095](https://github.com/BerriAI/litellm/pull/37095)), letting you force chat-completions backends even for Anthropic-shaped requests.

3. **Cost tracking corrections needed**: Audit Azure GPT-5.6 and Databricks Claude spend from past 2 weeks—known underreporting may affect budget alerts. Enable PTU attribution explicitly; new startup warning ([PR #37898](https://github.com/BerriAI/litellm/pull/37898)) catches silent misconfigurations.

4. **Scale cautions**: If running `use_shared_health_check: true` with multiple workers, monitor memory closely—unbounded table loading ([Issue #37611](https://github.com/BerriAI/litellm/issues/37611)) is a production risk until patched.

</details>

<details>
<summary><strong>Unsloth</strong> — <a href="https://github.com/unslothai/unsloth">unslothai/unsloth</a></summary>

# Unsloth Digest — 2026-08-22

## Today's Highlights

Unsloth's v0.1.801-beta desktop release triggered a wave of platform-specific regressions: macOS MLX loading broke ([unslothai/unsloth#9466](https://github.com/unslothai/unsloth/issues/9466)), Linux AppImage freezes on Model Hub access ([unslothai/unsloth#9453](https://github.com/unslothai/unsloth/issues/9453)), and preset persistence failed across platforms ([unslothai/unsloth#9500](https://github.com/unslothai/unsloth/issues/9500), [unslothai/unsloth#5130](https://github.com/unslothai/unsloth/issues/5130)). The team shipped same-day fixes for several issues and opened PRs for AppImage font crashes ([unslothai/unsloth#9473](https://github.com/unslothai/unsloth/pull/9473)) and installer venv repair ([unslothai/unsloth#9501](https://github.com/unslothai/unsloth/pull/9501)).

---

## Releases & Breaking Changes

**No new releases** in the last 24h. The v0.1.801-beta desktop build remains the current shipping version.

---

## New Model & Hardware Support

| Item | Status | Details |
|------|--------|---------|
| **Kimi K3 API** | PR opened | Native support with vision, search, 1M output tokens, and reasoning controls (low/high/max); includes replay across tool calls and web-search turns ([unslothai/unsloth#9506](https://github.com/unslothai/unsloth/pull/9506)) |
| **AMD Strix Halo (gfx1151)** | Fix in progress | False VRAM check (~13 GB reported) blocked model loading on 96 GB unified memory; root cause identified in Vulkan backend probe ([unslothai/unsloth#9454](https://github.com/unslothai/unsloth/issues/9454), [unslothai/unsloth#9498](https://github.com/unslothai/unsloth/pull/9498)) |
| **ROCm AOTriton attention** | PR open | Library users (non-Studio) now get flash/mem-efficient SDPA kernels without manual `TORCH_ROCM_AOTRITON_ENABLE_EXPERIMENTAL` ([unslothai/unsloth#8821](https://github.com/unslothai/unsloth/pull/8821)) |
| **Ollama reasoning deltas** | PR opened | Normalizes `delta.reasoning` → `reasoning_content` at SSE boundary; fixes Deep Research first-output timeout ([unslothai/unsloth#9504](https://github.com/unslothai/unsloth/pull/9504)) |

---

## Performance & Optimization

| Item | Status | Details |
|------|--------|---------|
| **Auto context + RAM offload** | PR opened | "Auto" context now a real slider position; default context increased when system RAM offload is unavoidable. Remembered model settings shared across Studio entry points ([unslothai/unsloth#9492](https://github.com/unslothai/unsloth/pull/9492)) |
| **GPU telemetry** | PR opened | FloatingMonitor now shows GPU temperature and power via `/api/train/hardware` joined to `/api/system` inventory; serial polling to avoid monitor overhead ([unslothai/unsloth#9503](https://github.com/unslothai/unsloth/pull/9503)) |
| **Context compaction** | Feature requests triaged | Rolling context window ([unslothai/unsloth#7472](https://github.com/unslothai/unsloth/issues/7472)) and auto-compaction at 50%/80% thresholds ([unslothai/unsloth#8504](https://github.com/unslothai/unsloth/issues/8504)) closed as duplicates/resolved; no implementation landed yet |

---

## Stability & Regressions

| Severity | Issue | Fix Status |
|----------|-------|------------|
| **🔴 Critical — macOS MLX broken** | [unslothai/unsloth#9466](https://github.com/unslothai/unsloth/issues/9466): MLX models fail to load on Apple Silicon after v0.1.801-beta | **Closed** — same-day resolution |
| **🔴 Critical — Linux AppImage freeze** | [unslothai/unsloth#9453](https://github.com/unslothai/unsloth/issues/9453): Model Hub tab freezes, force-quit required; `atk-bridge` warning + error | Open, PR #9473 addresses Skia/COLRv1 font crash root cause |
| **🟡 High — Preset persistence** | [unslothai/unsloth#9500](https://github.com/unslothai/unsloth/issues/9500): 400 on `PUT /api/chat/settings`; [unslothai/unsloth#5130](https://github.com/unslothai/unsloth/issues/5130): KV cache settings not persisting | #5130 closed; #9500 open, zero comments |
| **🟡 High — Installer venv corruption** | [unslothai/unsloth#9479](https://github.com/unslothai/unsloth/issues/9479): `create venv` fails when interpreter missing; blocks repair | **PR open** #9501 replaces orphaned venv |
| **🟡 High — AMD GPU misidentification** | [unslothai/unsloth#9498](https://github.com/unslothai/unsloth/issues/9498): rocminfo reports CPU as GPU; wrong PyTorch wheel kept by dependency fast-path | **PRs open** #9498, #9499, #8606 |
| **🟡 High — 16GB iGPU blocked** | [unslothai/unsloth#9482](https://github.com/unslothai/unsloth/issues/9482): `UNSLOTH_ALLOW_HOST_OFFLOAD=1` required for models that previously loaded; ~10 GB weights + 8 GB usable gap | Open, 5 comments |
| **🟢 Medium — WebKit crash (Fedora/Wayland)** | [unslothai/unsloth#9480](https://github.com/unslothai/unsloth/issues/9480): Skia COLRv1 font assert kills WebKitWebProcess; distinct from #9393 | Open, PR #9473 likely fixes |
| **🟢 Medium — Model Hub empty** | [unslothai/unsloth#9456](https://github.com/unslothai/unsloth/issues/9456): `likes>=30` filter + `id: t._id` parsing bug removes all new models | Open |
| **🟢 Medium — Qwen3.8 API guide** | [unslothai/unsloth#9428](https://github.com/unslothai/unsloth/issues/9428): HuggingFace model resolution fails in API serving docs | Open |

---

## What This Means for Application Developers

1. **Pin your Unsloth version if shipping on macOS or Linux desktop** — v0.1.801-beta introduced platform-specific loader regressions. The MLX and AppImage fixes were rapid but not instantaneous; production deployments should validate on target hardware before auto-updating.

2. **Context management remains manual** — Despite multiple feature requests for auto-compaction and rolling windows, no automatic context truncation shipped. Applications building long-running agents must still implement their own windowing or expect OOM/stall on context limit.

3. **AMD ROCm path is stabilizing but verify your PyTorch wheel** — The dependency fast-path optimization that skips reinstalls when versions match can leave CPU/CUDA wheels in place on AMD systems. Check `torch.cuda.is_available()` and backend name programmatically; don't trust setup output alone ([unslothai/unsloth#8606](https://github.com/unslothai/unsloth/pull/8606)).

4. **Agent builders: preset API is flaky** — The 400 error on settings persistence means ephemeral chat configurations; if your app relies on preset templating, implement client-side fallback or wait for #9500 resolution.

5. **New Kimi K3 integration offers long-context agent patterns** — 1M output tokens with structured reasoning controls (low/high/max) enables new agentic workflows; PR #9506 adds native support with tool-call replay across search turns.

</details>

---
*This digest is auto-generated by [agents-radar](https://github.com/brysoh435-dev/agents-radar).*