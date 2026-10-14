# AI Agent 工程

> **一条主线：给「AI 写出来的东西」加外部约束与反馈。**
>
> 代码由 AI 协同产出。所以问题**不在"模型更强"，在"约束更硬"** ——
> 把「**靠人记得住**」变成「**机器拦得住**」。
>
> *External constraints and feedback for AI-generated work — turning "hoping people remember" into "machines that block".*

**下面这些仓库** · 公开仓全部可点开 · **🔻 标出边界与未验证项**（不假装全绿）

---

## 从哪儿开始

> 如果只想看三个：**[agent-eval-gate](https://github.com/heweidong-ecco/agent-eval-gate)**（一道上线评测门，抓到过只有外部门能发现的缺陷）·
> **[agent-pitfalls-kb](https://github.com/heweidong-ecco/agent-pitfalls-kb)**（唯一读者是 Agent 的规则库）·
> **[memory-system](https://github.com/heweidong-ecco/memory-system)**（分层记忆系统 · Append-Only 由数据库触发器强制）。
> 详情都在下面。

---

## 约束与治理 · Constraint & Governance

> 本机 `agent-harness-engineering/`

| 仓库 | 一句话 |
| :-- | :-- |
| [**ai-delivery-harness-engineering**](https://github.com/heweidong-ecco/ai-delivery-harness-engineering) | AI 代码交付的**治理层**：规则 ＋ 十阶段流水线 ＋ 可程序化质量门禁 ＋ **三道防编造闸**。`make gate` 一次 15 步 · 137 测试 · 84% 覆盖 · **17 个可独立复用的门禁脚本**。<br>🔻 **已封存**：它是**容器不是内容** —— 缺可填充的真实来源，**十阶段从未被真实需求走过**（填充 0%）。 |

## 可运行的项目 · Runnable Projects

> 本机 `agent-projects/projects/`

| 仓库 | 一句话 |
| :-- | :-- |
| [**ess-ai-sales-system**](https://github.com/heweidong-ecco/ess-ai-sales-system) | 从**一句业务描述**拆出可执行的系统设计（需求基线 → 系统总纲 → 5 份决策记录）。<br>每条决策都是「**选 A 不选 B**」且**可追溯** —— 外部调研 21 来源 / 40 claim，**对抗核验后剔除了 4 条**。<br>🔻 **全部是 DEMO · 虚构厂商。** |
| [**memory-system**](https://github.com/heweidong-ecco/memory-system) | 分层记忆系统的**架构**：多租户 · 6 类存储 · 5 层检索 · 程序性记忆自进化。<br>**Append-Only 用数据库触发器强制** —— 有测试证明 `UPDATE` / `DELETE` 真被拒绝（26 个用例）。 |
| 🔒 *生产级 RAG ＋ Agent API 服务* | **单 Agent ＋ LangGraph**（不上多 Agent）；**Agent loop 不参与业务循环**（理由：系统整体稳定性）。<br>混合检索 **BM25 ＋ 向量 ＋ RRF 融合**（只看排名、不吃分数 —— 两路量纲不同）· NLP 数据对齐（中英 / 全角半角）· 查询改写**两套方案比过**（本地 LoRA vs 云端 API）。<br>**10,019 行** · 全面复审修掉 24 个问题（最危险的一类是**静默**）。<br>🔻 **边界三条**：是三版删减后的版本 · 重排序模块未并入本仓全量链路 · README 里的 P99<800ms 是**目标不是实测**。（**private**） |

## 评测与数据 · Evaluation & Data

> 本机 `agent-projects/eval-gate/` · `agent-projects/lora-data/`

| 仓库 | 一句话 |
| :-- | :-- |
| [**agent-eval-gate**](https://github.com/heweidong-ecco/agent-eval-gate) | **判分链路五段**：schema 校验 → 调被测 → 确定性规则 → 大模型判分 → 阈值 → **输出退出码**。<br>对外接口**用退出码不用「提示」**：`0 放行 / 1 阻断 / 2 人工复核 / 3 运行错误` —— **「跑不出来」和「跑出来合格」是两件事**。<br>门自己**必须可复现地被验证**：喂劣化输入 → **必须 `exit 1`**。<br>基线四轮 **0.75 → 0.975** · **12,305 行** · 覆盖率 **94.57%**。<br>🔻 **边界**：判别力**仍标定不出**（16 轮 A/B 两臂全 1.0）；有一道门标成「硬」后**主动降级为「半硬」** —— **诚实标强度优先于把门做硬**。 |
| [**lora-finetune**](https://github.com/heweidong-ecco/lora-finetune) | LoRA 微调的**数据与实验库** —— 数据集 · 训练配置 · 实验记录 · 评测结果集中存放。<br>许可：代码 `MIT` ／ 数据 `CC BY-NC 4.0`（⛔ 非商用）。 |

## 知识库 · Knowledge Base

> 本机 `agent-facing/`（直接给 Agent 读）

| 仓库 | 一句话 |
| :-- | :-- |
| [**agent-pitfalls-kb**](https://github.com/heweidong-ecco/agent-pitfalls-kb) | **给 Agent 读的可执行规则库**。只收**严重**问题：*不报错、不变红，却能让错的结论被当成正确结论交付*。<br>**每篇必备五节，第 5 节是「怎么验证它被落地」** —— 判据：「**如果这条被违反了，有没有任何东西会变红或出声？**」答「没有」⇒ **它还没落地**。<br>⭐ **库自己被实测打过两次脸，两次都写进了库**（一篇里七条规则只有 1 条是真门；一道门标着「已装」却挡不住）。<br>**4,257 行** · 结构 = 四道门 ＋ **门自己有防腐**。 |

## 工具与 Kit · Toolkits

> 本机 `agent-projects/kit/` · `agent-projects/fde/`

| 仓库 | 一句话 |
| :-- | :-- |
| [**ai-field-delivery-toolkit**](https://github.com/heweidong-ecco/ai-field-delivery-toolkit) | 模块化、**离线优先**的 AI 现场交付工具包（FDE）：需求诊断 → 数据准备 → 原型组装 → 交付加固 → 监控 → 数据飞轮。<br>🔻 **半成品 · 已停更。** |
| 🔒 *Agent 开发 Kit* | 一个用来自动开新 Agent 项目的脚手架（**private**）。 |

## 其他 · Others

| 仓库 | 一句话 |
| :-- | :-- |
| 🔒 *项目级 Agent 约束系统* | 约束系统设计（**private** · 设计阶段）。 |
| 🔒 *设置与方案* | Agent 的配置与纪律方案集（**private**）。 |

> 带 🔒 的条目 = **private 仓：只给描述、不给仓名**。

---

## 关于这份清单

- **分类对齐本机目录** —— 每个分类下都标了 `本机 …/` 路径；也可点仓上的 topic（`agent-harness-engineering` / `agent-projects` / `agent-facing`）筛同类
- **🔻 表示边界** —— 标出**未验证 / 已封存 / 未并入**的部分。**不假装全绿**是这套工程的默认纪律。
- ⚠️ 有一道门标着「硬」，实测后发现它其实只是「半硬」，**已降级重标**。
