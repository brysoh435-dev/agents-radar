# Hugging Face 热门模型日报 2026-08-23

> 数据来源: [Hugging Face Hub](https://huggingface.co/) | 共 30 个模型 | 生成时间: 2026-08-23 03:30 UTC

---

# Hugging Face 热门模型日报 | 2026-08-23

---

## 今日速览

本周 Hugging Face 热度被 **Qwen3.8-27B** 家族绝对主导，前 7 名中 6 个为其衍生版本，原生模型以 12,162 点赞、209 万下载稳居榜首。**Kimi-K3** 以 10,930 点赞成为唯一能在热度上与之抗衡的国产模型。**MiniMax-H3** 视频生成模型异军突起，393 万下载验证了其商业化潜力。社区量化与"去审查化"（abliterated/uncensored）微调异常活跃，GGUF 格式占据半壁江山。DeepSeek-V4 系列保持稳健输出，但风头已被 Qwen 与 MiniMax 盖过。

---

## 热门模型

### 🧠 语言模型（LLM、对话模型、指令微调）

| 模型 | 作者 | 点赞 | 下载 | 简要说明 |
|:---|:---|---:|---:|:---|
| [Qwen/Qwen3.8-27B](https://huggingface.co/Qwen/Qwen3.8-27B) | Qwen | 12,162 | 2,090,699 | 阿里云 Qwen 3.8 系列旗舰，27B 参数实现图像-文本到文本的多模态对话，209 万周下载量彰显其基础设施级地位。标签中同时出现 qwen3_5 与 qwen3.8，暗示架构代际过渡中的命名策略。 |
| [moonshotai/Kimi-K3](https://huggingface.co/moonshotai/Kimi-K3) | moonshotai | 10,930 | 2,612,739 | 月之暗面 Kimi K3 以 261 万下载紧追 Qwen，compressed-tensors 标签表明采用新型压缩技术，feature-extraction 任务定位显示其向嵌入与表示学习延伸的野心。 |
| [deepseek-ai/DeepSeek-V4-Flash-0731](https://huggingface.co/deepseek-ai/DeepSeek-V4-Flash-0731) | deepseek-ai | 3,633 | 2,976,281 | DeepSeek V4 Flash 版本以近 300 万下载成为效率优先用户的首选，conversational 标签确认其对话优化定位，是长文本与实时场景的平衡点。 |
| [deepseek-ai/DeepSeek-V4-Pro-0813](https://huggingface.co/deepseek-ai/DeepSeek-V4-Pro-0813) | deepseek-ai | 719 | 54,566 | Pro 版本定位高端推理，但下载量仅为 Flash 的 1.8%，反映市场对"够用且快"的偏好远超极致性能追求，或受限于部署成本。 |
| [ornith-ai/Ornith-1.5-35B-A3B](https://huggingface.co/ornith-ai/Ornith-1.5-35B-A3B) | ornith-ai | 326 | 12,611 | 基于 Qwen3.5 MoE 架构的 35B 总参数/3B 激活参数稀疏模型，以极小激活成本实现大模型能力，是边缘部署与成本敏感场景的技术探针。 |
| [ornith-ai/Ornith-1.5-9B](https://huggingface.co/ornith-ai/Ornith-1.5-9B) | ornith-ai | 165 | 15,301 | 同系列 9B 稠密版本，下载反超 35B 版本，验证小参数模型在可达性与易用性上的优势，image-text-to-text 能力保留使其具备多模态性价比。 |
| [superwhisper/s1-mini](https://huggingface.co/superwhisper/s1-mini) | superwhisper | 202 | 1,913 | 融合 ASR（自动语音识别）与文本生成的跨界模型，1,913 下载尚处早期，但"语音-文本统一"标签预示多模态输入范式的演进方向。 |
| [empero-ai/Qwen3.8-9B-Distill](https://huggingface.co/empero-ai/Qwen3.8-9B-Distill) | empero-ai | 165 | 9,260 | 9B 蒸馏版本，从 27B 教师模型压缩而来，在保持 image-text-to-text 能力的同时大幅降低推理成本，是端侧部署的务实选择。 |
| [Qwen/Qwen3.8-2.4T-A95B](https://huggingface.co/Qwen/Qwen3.8-2.4T-A95B) | Qwen | 1,146 | 17,386 | 惊人的 2.4T 总参数/95B 激活参数 MoE 巨兽，17,386 下载与其体量形成反差，或受限于极少数企业的基础设施承载能力，更多是技术宣示。 |
| [z-lab/Qwen3.8-27B-DFlash2](https://huggingface.co/z-lab/Qwen3.8-27B-DFlash2) | z-lab | 194 | 29,705 | 引入 speculative-decoding（推测解码）加速技术，29,705 下载证明社区对推理速度优化的强烈需求，是性能工程方向的积极探索。 |

### 🎨 多模态与生成（图像、视频、音频、文本到X）

| 模型 | 作者 | 点赞 | 下载 | 简要说明 |
|:---|:---|---:|---:|:---|
| [MiniMaxAI/MiniMax-H3](https://huggingface.co/MiniMaxAI/MiniMax-H3) | MiniMaxAI | 4,344 | 3,899,160 | 本周最大黑马，389 万下载超越所有语言模型，text-to-video/image-to-video/image-text-to-video 三任务覆盖，diffusers 框架兼容降低接入门槛，标志国产视频生成进入规模化应用阶段。 |
| [Lightricks/LTX-2.5](https://huggingface.co/Lightricks/LTX-2.5) | Lightricks | 1,572 | 694,670 | 知名视觉工具商 Lightricks 的图像/视频生成升级，diffusion-single-file 格式简化部署，69 万下载验证其在创作者工具链中的稳固位置。 |
| [MiniMaxAI/MiniMax-Music3](https://huggingface.co/MiniMaxAI/MiniMax-Music3) | MiniMaxAI | 1,186 | 16,644 | MiniMax 音乐生成第三条产品线，text-to-music/music-generation 双标签，16,644 下载相对温和，反映音频生成赛道仍处培育期。 |
| [TenStrip/10Eros-Max](https://huggingface.co/TenStrip/10Eros-Max) | TenStrip | 317 | 0 | 基于 MiniMax-H3 的微调实验，0 下载但 317 点赞显示社区对视频模型二次创作的高度兴趣，finetune 标签揭示开源视频生态的萌芽。 |

### 📦 微调与量化（社区微调、GGUF、AWQ）

| 模型 | 作者 | 点赞 | 下载 | 简要说明 |
|:---|:---|---:|---:|:---|
| [unsloth/Qwen3.8-27B-GGUF](https://huggingface.co/unsloth/Qwen3.8-27B-GGUF) | unsloth | 2,642 | 6,320,542 | **本周下载冠军**，632 万下载碾压所有模型，unsloth 量化技术+GGUF 格式的组合成为本地部署事实标准，base_model 双标签确认其"官方量化衍生品"的权威地位。 |
| [JonathanColetti/Qwen3.8-27B-Uncensored-GGUF](https://huggingface.co/JonathanColetti/Qwen3.8-27B-Uncensored-GGUF) | JonathanColetti | 626 | 1,223,422 | "Uncensored"社区量化版突破 122 万下载，llama.cpp 兼容+mtp（多模态？）标签，反映用户对内容过滤移除的强烈需求与本地私有化部署的叠加效应。 |
| [huihui-ai/Huihui-Qwen3.8-27B-abliterated-GGUF](https://huggingface.co/huihui-ai/Huihui-Qwen3.8-27B-abliterated-GGUF) | huihui-ai | 257 | 635,416 | "abliterated"（消融审查）量化版，63 万下载验证该细分市场的持续性，transformers+gguf 双框架支持扩大用户覆盖。 |
| [0bserverx/Qwen3.8-27B-Heretic-Abliterated-Uncensored-GGUF](https://huggingface.co/0bserverx/Qwen3.8-27B-Heretic-Abliterated-Uncensored-GGUF) | 0bserverx | 229 | 505,813 | 命名激进的"异端"版本，50 万下载显示极端去审查定位仍有市场，heretic 标签本身成为社区亚文化标识。 |
| [HauhauCS/Qwen3.8-27B-Uncensored-HauhauCS-Aggressive-MTP-GGUF](https://huggingface.co/HauhauCS/Qwen3.8-27B-Uncensored-HauhauCS-Aggressive-MTP-GGUF) | HauhauCS | 490 | 486,221 | "Aggressive MTP"多模态量化方案，48 万下载且 vision 标签保留图像能力，是去审查化与多模态本地化的罕见结合。 |
| [DavidAU/Qwen3.8-27B-Cold-Fusion-GAIN-V1.1-NM-DAU-NEO-MAX-MTP-GGUF](https://huggingface.co/DavidAU/Qwen3.8-27B-Cold-Fusion-GAIN-V1.1-NM-DAU-NEO-MAX-MTP-GGUF) | DavidAU | 195 | 176,969 | 命名最长的"缝合怪"，GAIN Training/COLD-FUSION/unsloth 多技术叠加，17 万下载证明实验性合并模型仍有探索价值。 |
| [ornith-ai/Ornith-1.5-35B-A3B-GGUF](https://huggingface.co/ornith-ai/Ornith-1.5-35B-A3B-GGUF) | ornith-ai | 237 | 173,935 | MoE 架构的 GGUF 量化尝试，license:mit 与 endpoints_compatible 标签强调开放与可服务性，是稀疏模型本地化的早期样本。 |
| [orcarouter/Qwen3.8-27B-Uncensored-FP8](https://huggingface.co/orcarouter/Qwen3.8-27B-Uncensored-FP8) | orcarouter | 994 | 142,846 | FP8 精度而非 GGUF 的替代量化路径，14 万下载显示 NVIDIA 新一代 GPU 用户群体的存在，transformers 原生兼容降低迁移成本。 |
| [empero-ai/Qwen3.8-27B-Ridge-GGUF](https://huggingface.co/empero-ai/Qwen3.8-27B-Ridge-GGUF) | empero-ai | 245 | 97,247 | 标准量化衍生，llama.cpp 兼容，9.7 万下载处于中游，qwen3.8/qwen3.5 双标签反映版本迭代的社区适应期。 |
| [orcarouter/Qwen3.8-27B-Uncensored-GGUF](https://huggingface.co/orcarouter/Qwen3.8-27B-Uncensored-GGUF) | orcarouter | 339 | 85,371 | orcarouter 同名系列的 GGUF 版本，8.5 万下载仅为 FP8 版本的 60%，精度-兼容性权衡中用户倾向后者。 |
| [orcarouter/Qwen3.8-27B-Uncensored-MLX](https://huggingface.co/orcarouter/Qwen3.8-27B-Uncensored-MLX) | orcarouter | 888 | 34,909 | Apple Silicon 专用 MLX 框架版本，3.5 万下载精准覆盖 Mac 用户群体，是芯片生态分化在模型分发层面的直接体现。 |
| [huihui-ai/Huihui-Qwen3.8-27B-abliterated](https://huggingface.co/huihui-ai/Huihui-Qwen3.8-27B-abliterated) | huihui-ai | 246 | 21,612 | 未量化的原始微调版本，2.1 万下载远低于其 GGUF  counterpart，硬件门槛有效筛选了用户群体。 |
| [froggeric/Qwen-Fixed-Chat-Templates](https://huggingface.co/froggeric/Qwen-Fixed-Chat-Templates) | froggeric | 1,400 | 0 | 独特的"零下载高点赞"现象，专注修复 Qwen 对话模板，1,400 点赞纯因工具价值，MLX/jinja 标签锁定 Apple 生态开发者，是基础设施层贡献的典范。 |
| [OBLITERATUS/Qwen3.8-27B-OBLITERATED](https://huggingface.co/OBLITERATUS/Qwen3.8-27B-OBLITERATED) | OBLITERATUS | 544 | 164,950 | 同时支持 MLX/GGUF 双格式的"彻底消融"版本，16 万下载，text-generation 任务剥离多模态能力，专注纯文本极端场景。 |

---

## 生态信号

**Qwen 帝国化与国产模型三足鼎立。** Qwen3.8-27B 以单一模型衍生出 15+ 社区版本，覆盖 GGUF/FP8/MLX/原始权重全格式，形成事实上的开源基础设施。与此同时，Kimi-K3（月之暗面）、MiniMax-H3（MiniMax）、DeepSeek-V4（深度求索）构成国产模型"新四强"，阿里、月之暗面、MiniMax、DeepSeek 各据一条赛道。开源权重已成为基础模型触达用户的**必要非充分条件**——无开源则无生态，但有开源亦难变现，MiniMax-H3 的 389 万下载与闭源 API 的潜在张力值得观察。

量化与"去审查化"构成社区创新的两大主轴。GGUF 格式占据绝对主导，unsloth 成为量化基础设施的标准供应商；abliterated/uncensored/heretic 等标签的批量出现，既是技术现象也是文化现象，反映开源社区对模型安全对齐的反弹与重构。值得警惕的是，此类模型的下载量（JonathanColetti 122 万、0bserverx 50 万）已超越多数"干净"学术模型，其长期影响尚难评估。

---

## 值得探索

| 模型 | 推荐理由 |
|:---|:---|
| **[MiniMaxAI/MiniMax-H3](https://huggingface.co/MiniMaxAI/MiniMax-H3)** | 389 万下载验证的视频生成新标杆，diffusers 框架兼容意味着可立即接入现有管线。若从事内容创作、广告或影视预演，这是当前开源视频生成的最优起点；其 latent 空间已被社区用于 upscaler 开发（LBH-123-AI），生态延展性初显。 |
| **[unsloth/Qwen3.8-27B-GGUF](https://huggingface.co/unsloth/Qwen3.8-27B-GGUF)** | 632 万周下载的量化奇迹，unsloth 的技术实力使其成为本地部署 Qwen 的"默认选项"。若需在消费级 GPU 运行 27B 多模态模型，或研究量化对视觉-语言能力的保留程度，此为必测基准。 |
| **[ornith-ai/Ornith-1.5-35B-A3B](https://huggingface.co/ornith-ai/Ornith-1.5-35B-A3B)** | MoE 架构的激进实践——35B 总参数仅激活 3B，12,611 下载虽小众，却是稀疏模型能否挑战稠密模型效率极限的关键实验。若关注下一代模型架构或边缘部署的终极形态，值得跟踪其 GGUF 版本的后续表现。 |

---

---
*本日报由 [agents-radar](https://github.com/brysoh435-dev/agents-radar) 自动生成。*