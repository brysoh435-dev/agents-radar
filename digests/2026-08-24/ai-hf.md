# Hugging Face 热门模型日报 2026-08-24

> 数据来源: [Hugging Face Hub](https://huggingface.co/) | 共 30 个模型 | 生成时间: 2026-08-24 03:32 UTC

---

# Hugging Face 热门模型日报 · 2026-08-24

---

## 今日速览

Qwen 3.8 系列以绝对统治力霸榜，30 个热门模型中 Qwen 3.8-27B 及其衍生版本占据 18 席，涵盖官方原版、GGUF 量化、MLX 适配及大量社区"去审查"微调。多模态生成赛道同样火热，MiniMax-H3 视频生成模型与 Lightricks LTX-2.5 分别斩获超 400 万和 73 万下载。DeepSeek-V4 系列与 Kimi-K3 作为非 Qwen 系代表跻身前列，显示头部基座模型竞争仍存变数。值得关注的是，"abliterated/uncensored"标签高频出现，反映社区对模型安全对齐的反向需求显著。

---

## 热门模型

### 🧠 语言模型（LLM、对话模型、指令微调）

| 模型 | 作者 | 点赞 | 下载 | 简要说明 |
|:---|:---|---:|---:|:---|
| [Qwen/Qwen3.8-27B](https://huggingface.co/Qwen/Qwen3.8-27B) | Qwen | 12,339 | 2,358,347 | 阿里云 Qwen 3.8 系列旗舰开源模型，支持 image-text-to-text 多模态对话。本周点赞数断层第一，官方 FP8 量化版同步发布，下载量超 265 万。 |
| [deepseek-ai/DeepSeek-V4-Flash-0731](https://huggingface.co/deepseek-ai/DeepSeek-V4-Flash-0731) | deepseek-ai | 3,654 | 3,089,709 | DeepSeek V4 轻量版，主打高效推理与对话能力。下载量突破 308 万，是非 Qwen 系中人气最高的基座模型。 |
| [moonshotai/Kimi-K3](https://huggingface.co/moonshotai/Kimi-K3) | moonshotai | 10,952 | 2,727,920 | 月之暗面 Kimi K3 多模态模型，支持图像理解与生成长文本。点赞数仅次于 Qwen 3.8-27B，压缩张量技术降低部署门槛。 |
| [deepseek-ai/DeepSeek-V4-Pro-0813](https://huggingface.co/deepseek-ai/DeepSeek-V4-Pro-0813) | deepseek-ai | 736 | 57,928 | DeepSeek V4 专业版，定位更高精度的文本生成与复杂推理。发布较晚但增长稳健，Pro 系列技术路线值得关注。 |
| [ornith-ai/Ornith-1.5-35B-A3B](https://huggingface.co/ornith-ai/Ornith-1.5-35B-A3B) | ornith-ai | 368 | 23,516 | 基于 Qwen3.5-MoE 架构的社区衍生模型，35B 激活 3B 参数。以 MIT 许可证开源，GGUF 版本下载量达 36.9 万。 |
| [ornith-ai/Ornith-1.5-9B](https://huggingface.co/ornith-ai/Ornith-1.5-9B) | ornith-ai | 186 | 31,496 | Ornith 系列轻量版，9B 参数适合端侧部署。同样采用宽松 MIT 许可证，社区友好度高。 |
| [z-lab/Qwen3.8-27B-DFlash2](https://huggingface.co/z-lab/Qwen3.8-27B-DFlash2) | z-lab | 206 | 36,234 | 引入 DFlash2 投机解码技术，加速 Qwen 3.8 推理。小众但技术导向明确，适合 latency-sensitive 场景研究。 |
| [incoai/Qwen3.8-27B-DFlash2](https://huggingface.co/incoai/Qwen3.8-27B-DFlash2) | incoai | 164 | 69,783 | 另一 DFlash2 投机解码实现，下载量更高验证社区需求。与 z-lab 版本形成技术对照，生态多样性显现。 |
| [superwhisper/s1-mini](https://huggingface.co/superwhisper/s1-mini) | superwhisper | 215 | 2,280 | 面向语音识别的 Qwen3 微调模型，ASR 标签明确。下载量尚低但定位独特，语音-文本跨模态值得跟踪。 |

### 🎨 多模态与生成（图像、视频、音频、文本到X）

| 模型 | 作者 | 点赞 | 下载 | 简要说明 |
|:---|:---|---:|---:|:---|
| [MiniMaxAI/MiniMax-H3](https://huggingface.co/MiniMaxAI/MiniMax-H3) | MiniMaxAI | 4,384 | 4,039,236 | MiniMax 新一代视频生成模型，支持文本/图像/视频多条件输入。本周下载量破 400 万，点赞数居视频类首位，国产视频模型出海标杆。 |
| [Lightricks/LTX-2.5](https://huggingface.co/Lightricks/LTX-2.5) | Lightricks | 1,645 | 738,345 | 知名创意工具商 Lightricks 开源视频生成模型，支持图像/文本/视频三重输入。单文件扩散格式简化部署，专业创作者关注度高。 |
| [MiniMaxAI/MiniMax-Music3](https://huggingface.co/MiniMaxAI/MiniMax-Music3) | MiniMaxAI | 1,208 | 17,421 | MiniMax 音乐生成模型，文本到音频/音乐创作。下载量相对温和，但填补了本周音乐生成赛道空白。 |

### 📦 微调与量化（社区微调、GGUF、AWQ）

| 模型 | 作者 | 点赞 | 下载 | 简要说明 |
|:---|:---|---:|---:|:---|
| [unsloth/Qwen3.8-27B-GGUF](https://huggingface.co/unsloth/Qwen3.8-27B-GGUF) | unsloth | 2,743 | 6,674,515 | Unsloth 官方 GGUF 量化版，本周下载量全榜最高超 667 万。量化效率与兼容性平衡优异，本地部署首选。 |
| [JonathanColetti/Qwen3.8-27B-Uncensored-GGUF](https://huggingface.co/JonathanColetti/Qwen3.8-27B-Uncensored-GGUF) | JonathanColetti | 652 | 1,334,820 | 社区"去审查"GGUF 版本，支持 llama.cpp 与 MTP 多令牌预测。下载破 133 万，"uncensored"需求真实且庞大。 |
| [HauhauCS/Qwen3.8-27B-Uncensored-HauhauCS-Aggressive-MTP-GGUF](https://huggingface.co/HauhauCS/Qwen3.8-27B-Uncensored-HauhauCS-Aggressive-MTP-GGUF) | HauhauCS | 539 | 676,697 | 激进 MTP 优化的去审查多模态版本，vision 标签保留图像能力。GGUF 格式下载量近 68 万，量化+微调+架构改造三重叠加。 |
| [huihui-ai/Huihui-Qwen3.8-27B-abliterated-GGUF](https://huggingface.co/huihui-ai/Huihui-Qwen3.8-27B-abliterated-GGUF) | huihui-ai | 304 | 943,360 | "Abliterated"安全对齐移除版本，GGUF 格式下载近 94 万。社区量化模型中下载增速最快，命名形成品牌辨识度。 |
| [0bserverx/Qwen3.8-27B-Heretic-Abliterated-Uncensored-GGUF](https://huggingface.co/0bserverx/Qwen3.8-27B-Heretic-Abliterated-Uncensored-GGUF) | 0bserverx | 245 | 579,334 | "Heretic"异端命名策略吸睛，三重去标签叠加。下载量 58 万验证极端定位仍有市场，社区文化现象值得记录。 |
| [orcarouter/Qwen3.8-27B-Uncensored-FP8](https://huggingface.co/orcarouter/Qwen3.8-27B-Uncensored-FP8) | orcarouter | 1,051 | 190,062 | 罕见 FP8 精度去审查版本，保留官方多模态能力。点赞数在量化模型中居首，精度-效率-自由度三角平衡尝试。 |
| [orcarouter/Qwen3.8-27B-Uncensored-MLX](https://huggingface.co/orcarouter/Qwen3.8-27B-Uncensored-MLX) | orcarouter | 964 | 47,098 | Apple Silicon 专用 MLX 框架适配，去审查+本地苹果生态。点赞数高但下载量偏低，反映 Mac 开发者热情与硬件门槛。 |
| [orcarouter/Qwen3.8-27B-Uncensored-GGUF](https://huggingface.co/orcarouter/Qwen3.8-27B-Uncensored-GGUF) | orcarouter | 382 | 108,666 | orcarouter 系列 GGUF 基础版，形成 MLX/FP8/GGUF 三形态矩阵。作者持续输出构建社区影响力。 |
| [OBLITERATUS/Qwen3.8-27B-OBLITERATED](https://huggingface.co/OBLITERATUS/Qwen3.8-27B-OBLITERATED) | OBLITERATUS | 645 | 244,834 | 名称极端化策略，同时提供 MLX/GGUF/safetensors 多格式。下载 24 万，"OBLITERATED"成为去审查子文化符号。 |
| [DavidAU/Qwen3.8-27B-Cold-Fusion-GAIN-V1.1-NM-DAU-NEO-MAX-MTP-GGUF](https://huggingface.co/DavidAU/Qwen3.8-27B-Cold-Fusion-GAIN-V1.1-NM-DAU-NEO-MAX-MTP-GGUF) | DavidAU | 211 | 193,794 | 命名最长的技术堆叠模型，融合 GAIN Training 与 COLD-FUSION 等方法。下载近 20 万，实验性方法论展示窗口。 |
| [empero-ai/Qwen3.8-27B-Ridge-GGUF](https://huggingface.co/empero-ai/Qwen3.8-27B-Ridge-GGUF) | empero-ai | 249 | 131,435 | 常规量化定位，"Ridge"品牌区分度有限。下载 13 万，量化赛道竞争激烈下的中等表现。 |
| [huihui-ai/Huihui-Qwen3.8-27B-abliterated](https://huggingface.co/huihui-ai/Huihui-Qwen3.8-27B-abliterated) | huihui-ai | 259 | 24,844 | 同系列全精度版本，下载量远低于 GGUF 版（24,844 vs 943,360）。量化压倒性优势再次验证，全精度开源更多象征意义。 |
| [ornith-ai/Ornith-1.5-35B-A3B-GGUF](https://huggingface.co/ornith-ai/Ornith-1.5-35B-A3B-GGUF) | ornith-ai | 254 | 369,478 | Ornith MoE 模型 GGUF 版，下载量反超全精度版 15 倍。MIT 许可证+端点兼容标签，企业友好型量化代表。 |
| [ornith-ai/Ornith-1.5-9B-GGUF](https://huggingface.co/ornith-ai/Ornith-1.5-9B-GGUF) | ornith-ai | 177 | 359,078 | 9B 轻量 GGUF，下载与 35B 版持平。小模型量化性价比凸显，边缘部署场景匹配度高。 |
| [froggeric/Qwen-Fixed-Chat-Templates](https://huggingface.co/froggeric/Qwen-Fixed-Chat-Templates) | froggeric | 1,422 | 0 | 零下载但点赞破 1400，纯聊天模板修复工具。MLX+Jinja 技术栈，解决 Qwen 3.5 对话格式痛点，工具型模型新形态。 |
| [peculiar-ragdoll/Qwen-Sharp-Chat-Templates](https://huggingface.co/peculiar-ragdoll/Qwen-Sharp-Chat-Templates) | peculiar-ragdoll | 200 | 0 | 另一聊天模板修复方案，"Sharp"对标 "Fixed"。同样零下载高点赞，模板工程成为独立贡献赛道。 |

---

## 生态信号

**Qwen 帝国与反制叙事**：Qwen 3.8-27B 形成绝对中心引力，官方-社区-量化三层生态完整。但"abliterated/uncensored/heretic"等标签的爆发式涌现（至少 10 个模型），揭示开源社区对厂商安全对齐的系统性反弹，这已超越技术范畴成为文化运动。DeepSeek、Kimi、MiniMax 等非阿里系模型维持存在感，证明头部基座市场未完全垄断。量化格式上 GGUF 统治力无可争议（Unsloth 单版本 667 万下载），FP8/MLX 仍属小众。值得关注的是"零下载高点赞"的聊天模板模型，暗示 Hugging Face 正从模型托管向**开发生态基础设施**演进——工具、模板、工作流的价值被重新定价。

---

## 值得探索

1. **[MiniMax-H3](https://huggingface.co/MiniMaxAI/MiniMax-H3)** — 视频生成赛道下载量冠军（403 万），且支持图像/文本/视频三模态输入。若需研究国产视频模型技术路线或探索商业级视频生成落地，此为当前开源最优入口。

2. **[ornith-ai/Ornith-1.5-35B-A3B](https://huggingface.co/ornith-ai/Ornith-1.5-35B-A3B)** — 罕见的 MIT 许可证 + MoE 架构 + 多模态组合，GGUF 版本端点兼容。对于寻求可商用基座、且希望以较小激活参数（3B）运行大模型（35B）的企业开发者，性价比与合规性兼具。

3. **[froggeric/Qwen-Fixed-Chat-Templates](https://huggingface.co/froggeric/Qwen-Fixed-Chat-Templates)** — 零下载却获 1422 点赞的"异类"，代表模型生态的"隐形基础设施"层。研究其 Jinja 模板修复逻辑，可深入理解 Qwen 3.5 对话格式的实际痛点，对构建生产级应用有直接参考价值。

---

---
*本日报由 [agents-radar](https://github.com/brysoh435-dev/agents-radar) 自动生成。*