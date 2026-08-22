# ArXiv AI 研究日报 2026-08-22

> 数据来源: [ArXiv](https://arxiv.org/) (cs.AI, cs.CL, cs.LG) | 共 50 篇论文 | 生成时间: 2026-08-22 03:08 UTC

---

# ArXiv AI 研究日报 | 2026-08-22

---

## 今日速览

今日50篇论文中，**智能体能力进化**成为最突出的主线：从工具使用的中期训练优化（MidTool）、跨任务技能迁移（Break It Down, Pass It On）到自适应推理计算分配（Learning When to Think），研究者正系统性地提升LLM Agent的自主性与效率。与此同时，**模型自我改进的真实性验证**引发关注——AI4AI-Bench与Phantom Gains分别从基准构建和审计方法论角度，揭示了递归自我改进评估中的关键陷阱。医疗、法律等垂直领域的专用基准（G-CARL、InsufficiencyBench、ContractScrub）也显著增多，反映出AI应用向高风险场景渗透的加速趋势。

---

## 重点论文

### 🧠 大语言模型（架构、训练、对齐、评估）

| 论文 | 作者 | 简要说明 |
| :--- | :--- | :--- |
| [ConceptGuard: Benchmarking Context-Sensitive Unlearning in Large Language Models](http://arxiv.org/abs/2608.20338v1) | Kale et al. | 首次系统评估LLM"上下文敏感遗忘"能力，指出现有基准因使用独立事实集而低估了真实场景中的遗忘难度。对AI安全合规具有直接指导意义。 |
| [Inject, Align, Recover: Staged Post-Training for Retrieval-Free Document Knowledge Internalization](http://arxiv.org/abs/2608.20281v1) | Kou et al. | 提出三阶段后训练方法，将固定文档集转化为参数化知识，实现无需检索的精准问答。为RAG替代方案提供了新思路。 |
| [Phantom Gains: Auditing Self-Improvement Against a Measured Null](http://arxiv.org/abs/2608.20290v1) | Xu et al. | 揭示LoRA自迭代中的"虚假增益"现象：问题级别的准确率波动可能源于测量噪声而非真实改进。建立了基于零模型的严格审计框架。 |
| [Daedalus-150M: A Convolution-Attention Hybrid Designed for CPU Inference](http://arxiv.org/abs/2608.20210v1) | Koutsiaris | 反向设计的小型语言模型：先固定CPU单用户4-bit推理约束，再选择架构。18层中仅6层保留完整注意力，其余用卷积替代。 |
| [Which Eviction Policy Should an LLM Cache Use? A Systematic Study Across Workloads, Capacities, and Encoders](http://arxiv.org/abs/2608.20280v1) | Kulkarni et al. | 首次在统一协议下系统比较7种语义缓存淘汰策略（FIFO/LRU/ARC/GDSF等），发现语义冗余策略在多数场景下最优。 |

---

### 🤖 智能体与推理（规划、工具使用、多智能体、思维链）

| 论文 | 作者 | 简要说明 |
| :--- | :--- | :--- |
| [MidTool: Mid-training Data Synthesis for Agentic Tool Use](http://arxiv.org/abs/2608.20314v1) | Jiang et al. | 针对"中期训练"阶段合成工具使用数据，证明该阶段对塑造智能体能力至关重要。在SWE-bench和OSWorld上取得显著提升。 |
| [Break It Down, Pass It On: Cross-Task Skill Transfer in LLM Agents](http://arxiv.org/abs/2608.20274v1) | Feng et al. | 系统研究智能体诱导技能的跨任务迁移：发现技能在相似任务间可靠转移，但不当迁移会损害性能。提出迁移安全性判定准则。 |
| [Learning When to Think: Adaptive Reasoning for Test-Time Compute Allocation](http://arxiv.org/abs/2608.20256v1) | Kassenaar et al. | 让模型自主决定推理时长：通过强化学习实现测试时计算的自适应分配，避免简单问题过度计算、难题计算不足。 |
| [Inducing Task Models from Computer-Use Traces](http://arxiv.org/abs/2608.20319v1) | Jiang et al. | 从被动记录的屏幕截图和键鼠操作中提取可审计的符号化任务模型，为计算机使用智能体提供可复用的工作流知识。 |
| [AI4AI-Bench: Benchmarking LLM Agents in Algorithmic Design for Recursive Self-Improvement](http://arxiv.org/abs/2608.20318v1) | Chi et al. | 首个评估LLM设计训练算法以实现递归自我改进的基准。测试智能体能否改进目标函数、更新规则等核心训练组件。 |

---

### 🔧 方法与框架（新技术、基准测试、效率优化）

| 论文 | 作者 | 简要说明 |
| :--- | :--- | :--- |
| [Pandora's AI Model Routing Box: Efficient Allocation with Costly Value Estimation](http://arxiv.org/abs/2608.20316v1) | Fisch et al. | 模型路由中的价值估计成本问题：提出在估计成本不可忽略时的最优分配策略，平衡探索-利用与估计开销。 |
| [Task-CoEvolve: Efficient Harness Optimization via Adaptive Validation Task Selection](http://arxiv.org/abs/2608.20169v1) | Miyai et al. | 智能体Harness优化的新范式：自适应选择验证任务子集，将迭代重写效率提升数倍，无需更新模型权重。 |
| [DICS: Data-Informed Centroid Splitting for Decision Tree Classifiers](http://arxiv.org/abs/2608.20258v1) | Mazumder et al. | 用数据驱动的质心分裂替代决策树的穷举搜索，将训练复杂度从O(dn log n)降至近线性，保持可解释性。 |
| [Ask Self, Ask Others: Relation Is All You Need](http://arxiv.org/abs/2608.20172v1) | Ge et al. | 替代注意力的新token混合原语"Relation"：显式分离自关系与交换关系后再推导信息流，为Transformer架构变体提供新方向。 |

---

### 📊 应用（垂直领域、多模态、代码生成）

| 论文 | 作者 | 简要说明 |
| :--- | :--- | :--- |
| [G-CARL: Grounded Checklist-Aligned Reward Learning for Patient-Oriented Medical Report Interpretation](http://arxiv.org/abs/2608.20331v1) | Xie et al. | 面向患者的医学报告个性化解读：结合证据锚定的医学事实性与上下文依赖的患者沟通，填补现有医疗VLM任务空白。 |
| [InsufficiencyBench: Evaluating LLM legal advice on underspecified user queries](http://arxiv.org/abs/2608.20220v1) | Vincent et al. | 首个针对"查询端信息不足"的法律AI基准：测试LLM能否识别用户遗漏的关键事实，而非直接给出错误建议。 |
| [ContractScrub: A benchmark for final review of legal contracts](http://arxiv.org/abs/2608.20204v1) | Bang et al. | 聚焦合同"scrubbing"（最终审查）的专用基准：检测交易协议中的错误与不一致，高度适合LLM自动化。 |
| [Rule-Compliant Visual Spatial Planning for Multimodal Large Language Models](http://arxiv.org/abs/2608.20237v1) | Chen et al. | 测试MLLM在显式或未见规则约束下的视觉空间规划能力，要求联合理解空间布局、解释规则并生成合规计划。 |
| [OenoBench: A Wine-Domain Benchmark for Knowledge-Grounded Evaluation of Large Language Models](http://arxiv.org/abs/2608.20106v1) | Khudov | 葡萄酒领域知识基准：3,266道多选题覆盖六大支柱，全部基于38,104条原子化、可溯源的事实构建。 |

---

## 研究趋势信号

**"智能体能力工程化"正从单点优化走向全生命周期管理**：今日多篇论文覆盖智能体的技能获取（MidTool）、迁移（Break It Down）、验证（Task-CoEvolve）与计算效率（Learning When to Think），形成完整技术栈。与此同时，**"自我改进"从口号变为可审计对象**：AI4AI-Bench和Phantom Gains分别从正向构建和反向审计切入，标志着该领域进入方法论成熟阶段。值得注意的是，**垂直领域基准的"法律化"趋势**——InsufficiencyBench、ContractScrub均强调识别信息缺口而非直接作答，反映高风险场景对AI"知其所不知"的刚性需求。

---

## 值得精读

| 论文 | 精读理由 |
|:---|:---|
| **[Phantom Gains: Auditing Self-Improvement Against a Measured Null](http://arxiv.org/abs/2608.20290v1)** | 递归自我改进是AGI讨论的核心议题，但本文以扎实的实证揭示：三轮LoRA迭代中大量"增益"可能是统计假象。其提出的"测量零模型"方法论可直接应用于任何自改进系统的评估，具有范式级影响。 |
| **[Break It Down, Pass It On: Cross-Task Skill Transfer in LLM Agents](http://arxiv.org/abs/2608.20274v1)** | 首次系统量化智能体技能的跨任务迁移边界，回答"何时该用记忆、何时该重新学习"这一工程关键问题。对构建可累积经验的Agent系统至关重要。 |
| **[Learning When to Think: Adaptive Reasoning for Test-Time Compute Allocation](http://arxiv.org/abs/2608.20256v1)** | 测试时计算扩展（Test-Time Scaling）是当前热点，但多数工作假设固定预算。本文让模型自主学会"何时停止思考"，是实现推理效率数量级提升的关键一步，与o1类模型的工程实践高度相关。 |

---

---
*本日报由 [agents-radar](https://github.com/brysoh435-dev/agents-radar) 自动生成。*