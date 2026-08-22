# Hugging Face 热门模型日报 2026-08-22

> 数据来源: [Hugging Face Hub](https://huggingface.co/) | 共 30 个模型 | 生成时间: 2026-08-22 03:08 UTC

---

# Hugging Face 热门模型日报 | 2026-08-22

---

## 今日速览

本周 Hugging Face 热度被 **Qwen3.8-27B** 家族彻底统治，前 30 名中近半数为其衍生版本，涵盖官方原版、GGUF 量化、FP8 精简及大量社区"去审查"微调。**Kimi-K3** 以 10,915 点赞稳居第二梯队，挑战 Qwen 霸主地位。视频生成领域 **MiniMax-H3** 下载量突破 360 万，成为多模态赛道最大黑马。值得注意的是，"abliterated/uncensored"标签模型泛滥，反映社区对模型安全对齐的强烈反向需求。

---

## 热门模型

### 🧠 语言模型（LLM、对话模型、指令微调）

| 模型 | 作者 | 点赞 | 下载 | 简要说明 |
|:---|:---|---:|---:|:---|
| [Qwen/Qwen3.8-27B](https://huggingface.co/Qwen/Qwen3.8-27B) | Qwen | 11,981 | 1,726,651 | 阿里云 Qwen 系列旗舰多模态大模型，支持图像-文本到文本的端到端理解。本周点赞数断层领先，官方 FP8 版本下载量近 200 万，验证其工业级落地能力。 |
| [moonshotai/Kimi-K3](https://huggingface.co/moonshotai/Kimi-K3) | moonshotai | 10,915 | 2,448,810 | 月之暗面第三代 Kimi 模型，采用压缩张量技术优化推理效率。下载量超越 Qwen 原版，成为实际部署最活跃的中文大模型之一。 |
| [deepseek-ai/DeepSeek-V4-Flash-0731](https://huggingface.co/deepseek-ai/DeepSeek-V4-Flash-0731) | deepseek-ai | 3,613 | 2,833,064 | DeepSeek V4 轻量版，以接近 300 万下载量成为效率优先场景的首选。Flash 版本在保持对话能力的同时大幅降低推理成本。 |
| [deepseek-ai/DeepSeek-V4-Pro-0813](https://huggingface.co/deepseek-ai/DeepSeek-V4-Pro-0813) | deepseek-ai | 709 | 49,601 | DeepSeek V4 专业版，定位高端推理场景。虽下载量受限，但代表国产模型在复杂任务上的技术纵深。 |
| [Qwen/Qwen3.8-2.4T-A95B](https://huggingface.co/Qwen/Qwen3.8-2.4T-A95B) | Qwen | 1,141 | 15,702 | Qwen 3.5 MoE 架构巨兽，2.4T 总参数/95B 激活参数，探索稀疏激活的 scaling law 边界。高门槛使其成为研究型用户专属。 |
| [ornith-ai/Ornith-1.5-35B-A3B](https://huggingface.co/ornith-ai/Ornith-1.5-35B-A3B) | ornith-ai | 291 | 9,165 | 基于 Qwen3.5 MoE 架构的社区衍生模型，35B 总参数/3B 激活参数。验证 MoE 小型化路径的可行性，适合边缘部署实验。 |
| [superwhisper/s1-mini](https://huggingface.co/superwhisper/s1-mini) | superwhisper | 191 | 1,136 | 融合 ASR 能力的文本生成模型，瞄准语音-文本统一处理场景。小众但精准切入实时转写+智能回复的垂直需求。 |
| [z-lab/Qwen3.8-27B-DFlash2](https://huggingface.co/z-lab/Qwen3.8-27B-DFlash2) | z-lab | 178 | 21,092 | 引入投机解码（speculative decoding）加速技术，为 Qwen3.8 提供低延迟推理方案。技术实验性质强，适合推理优化研究者。 |

### 🎨 多模态与生成（图像、视频、音频、文本到X）

| 模型 | 作者 | 点赞 | 下载 | 简要说明 |
|:---|:---|---:|---:|:---|
| [MiniMaxAI/MiniMax-H3](https://huggingface.co/MiniMaxAI/MiniMax-H3) | MiniMaxAI | 4,297 | 3,614,443 | MiniMax 第三代视频生成模型，支持文本/图像到视频及混合条件输入。本周下载量登顶全榜，国产视频模型首次在生态热度上匹敌 Runway/Pika。 |
| [Lightricks/LTX-2.5](https://huggingface.co/Lightricks/LTX-2.5) | Lightricks | 1,499 | 654,175 | Lightricks 视频生成套件升级，覆盖图生视频、文生视频、视频编辑全链路。65 万下载量验证专业创作工具的商业化潜力。 |
| [MiniMaxAI/MiniMax-Music3](https://huggingface.co/MiniMaxAI/MiniMax-Music3) | MiniMaxAI | 1,165 | 15,678 | MiniMax 音乐生成模型，基于扩散架构实现文本到音频创作。音频生成赛道相对冷清，其存在填补了高质量开源音乐模型的空白。 |
| [meta-models/Muse-Glimmer-30B](https://huggingface.co/meta-models/Muse-Glimmer-30B) | meta-models | 1,739 | 505,113 | 30B 参数多模态对话模型，定位图像-文本联合理解。50 万下载量显示中等规模多模态模型的实用 sweet spot。 |
| [TenStrip/10Eros-Max](https://huggingface.co/TenStrip/10Eros-Max) | TenStrip | 311 | 0 | 基于 MiniMax-H3 微调的社区衍生视频模型，零下载但获赞超 300，反映社区对"成人向"内容微调的高度关注与平台限制的张力。 |

### 📦 微调与量化（社区微调、GGUF、AWQ）

| 模型 | 作者 | 点赞 | 下载 | 简要说明 |
|:---|:---|---:|---:|:---|
| [unsloth/Qwen3.8-27B-GGUF](https://huggingface.co/unsloth/Qwen3.8-27B-GGUF) | unsloth | 2,518 | 5,804,917 | Unsloth 官方 GGUF 量化版，580 万下载量为全榜最高。证明消费级硬件运行 27B 多模态模型的刚性需求，量化生态已成基础设施。 |
| [unsloth/Qwen3.8-27B-NVFP4](https://huggingface.co/unsloth/Qwen3.8-27B-NVFP4) | unsloth | 328 | 1,013,917 | NVIDIA FP4 格式量化实验，百万级下载量验证新量化标准的市场接受度。与 GGUF 形成"通用 vs 专用"的双轨格局。 |
| [JonathanColetti/Qwen3.8-27B-Uncensored-GGUF](https://huggingface.co/JonathanColetti/Qwen3.8-27B-Uncensored-GGUF) | JonathanColetti | 572 | 1,126,222 | 社区"去审查"GGUF 量化版，下载量破百万。Uncensored + GGUF 的组合精准命中本地部署用户的双重诉求。 |
| [huihui-ai/Huihui-Qwen3.8-27B-abliterated-GGUF](https://huggingface.co/huihui-ai/Huihui-Qwen3.8-27B-abliterated-GGUF) | huihui-ai | 233 | 338,221 | 中文社区主导的 abliterated 量化版本，33 万下载量反映中文用户对安全对齐绕过工具的特定需求。 |
| [0bserverx/Qwen3.8-27B-Heretic-Abliterated-Uncensored-GGUF](https://huggingface.co/0bserverx/Qwen3.8-27B-Heretic-Abliterated-Uncensored-GGUF) | 0bserverx | 213 | 421,918 | "Heretic"品牌化的激进去审查版本，42 万下载量。命名策略本身成为社区亚文化现象，标志模型微调进入身份政治阶段。 |
| [HauhauCS/Qwen3.8-27B-Uncensored-HauhauCS-Aggressive-MTP-GGUF](https://huggingface.co/HauhauCS/Qwen3.8-27B-Uncensored-HauhauCS-Aggressive-MTP-GGUF) | HauhauCS | 426 | 357,225 | 集成 MTP（多 token 预测）加速的激进去审查版本，技术叠加特征明显。35 万下载量验证"速度+无限制"的产品化吸引力。 |
| [orcarouter/Qwen3.8-27B-Uncensored-FP8](https://huggingface.co/orcarouter/Qwen3.8-27B-Uncensored-FP8) | orcarouter | 834 | 107,520 | FP8 精度去审查版本，兼顾显存效率与内容自由度。orcarouter 同时提供 MLX 苹果芯片版本，覆盖全硬件生态。 |
| [orcarouter/Qwen3.8-27B-Uncensored-MLX](https://huggingface.co/orcarouter/Qwen3.8-27B-Uncensored-MLX) | orcarouter | 825 | 18,193 | 苹果 MLX 框架原生去审查版本，1.8 万下载量虽小但精准服务 Apple Silicon 用户。orcarouter 双版本策略展现平台覆盖意识。 |
| [orcarouter/Qwen3.8-27B-Uncensored-GGUF](https://huggingface.co/orcarouter/Qwen3.8-27B-Uncensored-GGUF) | orcarouter | 298 | 68,275 | orcarouter 的 GGUF 补充版本，形成 FP8/MLX/GGUF 三形态矩阵，是去审查生态中产品化最完整的社区作者之一。 |
| [OBLITERATUS/Qwen3.8-27B-OBLITERATED](https://huggingface.co/OBLITERATUS/Qwen3.8-27B-OBLITERATED) | OBLITERATUS | 451 | 123,956 | 同时提供 MLX/GGUF/Safetensors 三格式的"彻底抹除"版本，品牌命名极端化。12 万下载量显示激进营销在细分市场的有效性。 |
| [empero-ai/Qwen3.8-27B-Ridge-GGUF](https://huggingface.co/empero-ai/Qwen3.8-27B-Ridge-GGUF) | empero-ai | 238 | 74,038 | 相对温和的量化版本，命名去情绪化。7.4 万下载量说明仍有用户偏好"正常"品牌的中立选择。 |
| [Blackfrost-AI/Qwen3.8-27B-ABLITERATED-GGUF](https://huggingface.co/Blackfrost-AI/Qwen3.8-27B-ABLITERATED-GGUF) | Blackfrost-AI | 201 | 197,667 | 近 20 万下载量的"ABLITERATED"大写品牌化版本，去审查生态的同质化竞争中，命名差异化成为关键区分变量。 |
| [DavidAU/Qwen3.8-27B-Cold-Fusion-GAIN-V1.1-NM-DAU-NEO-MAX-MTP-GGUF](https://huggingface.co/DavidAU/Qwen3.8-27B-Cold-Fusion-GAIN-V1.1-NM-DAU-NEO-MAX-MTP-GGUF) | DavidAU | 173 | 155,208 | 名称最长的技术堆叠版本，整合 GAIN Training/COLD-FUSION/MTP 等多项技术。15 万下载量验证"功能堆砌"策略在发烧友市场的吸引力。 |
| [huihui-ai/Huihui-Qwen3.8-27B-abliterated](https://huggingface.co/huihui-ai/Huihui-Qwen3.8-27B-abliterated) | huihui-ai | 231 | 17,521 | 非量化原始权重版本，满足不愿牺牲精度的用户需求。与 GGUF 版形成"质量优先"vs"速度优先"的互补。 |
| [froggeric/Qwen-Fixed-Chat-Templates](https://huggingface.co/froggeric/Qwen-Fixed-Chat-Templates) | froggeric | 1,373 | 0 | 零下载但获赞超 1300 的聊天模板修复项目，揭示 Qwen 官方模板存在社区公认的缺陷。高赞零下载的反差说明"基础设施补丁"的价值被认可但难以直接消费。 |
| [ornith-ai/Ornith-1.5-35B-A3B-GGUF](https://huggingface.co/ornith-ai/Ornith-1.5-35B-A3B-GGUF) | ornith-ai | 208 | 123,237 | MoE 架构的 GGUF 量化实验，12 万下载量验证稀疏模型量化可行性。MIT 许可证与端点兼容标签指向商业化友好定位。 |

> **🔧 专用模型（代码、数学、医疗、嵌入）**：本周无此类模型进入前 30 名。

---

## 生态信号

**Qwen 家族形成绝对引力中心**。30 个热门模型中 18 个直接基于 Qwen3.8-27B，衍生链条覆盖官方量化（FP8/NVFP4）、社区去审查（abliterated/uncensored/OBLITERATED）、硬件适配（GGUF/MLX）及技术增强（MTP/投机解码）。这种"一超多强"格局比 2024 年 Llama 生态更极端，反映中文模型在全球开源社区的影响力跃升。

**"去审查"成为最大垂直赛道**。带 abliterated/uncensored/obliterated 标签的模型达 10 个，合计下载量超 250 万，形成与官方安全版本分庭抗礼的"影子生态"。这不仅是技术现象，更是开源社区对对齐（alignment）政策的集体反制，可能倒逼基础模型提供者重新评估安全策略的边界。

**量化格式战争暗流涌动**。GGUF 仍以绝对下载量统治消费级市场，但 NVFP4（NVIDIA 专用）、FP8（通用精简）、MLX（苹果原生）三足鼎立初现。Unsloth 作为量化基础设施提供者的地位稳固，其双版本策略（GGUF+NVFP4）预示未来量化服务将按硬件生态分化。

**视频生成成为多模态新前线**。MiniMax-H3 以 360 万下载量超越所有语言模型衍生版，LTX-2.5 紧随其后，国产视频模型首次在开源生态中建立全球级存在感。这与 2025 年 Runway 闭源主导的格局形成鲜明对比。

---

## 值得探索

| 模型 | 推荐理由 |
|:---|:---|
| **[MiniMaxAI/MiniMax-H3](https://huggingface.co/MiniMaxAI/MiniMax-H3)** | 视频生成领域的"ChatGPT 时刻"候选。360 万下载量不仅是数字，更代表创作者实际工作流的迁移。其图像-文本-视频三模态输入能力，可能重新定义短视频、广告、游戏资产生产的成本结构。建议关注其与 Sora、可灵等闭源方案的性价比对比。 |
| **[moonshotai/Kimi-K3](https://huggingface.co/moonshotai/Kimi-K3)** | 唯一能在热度上与 Qwen 家族抗衡的国产模型。压缩张量技术（compressed-tensors）是其差异化标签，若能在长文本场景保持优势，有望成为企业知识库、法律文档等垂直领域的首选基座。其 245 万下载量已证明市场认可。 |
| **[froggeric/Qwen-Fixed-Chat-Templates](https://huggingface.co/froggeric/Qwen-Fixed-Chat-Templates)** | 零下载但 1373 赞的"反常"存在，揭示 Qwen 生态的深层痛点。对于实际部署 Qwen 的开发者，官方聊天模板的缺陷可能导致指令跟随能力断崖式下降。该项目虽非模型，却是解锁 Qwen 潜力的关键基础设施，值得所有 Qwen 用户优先排查。 |

---

---
*本日报由 [agents-radar](https://github.com/brysoh435-dev/agents-radar) 自动生成。*