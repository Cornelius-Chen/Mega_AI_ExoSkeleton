# CCOS 复合架构 v1

## 0. 目标

本架构的目标不是给人类“看起来懂”，而是给 AI **稳定解析、稳定调用、稳定扩展**。

核心要求：

* 中控只做路由、仲裁、准入，不处理细节
* 主域之间严格隔离，不允许互相侵扰
* 分域与子域之间通过明确接口通信，不允许隐式越界
* 所有核心对象必须采用 AI-native 结构化记录
* 每次任务结束后，优先沉淀为可复用资产
* 允许占位符，方便后续由 Codex 实现

---

## 1. 总体结构

```text
输入层
→ 中控脑 / 总主文件
→ 主域路由
→ 域内执行（规则 / 路径 / 资源 / 接口）
→ 执行层
→ 结果回收
→ 资产准入
→ 巨型仓库
→ 中控脑再次调用
```

系统由 5 个一级核心件构成：

1. 中控脑（Master Control File）
2. 主域系统（Domain System）
3. 执行层（Execution Layer）
4. 巨型仓库（Asset Warehouse）
5. 私人外接口层（Private Interface Layer）

---

## 2. 一级核心件定义

### 2.1 中控脑（Master Control File）

**职责**

* 识别输入类型
* 判断任务目标
* 选择主域
* 决定辅助域（如有）
* 决定是否允许调用仓库资产
* 决定结果是否可入库
* 维持全局命名、权限、优先级一致性

**禁止事项**

* 不处理域内细节
* 不直接操作底层资源
* 不直接跨域拼接细节逻辑
* 不直接承担具体执行任务

**输入**

* 用户输入
* 文件输入
* 图片输入
* 外部接口回传
* 巨型仓库检索结果

**输出**

* 主域选择结果
* 路径选择结果
* 调用许可结果
* 资产准入判断结果

**占位符**

* `{{MASTER_CONTROL_VERSION}}`
* `{{GLOBAL_ROUTE_RULESET}}`
* `{{GLOBAL_PERMISSION_POLICY}}`
* `{{ASSET_ADMISSION_POLICY}}`

---

### 2.2 主域系统（Domain System）

每个主域都必须独立、自洽、封闭。

**主域之间规则**

* 不允许直接调用彼此内部节点
* 不允许共享未声明资源
* 不允许跨域复用命名空间
* 不允许跨域写入
* 只能通过中控脑 + 域接口通信

**每个主域内部固定分层**

1. 宪法层（Constitution）
2. 规则层（Rules）
3. 路径层（Paths）
4. 资源层（Resources）
5. 接口层（Interfaces）
6. 资产层（Assets）
7. 审计层（Audit）

---

### 2.3 执行层（Execution Layer）

**职责**

* 接收已结构化调用请求
* 执行模型、工具、脚本、外部接口调用
* 返回标准化执行结果

**典型执行器**

* LLM
* 图像生成器
* Python / 代码执行器
* 文件处理器
* GitHub / 外部工具调用器
* 私人接口调用器

**占位符**

* `{{EXECUTOR_REGISTRY}}`
* `{{EXECUTOR_CAPABILITY_MAP}}`

---

### 2.4 巨型仓库（Asset Warehouse）

巨型仓库不是普通存储空间，而是“长期可调用资产中心”。

**固定三层**

* 存储层：真正存内容
* 索引层：支持 AI 精准检索
* 调用层：支持中控脑稳定调用

**固定能力**

* 统一版本
* 统一权限
* 统一检索
* 统一生命周期管理
* 统一资产类型标准

**占位符**

* `{{WAREHOUSE_STORAGE_BACKEND}}`
* `{{WAREHOUSE_INDEX_BACKEND}}`
* `{{WAREHOUSE_RETRIEVAL_POLICY}}`
* `{{WAREHOUSE_LIFECYCLE_POLICY}}`

---

### 2.5 私人外接口层（Private Interface Layer）

这是你的专属程序 / API / 自动化流程的统一入口。

**职责**

* 接入自建 API
* 接入本地程序
* 接入数据库
* 接入文件系统
* 接入自动化脚本
* 接入第三方外部服务

**规则**

* 所有私人外接口必须注册为 Interface Card
* 只能通过中控脑或域接口触发
* 不允许野生调用
* 每个接口必须声明输入、输出、错误、权限、版本

**占位符**

* `{{PRIVATE_API_GATEWAY}}`
* `{{PRIVATE_INTERFACE_REGISTRY}}`
* `{{PRIVATE_AUTH_POLICY}}`

---

## 3. 主域定义（当前正式版）

> 原则：主域之间严格独立，互不侵扰。

### D1 图像创作域

**边界**

* 只负责视觉生成、视觉编辑、视觉风格、视觉还原
* 不处理工程实现
* 不处理量化逻辑
* 不处理研究文献治理

**子域**

* 风格库
* 参考图库
* Prompt 训练库
* 还原模板库
* 误差审计库

**占位符**

* `{{D1_STYLE_LIBRARY}}`
* `{{D1_REFERENCE_LIBRARY}}`
* `{{D1_PROMPT_TRAINING_LIBRARY}}`
* `{{D1_RECONSTRUCTION_TEMPLATES}}`
* `{{D1_IMAGE_AUDIT_LOG}}`

---

### D2 工程 / API 域

**边界**

* 只负责工程实现、接口封装、调用协议、仓库代码理解
* 不负责视觉风格决策
* 不负责量化策略判断
* 不负责研究知识治理

**子域**

* 接口库
* 协议库
* GitHub 仓库库
* 脚本工具库
* 报错修复库

**占位符**

* `{{D2_INTERFACE_LIBRARY}}`
* `{{D2_PROTOCOL_LIBRARY}}`
* `{{D2_GITHUB_REPO_LIBRARY}}`
* `{{D2_SCRIPT_LIBRARY}}`
* `{{D2_ERROR_FIX_LIBRARY}}`

---

### D3 认知 / 架构域

**边界**

* 只负责总主文件、路径规则、命名规范、架构治理、系统演化
* 不直接做视觉生成
* 不直接做工程执行
* 不直接做量化决策

**子域**

* 总规范库
* 路径库
* 命名规范库
* 资产治理库
* 系统审计库

**占位符**

* `{{D3_GLOBAL_SPEC_LIBRARY}}`
* `{{D3_PATH_LIBRARY}}`
* `{{D3_NAMING_STANDARD_LIBRARY}}`
* `{{D3_ASSET_GOVERNANCE_LIBRARY}}`
* `{{D3_SYSTEM_AUDIT_LIBRARY}}`

---

### D4 研究 / 学习域

**边界**

* 只负责知识提取、资料整理、研究模板、学习沉淀
* 不负责图像风格控制
* 不负责工程接口实现
* 不负责量化执行判断

**子域**

* 文献资料库
* 知识卡片库
* 研究模板库
* 数据集库
* 学习笔记库

**占位符**

* `{{D4_LITERATURE_LIBRARY}}`
* `{{D4_KNOWLEDGE_CARD_LIBRARY}}`
* `{{D4_RESEARCH_TEMPLATE_LIBRARY}}`
* `{{D4_DATASET_LIBRARY}}`
* `{{D4_NOTE_LIBRARY}}`

---

### D5 Quant / 决策域

**边界**

* 只负责量化研究、状态表达、策略逻辑、决策审计、复盘
* 不负责图像生成
* 不负责架构治理
* 不负责研究资料治理

**子域**

* 策略库
* 状态库
* 回测库
* 复盘优化库
* 风险控制库

**占位符**

* `{{D5_STRATEGY_LIBRARY}}`
* `{{D5_STATE_LIBRARY}}`
* `{{D5_BACKTEST_LIBRARY}}`
* `{{D5_REVIEW_OPT_LIBRARY}}`
* `{{D5_RISK_LIBRARY}}`

---

## 4. 域隔离规则（硬规则）

### 4.1 主域隔离

* 主域之间禁止直接调用内部节点
* 主域之间禁止共享隐式状态
* 主域之间禁止写入彼此资源层
* 主域之间禁止命名复用
* 主域之间禁止绕过中控脑直接通信

### 4.2 主域与子域隔离

* 子域只能归属于一个主域
* 子域不能跨主域复用身份
* 子域资源只能在本主域内部声明
* 子域对外通信必须经过本域接口层

### 4.3 跨域唯一合法路径

```text
源域
→ 源域接口层
→ 中控脑
→ 目标域接口层
→ 目标域
```

任何不走这条路径的跨域通信，视为非法。

---

## 5. AI-native 对象系统（统一语言）

系统内只允许以下核心对象作为正式对象：

1. `MasterControlCard`
2. `DomainMasterCard`
3. `ResourceCard`
4. `InterfaceCard`
5. `PathCard`
6. `PromptCard`
7. `AssetCard`
8. `AuditCard`

### 5.1 统一字段基线

所有正式对象至少包含：

* `id`
* `type`
* `domain`
* `subdomain`
* `version`
* `status`
* `purpose`
* `inputs`
* `outputs`
* `dependencies`
* `trigger_conditions`
* `forbidden_conditions`
* `writeback_policy`
* `notes`

---

## 6. 资产准入逻辑

不是所有结果都能入库。

### 6.1 资产候选条件

至少满足以下 4 项：

* 可复用
* 可调用
* 可结构化表达
* 域归属明确
* 对质量/效率有提升
* 已验证或部分验证
* 不与现有资产重复污染

### 6.2 准入结果

* `accepted`
* `candidate`
* `temporary`
* `rejected`

### 6.3 生命周期

```text
创建 → 试用 → 验证 → 正式入库 → 高频使用 / 降权 / 归档 / 淘汰
```

---

## 7. 中控脑主循环

```text
1. 接收输入
2. 识别任务类型
3. 选择主域
4. 判定辅助域（可选）
5. 加载对应 PathCard
6. 调用域资源 / 接口 / 资产
7. 下发执行层
8. 接收执行结果
9. 进行回收与评估
10. 判定是否生成 AssetCard
11. 入仓或候选观察
12. 更新中控可调用索引
```

---

## 8. 域卡模板（给 Codex 实现）

```yaml
id: {{DOMAIN_CARD_ID}}
type: DomainMasterCard
domain: {{DOMAIN_NAME}}
version: {{DOMAIN_VERSION}}
status: active
purpose:
  - {{DOMAIN_PURPOSE_1}}
  - {{DOMAIN_PURPOSE_2}}
boundaries:
  include:
    - {{ALLOWED_SCOPE_1}}
    - {{ALLOWED_SCOPE_2}}
  exclude:
    - {{FORBIDDEN_SCOPE_1}}
    - {{FORBIDDEN_SCOPE_2}}
subdomains:
  - {{SUBDOMAIN_1}}
  - {{SUBDOMAIN_2}}
  - {{SUBDOMAIN_3}}
interfaces:
  inbound:
    - {{INBOUND_INTERFACE_1}}
  outbound:
    - {{OUTBOUND_INTERFACE_1}}
asset_policy:
  writable: true
  accepted_types:
    - ResourceCard
    - PromptCard
    - AssetCard
isolation_rules:
  - no_direct_cross_domain_calls
  - no_shared_implicit_state
  - domain_local_namespace_only
notes:
  - {{DOMAIN_NOTE_1}}
```

---

## 9. 接口卡模板（给 Codex 实现）

```yaml
id: {{INTERFACE_ID}}
type: InterfaceCard
domain: {{DOMAIN_NAME}}
subdomain: {{SUBDOMAIN_NAME}}
version: {{INTERFACE_VERSION}}
status: active
purpose:
  - {{INTERFACE_PURPOSE}}
inputs:
  - {{INPUT_1}}
  - {{INPUT_2}}
outputs:
  - {{OUTPUT_1}}
  - {{OUTPUT_2}}
trigger_conditions:
  - {{TRIGGER_1}}
forbidden_conditions:
  - {{FORBIDDEN_1}}
dependencies:
  - {{DEPENDENCY_1}}
  - {{DEPENDENCY_2}}
execution_target: {{PRIVATE_API_OR_TOOL}}
error_policy:
  retry: {{RETRY_POLICY}}
  fallback: {{FALLBACK_POLICY}}
writeback_policy: {{WRITEBACK_POLICY}}
notes:
  - {{INTERFACE_NOTE_1}}
```

---

## 10. 路径卡模板（给 Codex 实现）

```yaml
id: {{PATH_ID}}
type: PathCard
domain: {{DOMAIN_NAME}}
version: {{PATH_VERSION}}
status: active
purpose:
  - {{PATH_PURPOSE}}
steps:
  - step_id: 1
    action: {{ACTION_1}}
    inputs:
      - {{INPUT_A}}
    outputs:
      - {{OUTPUT_A}}
  - step_id: 2
    action: {{ACTION_2}}
    inputs:
      - {{INPUT_B}}
    outputs:
      - {{OUTPUT_B}}
trigger_conditions:
  - {{TRIGGER_1}}
forbidden_conditions:
  - {{FORBIDDEN_1}}
dependencies:
  - {{DEPENDENCY_1}}
writeback_policy: {{WRITEBACK_POLICY}}
notes:
  - {{PATH_NOTE_1}}
```

---

## 11. 目录骨架（给 Codex 执行）

```text
CCOS/
  00_master_control/
    master_control_file.yaml
    global_route_rules.yaml
    global_permission_policy.yaml
    asset_admission_policy.yaml

  01_domains/
    D1_image/
      domain_master.yaml
      constitution/
      rules/
      paths/
      resources/
      interfaces/
      assets/
      audit/
    D2_engineering_api/
      domain_master.yaml
      constitution/
      rules/
      paths/
      resources/
      interfaces/
      assets/
      audit/
    D3_cognitive_architecture/
      domain_master.yaml
      constitution/
      rules/
      paths/
      resources/
      interfaces/
      assets/
      audit/
    D4_research_learning/
      domain_master.yaml
      constitution/
      rules/
      paths/
      resources/
      interfaces/
      assets/
      audit/
    D5_quant_decision/
      domain_master.yaml
      constitution/
      rules/
      paths/
      resources/
      interfaces/
      assets/
      audit/

  02_private_interfaces/
    registry/
    adapters/
    auth/
    docs/

  03_execution/
    executor_registry.yaml
    executor_capability_map.yaml

  04_asset_warehouse/
    storage/
    index/
    retrieval/
    lifecycle/

  05_templates/
    cards/
    paths/
    prompts/

  06_audit/
    system/
    domain/
    interface/
```

---

## 12. Codex 执行要求

Codex 在实现时必须遵守：

1. 不得跳过中控脑直接构建跨域调用
2. 不得让主域直接共享内部状态
3. 不得让任何对象缺少统一字段基线
4. 不得把散文化说明当成正式对象
5. 不得让私人接口脱离 Interface Card 注册系统
6. 所有路径必须可以被 AI 结构化解析
7. 所有目录命名必须稳定、低歧义、不可漂移

---

## 13. 给你的注释（人类理解层）

这套骨架最重要的不是“复杂”，而是三件事：

### A. 中控脑不做细节

这样中控不会被边缘功能拖死。

### B. 主域严格隔离

这样网络扩张不会互相污染。

### C. 所有东西都写成 AI 能吃得下的对象卡

这样后续不管是我还是 Codex，都能稳定调用。

如果后续你要推进，最先落地的顺序应该是：

1. `00_master_control/`
2. 一个域（建议 D1 或 D2）
3. `02_private_interfaces/`
4. `04_asset_warehouse/`

这就是第一阶段。
