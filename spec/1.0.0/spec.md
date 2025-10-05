# Prompt 语义化版本规范 (PromptVer) 1.0.0

> 由 LangGPT 社区提出 | Created by 云中江树

## 摘要

给定 Prompt 版本号 **MAJOR.MINOR.PATCH**，按以下规则递增：

- **MAJOR** 版本：当您对 Prompt 的行为契约做出不兼容的变更时
- **MINOR** 版本：当您以向后兼容的方式添加新功能或场景时  
- **PATCH** 版本：当您进行向后兼容的优化或修复时

可扩展添加 **模型标识符** 和 **构建元数据** 的附加标签。

---

## 引言

随着 AI 应用的普及，Prompt 已成为关键的生产资产。然而，缺乏统一的版本管理标准导致：

- **集成风险**：不知道 Prompt 升级是否会破坏现有功能
- **协作困难**：团队成员难以理解版本间的差异
- **维护混乱**：无法追踪 Prompt 的演进历史和兼容性

**PromptVer** 旨在解决这些问题，提供一套清晰、可预测的版本管理规范。

PromptVer 是 LangGPT 生态的重要组成部分，与 LangGPT 的结构化设计理念完美互补：
- **LangGPT** 解决"如何写好 Prompt"
- **PromptVer** 解决"如何管理 Prompt 版本"

---

## 核心概念

### 1. 行为契约 (Behavioral Contract)

Prompt 的"公共 API"即其**行为契约**，包括：

- **输出格式**：文本结构、JSON schema、Markdown 格式等
- **功能范围**：承诺完成的任务类型和能力边界
- **核心约束**：语言、语气、长度、安全规则等关键限制
- **预期行为**：在标准场景下的响应模式

### 2. 向后兼容性 (Backward Compatibility)

如果满足以下条件，则认为版本 B 与版本 A 向后兼容：

- 版本 A 的所有有效输入在版本 B 中仍能产生**语义等价**的输出
- 版本 B 的输出格式包含版本 A 的所有必需字段
- 版本 B 不违反版本 A 声明的任何核心约束

---

## PromptVer 规范

### 规则 1：行为契约声明

使用 PromptVer 的 Prompt **必须**声明其行为契约。契约可以通过以下方式定义：

- 结构化元数据文件（推荐）
- Prompt 文件头部注释
- 独立的文档说明

**示例元数据：**

```yaml
# prompt_contract.yaml
version: "1.2.3"
contract:
  output_format: "JSON"
  output_schema:
    type: "object"
    required: ["response", "sentiment"]
  capabilities:
    - "customer_inquiry"
    - "complaint_handling"
  constraints:
    language: "zh-CN"
    tone: "professional"
    max_length: 500
```

### 规则 2：版本号格式

正常版本号**必须**采用 `MAJOR.MINOR.PATCH` 格式，其中：

- 三个数字均为非负整数
- 不得包含前导零
- 每个元素必须按数值递增

示例：`1.0.0` → `1.1.0` → `1.1.1` → `2.0.0`

### 规则 3：初始开发阶段

- 主版本号为 `0` 时（`0.y.z`）表示初始开发阶段
- 此阶段任何内容都可能随时更改
- 行为契约不应被视为稳定

### 规则 4：稳定版本发布

版本 `1.0.0` 定义了稳定的行为契约。发布条件：

- Prompt 已在生产环境使用
- 行为契约已明确定义
- 团队承诺维护向后兼容性

### 规则 5：PATCH 版本递增

当且仅当进行**向后兼容的优化或修复**时，递增 PATCH 版本（`x.y.Z`）：

- ✅ 修正错别字或语法错误
- ✅ 优化指令措辞以提高清晰度（不改变行为）
- ✅ 调整示例数据（不改变功能）
- ✅ 性能优化（如减少 token 使用）

**示例：**
```
v1.2.0 → v1.2.1
变更：修复 "您的" 拼写为 "你的"（统一措辞）
```

### 规则 6：MINOR 版本递增

当以**向后兼容方式添加新功能**时，递增 MINOR 版本（`x.Y.z`）：

- ✅ 添加新的使用场景或功能模块
- ✅ 扩展输出格式（新增可选字段）
- ✅ 添加新的示例或边缘情况处理
- ✅ 标记某功能为弃用（但仍保留）

MINOR 版本递增时，PATCH 版本**必须**重置为 0。

**示例：**
```
v1.2.3 → v1.3.0
变更：新增退款申请处理场景
契约：保持原有客服咨询功能不变
```

### 规则 7：MAJOR 版本递增

当进行**向后不兼容的变更**时，递增 MAJOR 版本（`X.y.z`）：

- ⚠️ 改变输出格式结构
- ⚠️ 修改核心功能定义或角色设定
- ⚠️ 删除已有功能或字段
- ⚠️ 修改核心约束（如改变语言、语气）

MAJOR 版本递增时，MINOR 和 PATCH **必须**重置为 0。

**示例：**
```
v1.3.2 → v2.0.0
变更：输出从纯文本改为 JSON 格式
影响：所有调用方需要修改解析逻辑
```

### 规则 8：模型标识符（可选）

可在版本号后附加模型标识符，格式为 `@model-name`：

```
v1.2.3@gpt-4-turbo
v1.2.3@claude-sonnet-4
v2.0.0@gemini-pro
```

**用途：**
- 标注 Prompt 测试和优化所基于的模型
- 警示跨模型兼容性问题

### 规则 9：预发布版本

预发布版本通过在 PATCH 后附加 `-` 和标识符表示：

```
v1.0.0-alpha        # 内部测试
v1.0.0-alpha.1      # 第一轮修复
v1.0.0-beta         # 公开测试
v1.0.0-beta.2       # 第二轮优化
v1.0.0-rc.1         # 候选发布
v1.0.0              # 正式发布
```

**规则：**
- 标识符只能包含 `[0-9A-Za-z-]`
- 预发布版本优先级低于正式版本
- 示例：`1.0.0-alpha < 1.0.0`

### 规则 10：构建元数据

构建元数据通过 `+` 附加，用于标注非功能性信息：

```
v1.2.3+20251005              # 构建日期
v1.2.3+customer-service      # 功能标识
v1.2.3-beta+exp.sha.a1b2c3   # 实验版本 + Git hash
```

**注意：** 构建元数据不影响版本优先级。

---

## 兼容性测试

### 测试套件要求

每个 Prompt 版本**应该**维护一套测试用例：

```yaml
# test_suite.yaml
version: "1.2.3"
tests:
  - name: "标准客服咨询"
    input: "我想查询订单状态"
    expected_behavior:
      format: "JSON"
      required_fields: ["response", "sentiment"]
      sentiment: "neutral"
    
  - name: "投诉处理"
    input: "你们的服务太差了！"
    expected_behavior:
      format: "JSON"
      tone: "apologetic"
      sentiment: "negative"
```

### 验证向后兼容性

升级前后运行相同测试用例，验证：

1. **格式一致性**：输出结构未改变
2. **语义等价性**：响应含义基本相同
3. **约束遵守**：核心规则未违反

---

## 变更日志规范

每个版本**必须**维护变更日志（CHANGELOG.md）：

```markdown
# Changelog

## [2.0.0] - 2025-10-05
### ⚠️ Breaking Changes
- 输出格式从纯文本改为 JSON
- 移除 `简洁模式` 参数

### Added
- 新增结构化情感分析字段
- 支持多轮对话上下文

### Migration Guide
参见 [v1-to-v2-migration.md](./docs/migration.md)

## [1.3.0] - 2025-09-28
### Added
- 新增退款申请处理场景
- 添加 5 个投诉升级示例

### Changed
- 优化道歉语气的表达

## [1.2.1] - 2025-09-20
### Fixed
- 修正 "感谢" 场景的措辞
- 更正示例中的错别字
```

---

## 依赖管理

### 声明依赖版本

在调用方代码中明确 Prompt 版本依赖：

```json
{
  "prompts": {
    "customer-service": "^1.2.0",
    "sentiment-analysis": "~2.1.3"
  }
}
```

**版本范围语法：**
- `1.2.3`：精确版本
- `^1.2.3`：兼容 1.x.x（≥1.2.3 且 <2.0.0）
- `~1.2.3`：兼容 1.2.x（≥1.2.3 且 <1.3.0）
- `>=1.2.0 <2.0.0`：显式范围

---

## 最佳实践

### 1. 何时递增 MAJOR 版本？

✅ **应该递增的情况：**
- 改变 JSON schema 的必需字段
- 从中文改为英文输出
- 删除已有的功能模块

❌ **不应递增的情况：**
- 添加新的可选 JSON 字段
- 内部实现优化（用户无感知）
- 示例数据更新

### 2. 快速迭代策略

在 `0.x.x` 阶段快速实验：

```
v0.1.0  初始版本
v0.2.0  每日快速迭代
v0.3.0  不保证兼容性
...
v1.0.0  稳定后发布
```

### 3. 弃用流程

标记功能为弃用的标准流程：

```
v1.5.0  添加新功能 X，标记旧功能 Y 为弃用
v1.6.0  继续保留功能 Y，再次警告
v2.0.0  移除功能 Y
```

### 4. 多模型支持

同一逻辑版本适配多个模型：

```
v1.2.3@gpt-4      # GPT-4 优化版本
v1.2.3@claude     # Claude 优化版本
v1.2.3@generic    # 通用版本
```

### 5. 文件组织

```
prompts/
├── customer-service/
│   ├── v1.2.3/
│   │   ├── prompt.txt
│   │   ├── contract.yaml
│   │   ├── test_suite.yaml
│   │   └── README.md
│   ├── v2.0.0/
│   └── CHANGELOG.md
└── sentiment-analysis/
    └── ...
```

---

## Git 集成

### 标签命名

```bash
# 创建版本标签
git tag -a v1.2.3 -m "Release: Add refund scenario"
git tag -a v2.0.0 -m "Breaking: Change to JSON output"

# 推送标签
git push origin v1.2.3
```

### 分支策略

```
main              → v1.x.x (生产稳定版)
develop           → v2.0.0-alpha (下一个大版本)
feature/rag       → v1.3.0-experimental
hotfix/typo-fix   → v1.2.4
```

---

## 常见问题

### Q1：如何判断措辞优化是 PATCH 还是 MINOR？

**规则：** 如果优化后行为契约保持不变，则为 PATCH；如果行为明显改变，则为 MINOR 或 MAJOR。

**示例：**
- PATCH：`"您好"` → `"你好"`（仅措辞）
- MINOR：添加更多礼貌用语（增强功能）
- MAJOR：`"专业语气"` → `"幽默语气"`（改变约束）

### Q2：跨模型的版本如何管理？

**推荐方案：**
- 使用模型标识符：`v1.2.3@gpt-4`、`v1.2.3@claude`
- 在元数据中声明模型兼容性
- 维护模型特定的测试套件

### Q3：LLM 输出的随机性如何处理？

**解决方案：**
- 关注**结构一致性**而非逐字相同
- 测试时使用 `temperature=0` 降低随机性
- 验证语义等价性而非字符串匹配

### Q4：如何处理 Prompt 的 A/B 测试？

**建议：**
- 使用预发布版本：`v1.3.0-variant-a`、`v1.3.0-variant-b`
- 或使用构建元数据：`v1.3.0+variant-a`
- 稳定后选择最优版本作为正式版

---

## 版本验证正则表达式

完整 PromptVer 格式（含可选的模型标识符）：

```regex
^(0|[1-9]\d*)\.(0|[1-9]\d*)\.(0|[1-9]\d*)(?:-((?:0|[1-9]\d*|\d*[a-zA-Z-][0-9a-zA-Z-]*)(?:\.(?:0|[1-9]\d*|\d*[a-zA-Z-][0-9a-zA-Z-]*))*))?(?:\+([0-9a-zA-Z-]+(?:\.[0-9a-zA-Z-]+)*))?(?:@([a-z0-9-]+))?$
```

**匹配示例：**
- `1.2.3` ✅
- `1.2.3-alpha` ✅
- `1.2.3+20251005` ✅
- `1.2.3@gpt-4` ✅
- `1.2.3-beta+build@claude` ✅
- `v1.2.3` ❌（不含 v 前缀）

---

## 许可与贡献

本规范采用 **CC BY 4.0** 许可，欢迎社区贡献和改进。

**规范版本：** PromptVer 1.0.0  
**发布日期：** 2025-10-05  
**维护者：** AI Prompt Engineering Community

---

## 采纳 PromptVer

如果您的项目采用本规范，请在 README 中添加徽章：

```markdown
[![PromptVer](https://img.shields.io/badge/PromptVer-1.0.0-blue)](https://promptver.org)
```

并在 Prompt 文件头部注明：

```
# Customer Service Prompt
# Version: 1.2.3
# PromptVer: 1.0.0
# License: MIT
```

---

## 总结

PromptVer 提供了一套清晰、可预测的 Prompt 版本管理规范，帮助团队：

- ✅ 明确版本间的兼容性关系
- ✅ 安全地升级和回滚 Prompt
- ✅ 有效协作和知识传承
- ✅ 降低集成风险

**立即开始使用 PromptVer，让您的 Prompt 版本管理更加专业！**