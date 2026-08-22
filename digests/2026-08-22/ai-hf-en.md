# Hugging Face Trending Models Digest 2026-08-22

> Source: [Hugging Face Hub](https://huggingface.co/) | 30 models | Generated: 2026-08-22 03:08 UTC

---

# Hugging Face Trending Models Digest — 2026-08-22

## 1. Today's Highlights

The Qwen 3.8 family dominates this week's trending list, with the 27B parameter variant generating extraordinary ecosystem activity across 18 distinct model entries. Moonshot AI's **Kimi-K3** emerges as the strongest challenger, ranking second in likes with 10,915 and accumulating 2.45M downloads, signaling China's continued ascendancy in open-weight multimodal AI. The proliferation of "abliterated" and "uncensored" fine-tunes—often with aggressive branding like "OBLITERATED" and "Heretic"—reflects persistent community demand for unaligned model variants, with some derivatives achieving surprising download velocity despite modest like counts. DeepSeek's V4 series maintains strong presence with both Pro and Flash variants, while video generation sees innovation through MiniMax's H3 ecosystem and Lightricks' LTX-2.5. Notably, **froggeric/Qwen-Fixed-Chat-Templates** achieved 1,373 likes with zero downloads, indicating strong interest in infrastructure improvements for MLX deployment.

---

## 2. Trending Models

### 🧠 Language Models (LLMs, chat models, instruction-tuned)

| Model | Author | Likes | Downloads | Summary |
| :--- | :--- | ---: | ---: | :--- |
| [Qwen/Qwen3.8-27B](https://huggingface.co/Qwen/Qwen3.8-27B) | Qwen | 11,981 | 1,726,651 | Flagship 27B multimodal language model with image-text-to-text capabilities, leading all models in likes this week. Its conversational pipeline and strong download figures establish it as the dominant open-weight release of late August 2026. |
| [moonshotai/Kimi-K3](https://huggingface.co/moonshotai/Kimi-K3) | moonshotai | 10,915 | 2,448,810 | Moonshot AI's K3 series delivers 2.45M downloads with compressed-tensor efficiency, nearly matching Qwen's popularity. The feature-extraction tag suggests enhanced embedding capabilities alongside its core conversational functions. |
| [deepseek-ai/DeepSeek-V4-Flash-0731](https://huggingface.co/deepseek-ai/DeepSeek-V4-Flash-0731) | deepseek-ai | 3,613 | 2,833,064 | Optimized inference variant of DeepSeek's V4 architecture achieving the highest download count in this category. The Flash designation indicates latency-optimized deployment without sacrificing the 2.8M+ download traction. |
| [deepseek-ai/DeepSeek-V4-Pro-0813](https://huggingface.co/deepseek-ai/DeepSeek-V4-Pro-0813) | deepseek-ai | 709 | 49,601 | Higher-capability Pro variant with August 2023 dating, showing sustained interest in DeepSeek's tiered model strategy. The 49K downloads demonstrate reliable enterprise adoption despite lower community visibility. |
| [Qwen/Qwen3.8-2.4T-A95B](https://huggingface.co/Qwen/Qwen3.8-2.4T-A95B) | Qwen | 1,141 | 15,702 | Massive 2.4 trillion parameter MoE model with 95B activated parameters, representing Qwen's frontier-scale research direction. Limited downloads reflect deployment constraints, but 1,141 likes indicate significant researcher interest. |
| [ornith-ai/Ornith-1.5-35B-A3B](https://huggingface.co/ornith-ai/Ornith-1.5-35B-A3B) | ornith-ai | 291 | 9,165 | Qwen3.5-MoE-derived architecture with 35B total / 3B active parameters, offering efficient inference via Mixture-of-Experts routing. The MIT license and endpoint compatibility signal deliberate positioning for commercial deployment. |
| [superwhisper/s1-mini](https://huggingface.co/superwhisper/s1-mini) | superwhisper | 191 | 1,136 | Compact Qwen3-based model integrating ASR capabilities, addressing voice-to-text workflows within the language model stack. Niche positioning explains modest metrics, but the pipeline fusion represents a notable capability extension. |
| [z-lab/Qwen3.8-27B-DFlash2](https://huggingface.co/z-lab/Qwen3.8-27B-DFlash2) | z-lab | 178 | 21,092 | Experimental speculative decoding implementation for Qwen3.8-27B, optimizing inference speed through draft-model acceleration. The 21K downloads validate community interest in latency reduction techniques for popular architectures. |

### 🎨 Multimodal & Generation (image, video, audio, text-to-X)

| Model | Author | Likes | Downloads | Summary |
| :--- | :--- | ---: | ---: | :--- |
| [MiniMaxAI/MiniMax-H3](https://huggingface.co/MiniMaxAI/MiniMax-H3) | MiniMaxAI | 4,297 | 3,614,443 | Leading video generation model with image-text-to-video pipeline, achieving the highest downloads in this category. The diffusers-native implementation and dual text-to-video / image-to-video capabilities drive its 3.6M adoption figure. |
| [Lightricks/LTX-2.5](https://huggingface.co/Lightricks/LTX-2.5) | Lightricks | 1,499 | 654,175 | Comprehensive video generation suite supporting four distinct pipelines: image-to-video, text-to-video, video-to-video, and image-text-to-video. The 654K downloads reflect Lightricks' established reputation in creative AI tooling. |
| [meta-models/Muse-Glimmer-30B](https://huggingface.co/meta-models/Muse-Glimmer-30B) | meta-models | 1,739 | 505,113 | 30B parameter multimodal model with image-text-to-text conversational capabilities, distinguishing itself from pure generation models. The 505K downloads and strong like ratio suggest effective positioning as a creative assistant rather than tool. |
| [MiniMaxAI/MiniMax-Music3](https://huggingface.co/MiniMaxAI/MiniMax-Music3) | MiniMaxAI | 1,165 | 15,678 | Dedicated text-to-music / text-to-audio generation model using diffusion architecture, expanding MiniMax's generative portfolio beyond video. Lower downloads reflect the narrower music-generation market compared to visual media. |
| [TenStrip/10Eros-Max](https://huggingface.co/TenStrip/10Eros-Max) | TenStrip | 311 | 0 | Community fine-tune of MiniMax-H3 for image-text-to-video generation, achieving notable likes despite zero recorded downloads. The explicit base_model attribution and zero downloads suggest very recent publication or tagging anomaly. |

### 📦 Fine-tunes & Quantizations (community fine-tunes, GGUF, AWQ)

| Model | Author | Likes | Downloads | Summary |
| :--- | :--- | ---: | ---: | :--- |
| [unsloth/Qwen3.8-27B-GGUF](https://huggingface.co/unsloth/Qwen3.8-27B-GGUF) | unsloth | 2,518 | 5,804,917 | Dominant quantization release with 5.8M downloads—highest across all 30 models—demonstrating massive demand for llama.cpp-compatible Qwen variants. Unsloth's optimization pipeline delivers exceptional inference efficiency for consumer hardware. |
| [unsloth/Qwen3.8-27B-NVFP4](https://huggingface.co/unsloth/Qwen3.8-27B-NVFP4) | unsloth | 328 | 1,013,917 | NVIDIA FP4-format quantization crossing 1M downloads, targeting RTX 50-series and datacenter GPU deployment. The specialized format indicates Unsloth's rapid adaptation to emerging hardware capabilities. |
| [JonathanColetti/Qwen3.8-27B-Uncensored-GGUF](https://huggingface.co/JonathanColetti/Qwen3.8-27B-Uncensored-GGUF) | JonathanColetti | 572 | 1,126,222 | High-velocity uncensored GGUF release with 1.1M downloads, among the most popular community derivatives. The "MTP" (Multi-Token Prediction) tag suggests speculative decoding integration for accelerated inference. |
| [Qwen/Qwen3.8-27B-FP8](https://huggingface.co/Qwen/Qwen3.8-27B-FP8) | Qwen | 660 | 1,939,895 | Official FP8 quantization from Qwen team, achieving 1.94M downloads with native image-text-to-text pipeline preservation. The near-2M figure validates official quantization as a trusted deployment path. |
| [0bserverx/Qwen3.8-27B-Heretic-Abliterated-Uncensored-GGUF](https://huggingface.co/0bserverx/Qwen3.8-27B-Heretic-Abliterated-Uncensored-GGUF) | 0bserverx | 213 | 421,918 | Aggressively branded abliterated variant with 422K downloads despite modest 213 likes, indicating strong niche demand. The "Heretic" naming exemplifies the provocative branding trend in uncensored model distribution. |
| [HauhauCS/Qwen3.8-27B-Uncensored-HauhauCS-Aggressive-MTP-GGUF](https://huggingface.co/HauhauCS/Qwen3.8-27B-Uncensored-HauhauCS-Aggressive-MTP-GGUF) | HauhauCS | 426 | 357,225 | Multimodal-capable uncensored GGUF with vision tag and aggressive MTP optimization, achieving 357K downloads. The image-text-to-text pipeline preservation in quantized form represents notable technical achievement. |
| [huihui-ai/Huihui-Qwen3.8-27B-abliterated-GGUF](https://huggingface.co/huihui-ai/Huihui-Qwen3.8-27B-abliterated-GGUF) | huihui-ai | 233 | 338,221 | Established abliterated quantization brand with 338K downloads, demonstrating sustained community trust. The consistent "huihui" naming across variants indicates deliberate portfolio building in the fine-tune ecosystem. |
| [OBLITERATUS/Qwen3.8-27B-OBLITERATED](https://huggingface.co/OBLITERATUS/Qwen3.8-27B-OBLITERATED) | OBLITERATUS | 451 | 123,956 | Multi-format release providing MLX, Safetensors, and GGUF in single repository, maximizing platform coverage. The 124K downloads and distinctive branding establish OBLITERATUS as a recognizable uncensored model source. |
| [ornith-ai/Ornith-1.5-35B-A3B-GGUF](https://huggingface.co/ornith-ai/Ornith-1.5-35B-A3B-GGUF) | ornith-ai | 208 | 123,237 | MIT-licensed MoE quantization with endpoint compatibility, differentiating through permissive licensing. The 123K downloads suggest commercial deployment interest enabled by license clarity. |
| [DavidAU/Qwen3.8-27B-Cold-Fusion-GAIN-V1.1-NM-DAU-NEO-MAX-MTP-GGUF](https://huggingface.co/DavidAU/Qwen3.8-27B-Cold-Fusion-GAIN-V1.1-NM-DAU-NEO-MAX-MTP-GGUF) | DavidAU | 173 | 155,208 | Elaborately named training-methodology showcase incorporating GAIN Training and COLD-FUSION techniques with Unsloth optimization. The 155K downloads validate interest in experimental training methodologies despite complex branding. |
| [Blackfrost-AI/Qwen3.8-27B-ABLITERATED-GGUF](https://huggingface.co/Blackfrost-AI/Qwen3.8-27B-ABLITERATED-GGUF) | Blackfrost-AI | 201 | 197,667 | Dense 27B GGUF variant with explicit "dense" architecture labeling, achieving 198K downloads. The technical specificity in tagging suggests targeting of users with particular deployment constraints. |
| [empero-ai/Qwen3.8-27B-Ridge-GGUF](https://huggingface.co/empero-ai/Qwen3.8-27B-Ridge-GGUF) | empero-ai | 238 | 74,038 | llama.cpp-optimized quantization with image-text-to-text pipeline preservation, though with lower adoption at 74K downloads. The "Ridge" branding indicates potential series development with distinctive optimization focus. |
| [orcarouter/Qwen3.8-27B-Uncensored-FP8](https://huggingface.co/orcarouter/Qwen3.8-27B-Uncensored-FP8) | orcarouter | 834 | 107,520 | Full-precision FP8 abliterated variant maintaining native transformers compatibility, offering uncensored output without quantization loss. The 834 likes represent strong community validation of this balanced approach. |
| [orcarouter/Qwen3.8-27B-Uncensored-MLX](https://huggingface.co/orcarouter/Qwen3.8-27B-Uncensored-MLX) | orcarouter | 825 | 18,193 | Apple Silicon-optimized MLX format for uncensored deployment, serving the growing Mac-based inference community. Lower downloads reflect platform specificity, but 825 likes indicate strong niche demand. |
| [orcarouter/Qwen3.8-27B-Uncensored-GGUF](https://huggingface.co/orcarouter/Qwen3.8-27B-Uncensored-GGUF) | orcarouter | 298 | 68,275 | Companion GGUF release to orcarouter's FP8 and MLX variants, completing cross-platform coverage. The portfolio strategy demonstrates systematic market segmentation by inference hardware. |
| [huihui-ai/Huihui-Qwen3.8-27B-abliterated](https://huggingface.co/huihui-ai/Huihui-Qwen3.8-27B-abliterated) | huihui-ai | 231 | 17,521 | Full-precision abliterated variant preserving native image-text-to-text capabilities, serving users prioritizing quality over quantization efficiency. The 17K downloads complement the higher-traffic GGUF counterpart. |

---

## 3. Ecosystem Signal

The Qwen 3.8 family exhibits unprecedented ecosystem dominance, with 18 of 30 trending models deriving from this single architecture—surpassing even Llama's historical concentration. This momentum reflects Alibaba's sustained investment in open-weight releases and the community's coalescence around proven training methodologies. Chinese model families (Qwen, DeepSeek, MiniMax, Moonshot) now constitute approximately 75% of trending entries, marking a decisive shift from the 2023-2024 US-centric landscape.

The quantization ecosystem reveals bifurcated demand: Unsloth's official optimizations achieve massive scale (5.8M downloads), while numerous community "abliterated" variants collectively demonstrate substantial but fragmented demand for unaligned models. The proliferation of provocative branding ("Heretic," "OBLITERATED") suggests market differentiation challenges in an oversaturated fine-tune space. Notably, multimodal capability preservation in quantized forms—particularly image-text-to-text—has become technically feasible and commercially valued, with several GGUF variants maintaining vision tags.

Format diversification accelerates: GGUF maintains dominance for consumer deployment, FP8 gains traction for datacenter efficiency, MLX serves Apple's ecosystem, and NVFP4 emerges for next-generation NVIDIA hardware. The near-absence of AWQ and GPTQ in trending results suggests format consolidation around llama.cpp-compatible and vendor-native quantization schemes.

---

## 4. Worth Exploring

**[moonshotai/Kimi-K3](https://huggingface.co/moonshotai/Kimi-K3)** — With 10,915 likes and 2.45M downloads, Kimi-K3 represents the most credible challenger to Qwen's dominance, particularly given its compressed-tensors implementation for efficient deployment. The feature-extraction capability alongside conversational functions suggests broader applicability than pure chat models, making it essential for benchmarking against Qwen3.8-27B in production environments.

**[unsloth/Qwen3.8-27B-NVFP4](https://huggingface.co/unsloth/Qwen3.8-27B-NVFP4)** — As NVIDIA's FP4 format achieves 1M+ downloads, this model offers early-mover advantage in next-generation GPU efficiency. For organizations with Blackwell-generation hardware, this represents the optimal performance-per-watt deployment path for Qwen's architecture, with Unsloth's optimization credibility reducing integration risk.

**[Lightricks/LTX-2.5](https://huggingface.co/Lightricks/LTX-2.5)** — The most versatile video generation pipeline available, supporting four distinct input modalities in a single model. For creative professionals and media startups, the 654K downloads and established vendor backing provide reliability that experimental community fine-tunes cannot match, particularly given the complexity of video model deployment.

---
*This digest is auto-generated by [agents-radar](https://github.com/brysoh435-dev/agents-radar).*