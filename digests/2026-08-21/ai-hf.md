# Hugging Face 热门模型日报 2026-08-21

> 数据来源: [Hugging Face Hub](https://huggingface.co/) | 共 30 个模型 | 生成时间: 2026-08-21 03:30 UTC

---

# Hugging Face 热门模型日报 · 2026-08-21

---

## 今日速览

Qwen 3.8 系列以绝对统治力霸榜，27B 参数规格衍生出数十个社区量化与去审查版本，形成现象级生态。Kimi K3 以 10,887 点赞强势跻身头部，成为国产多模态模型新标杆。视频生成领域 MiniMax-H3 及其衍生 Turbo 版本下载量突破 370 万，显示推理加速需求旺盛。值得注意的是，"abliterated/uncensored" 标签在榜单中高频出现，反映社区对模型安全对齐的逆向工程已形成规模化产业。DeepSeek V4 系列保持稳健输出，Pro 与 Flash 双版本覆盖不同性能层级。

---

## 热门模型

### 🧠 语言模型（LLM、对话模型、指令微调）

| 模型 | 作者 | 点赞 | 下载 | 简要说明 |
|:---|:---|---:|---:|:---|
| [Qwen/Qwen3.8-27B](https://huggingface.co/Qwen/Qwen3.8-27B) | Qwen | 11,769 | 1,373,584 | Qwen 3.8 系列旗舰稠密模型，支持 image-text-to-text 多模态对话，单周点赞破万展现极强社区热度。 |
| [moonshotai/Kimi-K3](https://huggingface.co/moonshotai/Kimi-K3) | moonshotai | 10,887 | 2,349,853 | 月之暗面新一代多模态大模型，下载量超 234 万，feature-extraction 与压缩张量技术为其核心卖点。 |
| [deepseek-ai/DeepSeek-V4-Flash-0731](https://huggingface.co/deepseek-ai/DeepSeek-V4-Flash-0731) | deepseek-ai | 3,578 | 2,547,549 | DeepSeek V4 轻量版本，兼顾性能与推理效率，250 万+下载验证其工程化落地能力。 |
| [deepseek-ai/DeepSeek-V4-Pro-0813](https://huggingface.co/deepseek-ai/DeepSeek-V4-Pro-0813) | deepseek-ai | 683 | 43,287 | V4 系列专业版，定位高端推理场景，发布时间较新但增长潜力明确。 |
| [Qwen/Qwen3.8-2.4T-A95B](https://huggingface.co/Qwen/Qwen3.8-2.4T-A95B) | Qwen | 1,122 | 14,592 | 2.4T 总参数 MoE 架构，仅 95B 激活参数，展示 Qwen 在超大规模稀疏模型上的技术纵深。 |
| [meta-models/Muse-Glimmer-30B](https://huggingface.co/meta-models/Muse-Glimmer-30B) | meta-models | 1,719 | 478,622 | 30B 参数多模态对话模型，命名与架构暗示 Meta 系技术脉络，视觉理解能力突出。 |
| [ornith-ai/Ornith-1.5-35B-A3B](https://huggingface.co/ornith-ai/Ornith-1.5-35B-A3B) | ornith-ai | 227 | 1,713 | 基于 Qwen3.5 MoE 的 35B 模型，仅 3B 激活参数，探索极致稀疏化效率边界。 |
| [superwhisper/s1-mini](https://huggingface.co/superwhisper/s1-mini) | superwhisper | 159 | 348 | 轻量级语音文本模型，ASR 与文本生成融合，定位端侧实时语音交互场景。 |
| [dots-studio/dots3-note-prev](https://huggingface.co/dots-studio/dots3-note-prev) | dots-studio | 242 | 1,373 | 新兴工作室预览模型，dots3 系列早期版本，多模态文本生成能力待观察。 |

### 🎨 多模态与生成（图像、视频、音频、文本到X）

| 模型 | 作者 | 点赞 | 下载 | 简要说明 |
|:---|:---|---:|---:|:---|
| [MiniMaxAI/MiniMax-H3](https://huggingface.co/MiniMaxAI/MiniMax-H3) | MiniMaxAI | 4,245 | 3,308,673 | MiniMax 第三代视频生成基座模型，text-to-video 与 image-to-video 双模态，330 万下载居全榜第二。 |
| [Lightricks/LTX-2.5](https://huggingface.co/Lightricks/LTX-2.5) | Lightricks | 1,421 | 611,825 | 专业视频工具厂商迭代版本，覆盖 image-to-video / text-to-video / video-to-video 全链路，61 万下载验证商业化潜力。 |
| [MiniMaxAI/MiniMax-Music3](https://huggingface.co/MiniMaxAI/MiniMax-Music3) | MiniMaxAI | 1,116 | 14,471 | MiniMax 音乐生成第三代，text-to-music 与 text-to-audio 能力，填补榜单音频生成空白。 |
| [lightx2v/Minimax-h3-Turbo](https://huggingface.co/lightx2v/Minimax-h3-Turbo) | lightx2v | 659 | 380,072 | 社区加速版 MiniMax-H3，diffusers 框架优化，38 万下载显示推理效率优化需求迫切。 |
| [TenStrip/10Eros-Max](https://huggingface.co/TenStrip/10Eros-Max) | TenStrip | 300 | 0 | MiniMax-H3 衍生微调版本，命名暗示特定内容方向，零下载但上榜反映社区创作多样性。 |

### 🔧 专用模型（代码、数学、医疗、嵌入）

*本分类下无符合模型，表格省略。*

### 📦 微调与量化（社区微调、GGUF、AWQ）

| 模型 | 作者 | 点赞 | 下载 | 简要说明 |
|:---|:---|---:|---:|:---|
| [unsloth/Qwen3.8-27B-GGUF](https://huggingface.co/unsloth/Qwen3.8-27B-GGUF) | unsloth | 2,372 | 5,126,652 | Unsloth 官方 GGUF 量化，512 万下载全榜最高，llama.cpp 生态事实标准。 |
| [unsloth/Qwen3.8-27B-NVFP4](https://huggingface.co/unsloth/Qwen3.8-27B-NVFP4) | unsloth | 308 | 831,483 | NVIDIA FP4 格式量化，83 万下载显示新硬件格式采纳加速。 |
| [Qwen/Qwen3.8-27B-FP8](https://huggingface.co/Qwen/Qwen3.8-27B-FP8) | Qwen | 634 | 1,517,643 | 官方 FP8 量化版本，152 万下载，精度与效率的官方平衡方案。 |
| [JonathanColetti/Qwen3.8-27B-Uncensored-GGUF](https://huggingface.co/JonathanColetti/Qwen3.8-27B-Uncensored-GGUF) | JonathanColetti | 520 | 979,768 | 社区去审查 GGUF，98 万下载逼近百万，"uncensored" 标签吸引力显著。 |
| [orcarouter/Qwen3.8-27B-Uncensored-FP8](https://huggingface.co/orcarouter/Qwen3.8-27B-Uncensored-FP8) | orcarouter | 684 | 76,109 | 去审查 + FP8 双属性，点赞反超官方 FP8，安全对齐逆向工程需求真实存在。 |
| [orcarouter/Qwen3.8-27B-Uncensored-MLX](https://huggingface.co/orcarouter/Qwen3.8-27B-Uncensored-MLX) | orcarouter | 719 | 2,628 | Apple Silicon MLX 框架专用，去审查版本，点赞数在同类中居首。 |
| [orcarouter/Qwen3.8-27B-Uncensored-GGUF](https://huggingface.co/orcarouter/Qwen3.8-27B-Uncensored-GGUF) | orcarouter | 243 | 52,382 | 同作者 GGUF 版本，MLX 与 GGUF 双轨覆盖不同硬件生态。 |
| [HauhauCS/Qwen3.8-27B-Uncensored-HauhauCS-Aggressive-MTP-GGUF](https://huggingface.co/HauhauCS/Qwen3.8-27B-Uncensored-HauhauCS-Aggressive-MTP-GGUF) | HauhauCS | 371 | 268,258 | "Aggressive MTP" 多 token 预测激进优化，26 万下载验证推理加速创新。 |
| [huihui-ai/Huihui-Qwen3.8-27B-abliterated-GGUF](https://huggingface.co/huihui-ai/Huihui-Qwen3.8-27B-abliterated-GGUF) | huihui-ai | 201 | 187,008 | 社区 abliterated 品牌 GGUF，18 万下载，"huihui" 系列形成稳定输出。 |
| [huihui-ai/Huihui-Qwen3.8-27B-abliterated](https://huggingface.co/huihui-ai/Huihui-Qwen3.8-27B-abliterated) | huihui-ai | 202 | 10,540 | 同系列原始精度版本，GGUF 版本下载量为其 18 倍，量化偏好显著。 |
| [0bserverx/Qwen3.8-27B-Heretic-Abliterated-Uncensored-GGUF](https://huggingface.co/0bserverx/Qwen3.8-27B-Heretic-Abliterated-Uncensored-GGUF) | 0bserverx | 191 | 326,638 | "Heretic" 异端命名 + 三重去标签，32 万下载，极端定位策略有效。 |
| [Blackfrost-AI/Qwen3.8-27B-ABLITERATED-GGUF](https://huggingface.co/Blackfrost-AI/Qwen3.8-27B-ABLITERATED-GGUF) | Blackfrost-AI | 184 | 186,470 | 全大写 ABLITERATED 品牌标识，18 万下载，视觉差异化竞争。 |
| [empero-ai/Qwen3.8-27B-Ridge-GGUF](https://huggingface.co/empero-ai/Qwen3.8-27B-Ridge-GGUF) | empero-ai | 225 | 55,074 | "Ridge" 系列量化，定位相对中性，5 万级下载稳健表现。 |
| [ornith-ai/Ornith-1.5-35B-A3B-GGUF](https://huggingface.co/ornith-ai/Ornith-1.5-35B-A3B-GGUF) | ornith-ai | 171 | 53,691 | MoE 架构 GGUF 量化，MIT 许可证 + 端点兼容，开源友好度高。 |
| [OBLITERATUS/Qwen3.8-27B-OBLITERATED](https://huggingface.co/OBLITERATUS/Qwen3.8-27B-OBLITERATED) | OBLITERATUS | 287 | 4,415 | MLX/GGUF/Safetensors 三格式全栈，"OBLITERATED" 极致命名，下载量偏低但格式覆盖最全。 |
| [froggeric/Qwen-Fixed-Chat-Templates](https://huggingface.co/froggeric/Qwen-Fixed-Chat-Templates) | froggeric | 1,342 | 0 | MLX 聊天模板修复，零下载但 1342 点赞异常突出，开发者工具属性强，解决 Qwen 生态痛点。 |

---

## 生态信号

**Qwen 3.8 已形成开源领域最具统治力的模型家族**，27B 规格衍生出 15+ 上榜变体，覆盖官方、量化、去审查、多框架适配全链条。这种"一个基座、百种分身"的生态模式，标志着开源模型竞争已从单点性能转向工程化分发能力。**去审查/abliterated 微调呈现产业化特征**：orcarouter、huihui-ai、0bserverx 等账号持续输出品牌化名，累计下载超 150 万，反映安全对齐与开放需求之间的张力正在催生固定细分市场。**量化格式迭代加速**：GGUF 仍为主流，但 NVFP4、FP8、MLX 等新硬件原生格式快速崛起，Unsloth 作为量化基础设施地位巩固。视频生成领域 MiniMax-H3 与 LTX-2.5 双雄并立，但社区 Turbo 加速版 38 万下载提示**推理效率已成为生成模型竞争第二战场**。

---

## 值得探索

| 模型 | 推荐理由 |
|:---|:---|
| **[moonshotai/Kimi-K3](https://huggingface.co/moonshotai/Kimi-K3)** | 10,887 点赞、234 万下载的双高数据，feature-extraction 与 compressed-tensors 技术标签独特，可能是 MoE 架构与动态压缩结合的新范式，值得研究其效率-性能平衡策略。 |
| **[froggeric/Qwen-Fixed-Chat-Templates](https://huggingface.co/froggeric/Qwen-Fixed-Chat-Templates)** | 零下载却获 1342 点赞的极端反差，揭示开发者工具类模型的价值评估盲区——它解决的是 Qwen 生态中 MLX 部署的模板兼容性痛点，小体量高影响力的典型。 |
| **[MiniMaxAI/MiniMax-H3](https://huggingface.co/MiniMaxAI/MiniMax-H3)** | 330 万下载仅次于量化版 Qwen，原生支持 image-to-video / text-to-video 双模态，且社区已衍生 Turbo 加速与 10Eros 内容微调版本，视频生成基座模型的生态扩张速度值得持续跟踪。 |

---
*本日报由 [agents-radar](https://github.com/brysoh435-dev/agents-radar) 自动生成。*