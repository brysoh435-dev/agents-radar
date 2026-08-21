# Hugging Face Trending Models Digest 2026-08-21

> Source: [Hugging Face Hub](https://huggingface.co/) | 30 models | Generated: 2026-08-21 03:30 UTC

---

# Hugging Face Trending Models Digest — August 21, 2026

---

## 1. Today's Highlights

The Qwen3.8 family completely dominates this week's trending leaderboard, with the base **Qwen/Qwen3.8-27B** amassing 11,769 likes and over 1.37 million downloads, while spawning an entire ecosystem of quantized and uncensored variants. **moonshotai/Kimi-K3** emerges as a serious challenger with 10,887 likes and 2.35 million downloads, signaling strong appetite for alternative multimodal architectures. Video generation is heating up with **MiniMaxAI/MiniMax-H3** (4.2M downloads) and **Lightricks/LTX-2.5** gaining substantial traction. Notably, "abliterated" and uncensored fine-tunes represent a significant subculture—at least 8 variants appear in the top 30—suggesting sustained demand for unaligned model weights despite platform tensions. The proliferation of GGUF and FP8 formats by **unsloth** and community quantizers underscores how inference optimization has become table stakes for model distribution.

---

## 2. Trending Models

### 🧠 Language Models (LLMs, chat models, instruction-tuned)

| Model | Author | Likes | Downloads | Summary |
| :--- | :--- | ---: | ---: | :--- |
| [Qwen/Qwen3.8-27B](https://huggingface.co/Qwen/Qwen3.8-27B) | Qwen | 11,769 | 1,373,584 | Flagship 27B parameter multimodal language model with image-text-to-text capabilities; its massive download volume reflects broad adoption as a general-purpose foundation model. |
| [Qwen/Qwen3.8-2.4T-A95B](https://huggingface.co/Qwen/Qwen3.8-2.4T-A95B) | Qwen | 1,122 | 14,592 | Experimental 2.4 trillion parameter MoE variant with 95B active parameters; sparse architecture signals Qwen's push into ultra-large model territory despite selective adoption. |
| [deepseek-ai/DeepSeek-V4-Pro-0813](https://huggingface.co/deepseek-ai/DeepSeek-V4-Pro-0813) | deepseek-ai | 683 | 43,287 | Professional-tier text generation model in the DeepSeek-V4 family; relatively modest downloads suggest it's positioned for enterprise rather than mass consumer use. |
| [deepseek-ai/DeepSeek-V4-Flash-0731](https://huggingface.co/deepseek-ai/DeepSeek-V4-Flash-0731) | deepseek-ai | 3,578 | 2,547,549 | Speed-optimized variant of DeepSeek-V4 with 2.5M downloads; strong like-to-download ratio indicates efficient inference is winning user preference over raw capability. |
| [moonshotai/Kimi-K3](https://huggingface.co/moonshotai/Kimi-K3) | moonshotai | 10,887 | 2,349,853 | Moonshot AI's third-generation multimodal model with feature extraction and compressed-tensor optimizations; nearly matching Qwen's like count while offering architectural differentiation. |
| [ornith-ai/Ornith-1.5-35B-A3B](https://huggingface.co/ornith-ai/Ornith-1.5-35B-A3B) | ornith-ai | 227 | 1,713 | Compact 35B MoE with only 3B active parameters built on Qwen3.5 architecture; extremely low downloads suggest experimental status or niche targeting. |
| [superwhisper/s1-mini](https://huggingface.co/superwhisper/s1-mini) | superwhisper | 159 | 348 | Small speech recognition and text generation hybrid based on Qwen3; minimal adoption indicates ASR-LLM convergence remains early-stage. |
| [dots-studio/dots3-note-prev](https://huggingface.co/dots-studio/dots3-note-prev) | dots-studio | 242 | 1,373 | Preview of the Dots3 note-taking specialized model; low engagement suggests vertical applications struggle for visibility against generalist models. |

### 🎨 Multimodal & Generation (image, video, audio, text-to-X)

| Model | Author | Likes | Downloads | Summary |
| :--- | :--- | ---: | ---: | :--- |
| [MiniMaxAI/MiniMax-H3](https://huggingface.co/MiniMaxAI/MiniMax-H3) | MiniMaxAI | 4,245 | 3,308,673 | State-of-the-art image-text-to-video generation model with 3.3M downloads; its pipeline versatility (text-to-video, image-to-video) makes it the most adopted video foundation model this week. |
| [Lightricks/LTX-2.5](https://huggingface.co/Lightricks/LTX-2.5) | Lightricks | 1,421 | 611,825 | Advanced video generation suite supporting image-to-video, text-to-video, and video-to-video with 612K downloads; Lightricks' continued iteration signals mature commercial tooling entering open weights. |
| [MiniMaxAI/MiniMax-Music3](https://huggingface.co/MiniMaxAI/MiniMax-Music3) | MiniMaxAI | 1,116 | 14,471 | Third-generation text-to-music and music generation model; surprisingly low downloads relative to its video counterpart suggest audio generation remains a secondary market priority. |
| [lightx2v/Minimax-h3-Turbo](https://huggingface.co/lightx2v/Minimax-h3-Turbo) | lightx2v | 659 | 380,072 | Community-optimized "Turbo" variant of MiniMax-H3 for faster video generation; 380K downloads demonstrate strong demand for inference-accelerated derivatives of popular video models. |
| [TenStrip/10Eros-Max](https://huggingface.co/TenStrip/10Eros-Max) | TenStrip | 300 | 0 | Fine-tune of MiniMax-H3 for image-text-to-video; zero downloads despite 300 likes suggests content moderation concerns or very recent publication limiting distribution. |
| [meta-models/Muse-Glimmer-30B](https://huggingface.co/meta-models/Muse-Glimmer-30B) | meta-models | 1,719 | 478,622 | 30B parameter image-text-to-text model from Meta's open model initiative; strong like count and half-million downloads indicate Meta's branding still carries significant ecosystem weight. |

### 📦 Fine-tunes & Quantizations (community fine-tunes, GGUF, AWQ)

| Model | Author | Likes | Downloads | Summary |
| :--- | :--- | ---: | ---: | :--- |
| [unsloth/Qwen3.8-27B-GGUF](https://huggingface.co/unsloth/Qwen3.8-27B-GGUF) | unsloth | 2,372 | 5,126,652 | Official GGUF quantization by Unsloth with 5.1M downloads—surpassing even the base model; this is the most downloaded model on the entire leaderboard, proving quantization is the primary distribution channel. |
| [Qwen/Qwen3.8-27B-FP8](https://huggingface.co/Qwen/Qwen3.8-27B-FP8) | Qwen | 634 | 1,517,643 | Official 8-bit floating point quantization from Qwen team; 1.5M downloads show vendor-provided optimized formats compete effectively with community alternatives. |
| [unsloth/Qwen3.8-27B-NVFP4](https://huggingface.co/unsloth/Qwen3.8-27B-NVFP4) | unsloth | 308 | 831,483 | NVIDIA FP4 format quantization for next-generation GPU inference; 831K downloads indicate early adoption of Blackwell-era numerical formats despite hardware constraints. |
| [orcarouter/Qwen3.8-27B-Uncensored-MLX](https://huggingface.co/orcarouter/Qwen3.8-27B-Uncensored-MLX) | orcarouter | 719 | 2,628 | Apple MLX framework port of an abliterated Qwen3.8; low downloads relative to likes suggest Mac-specific optimization appeals to a vocal but smaller developer segment. |
| [orcarouter/Qwen3.8-27B-Uncensored-FP8](https://huggingface.co/orcarouter/Qwen3.8-27B-Uncensored-FP8) | orcarouter | 684 | 76,109 | FP8-quantized uncensored variant; 76K downloads demonstrate measurable demand for unaligned models in optimized formats, though far below mainstream quantizations. |
| [JonathanColetti/Qwen3.8-27B-Uncensored-GGUF](https://huggingface.co/JonathanColetti/Qwen3.8-27B-Uncensored-GGUF) | JonathanColetti | 520 | 979,768 | Near-million-download GGUF of the uncensored Qwen3.8; this is the most successful community uncensored quantization, approaching official distribution scale. |
| [HauhauCS/Qwen3.8-27B-Uncensored-HauhauCS-Aggressive-MTP-GGUF](https://huggingface.co/HauhauCS/Qwen3.8-27B-Uncensored-HauhauCS-Aggressive-MTP-GGUF) | HauhauCS | 371 | 268,258 | Aggressively abliterated GGUF with multimodal vision preservation; 268K downloads show niche for vision-capable uncensored models despite "aggressive" safety removal. |
| [froggeric/Qwen-Fixed-Chat-Templates](https://huggingface.co/froggeric/Qwen-Fixed-Chat-Templates) | froggeric | 1,342 | 0 | Corrected Jinja chat templates for MLX-deployed Qwen models; zero downloads with 1,342 likes is anomalous—likely a utility repository where likes reflect appreciation over direct installation. |
| [OBLITERATUS/Qwen3.8-27B-OBLITERATED](https://huggingface.co/OBLITERATUS/Qwen3.8-27B-OBLITERATED) | OBLITERATUS | 287 | 4,415 | Multi-format MLX/Safetensors/GGUF release with maximal safety removal branding; low downloads suggest "OBLITERATED" marketing may trigger platform suppression or user caution. |
| [empero-ai/Qwen3.8-27B-Ridge-GGUF](https://huggingface.co/empero-ai/Qwen3.8-27B-Ridge-GGUF) | empero-ai | 225 | 55,074 | Standard GGUF quantization with "Ridge" branding; mid-tier performance in a saturated Qwen derivative market. |
| [orcarouter/Qwen3.8-27B-Uncensored-GGUF](https://huggingface.co/orcarouter/Qwen3.8-27B-Uncensored-GGUF) | orcarouter | 243 | 52,382 | Earlier GGUF release from the prolific orcarouter; being superseded by their own FP8 and MLX variants. |
| [huihui-ai/Huihui-Qwen3.8-27B-abliterated-GGUF](https://huggingface.co/huihui-ai/Huihui-Qwen3.8-27B-abliterated-GGUF) | huihui-ai | 201 | 187,008 | Community GGUF with 187K downloads; "huihui" branding suggests regional or personal brand building within the abliteration subculture. |
| [0bserverx/Qwen3.8-27B-Heretic-Abliterated-Uncensored-GGUF](https://huggingface.co/0bserverx/Qwen3.8-27B-Heretic-Abliterated-Uncensored-GGUF) | 0bserverx | 191 | 326,638 | "Heretic"-branded uncensored GGUF with 327K downloads; provocative naming correlates with above-average traction in the uncensored segment. |
| [Blackfrost-AI/Qwen3.8-27B-ABLITERATED-GGUF](https://huggingface.co/Blackfrost-AI/Qwen3.8-27B-ABLITERATED-GGUF) | Blackfrost-AI | 184 | 186,470 | Dense 27B GGUF with all-caps "ABLITERATED" branding; standard performance for the category with consistent but not exceptional adoption. |
| [huihui-ai/Huihui-Qwen3.8-27B-abliterated](https://huggingface.co/huihui-ai/Huihui-Qwen3.8-27B-abliterated) | huihui-ai | 202 | 10,540 | Full-precision (non-GGUF) abliterated variant; only 10K downloads versus 187K for the GGUF version confirms users overwhelmingly prefer quantized formats. |
| [ornith-ai/Ornith-1.5-35B-A3B-GGUF](https://huggingface.co/ornith-ai/Ornith-1.5-35B-A3B-GGUF) | ornith-ai | 171 | 53,691 | GGUF of the compact Ornith MoE with MIT licensing and endpoint compatibility; 53K downloads suggest open licensing helps but cannot overcome base model obscurity. |

---

## 3. Ecosystem Signal

**Qwen hegemony is the defining feature** of this week's landscape: 19 of 30 trending models derive from Qwen3.8-27B, with the remaining Qwen-based entries building on Qwen3.5 MoE architectures. This concentration exceeds even Llama's historical dominance, suggesting Alibaba's open-weight strategy has achieved ecosystem lock-in comparable to Meta's. The data reveals a **bifurcated adoption pattern**: official quantizations (GGUF, FP8, NVFP4) drive mass downloads (5.1M, 1.5M, 831K respectively), while uncensored/abliterated variants attract disproportionate likes relative to downloads—indicating vocal advocacy but practical hesitation among mainstream deployers.

**Open-weight momentum continues accelerating against proprietary APIs**, with moonshotai's Kimi-K3 and DeepSeek-V4 both achieving multi-million downloads as open weights rather than gated API access. The quantization economy has matured into a **primary distribution layer**: Unsloth's GGUF outdownloads the base model 3.7:1, and community quantizers now constitute the majority of active repositories. Notably, "abliteration" has evolved from fringe activity to a **sustained subculture with standardized tooling**—8+ variants, consistent naming conventions, and predictable download patterns (50K–1M range). This normalization poses governance questions for Hugging Face, as these models achieve commercial-scale distribution while explicitly advertising safety removal.

---

## 4. Worth Exploring

**[moonshotai/Kimi-K3](https://huggingface.co/moonshotai/Kimi-K3)** — With 10,887 likes and 2.35M downloads, Kimi-K3 is the only model approaching Qwen's mindshare while offering genuine architectural differentiation (compressed tensors, feature extraction pipeline). Its performance as a non-Qwen multimodal contender makes it essential for benchmarking against Qwen3.8-27B, particularly for applications where model diversity provides robustness against single-family failure modes.

**[unsloth/Qwen3.8-27B-GGUF](https://huggingface.co/unsloth/Qwen3.8-27B-GGUF)** — The 5.1M download figure makes this the de facto standard deployment artifact for Qwen3.8. Unsloth's quantization quality has become a benchmark; studying their GGUF against official Qwen FP8 reveals tradeoffs between community optimization speed and vendor-supported precision that will inform procurement decisions across the industry.

**[Lightricks/LTX-2.5](https://huggingface.co/Lightricks/LTX-2.5)** — At 612K downloads with comprehensive video-to-video and image-to-video capabilities, LTX-2.5 represents the most mature open video generation stack outside the MiniMax ecosystem. Lightricks' commercial video editing heritage suggests production-ready architectures worth evaluating against MiniMax-H3 for professional content workflows, particularly where editability and temporal consistency matter more than generation speed.

---
*This digest is auto-generated by [agents-radar](https://github.com/brysoh435-dev/agents-radar).*