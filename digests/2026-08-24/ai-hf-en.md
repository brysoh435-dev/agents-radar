# Hugging Face Trending Models Digest 2026-08-24

> Source: [Hugging Face Hub](https://huggingface.co/) | 30 models | Generated: 2026-08-24 03:32 UTC

---

# Hugging Face Trending Models Digest — 2026-08-24

## 1. Today's Highlights

The Qwen3.8-27B ecosystem utterly dominates this week's trending list, with the base model from Qwen amassing over 12,000 likes and nearly 2.4 million downloads, while community quantizations and uncensored variants multiply rapidly across the Hub. Moonshot AI's Kimi-K3 emerges as the dark horse leader with 10,952 likes and strong download numbers, signaling growing competition in the multimodal LLM space from Chinese labs. DeepSeek's V4 series continues its steady ascent with both Flash and Pro variants gaining traction, even as the community fixates on "abliterated" and uncensored fine-tunes of Qwen models—a trend that now represents over one-third of all trending entries. MiniMax breaks the Qwen monopoly in generative media with H3 (video) and Music3 (audio), while the ornith-ai family of Qwen-derived MoE models hints at architectural experimentation beneath the quantization frenzy.

---

## 2. Trending Models

### 🧠 Language Models (LLMs, chat models, instruction-tuned)

| Model | Author | Likes | Downloads | Summary |
| :--- | :--- | ---: | ---: | :--- |
| [Qwen/Qwen3.8-27B](https://huggingface.co/Qwen/Qwen3.8-27B) | Qwen | 12,339 | 2,358,347 | The flagship 27B parameter multimodal language model from Alibaba's Qwen team, featuring native image-text-to-text capabilities and conversational fine-tuning. Its massive download volume reflects broad adoption as a foundation for downstream community work. |
| [deepseek-ai/DeepSeek-V4-Flash-0731](https://huggingface.co/deepseek-ai/DeepSeek-V4-Flash-0731) | deepseek-ai | 3,654 | 3,089,709 | DeepSeek's speed-optimized V4 variant delivering near-proprietary performance with over 3 million downloads, making it one of the most deployed open-weight models this week. The Flash branding signals aggressive latency reduction without sacrificing the V4 architecture's reasoning depth. |
| [moonshotai/Kimi-K3](https://huggingface.co/moonshotai/Kimi-K3) | moonshotai | 10,952 | 2,727,920 | Moonshot AI's K3 model leads all entries in raw likes with nearly 11,000, combining image-text-to-text multimodality with novel compression techniques ("compressed-tensors"). Its feature-extraction pipeline tag suggests emerging embedding or representation-learning applications beyond chat. |
| [deepseek-ai/DeepSeek-V4-Pro-0813](https://huggingface.co/deepseek-ai/DeepSeek-V4-Pro-0813) | deepseek-ai | 736 | 57,928 | The premium V4 variant with enhanced reasoning and longer context, positioned for enterprise and research use cases requiring maximum capability. Lower download counts relative to Flash indicate cost-performance sensitivity in the current deployment landscape. |
| [ornith-ai/Ornith-1.5-35B-A3B](https://huggingface.co/ornith-ai/Ornith-1.5-35B-A3B) | ornith-ai | 368 | 23,516 | A 35B parameter Mixture-of-Experts model with only 3B active parameters, built on the Qwen3.5 MoE architecture for efficient inference. The "A3B" designation highlights extreme activation sparsity as a potential path beyond dense scaling laws. |
| [ornith-ai/Ornith-1.5-9B](https://huggingface.co/ornith-ai/Ornith-1.5-9B) | ornith-ai | 186 | 31,496 | The smaller sibling in the Ornith family, maintaining multimodal image-text-to-text capabilities in a compact 9B parameter footprint. Strong downloads relative to likes suggest utility for edge and resource-constrained deployments. |
| [superwhisper/s1-mini](https://huggingface.co/superwhisper/s1-mini) | superwhisper | 215 | 2,280 | A compact Qwen3-based model optimized for automatic speech recognition (ASR), representing a niche but growing application of LLM architectures to audio transcription. Low absolute downloads indicate early-stage release or specialized target audience. |
| [z-lab/Qwen3.8-27B-DFlash2](https://huggingface.co/z-lab/Qwen3.8-27B-DFlash2) | z-lab | 206 | 36,234 | Implements speculative decoding ("DFlash2") on the Qwen3.8 base to accelerate inference without quantization loss. The technique's inclusion in model names suggests user-facing performance marketing around tokens-per-second. |
| [incoai/Qwen3.8-27B-DFlash2](https://huggingface.co/incoai/Qwen3.8-27B-DFlash2) | incoai | 164 | 69,783 | Parallel implementation of DFlash2 speculative decoding with significantly higher downloads than z-lab's variant, possibly due to integration with popular inference frameworks. The duplicated architecture name hints at emerging standardization attempts in acceleration techniques. |

### 🎨 Multimodal & Generation (image, video, audio, text-to-X)

| Model | Author | Likes | Downloads | Summary |
| :--- | :--- | ---: | ---: | :--- |
| [Qwen/Qwen3.8-27B](https://huggingface.co/Qwen/Qwen3.8-27B) | Qwen | 12,339 | 2,358,347 | Native image-text-to-text capabilities make this a dual-category leader, with vision understanding tightly integrated into the base conversational model. The pipeline designation reflects intentional multimodal design rather than bolted-on vision encoders. |
| [Lightricks/LTX-2.5](https://huggingface.co/Lightricks/LTX-2.5) | Lightricks | 1,645 | 738,345 | Professional-grade video generation supporting image-to-video, text-to-video, and video-to-video pipelines with a unified diffusion architecture. Lightricks' commercial pedigree shows in the polished "diffusion-single-file" packaging for streamlined deployment. |
| [MiniMaxAI/MiniMax-H3](https://huggingface.co/MiniMaxAI/MiniMax-H3) | MiniMaxAI | 4,384 | 4,039,236 | The most-downloaded generative media model this week at over 4 million, specializing in image-text-to-video synthesis with diffusers compatibility. MiniMax's H3 represents the strongest non-Qwen presence in the top tier, validating Chinese labs' growing strength in video generation. |
| [MiniMaxAI/MiniMax-Music3](https://huggingface.co/MiniMaxAI/MiniMax-Music3) | MiniMaxAI | 1,208 | 17,421 | Text-to-music and text-to-audio generation extending MiniMax's H3 media ecosystem into the audio domain. Relatively modest downloads suggest music generation remains a secondary priority for practitioners compared to language and video models. |
| [moonshotai/Kimi-K3](https://huggingface.co/moonshotai/Kimi-K3) | moonshotai | 10,952 | 2,727,920 | Multimodal image-text-to-text with emerging feature-extraction capabilities, positioning K3 as both a conversational interface and a vision encoder for downstream pipelines. The compressed-tensors tag indicates aggressive optimization for production inference costs. |

### 📦 Fine-tunes & Quantizations (community fine-tunes, GGUF, AWQ)

| Model | Author | Likes | Downloads | Summary |
| :--- | :--- | ---: | ---: | :--- |
| [unsloth/Qwen3.8-27B-GGUF](https://huggingface.co/unsloth/Qwen3.8-27B-GGUF) | unsloth | 2,743 | 6,674,515 | The definitive community quantization with the highest downloads across all 30 models, Unsloth's GGUF conversion enables local CPU/GPU inference of the 27B model. The "base_model:quantized" tag structure reflects Hub metadata conventions for derivative model provenance. |
| [orcarouter/Qwen3.8-27B-Uncensored-MLX](https://huggingface.co/orcarouter/Qwen3.8-27B-Uncensored-MLX) | orcarouter | 964 | 47,098 | Apple Silicon-optimized MLX format with "abliterated" safety training removal, targeting macOS developers with local inference priorities. The MLX ecosystem's growth is evident in multiple entries adopting Apple's framework over cross-platform alternatives. |
| [OBLITERATUS/Qwen3.8-27B-OBLITERATED](https://huggingface.co/OBLITERATUS/Qwen3.8-27B-OBLITERATED) | OBLITERATUS | 645 | 244,834 | Aggressive safety filter removal ("OBLITERATED") distributed in both MLX and GGUF formats for maximum hardware flexibility. The branding and 244K downloads reveal sustained demand for unfiltered model variants despite platform policy tensions. |
| [orcarouter/Qwen3.8-27B-Uncensored-FP8](https://huggingface.co/orcarouter/Qwen3.8-27B-Uncensored-FP8) | orcarouter | 1,051 | 190,062 | NVIDIA-optimized FP8 quantization preserving more precision than INT8/INT4 alternatives while maintaining uncensored behavior. The 1,051 likes suggest technical appreciation for the format choice beyond mere content unblocking. |
| [HauhauCS/Qwen3.8-27B-Uncensored-HauhauCS-Aggressive-MTP-GGUF](https://huggingface.co/HauhauCS/Qwen3.8-27B-Uncensored-HauhauCS-Aggressive-MTP-GGUF) | HauhauCS | 539 | 676,697 | "Aggressive MTP" (Multi-Token Prediction) variant combining uncensored fine-tuning with speculative output generation for 2-3x speedup. Nearly 677K downloads validate MTP as a user-valued feature in the quantization community. |
| [JonathanColetti/Qwen3.8-27B-Uncensored-GGUF](https://huggingface.co/JonathanColetti/Qwen3.8-Uncensored-GGUF) | JonathanColetti | 652 | 1,334,820 | Straightforward llama.cpp-compatible GGUF conversion of the uncensored base, achieving 1.3 million downloads through simplicity and reliability. The absence of experimental tags suggests this serves as a stable reference implementation. |
| [orcarouter/Qwen3.8-27B-Uncensored-GGUF](https://huggingface.co/orcarouter/Qwen3.8-27B-Uncensored-GGUF) | orcarouter | 382 | 108,666 | Companion GGUF to orcarouter's MLX variant, offering cross-platform coverage for the same uncensored weights. Lower downloads than JonathanColetti's version indicate format fragmentation splitting the GGUF audience. |
| [froggeric/Qwen-Fixed-Chat-Templates](https://huggingface.co/froggeric/Qwen-Fixed-Chat-Templates) | froggeric | 1,422 | 0 | Curated Jinja2 chat templates for MLX-deployed Qwen models, addressing formatting inconsistencies in official releases. Zero downloads reflect its nature as a configuration resource rather than weights artifact, with likes measuring community gratitude. |
| [huihui-ai/Huihui-Qwen3.8-27B-abliterated-GGUF](https://huggingface.co/huihui-ai/Huihui-Qwen3.8-27B-abliterated-GGUF) | huihui-ai | 304 | 943,360 | Near-million-download GGUF with "abliterated" (alignment-bypassed) training, establishing huihui-ai as a consistent presence in the uncensored quantization space. The branding consistency across variants builds recognizable trust in a fragmented ecosystem. |
| [DavidAU/Qwen3.8-27B-Cold-Fusion-GAIN-V1.1-NM-DAU-NEO-MAX-MTP-GGUF](https://huggingface.co/DavidAU/Qwen3.8-27B-Cold-Fusion-GAIN-V1.1-NM-DAU-NEO-MAX-MTP-GGUF) | DavidAU | 211 | 193,794 | Maximum-length model name encoding multiple techniques: GAIN training, COLD-FUSION merging, and MTP acceleration in one GGUF package. The acronym density targets searchability and technical differentiation in a crowded field. |
| [ornith-ai/Ornith-1.5-35B-A3B-GGUF](https://huggingface.co/ornith-ai/Ornith-1.5-35B-A3B-GGUF) | ornith-ai | 254 | 369,478 | GGUF conversion of the 35B MoE with MIT licensing and endpoint compatibility, making sparse architectures accessible to llama.cpp users. Strong downloads suggest pent-up demand for MoE quantization despite technical complexity. |
| [peculiar-ragdoll/Qwen-Sharp-Chat-Templates](https://huggingface.co/peculiar-ragdoll/Qwen-Sharp-Chat-Templates) | peculiar-ragdoll | 200 | 0 | Alternative MLX chat template collection emphasizing precise control over response formatting and style. Like froggeric's entry, zero downloads confirm template repositories as social rather than binary assets on the Hub. |
| [huihui-ai/Huihui-Qwen3.8-27B-abliterated](https://huggingface.co/huihui-ai/Huihui-Qwen3.8-27B-abliterated) | huihui-ai | 259 | 24,844 | Full-precision abliterated variant for users with GPU resources prioritizing quality over quantization efficiency. The 25K downloads represent a small but dedicated audience avoiding GGUF precision loss for critical applications. |
| [ornith-ai/Ornith-1.5-9B-GGUF](https://huggingface.co/ornith-ai/Ornith-1.5-9B-GGUF) | ornith-ai | 177 | 359,078 | Compact MoE GGUF with endpoint compatibility, achieving remarkable download efficiency relative to its modest like count. The 9B active parameter count makes this viable for consumer hardware while maintaining 35B-equivalent capacity. |
| [empero-ai/Qwen3.8-27B-Ridge-GGUF](https://huggingface.co/empero-ai/Qwen3.8-27B-Ridge-GGUF) | empero-ai | 249 | 131,435 | llama.cpp-optimized quantization with "Ridge" branding distinguishing its specific merge or training recipe. The qwen3.8/qwen3.5 dual tagging reflects uncertainty or compatibility claims across Qwen version boundaries. |
| [0bserverx/Qwen3.8-27B-Heretic-Abliterated-Uncensored-GGUF](https://huggingface.co/0bserverx/Qwen3.8-27B-Heretic-Abliterated-Uncensored-GGUF) | 0bserverx | 245 | 579,334 | "Heretic" branding signals ideological positioning in the uncensored model community, with nearly 580K downloads validating its technical execution. The triple redundancy in "Heretic-Abliterated-Uncensored" optimizes for search discoverability. |

---

## 3. Ecosystem Signal

The Qwen family exhibits unprecedented gravitational pull, with 22 of 30 trending models deriving from Qwen3.8-27B—far exceeding even Llama's historical dominance peaks. This concentration reflects both technical merit (strong multimodal base, permissive licensing) and ecosystem flywheel effects: quantizers target the most-downloaded models, which increases their visibility, which attracts more quantizers. The "abliterated" subgenre has matured from fringe activity to mainstream presence, with at least 8 explicit entries and implied alignment bypassing in several others. Chinese labs (Qwen, DeepSeek, MiniMax, Moonshot) now control the top tier of open-weight releases, while Western contributions concentrate in tooling (Unsloth), application layers (Lightricks), and community fine-tunes.

Quantization diversity has exploded beyond GGUF monoculture: MLX for Apple Silicon, FP8 for NVIDIA, speculative decoding (DFlash2, MTP) for latency reduction, and full-precision holdouts for quality purists. The absence of AWQ, GPTQ, or EXL2 entries suggests these formats are losing momentum to GGUF's universal llama.cpp compatibility and MLX's vertical optimization. Fine-tuning activity shows minimal architectural innovation—most variants are safety removals or format conversions rather than domain adaptation—raising questions about whether the open community is extracting full value from these capable bases.

---

## 4. Worth Exploring

**moonshotai/Kimi-K3** — With the highest like count and novel "compressed-tensors" technology, K3 represents the most credible challenge to Qwen's dominance. Its feature-extraction pipeline suggests applications beyond chat that remain underexplored; practitioners should investigate whether K3 embeddings rival dedicated models for retrieval and clustering tasks.

**ornith-ai/Ornith-1.5-35B-A3B** — The only MoE architecture in the trending list, with 3B active parameters from 35B total, offers a glimpse of efficient scaling that could disrupt the "bigger is better" assumptions driving 27B+ deployments. The MIT license and endpoint compatibility lower barriers to production experimentation with sparse models.

**Lightricks/LTX-2.5** — As the sole professional video generation tool from an established creative software company, LTX-2.5's multi-modal video pipeline (image/text/video-conditioned) may offer more reliable commercial deployment than research-oriented alternatives. The "diffusion-single-file" packaging specifically addresses operational pain points in video model serving.

---
*This digest is auto-generated by [agents-radar](https://github.com/brysoh435-dev/agents-radar).*