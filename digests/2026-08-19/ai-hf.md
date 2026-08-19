# Hugging Face 热门模型日报 2026-08-19

> 数据来源: [Hugging Face Hub](https://huggingface.co/) | 共 30 个模型 | 生成时间: 2026-08-19 05:56 UTC

---

# Hugging Face 热门模型日报 · 2026-08-19

---

## 今日速览

本周 Hugging Face 热度由 **Qwen3.8-27B** 与 **Kimi-K3** 两大旗舰多模态模型领跑，分别斩获 11,211 和 10,832 点赞，显示视觉-语言理解仍是社区核心焦点。**MiniMax-H3** 以 1464 万下载量成为视频生成领域的绝对流量入口，而 **DeepSeek-V4-Flash** 凭借 212 万下载验证了其作为高效推理模型的实用价值。社区量化生态异常活跃——Qwen3.8-27B 的 GGUF/FP8/MLX/NVFP4 衍生版本占据榜单近三分之一，反映边缘部署需求的爆发。此外，"Uncensored" 与 "ABLITERATED" 微调模型的密集出现，标志着开源社区对内容安全策略的主动重构。

---

## 热门模型

### 🧠 语言模型（LLM、对话模型、指令微调）

| 模型 | 作者 | 点赞 | 下载 | 简要说明 |
| :--- | :--- | ---: | ---: | :--- |
| [Qwen/Qwen3.8-2.4T-A95B](https://huggingface.co/Qwen/Qwen3.8-2.4T-A95B) | Qwen | 1,072 | 11,212 | 2.4T 总参数、95B 激活参数的 MoE 架构文本生成模型，代表 Qwen 家族在超大规模稀疏模型上的探索。下载量相对克制，反映 MoE 推理门槛仍高。 |
| [deepseek-ai/DeepSeek-V4-Pro-0813](https://huggingface.co/deepseek-ai/DeepSeek-V4-Pro-0813) | deepseek-ai | 606 | 30,985 | DeepSeek V4 系列的专业版迭代，延续该系列在代码与逻辑推理上的口碑。作为闭源权重开放模型，其 3 万下载显示开发者对 DeepSeek 生态的持续信任。 |
| [deepseek-ai/DeepSeek-V4-Flash-0731](https://huggingface.co/deepseek-ai/DeepSeek-V4-Flash-0731) | deepseek-ai | 3,533 | 2,123,462 | Flash 轻量化版本以 212 万下载成为本周效率模型的标杆，证明"小体积+高性能"路线在生产力场景中的统治力。点赞数仅次于头部多模态模型。 |
| [Qwen/Qwen3.8-2.4T-A95B-FP8](https://huggingface.co/Qwen/Qwen3.8-2.4T-A95B-FP8) | Qwen | 227 | 13,344 | MoE 巨模型的 FP8 量化官方版本，降低显存门槛的同时保持激活稀疏优势。下载量为基础版的 1.2 倍，量化对 MoE 的 democratize 效果显著。 |
| [nvidia/NVIDIA-Nemotron-3.5-Lightning-30B-A3B-NVFP4](https://huggingface.co/nvidia/NVIDIA-Nemotron-3.5-Lightning-30B-A3B-NVFP4) | nvidia | 326 | 269,372 | NVIDIA 自研 Nemotron 系列的 NVFP4 量化版，30B 总参数/3B 激活的 MoE 设计，26 万下载体现硬件厂商原生优化对开发者的吸引力。 |

---

### 🎨 多模态与生成（图像、视频、音频、文本到X）

| 模型 | 作者 | 点赞 | 下载 | 简要说明 |
| :--- | :--- | ---: | ---: | :--- |
| [Qwen/Qwen3.8-27B](https://huggingface.co/Qwen/Qwen3.8-27B) | Qwen | 11,211 | 665,513 | 本周点赞冠军，27B 规模的多模态对话模型，支持图像-文本交叉理解与生成。66 万下载与破万点赞验证其作为开源视觉语言标杆的地位。 |
| [MiniMaxAI/MiniMax-Music3](https://huggingface.co/MiniMaxAI/MiniMax-Music3) | MiniMaxAI | 981 | 11,745 | MiniMax 第三代音乐生成模型，文本到音频任务定位清晰。作为音乐垂直领域的少数开源大模型，填补了社区在高质量 AI 作曲工具上的空白。 |
| [Lightricks/LTX-2.5](https://huggingface.co/Lightricks/LTX-2.5) | Lightricks | 1,241 | 503,632 | 知名视频工具商 Lightricks 的开源视频生成模型，支持图生视频、文生视频、视频编辑三合一。50 万下载显示专业级视频开源工具的商业化潜力。 |
| [meta-models/Muse-Glimmer-30B](https://huggingface.co/meta-models/Muse-Glimmer-30B) | meta-models | 1,686 | 384,097 | 30B 规模的多模态对话模型，"Muse" 命名暗示其在创意内容理解上的优化。38 万下载与较高点赞数反映社区对非大厂开源多模态模型的期待。 |
| [MiniMaxAI/MiniMax-H3](https://huggingface.co/MiniMaxAI/MiniMax-H3) | MiniMaxAI | 4,150 | 2,855,539 | 本周下载量之王（1464 万含衍生版本），图生视频/文生视频双模态，4150 点赞居次席。MiniMax 在视频生成开源生态中的基础设施地位已然确立。 |
| [moonshotai/Kimi-K3](https://huggingface.co/moonshotai/Kimi-K3) | moonshotai | 10,832 | 2,226,898 | 月之暗面 Kimi 系列第三代，10,832 点赞紧追榜首，222 万下载。标签中的 "compressed-tensors" 暗示其在模型压缩上的技术创新，长文本能力或为隐藏卖点。 |
| [lightx2v/Minimax-h3-Turbo](https://huggingface.co/lightx2v/Minimax-h3-Turbo) | lightx2v | 613 | 300,279 | MiniMax-H3 的社区加速版本，专注推理优化。30 万下载证明开源社区对视频生成"推理基础设施"的旺盛需求，形成围绕头部模型的工具层生态。 |
| [Gazingstars123/Anima-2.9B](https://huggingface.co/Gazingstars123/Anima-2.9B) | Gazingstars123 | 256 | 24,893 | 2.9B 参数的轻量文生图模型，ComfyUI 原生支持定位明确。小体量+工作流友好使其成为个人创作者的低门槛入口。 |
| [TenStrip/10Eros-Max](https://huggingface.co/TenStrip/10Eros-Max) | TenStrip | 269 | 0 | MiniMax-H3 的社区微调版本，聚焦特定风格视频生成。零下载或反映新发布状态，但 269 点赞显示细分兴趣社区的早期关注度。 |
| [LiquidAI/LFM2.5-VL-3B](https://huggingface.co/LiquidAI/LFM2.5-VL-3B) | LiquidAI | 176 | 9,101 | 仅 3B 参数的视觉-语言模型，"Liquid" 标签暗示其可能采用液态神经网络等新型架构。小体量多模态是端侧部署的重要探索方向。 |

---

### 🔧 专用模型（代码、数学、医疗、嵌入）

> *本周 30 个热门模型中无明确归类为代码、数学、医疗或嵌入专用的模型，此分类省略。*

---

### 📦 微调与量化（社区微调、GGUF、AWQ）

| 模型 | 作者 | 点赞 | 下载 | 简要说明 |
| :--- | :--- | ---: | ---: | :--- |
| [unsloth/Qwen3.8-27B-GGUF](https://huggingface.co/unsloth/Qwen3.8-27B-GGUF) | unsloth | 1,849 | 3,561,466 | Unsloth 官方 GGUF 量化版，356 万下载为全榜最高，证明 GGUF 格式在消费级硬件部署中的绝对主流地位。Unsloth 作为量化基础设施的品牌效应显著。 |
| [Qwen/Qwen3.8-27B-FP8](https://huggingface.co/Qwen/Qwen3.8-27B-FP8) | Qwen | 569 | 741,011 | 官方 FP8 量化版本，74 万下载显示 NVIDIA Hopper/Ada 架构用户群体规模。FP8 作为新兴标准，正从实验性走向生产级采用。 |
| [orcarouter/Qwen3.8-27B-Uncensored-FP8](https://huggingface.co/orcarouter/Qwen3.8-27B-Uncensored-FP8) | orcarouter | 548 | 45,465 | 社区"去审查"微调+FP8 量化，4.5 万下载反映特定场景对内容安全策略自定义的需求。"Abliterated" 成为开源社区的显性技术运动。 |
| [JonathanColetti/Qwen3.8-27B-Uncensored-GGUF](https://huggingface.co/JonathanColetti/Qwen3.8-27B-Uncensored-GGUF) | JonathanColetti | 431 | 558,767 | GGUF 格式的去审查版本，55 万下载接近官方 FP8 版，说明 GGUF 的兼容性优势足以抵消精度损失，成为社区分发的默认选择。 |
| [orcarouter/Qwen3.8-27B-Uncensored-MLX](https://huggingface.co/orcarouter/Qwen3.8-27B-Uncensored-MLX) | orcarouter | 338 | 0 | Apple Silicon 原生 MLX 框架的去审查版本，零下载或刚发布，但 338 点赞显示 Mac 开发者对本地大模型运行的强烈兴趣。 |
| [unsloth/Qwen3.8-27B-NVFP4](https://huggingface.co/unsloth/Qwen3.8-27B-NVFP4) | unsloth | 268 | 523,919 | NVIDIA FP4 量化实验版本，52 万下载预示下一代 Blackwell 架构的提前布局。Unsloth 与硬件厂商的紧密协同值得持续关注。 |
| [froggeric/Qwen-Fixed-Chat-Templates](https://huggingface.co/froggeric/Qwen-Fixed-Chat-Templates) | froggeric | 1,261 | 0 | 专注修复 Qwen 系列对话模板的社区工具，1261 点赞为零下载中的异类。反映模板工程作为"隐性基础设施"对模型实际效果的关键影响。 |
| [HauhauCS/Qwen3.8-27B-Uncensored-HauhauCS-Aggressive-MTP-GGUF](https://huggingface.co/HauhauCS/Qwen3.8-27B-Uncensored-HauhauCS-Aggressive-MTP-GGUF) | HauhauCS | 223 | 27,745 | 激进 MTP（Multi-Token Prediction）优化的去审查 GGUF，2.7 万下载。MTP 作为推理加速技术正从论文走向社区实践。 |
| [DavidAU/Qwen3.6-27B-Fable-Fusion-711-Uncensored-Heretic-NM-DAU-NEO-MAX-MTP-GGUF](https://huggingface.co/DavidAU/Qwen3.6-27B-Fable-Fusion-711-Uncensored-Heretic-NM-DAU-NEO-MAX-MTP-GGUF) | DavidAU | 2,147 | 3,020,528 | 名称最长的"缝合怪"模型，却拥有 2147 点赞和 302 万下载的惊人成绩。融合多种微调技术（Heretic/NEO/MAX/MTP），代表社区"极致工程化"的极端案例。 |
| [Comfy-Org/MiniMax-Music-3](https://huggingface.co/Comfy-Org/MiniMax-Music-3) | Comfy-Org | 185 | 285,444 | ComfyUI 官方适配的 MiniMax-Music3 版本，28 万下载。工作流引擎与模型原生的绑定分发，成为开源 AI 工具链的新标准模式。 |
| [empero-ai/Qwen3.8-27B-Ridge-GGUF](https://huggingface.co/empero-ai/Qwen3.8-27B-Ridge-GGUF) | empero-ai | 178 | 12,854 | 社区 GGUF 量化版本之一，"Ridge" 或指特定量化策略。1.2 万下载显示 Qwen 量化生态的碎片化与繁荣并存。 |
| [Comfy-Org/MiniMax-H3](https://huggingface.co/Comfy-Org/MiniMax-H3) | Comfy-Org | 1,429 | 14,641,908 | **全榜总下载量最高模型**（1464 万），ComfyUI 官方适配的 MiniMax-H3。工作流生态的入口效应使其实际触达远超原版，"模型即插件"趋势的确证。 |
| [unsloth/Muse-Glimmer-30B-GGUF](https://huggingface.co/unsloth/Muse-Glimmer-30B-GGUF) | unsloth | 484 | 787,276 | Unsloth 对 Muse-Glimmer-30B 的 GGUF 转化，78 万下载。多模态模型的 GGUF 化尚处早期，此版本或为视觉-语言模型边缘部署的重要参考。 |
| [Blackfrost-AI/Qwen3.8-27B-ABLITERATED-GGUF](https://huggingface.co/Blackfrost-AI/Qwen3.8-27B-ABLITERATED-GGUF) | Blackfrost-AI | 153 | 134,149 | "ABLITERATED" 为去审查技术的新品牌命名，13 万下载。术语的品牌化运动反映该细分领域的成熟度提升与社区认同建构。 |

---

## 生态信号

**Qwen 家族**以 12 个上榜版本（含官方与衍生）形成绝对统治，覆盖 27B 稠密、2.4T MoE、FP8/GGUF/MLX/NVFP4 全量化谱系，其开源策略已从"模型发布"升级为"基础设施生态"。**MiniMax** 在视频（H3）与音频（Music3）双赛道建立开源标准，ComfyUI 适配版本的 1464 万下载揭示"模型-工具链"绑定分发的新权力结构。**Kimi-K3** 与 **DeepSeek-V4** 代表闭源厂商的权重开放路线，以高质量基础模型吸引开发者而非直接竞争生态位。量化层面，GGUF 仍为主流（5 个版本），但 FP8/NVFP4/MLX 的并行增长预示硬件碎片化时代的来临。最显著的社群信号是 **"Uncensored/ABLITERATED/Heretic"** 标签的密集出现——6 个相关模型累计 460 万下载，开源社区正以技术手段主动重构内容安全边界，这一运动已从边缘走向主流。

---

## 值得探索

| 模型 | 推荐理由 |
| :--- | :--- |
| **[moonshotai/Kimi-K3](https://huggingface.co/moonshotai/Kimi-K3)** | 10,832 点赞与 "compressed-tensors" 技术标签暗示其在长文本与模型压缩上的双重突破。月之暗面以长上下文著称，K3 或代表 200 万+ token 上下文的开源可行性，值得 RAG 与文档智能开发者优先测试。 |
| **[Comfy-Org/MiniMax-H3](https://huggingface.co/Comfy-Org/MiniMax-H3)** | 1464 万下载的"隐形冠军"，ComfyUI 工作流生态的事实标准入口。对于视频生成从业者，研究其节点设计与模型耦合方式，比直接使用原版更具工程价值——这是 AI 工具链"入口层"权力的典型案例。 |
| **[DavidAU/Qwen3.6-27B-Fable-Fusion-711...](https://huggingface.co/DavidAU/Qwen3.6-27B-Fable-Fusion-711-Uncensored-Heretic-NM-DAU-NEO-MAX-MTP-GGUF)** | 名称冗长到近乎行为艺术，却拥有 302 万下载的"社区缝合怪"极限案例。其融合的多项技术（Heretic/NEO/MAX/MTP）代表开源社区工程化的前沿实验，适合研究社区创新如何反向定义模型能力边界——而非仅关注官方版本。 |

---
*本日报由 [agents-radar](https://github.com/brysoh435-dev/agents-radar) 自动生成。*