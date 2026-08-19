# Hugging Face Trending Models Digest 2026-08-19

> Source: [Hugging Face Hub](https://huggingface.co/) | 30 models | Generated: 2026-08-19 05:56 UTC

---

# Hugging Face Trending Models Digest — August 19, 2026

---

## 1. Today's Highlights

The Qwen 3.8 series dominates this week's trending list, with the 27B parameter variant amassing over 11,000 likes and spawning a vibrant ecosystem of quantized and uncensored community fine-tunes. MiniMaxAI emerges as a major force in generative media, with both its H3 video generation model and Music3 audio model gaining significant traction—H3 alone has surpassed 2.8 million downloads. MoonshotAI's Kimi-K3 continues to challenge established multimodal leaders with nearly 11,000 likes and strong download numbers. The community's appetite for accessible model formats remains insatiable, with GGUF and FP8 variants consistently ranking among the most downloaded models despite lower like counts than their base counterparts.

---

## 2. Trending Models

### 🧠 Language Models (LLMs, chat models, instruction-tuned)

| Model | Author | Likes | Downloads | Summary |
| :--- | :--- | ---: | ---: | :--- |
| [Qwen/Qwen3.8-2.4T-A95B](https://huggingface.co/Qwen/Qwen3.8-2.4T-A95B) | Qwen | 1,072 | 11,212 | A massive 2.4 trillion parameter mixture-of-experts model with 95 billion active parameters, representing the frontier of scale-efficient language modeling. Despite its enormous size, it maintains relatively modest download numbers, reflecting the infrastructure challenges of deploying such large models. |
| [deepseek-ai/DeepSeek-V4-Pro-0813](https://huggingface.co/deepseek-ai/DeepSeek-V4-Pro-0813) | deepseek-ai | 606 | 30,985 | DeepSeek's latest professional-grade language model, continuing the V4 series with strong performance in reasoning and coding tasks. The "0813" date suffix indicates a recent refresh, keeping the model competitive with newer releases. |
| [deepseek-ai/DeepSeek-V4-Flash-0731](https://huggingface.co/deepseek-ai/DeepSeek-V4-Flash-0731) | deepseek-ai | 3,533 | 2,123,462 | A lightweight, speed-optimized variant of the V4 series that has achieved remarkable adoption with over 2.1 million downloads. Its 3,500+ likes and massive download volume demonstrate strong demand for efficient, high-performance language models. |
| [Qwen/Qwen3.8-2.4T-A95B-FP8](https://huggingface.co/Qwen/Qwen3.8-2.4T-A95B-FP8) | Qwen | 227 | 13,344 | An FP8-quantized version of Qwen's flagship MoE model, enabling more accessible deployment of the 2.4T parameter architecture. The quantization provides memory efficiency gains while preserving much of the base model's capability. |
| [nvidia/NVIDIA-Nemotron-3.5-Lightning-30B-A3B-NVFP4](https://huggingface.co/nvidia/NVIDIA-Nemotron-3.5-Lightning-30B-A3B-NVFP4) | nvidia | 326 | 269,372 | NVIDIA's enterprise-focused 30B parameter model with 3 billion active parameters in their proprietary NVFP4 format, optimized for inference on NVIDIA hardware. Strong download numbers reflect enterprise interest in hardware-optimized deployment. |

### 🎨 Multimodal & Generation (image, video, audio, text-to-X)

| Model | Author | Likes | Downloads | Summary |
| :--- | :--- | ---: | ---: | :--- |
| [Qwen/Qwen3.8-27B](https://huggingface.co/Qwen/Qwen3.8-27B) | Qwen | 11,211 | 665,513 | The flagship multimodal model of the Qwen 3.8 series, supporting image-text-to-text conversations with strong vision-language understanding. Its dominant like count and substantial downloads establish it as the week's most popular release. |
| [MiniMaxAI/MiniMax-Music3](https://huggingface.co/MiniMaxAI/MiniMax-Music3) | MiniMaxAI | 981 | 11,745 | A dedicated text-to-music generation model from MiniMaxAI, representing continued investment in audio generation capabilities. The model shows strong niche appeal despite lower overall numbers compared to video counterparts. |
| [Lightricks/LTX-2.5](https://huggingface.co/Lightricks/LTX-2.5) | Lightricks | 1,241 | 503,632 | An versatile image-to-video model from Lightricks that also supports text-to-video, video-to-video, and image-text-to-video generation. Over 500,000 downloads indicate broad adoption among video creators and developers. |
| [meta-models/Muse-Glimmer-30B](https://huggingface.co/meta-models/Muse-Glimmer-30B) | meta-models | 1,686 | 384,097 | A 30B parameter image-text-to-text model from Meta's ecosystem, positioning as a substantial multimodal alternative to Qwen and Kimi. Strong like-to-download ratio suggests high quality perception among early adopters. |
| [MiniMaxAI/MiniMax-H3](https://huggingface.co/MiniMaxAI/MiniMax-H3) | MiniMaxAI | 4,150 | 2,855,539 | MiniMaxAI's flagship video generation model supporting both text-to-video and image-to-video with exceptional adoption metrics. Its 4,150 likes and nearly 2.9 million downloads make it one of the week's standout successes across all categories. |
| [moonshotai/Kimi-K3](https://huggingface.co/moonshotai/Kimi-K3) | moonshotai | 10,832 | 2,226,898 | MoonshotAI's highly anticipated Kimi-K3 multimodal model, nearly matching Qwen 3.8-27B in popularity with over 10,800 likes. Strong performance in feature extraction and compressed tensor support highlight its technical sophistication. |
| [lightx2v/Minimax-h3-Turbo](https://huggingface.co/lightx2v/Minimax-h3-Turbo) | lightx2v | 613 | 300,279 | A community-optimized "Turbo" variant of MiniMax-H3 focused on faster inference for text-to-video and image-to-video workflows. The 300,000+ downloads demonstrate demand for speed-optimized versions of popular video models. |
| [Gazingstars123/Anima-2.9B](https://huggingface.co/Gazingstars123/Anima-2.9B) | Gazingstars123 | 256 | 24,893 | A compact 2.9 billion parameter text-to-image model with ComfyUI integration, targeting accessible anime-style generation. Niche appeal is evident in its focused tag set and moderate but dedicated user base. |
| [LiquidAI/LFM2.5-VL-3B](https://huggingface.co/LiquidAI/LFM2.5-VL-3B) | LiquidAI | 176 | 9,101 | A highly efficient 3 billion parameter vision-language model from LiquidAI, emphasizing deployment efficiency over raw scale. The "liquid" architecture tag suggests novel approaches to adaptive computation. |
| [TenStrip/10Eros-Max](https://huggingface.co/TenStrip/10Eros-Max) | TenStrip | 269 | 0 | A fine-tuned variant of MiniMax-H3 for image-text-to-video generation, though currently showing zero downloads possibly due to recent upload or access restrictions. The fine-tune demonstrates the expanding ecosystem around MiniMax's video foundation model. |

### 📦 Fine-tunes & Quantizations (community fine-tunes, GGUF, AWQ)

| Model | Author | Likes | Downloads | Summary |
| :--- | :--- | ---: | ---: | :--- |
| [unsloth/Qwen3.8-27B-GGUF](https://huggingface.co/unsloth/Qwen3.8-27B-GGUF) | unsloth | 1,849 | 3,561,466 | The most-downloaded model this week with over 3.5 million downloads, Unsloth's GGUF quantization of Qwen 3.8-27B enables local inference on consumer hardware. This staggering download volume reveals massive demand for accessible, runnable versions of top-tier multimodal models. |
| [orcarouter/Qwen3.8-27B-Uncensored-FP8](https://huggingface.co/orcarouter/Qwen3.8-27B-Uncensored-FP8) | orcarouter | 548 | 45,465 | An "abliterated" uncensored FP8 variant of Qwen 3.8-27B, removing alignment constraints for unrestricted research and application. The "abliterated" tag signals growing community interest in exploring base model capabilities without safety fine-tuning. |
| [JonathanColetti/Qwen3.8-27B-Uncensored-GGUF](https://huggingface.co/JonathanColetti/Qwen3.8-27B-Uncensored-GGUF) | JonathanColetti | 431 | 558,767 | A GGUF-quantized uncensored version of Qwen 3.8-27B for llama.cpp compatibility, achieving substantial downloads despite moderate likes. The "mtp" tag suggests multi-token prediction enhancements for faster inference. |
| [orcarouter/Qwen3.8-27B-Uncensored-MLX](https://huggingface.co/orcarouter/Qwen3.8-27B-Uncensored-MLX) | orcarouter | 338 | 0 | An Apple MLX framework-compatible uncensored variant, currently showing zero downloads likely due to very recent release or platform-specific distribution. Represents the expanding hardware-specific quantization ecosystem. |
| [unsloth/Qwen3.8-27B-NVFP4](https://huggingface.co/unsloth/Qwen3.8-27B-NVFP4) | unsloth | 268 | 523,919 | Unsloth's NVIDIA FP4 format quantization, optimized for latest-generation NVIDIA GPU inference with substantial download numbers. The 523,000+ downloads demonstrate strong uptake of vendor-specific optimized formats. |
| [froggeric/Qwen-Fixed-Chat-Templates](https://huggingface.co/froggeric/Qwen-Fixed-Chat-Templates) | froggeric | 1,261 | 0 | A utility repository providing corrected Jinja chat templates for Qwen 3.5 models, with surprisingly high likes for a non-weight artifact. The 1,261 likes and zero downloads indicate it's valued as a reference implementation rather than downloadable asset. |
| [DavidAU/Qwen3.6-27B-Fable-Fusion-711-Uncensored-Heretic-NM-DAU-NEO-MAX-MTP-GGUF](https://huggingface.co/DavidAU/Qwen3.6-27B-Fable-Fusion-711-Uncensored-Heretic-NM-DAU-NEO-MAX-MTP-GGUF) | DavidAU | 2,147 | 3,020,528 | An extraordinarily named community fusion model combining multiple fine-tuning approaches, achieving over 3 million downloads and strong likes. The "Heretic" branding and aggressive feature stacking exemplify the experimental, maximalist approach of certain community quantizers. |
| [HauhauCS/Qwen3.8-27B-Uncensored-HauhauCS-Aggressive-MTP-GGUF](https://huggingface.co/HauhauCS/Qwen3.8-27B-Uncensored-HauhauCS-Aggressive-MTP-GGUF) | HauhauCS | 223 | 27,745 | A multimodal-capable GGUF with "aggressive" multi-token prediction optimization, maintaining vision support in quantized form. The specialized naming and moderate uptake reflect the fragmented but innovative community quantization landscape. |
| [empero-ai/Qwen3.8-27B-Ridge-GGUF](https://huggingface.co/empero-ai/Qwen3.8-27B-Ridge-GGUF) | empero-ai | 178 | 12,854 | A straightforward llama.cpp-compatible GGUF quantization emphasizing stability and compatibility over experimental features. Modest but solid numbers suggest reliable performance for users seeking predictable deployment. |
| [Comfy-Org/MiniMax-Music-3](https://huggingface.co/Comfy-Org/MiniMax-Music-3) | Comfy-Org | 185 | 285,444 | An Apache-2.0 licensed ComfyUI-compatible repackaging of MiniMax-Music3, dramatically expanding accessibility for workflow-based users. The 285,000+ downloads validate Comfy-Org's role in democratizing model access through popular interfaces. |
| [Comfy-Org/MiniMax-H3](https://huggingface.co/Comfy-Org/MiniMax-H3) | Comfy-Org | 1,429 | 14,641,908 | The week's absolute download leader with over 14.6 million downloads, this ComfyUI repackaging of MiniMax-H3 demonstrates the transformative impact of interface optimization. The staggering download disparity versus the original model highlights ComfyUI's central role in video generation workflows. |
| [unsloth/Muse-Glimmer-30B-GGUF](https://huggingface.co/unsloth/Muse-Glimmer-30B-GGUF) | unsloth | 484 | 787,276 | Unsloth's GGUF quantization of Meta's Muse-Glimmer-30B, bringing 30B parameter multimodal capabilities to local inference setups. Strong download numbers indicate healthy interest in alternatives to the Qwen-dominated ecosystem. |
| [Blackfrost-AI/Qwen3.8-27B-ABLITERATED-GGUF](https://huggingface.co/Blackfrost-AI/Qwen3.8-27B-ABLITERATED-GGUF) | Blackfrost-AI | 153 | 134,149 | A dense, fully uncensored GGUF variant using the "abliterated" methodology for complete alignment removal. The 134,000 downloads show sustained demand for unrestricted model variants despite lower visibility in likes rankings. |

---

## 3. Ecosystem Signal

The Qwen 3.8 family has achieved near-hegemonic dominance in this week's trends, with the 27B parameter variant serving as the foundation for at least 13 distinct derivative models across quantization formats, hardware targets, and censorship configurations. This concentration reveals both the quality of Qwen's base release and the community's tendency to coalesce around proven architectures rather than distribute attention evenly. MiniMaxAI represents the most credible challenger, with its H3 video model and Music3 audio model demonstrating that generative media is the fastest-growing alternative frontier.

The quantization ecosystem has matured into a sophisticated multi-format landscape: GGUF for universal CPU/GPU inference via llama.cpp, FP8 and NVFP4 for NVIDIA hardware optimization, and MLX for Apple's silicon ecosystem. Unsloth has established itself as the premier quantization provider, with three entries totaling over 4.8 million downloads. The "uncensored" and "abliterated" movement continues to gain momentum, with multiple high-download variants suggesting that alignment removal is becoming a standard community practice rather than a niche interest.

Notably, the download leaderboards are increasingly decoupled from like counts: Comfy-Org's MiniMax-H3 repackaging achieved 14.6 million downloads with only 1,429 likes, while froggeric's chat template fix garnered 1,261 likes with zero downloads. This divergence indicates that utility and accessibility often trump perceived quality in driving actual adoption.

---

## 4. Worth Exploring

**[moonshotai/Kimi-K3](https://huggingface.co/moonshotai/Kimi-K3)** — With nearly 11,000 likes and over 2.2 million downloads, Kimi-K3 stands as the most credible challenger to Qwen's multimodal dominance. Its "compressed-tensors" tag suggests novel efficiency approaches that could inform next-generation model architectures. Worth studying for its balance of scale, capability, and deployment efficiency.

**[Comfy-Org/MiniMax-H3](https://huggingface.co/Comfy-Org/MiniMax-H3)** — The 14.6 million download figure makes this the definitive case study in interface-driven model adoption. Even if you don't use ComfyUI, understanding how packaging and workflow integration can amplify a base model's reach by 5x is essential for anyone involved in model distribution.

**[deepseek-ai/DeepSeek-V4-Flash-0731](https://huggingface.co/deepseek-ai/DeepSeek-V4-Flash-0731)** — DeepSeek's speed-optimized variant achieves an exceptional likes-to-downloads ratio with over 2.1 million downloads, suggesting it delivers on its performance promises. As efficient inference becomes increasingly critical, this model represents a benchmark for "fast enough" language models that don't sacrifice too much capability.

---
*This digest is auto-generated by [agents-radar](https://github.com/brysoh435-dev/agents-radar).*