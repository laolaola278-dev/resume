# 个人简历

## 基本信息

| 项目 | 内容 |
|------|------|
| **姓名** | 谢键禧 |
| **求职岗位** | AI 应用开发 / 数据分析（大模型方向） |
| **期望城市** | 广州、深圳 |
| **期望薪资** | 面议 |
| **出生年月** | 2005.06 |
| **性别** | 男 |
| **民族** | 汉族 |
| **婚姻状况** | 未婚 |
| **现住城市** | 广东佛山高明 |
| **电话** | 138****0485 |
| **邮箱** | 138****0485@163.com |
| **GitHub** | [github.com/laolaola278-dev](https://github.com/laolaola278-dev) |

---

## 教育经历

**广东职业技术学院** | 大专 · 大数据技术 | 2024.09 - 2027.09

**主修课程：** Java编程、Python编程、MySQL数据库管理、Linux操作系统、Hadoop大数据处理、Office办公软件

**主要掌握：**
- 熟悉Java和Python编程语言
- 会使用MySQL进行数据库管理和操作
- 了解Linux操作系统的基本命令与配置
- 会运用Hadoop框架处理大规模数据集
- 熟悉Office系列软件以提高工作效率

**学习情况：** 专业课程分数位列专业前茅；学期综合分数位居班级前列。

---

## 专业技能

**大模型 / Agent 工程**（主方向）
- 熟悉 OpenAI-compatible 接口协议与 tool-calling 循环，可对接 OpenAI / DeepSeek / GLM / Ollama / vLLM 等多家模型
- 具备 Agent 运行时开发经验：任务规划、工具注册、权限确认、检查点恢复、验证闭环
- 熟悉 MCP（Model Context Protocol）Server 开发与接入（Claude Code / Cursor / Codex）
- 了解 Skill 工程：一次编写、编译分发到多 Agent 底盘；以及从团队 git 历史/代码评审中蒸馏可复用技能
- 了解多 Agent 协作编排：共享上下文、消息投递追踪、人工审核闸门

**数据 / 机器学习**
- 熟悉特征工程，能够识别并修复**数据泄漏**等建模陷阱（详见 RailMind 项目）
- 熟悉 scikit-learn / XGBoost / LightGBM / PyTorch，具备模型训练、评估与对比经验
- 掌握分类与回归任务的评估方法（Accuracy / F1 / ROC-AUC）与实验报告撰写
- 了解 Kafka + Flink 实时流处理链路

**工程基础**
- Python（FastAPI / SQLAlchemy / Pydantic）、TypeScript / Node.js
- PostgreSQL、Redis、SQLite；Docker Compose 多服务编排
- Vue / React / Tailwind 前端基础；Git 与 GitHub Actions CI

---

## 项目经历

> 完整技术细节、运行截图与使用教程见本站「作品集」页面。
> 所有项目均标注了验证状态，可复现性以标注为准。

### AgentOS — 本地自主 Agent 运行时

`TypeScript` `Node.js` `Next.js` `PostgreSQL` | 独立开发 | [GitHub](https://github.com/laolaola278-dev/agentos)

- **项目概述：** 一个可自主规划、执行、验证、评审并自我纠错的本地 Agent 运行时，用于自动完成软件工程任务。
- **核心工作：**
  - 实现 **tool-calling 主循环**（对齐 Claude Code / Codex 核心范式）：模型每轮提出工具调用、观察结构化结果、自主决定下一步，直到达成目标
  - 设计**证据化完成机制**：任务只有在「验证命令通过 + 独立评审 PASS」后才允许完成，杜绝模型自证
  - 实现分级**沙箱**（Docker 容器 / POSIX ulimit / 策略白名单）与**权限确认模式**，危险操作需人工批准
  - 实现 **AES-256-GCM 密钥保险库**，避免 API Key 明文落盘
  - 实现上下文自动压缩（LLM 摘要 + 确定性标记），保证长任务不超窗
- **工程亮点：** 无 LLM Key 时退化成确定性规划器（目标 DSL），有 Key 时切换自由规划——同一套验证与评审闸门在两种模式下都生效。

### clihive — CLI Agent 协作工作台

`Node.js` `Electron` `node-pty` `xterm.js` | 独立开发 | [GitHub](https://github.com/laolaola278-dev/clihive)

- **项目概述：** 单窗口内并行托管多个 CLI Agent（codex / claude / opencode），提供共享记录、编排器与完整投递追踪，回答"那条消息到底送达了吗"。
- **核心工作：**
  - 每个窗格是**真实 PTY**；根据命令行自动识别 CLI 类型（codex / claude / gemini / qwen / aider 等 13 种），匹配对应徽标与安全投递策略
  - 通过 CLI 的**结构化接口**（`codex exec --json`、`claude -p --output-format stream-json`）驱动托管 Agent，而非键盘模拟
  - 设计**投递追踪**：send → fanout → deliver → ack 全链路记录为 JSONL 并可视化，每条记录标注通道、目标与是否落地
  - 托管 Agent 的任务持久化、权限边界、**人工审核闸门**（"完成"仅代表待评审）
- **工程亮点：** 截图由 `scripts/screenshots.mjs` 驱动真实运行的 UI（headless Chromium + 真实 PTY）自动捕获，每步带 PASS/FAIL 断言，非人工摆拍。

### skillc — 多运行时 Skill 编译器

`TypeScript` `Node.js` | 独立开发 | [GitHub](https://github.com/laolaola278-dev/skillc)

- **项目概述：** 写一份 skill 源码，编译分发到 Claude Code / Codex / DSH / Hermes / OpenClaw 等任意 Agent 底盘。
- **核心工作：**
  - 实现 skill 源码解析器与多目标**发射器（emitters）**，一套源码产出各底盘的原生格式
  - 实现**多运行时 A/B 试用**、确定性发现机制与资源打包
  - 实现**安全同步（safe sync）**，避免覆盖用户本地改动
  - 随附 `gitguard` 提交卫生检查工具，可一键安装到所有仓库

### RailMind — 铁路票务实时数据智能平台

`Python` `Kafka` `Flink` `LightGBM` `FastAPI` `Vue` | 独立开发 | [GitHub](https://github.com/laolaola278-dev/RailMind)

- **项目概述：** 覆盖 12306 数据采集 → 流式特征计算 → 模型训练 → 在线预测 → 可视化的全链路 ML Pipeline，共 14 个 Docker 服务。
- **核心工作：**
  - 用 `curl_cffi` 做 TLS 指纹模拟采集 12306 数据，**321 万条**记录（400 条线路 / 3,795 个车次 / 10 种席别）
  - 搭建 Kafka + PyFlink 实时特征链路，落库到 PostgreSQL 实时特征表
  - 实现**模型热替换**：Serving Engine 支持运行时换模型，API 全程无感知
- **⭐ 关键技术突破 — 数据泄漏修复：**
  - 首版 Baseline 得到 **R² = 1.0**（RMSE ≈ 10⁻¹³，已达机器精度），判定为数据泄漏而非真实能力
  - 定位 3 个泄漏特征：`previous_remaining`（首条用当前余票回填）、`remaining_change`（可反推目标）、`rolling_mean`（窗口含当前值）
  - 模型实际只学到恒等式 `remaining_count ≈ previous_remaining + remaining_change`
  - 修复：目标由回归改为**三分类**（no_ticket / sold_out / available），移除泄漏特征，重建可信基准
  - 结果：虚假 R²=1.0 → 真实 ROC-AUC **0.6912**（LR 基准）→ **0.9065**（LightGBM 最优）
  - 4 模型同数据同特征同切分对比，LightGBM 训练耗时 0.50s（比 LR 快 **141×**）
- **性能指标：** API 顺序 QPS 184 / P50 4.58ms；5 分钟稳定性测试 QPS 193 / P50 4.38ms / 错误率 0.00%；单条推理 0.65ms。

### api-health-checker — LLM API 健康检测服务

`Python` `FastAPI` `SQLAlchemy` `SQLite` | 独立开发 | [GitHub](https://github.com/laolaola278-dev/api-health-checker)

- **项目概述：** 批量检测各家大模型 API Key 是否可用，并记录延迟、首 token 时间与可用模型列表。
- **核心工作：**
  - 抽象**统一 Provider 接口**，接入 OpenAI / Anthropic / Gemini / DeepSeek / Moonshot 及任意 OpenAI-compatible 端点
  - 实现**并发批量检测**（可配置并发度）、速率限制与 **SSRF 防护**（拦截内网/私网地址）
  - API Key 在存储与展示时全程**掩码**，检测历史落库可追溯
- **验证状态：** ✅ 已在真实环境启动验证——14 个 API 端点全部注册可用，健康检查、Provider 列表、历史查询均返回真实数据；非法 Provider 被正确拦截返回 `Unsupported provider`。

### 第十八届挑战杯肇庆专项赛 — 负责人

- **项目概述：** 组织并参与第十八届挑战杯肇庆专项赛，旨在通过创新项目解决实际问题，提升团队的科研与实践能力。
- **核心职责：** 作为负责人，统筹整个项目的策划与执行，包括团队组建、资料搜集、方案设计及最终的比赛准备；协调各方资源，确保项目顺利推进。

---

## 校园经历

### 校秘书部 — 小干

- **团队贡献：** 作为校秘书部的一员，积极参与部门日常运营与管理，协助组织多次重要会议及活动。通过优化文件管理和信息传达流程，显著提升了团队工作效率。
- **个人成就：** 因表现出色，在年度评估中获得"优秀学员"称号，并被授予荣誉证书，成为同期成员中的佼佼者。
- **技能提升：** 锻炼了良好的文档处理能力和沟通协调技巧，学会了如何在快节奏的工作环境中保持高效与专注。

### 院青年志愿者服务站 — 小干

- **志愿服务：** 组织并参与了超过10次的社区志愿服务活动，包括环保宣传、敬老院慰问等，累计服务时长达到50小时以上。
- **团队建设：** 协助完成了新志愿者的招募与培训工作，共吸引80余名同学加入团队，成功举办了2场内部交流分享会，增强了团队凝聚力。
- **项目管理：** 负责管理一项旨在提高校园环境意识的小型项目，从初期策划到最终实施全程跟进，引入创新性互动环节，直接参与者超过300人。

---

## 活动经历

### 首届大学生中医药知识提升活动 — 参与者

- **学习成就：** 在首届大学生中医药知识提升活动中，通过积极参与和努力学习，最终从众多参与者中脱颖而出，荣获"优秀学员"称号，并被正式授予荣誉证书。

### 首届大学生计算机办公知识竞答 — 参与者

- **竞赛参与：** 作为首届大学生计算机办公知识竞答的积极参与者，通过深入学习和准备，在众多参赛者中脱颖而出，荣获"优秀学员"称号。
- **技能提升：** 竞赛过程中，强化了Word、Excel等办公软件的操作技巧，提升了信息检索与问题解决能力。
