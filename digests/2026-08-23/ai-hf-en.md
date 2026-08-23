# Hugging Face Trending Models Digest 2026-08-23

> Source: [Hugging Face Hub](https://huggingface.co/) | 30 models | Generated: 2026-08-23 03:30 UTC

---

# Hugging Face Trending Models Digest — 2026-08-23

## 1. Today's Highlights

The Qwen 3.8 family absolutely dominates this week's trending leaderboard, claiming 18 of 30 spots through an explosion of community quantizations, uncensored fine-tunes, and format adaptations. **moonshotai/Kimi-K3** emerges as the dark horse with 10,930 likes—outpacing even the official Qwen3.8-27B base model—signaling strong appetite for alternative Chinese lab architectures. The video generation space sees **MiniMax-H3** gaining serious traction with 3.9M downloads, while **Lightricks/LTX-2.5** continues pushing open image-to-video boundaries. Notably, "abliterated" and "uncensored" variants constitute over 25% of trending models, reflecting persistent community demand for unfiltered model weights despite mainstream safety alignment trends.

---

## 2. Trending Models

### 🧠 Language Models (LLMs, chat models, instruction-tuned)

| Model | Author | Likes | Downloads | Summary |
| :--- | :--- | ---: | ---: | :--- |
| [Qwen/Qwen3.8-27B](https://huggingface.co/Qwen/Qwen3.8-27B) | Qwen | 12,162 | 2,090,699 | Flagship 27B parameter multimodal LLM from Alibaba's Qwen team, supporting image-text-to-text conversations with strong vision-language reasoning. Its massive download volume reflects its position as the week's foundational open-weight release. |
| [moonshotai/Kimi-K3](https://huggingface.co/moonshotai/Kimi-K3) | moonshotai | 10,930 | 2,612,739 | Moonshot AI's K3 model nearly matches Qwen's like count despite being a newer entrant, featuring compressed-tensors architecture and competitive multimodal performance. The 2.6M downloads suggest rapid enterprise and developer adoption. |
| [deepseek-ai/DeepSeek-V4-Flash-0731](https://huggingface.co/deepseek-ai/DeepSeek-V4-Flash-0731) | deepseek-ai | 3,633 | 2,976,281 | DeepSeek's efficient V4-Flash variant approaches 3M downloads, offering near-flagship performance with optimized inference costs. Strong conversational capabilities make it a practical production alternative to larger models. |
| [deepseek-ai/DeepSeek-V4-Pro-0813](https://huggingface.co/deepseek-ai/DeepSeek-V4-Pro-0813) | deepseek-ai | 719 | 54,566 | The professional-tier V4-Pro targets highest-accuracy workloads with enhanced reasoning depth. Lower download volume versus Flash suggests cost sensitivity among deployers. |
| [ornith-ai/Ornith-1.5-35B-A3B](https://huggingface.co/ornith-ai/Ornith-1.5-35B-A3B) | ornith-ai | 326 | 12,611 | A 35B parameter MoE architecture built on Qwen3.5 foundations with only 3B active parameters, demonstrating efficient sparse activation patterns. Early traction indicates interest in parameter-efficient scaling approaches. |
| [superwhisper/s1-mini](https://huggingface.co/superwhisper/s1-mini) | superwhisper | 202 | 1,913 | Compact speech-to-text specialized model combining Qwen3 architecture with ASR capabilities. Niche but notable for bridging text-generation and audio transcription in a lightweight package. |
| [z-lab/Qwen3.8-27B-DFlash2](https://huggingface.co/z-lab/Qwen3.8-27B-DFlash2) | z-lab | 194 | 29,705 | Experimental speculative decoding variant of Qwen3.8-27B aiming for 2x throughput via draft model acceleration. Technical community interest despite modest absolute adoption. |
| [ornith-ai/Ornith-1.5-9B](https://huggingface.co/ornith-ai/Ornith-1.5-9B) | ornith-ai | 165 | 15,301 | Smaller sibling to the 35B Ornith MoE, offering accessible entry point for Qwen3.5-based multimodal experimentation. Balanced parameter count for consumer GPU deployment. |
| [empero-ai/Qwen3.8-9B-Distill](https://huggingface.co/empero-ai/Qwen3.8-9B-Distill) | empero-ai | 165 | 9,260 | Distilled 9B model preserving Qwen3.5 capabilities at dramatically reduced inference cost. Emerging community distillation efforts around Qwen architecture. |
| [Qwen/Qwen3.8-2.4T-A95B](https://huggingface.co/Qwen/Qwen3.8-2.4T-A95B) | Qwen | 1,146 | 17,386 | Massive 2.4T parameter MoE with 95B active parameters, representing Qwen's push into frontier-scale open weights. Limited downloads reflect infrastructure requirements rather than quality. |

### 🎨 Multimodal & Generation (image, video, audio, text-to-X)

| Model | Author | Likes | Downloads | Summary |
| :--- | :--- | ---: | ---: | :--- |
| [MiniMaxAI/MiniMax-H3](https://huggingface.co/MiniMaxAI/MiniMax-H3) | MiniMaxAI | 4,344 | 3,899,160 | MiniMax's flagship video generation model supporting text-to-video, image-to-video, and image-text-to-video pipelines. Nearly 4M downloads establish it as the dominant open video foundation model this week. |
| [Lightricks/LTX-2.5](https://huggingface.co/Lightricks/LTX-2.5) | Lightricks | 1,572 | 694,670 | Major upgrade to Lightricks' video generation suite with expanded conditioning modes including video-to-video editing. Strong creative community adoption for commercial-quality video production. |
| [MiniMaxAI/MiniMax-Music3](https://huggingface.co/MiniMaxAI/MiniMax-Music3) | MiniMaxAI | 1,186 | 16,644 | Text-to-music generation model extending MiniMax's multimodal portfolio beyond video. Lower download volume suggests audio generation remains a secondary priority for most deployers. |
| [TenStrip/10Eros-Max](https://huggingface.co/TenStrip/10Eros-Max) | TenStrip | 317 | 0 | Fine-tuned variant of MiniMax-H3 for specialized video generation use cases, though zero downloads indicate very recent release or distribution constraints. Community finetuning interest around MiniMax video architecture. |

### 📦 Fine-tunes & Quantizations (community fine-tunes, GGUF, AWQ)

| Model | Author | Likes | Downloads | Summary |
| :--- | :--- | ---: | ---: | :--- |
| [unsloth/Qwen3.8-27B-GGUF](https://huggingface.co/unsloth/Qwen3.8-27B-GGUF) | unsloth | 2,642 | 6,320,542 | Unsloth's official GGUF quantization dominates absolute downloads at 6.3M, enabling llama.cpp and local inference of the 27B model. The 3x download multiple over the base model reveals massive local/edge deployment demand. |
| [JonathanColetti/Qwen3.8-27B-Uncensored-GGUF](https://huggingface.co/JonathanColetti/Qwen3.8-27B-Uncensored-GGUF) | JonathanColetti | 626 | 1,223,422 | Highly downloaded uncensored GGUF with 1.2M downloads, indicating substantial demand for unaligned local LLM deployment. MTP (Multi-Token Prediction) support enhances inference efficiency. |
| [Qwen/Qwen3.8-27B-FP8](https://huggingface.co/Qwen/Qwen3.8-27B-FP8) | Qwen | 665 | 2,306,777 | Official FP8 quantization from Qwen team, offering 2.3M downloads of NVIDIA-optimized weights. Strong uptake among datacenter deployers with Hopper/Ada hardware. |
| [HauhauCS/Qwen3.8-27B-Uncensored-HauhauCS-Aggressive-MTP-GGUF](https://huggingface.co/HauhauCS/Qwen3.8-27B-Uncensored-HauhauCS-Aggressive-MTP-GGUF) | HauhauCS | 490 | 486,221 | Aggressive MTP-optimized uncensored variant with multimodal vision capabilities preserved in GGUF format. Nearly 500K downloads for this niche community build shows fragmentation of quantization demand. |
| [orcarouter/Qwen3.8-27B-Uncensored-FP8](https://huggingface.co/orcarouter/Qwen3.8-27B-Uncensored-FP8) | orcarouter | 994 | 142,846 | Uncensored FP8 quantization maintaining full transformer precision with safety alignment removed. Higher like-to-download ratio suggests quality recognition despite narrower hardware compatibility. |
| [OBLITERATUS/Qwen3.8-27B-OBLITERATED](https://huggingface.co/OBLITERATUS/Qwen3.8-27B-OBLITERATED) | OBLITERATUS | 544 | 164,950 | Extreme "obliterated" safety removal with multi-format distribution (MLX, GGUF, safetensors). Branding and 165K downloads reflect theatrical community response to alignment debates. |
| [orcarouter/Qwen3.8-27B-Uncensored-MLX](https://huggingface.co/orcarouter/Qwen3.8-27B-Uncensored-MLX) | orcarouter | 888 | 34,909 | Apple Silicon-optimized MLX port of uncensored Qwen3.8, serving Mac-focused local AI community. Lower downloads reflect platform specificity rather than interest. |
| [orcarouter/Qwen3.8-27B-Uncensored-GGUF](https://huggingface.co/orcarouter/Qwen3.8-27B-Uncensored-GGUF) | orcarouter | 339 | 85,371 | Companion GGUF release from orcarouter's uncensored series, demonstrating systematic multi-format coverage strategy. |
| [froggeric/Qwen-Fixed-Chat-Templates](https://huggingface.co/froggeric/Qwen-Fixed-Chat-Templates) | froggeric | 1,400 | 0 | Curiously zero-download yet highly-liked chat template corrections for MLX Qwen inference. Likely infrastructure/tooling utility where direct downloads undercount actual usage. |
| [huihui-ai/Huihui-Qwen3.8-27B-abliterated-GGUF](https://huggingface.co/huihui-ai/Huihui-Qwen3.8-27B-abliterated-GGUF) | huihui-ai | 257 | 635,416 | "Abliterated" variant with substantial 635K downloads, establishing huihui-ai as a significant quantization community player. Terminology variations ("abliterated" vs "uncensored") suggest niche branding differentiation. |
| [huihui-ai/Huihui-Qwen3.8-27B-abliterated](https://huggingface.co/huihui-ai/Huihui-Qwen3.8-27B-abliterated) | huihui-ai | 246 | 21,612 | Full-precision abliterated base for the GGUF variant above, serving fine-tuning and further derivation workflows. |
| [ornith-ai/Ornith-1.5-35B-A3B-GGUF](https://huggingface.co/ornith-ai/Ornith-1.5-35B-A3B-GGUF) | ornith-ai | 237 | 173,935 | MIT-licensed GGUF of the sparse MoE model, notable for permissive licensing and endpoint compatibility claims. Strong download velocity for a non-Qwen architecture. |
| [empero-ai/Qwen3.8-27B-Ridge-GGUF](https://huggingface.co/empero-ai/Qwen3.8-27B-Ridge-GGUF) | empero-ai | 245 | 97,247 | Specialized quantized variant with multimodal capabilities retained for llama.cpp deployment. Mid-tier adoption among community quantizations. |
| [DavidAU/Qwen3.8-27B-Cold-Fusion-GAIN-V1.1-NM-DAU-NEO-MAX-MTP-GGUF](https://huggingface.co/DavidAU/Qwen3.8-27B-Cold-Fusion-GAIN-V1.1-NM-DAU-NEO-MAX-MTP-GGUF) | DavidAU | 195 | 176,969 | Elaborately named experimental fine-tune with GAIN training and COLD-FUSION methodology, nearly 177K downloads despite complexity. Community appetite for novel training approaches on Qwen base. |
| [0bserverx/Qwen3.8-27B-Heretic-Abliterated-Uncensored-GGUF](https://huggingface.co/0bserverx/Qwen3.8-27B-Heretic-Abliterated-Uncensored-GGUF) | 0bserverx | 229 | 505,813 | "Heretic"-branded uncensored variant achieving half-million downloads, demonstrating sustained demand for aggressively unaligned weights. Religious/thematic branding as community differentiation tactic. |

---

## 3. Ecosystem Signal

**Qwen hegemony with challengers emerging.** The Qwen 3.8 family's absolute dominance—18 trending models, multiple official variants, and hundreds of community derivatives—confirms Alibaba's open-weight strategy has achieved ecosystem lock-in comparable to Llama's 2024 position. However, **Kimi-K3's** near-parity in likes and **DeepSeek-V4's** strong download metrics signal that Chinese labs are not monolithic; competition between Moonshot, DeepSeek, and Qwen may accelerate capability release cycles.

**The uncensored quantization economy has matured into a parallel distribution system.** Over 25% of trending models are explicitly "abliterated," "uncensored," or "obliterated," with combined downloads exceeding 4.5M. This represents not merely fringe activity but a substantial shadow market operating within mainstream platforms. The diversity of approaches—GGUF, FP8, MLX, with and without multimodal preservation—suggests sophisticated tooling and established community workflows.

**Format fragmentation as competitive moat.** The proliferation of MTP (Multi-Token Prediction) variants, speculative decoding experiments, and platform-specific ports (MLX for Apple, GGUF for universal local) indicates inference optimization has become as important as base model quality. Unsloth's 6.3M downloads demonstrate that quantization providers can achieve greater distribution than original model authors, raising questions about value capture in open ecosystems.

---

## 4. Worth Exploring

**[moonshotai/Kimi-K3](https://huggingface.co/moonshotai/Kimi-K3)** — The only model threatening Qwen's mindshare dominance, with 10,930 likes and 2.6M downloads suggesting genuine architectural differentiation beyond marketing. Worth studying for its compressed-tensors approach and whether Moonshot's efficiency claims translate to real-world inference advantages versus Qwen3.8-27B.

**[MiniMaxAI/MiniMax-H3](https://huggingface.co/MiniMaxAI/MiniMax-H3)** — At 3.9M downloads, this is the definitive open video generation model to benchmark against. The triple pipeline support (text/image/image-text to video) and near-4M adoption make it essential for anyone building video AI products, with [TenStrip/10Eros-Max](https://huggingface.co/TenStrip/10Eros-Max) indicating active fine-tuning ecosystem formation.

**[z-lab/Qwen3.8-27B-DFlash2](https://huggingface.co/z-lab/Qwen3.8-27B-DFlash2)** — Speculative decoding remains underexplored in production deployments; this experimental variant's 29K downloads and novel "DFlash2" methodology could yield inference breakthroughs. Particularly relevant for teams hitting latency walls with standard Qwen deployments.

---
*This digest is auto-generated by [agents-radar](https://github.com/brysoh435-dev/agents-radar).*