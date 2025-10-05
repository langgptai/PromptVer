<div align="center">

# PromptVer

### Semantic Versioning for AI Prompts
### Prompt 语义化版本规范 | プロンプトのセマンティックバージョニング

[![License](https://img.shields.io/badge/License-CC%20BY%204.0-blue.svg)](LICENSE)
[![Version](https://img.shields.io/badge/PromptVer-1.0.0-green.svg)](spec/1.0.0/spec.md)
[![LangGPT](https://img.shields.io/badge/LangGPT-Community-orange.svg)](https://github.com/langgptai/LangGPT)
[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg)](CONTRIBUTING.md)

[English](README_en.md) | [日本語](README_ja.md) | 简体中文

**为 AI Prompt 提供清晰、可预测的版本管理**

</div>

---

## 🎯 什么是 PromptVer？

PromptVer（Prompt Semantic Versioning）是专为 **AI Prompt 设计的语义化版本管理规范**。

受 [Semantic Versioning](https://semver.org) 启发，PromptVer 让团队能够：
- ✅ **清晰沟通**：一眼看出 Prompt 升级的影响范围
- ✅ **安全迭代**：知道哪些改动会破坏现有功能
- ✅ **高效协作**：团队成员快速理解版本差异
- ✅ **追溯变更**：轻松回滚到稳定版本

---

## 📖 版本号格式

```
MAJOR.MINOR.PATCH

示例: 1.2.3
```

**递增规则：**

| 版本类型 | 何时递增 | 示例 |
|---------|---------|------|
| **MAJOR** | 破坏性变更（输出格式、核心功能） | 1.x.x → 2.0.0 |
| **MINOR** | 向后兼容的新功能 | x.1.x → x.2.0 |
| **PATCH** | 向后兼容的修复和优化 | x.x.1 → x.x.2 |

---

## 🚀 快速开始

### 1️⃣ 在 Prompt 中标注版本

```markdown
# Role: Customer Service Assistant

## Profile
- Version: 1.0.0          ← 添加版本号
- PromptVer: 1.0.0        ← 声明遵循 PromptVer 规范
- Language: 简体中文
- Description: 专业的客服助手
```

### 2️⃣ 维护变更日志

创建 `CHANGELOG.md`：

```markdown
# Changelog

## [1.1.0] - 2025-10-06
### Added
- 新增退款处理场景

## [1.0.0] - 2025-10-01
### Added
- 初始版本发布
- 客户咨询响应功能
```

### 3️⃣ 版本递增示例

```bash
# 修复错别字
1.0.0 → 1.0.1 (PATCH)

# 添加新功能
1.0.1 → 1.1.0 (MINOR)

# 改变输出格式
1.1.0 → 2.0.0 (MAJOR)
```

---

## 💡 实际案例

查看完整的版本演进示例：

### [客服助手 Prompt](examples/basic/customer-service/)

```
v1.0.0 → 基础客服功能
v1.1.0 → 添加退款处理（向后兼容）
v2.0.0 → 输出改为 JSON 格式（破坏性变更）
```

**为什么是 MAJOR？**  
从纯文本改为 JSON，调用方需要修改解析代码。

[查看完整演进过程 →](examples/basic/customer-service/CHANGELOG.md)

---

## 📚 完整文档

<table>
<tr>
<td width="50%">

### 📖 规范文档
- [中文完整规范](spec/1.0.0/spec.md)
- [English Specification](spec/1.0.0/spec_en.md)
- [日本語仕様書](spec/1.0.0/spec_ja.md)

</td>
<td width="50%">

### 📘 使用指南
- [5 分钟快速开始](docs/quickstart.md)
- [完整示例](examples/)
- [常见问题 FAQ](#-常见问题)

</td>
</tr>
</table>

---

## 🌟 核心特性

### 简单易懂
```
不需要复杂工具
只需要三个数字
```

### 向后兼容
```
MINOR 和 PATCH 保证兼容
MAJOR 明确标识破坏性变更
```

### 团队协作
```
统一的版本语言
清晰的变更沟通
```

### 生态友好
```
与 LangGPT 完美集成
可用于任何 Prompt 项目
```

---

## 🔄 与 LangGPT 集成

**完美互补：**

<table>
<tr>
<td align="center" width="50%">

### [LangGPT](https://github.com/langgptai/LangGPT)
**如何"写"好 Prompt**

结构化设计框架
模板化编写方法

</td>
<td align="center" width="50%">

### PromptVer
**如何"管"好 Prompt**

版本管理规范
变更追踪体系

</td>
</tr>
</table>

**使用示例：**

```markdown
# 用 LangGPT 结构编写
# Role: Code Assistant

## Profile
- Version: 1.2.3        ← PromptVer 版本管理
- PromptVer: 1.0.0
- Author: YourName

## Skills
- 代码生成
- 代码审查

## Rules
...
```

---

## 🎓 为什么需要 PromptVer？

### 痛点 1：不知道改动影响
```
❌ 改了 Prompt，不知道会不会影响现有功能
✅ MAJOR 版本明确标识破坏性变更
```

### 痛点 2：版本混乱
```
❌ v1、v2、v3-final、v3-final-final-really
✅ 1.0.0、1.1.0、1.2.0 语义清晰
```

### 痛点 3：难以协作
```
❌ 不知道同事改了什么
✅ CHANGELOG 记录每次变更
```

### 痛点 4：无法回滚
```
❌ 新版本有问题，不知道回到哪个版本
✅ 版本号 + Git 标签，轻松回滚
```

---

## 🛠 最佳实践

### ✅ Do

```markdown
✓ 在 Prompt 头部明确标注版本号
✓ 维护详细的 CHANGELOG.md
✓ 破坏性变更递增 MAJOR 版本
✓ 使用 Git 标签管理版本
```

### ❌ Don't

```markdown
✗ 修改已发布的版本内容
✗ 跳过版本号（1.0.0 → 1.2.0）
✗ 用模糊描述代替版本号
✗ 忽略 CHANGELOG 维护
```

---

## 📦 示例项目

### 基础示例
- [客服助手](examples/basic/customer-service/) - 完整的版本演进过程

### 即将推出
- 代码助手 - 多语言支持的演进
- 内容写作 - 风格调整的版本管理
- 数据分析 - 输出格式的迁移

---

## ❓ 常见问题

<details>
<summary><b>Q1: PromptVer 和 SemVer 有什么区别？</b></summary>

PromptVer 基于 SemVer 理念，但针对 Prompt 特性优化：
- **兼容性定义**：关注输出行为而非代码接口
- **灵活性**：考虑 LLM 的概率性输出特点
- **面向未来**：支持多模态、Agent 等场景
</details>

<details>
<summary><b>Q2: 什么时候应该递增 MAJOR 版本？</b></summary>

当出现以下情况时递增 MAJOR：
- 改变输出格式（JSON → 文本）
- 删除已有功能
- 修改核心约束（中文 → 英文）
- 任何调用方需要修改代码的变更
</details>

<details>
<summary><b>Q3: 必须从 1.0.0 开始吗？</b></summary>

不是。初始开发阶段可以使用 0.x.x：
```
0.1.0 - 初始开发
0.2.0 - 快速迭代
0.9.0 - 接近稳定
1.0.0 - 正式发布
```
</details>

<details>
<summary><b>Q4: 如何处理 LLM 的随机性？</b></summary>

关注**结构一致性**而非逐字相同：
- 验证输出格式是否一致
- 检查必需字段是否存在
- 测试关键功能是否正常
</details>

更多问题查看 [完整 FAQ](docs/faq.md)

---

## 🤝 参与贡献

我们欢迎所有形式的贡献！

### 如何贡献

- 📝 **改进文档**：修正错别字、改善表述
- 🐛 **报告问题**：发现 Bug 或有疑问
- 💡 **提出建议**：新功能或改进想法
- 🌍 **翻译规范**：帮助翻译成更多语言
- 📖 **分享案例**：提供真实使用案例

查看 [贡献指南](CONTRIBUTING.md) 了解详情

### 贡献者

感谢所有贡献者！

<!-- 自动生成贡献者列表 -->
<a href="https://github.com/langgptai/promptver/graphs/contributors">
  <img src="https://contrib.rocks/image?repo=langgptai/promptver" />
</a>

---

## 📊 采纳 PromptVer

如果你的项目采用了 PromptVer，欢迎告诉我们！

**在你的 README 中添加徽章：**

```markdown
[![PromptVer](https://img.shields.io/badge/PromptVer-1.0.0-blue.svg)](https://github.com/langgptai/promptver)
```

**在 Prompt 文件中注明：**

```markdown
# Customer Service Prompt
# Version: 1.2.3
# PromptVer: 1.0.0
```

[提交你的项目 →](https://github.com/langgptai/promptver/issues/new?title=Adoption:)

---

## 🔗 相关资源

### 项目
- [LangGPT](https://github.com/langgptai/LangGPT) - 结构化 Prompt 设计框架
- [Semantic Versioning](https://semver.org) - 软件语义化版本规范
- [Keep a Changelog](https://keepachangelog.com) - 变更日志规范

### 文章
- [为什么 Prompt 需要版本管理？](#) *(即将发布)*
- [PromptVer 最佳实践指南](#) *(即将发布)*
- [从 LangGPT 到 PromptVer：完整的 Prompt 工程体系](#) *(即将发布)*

---

## 📬 联系我们

- 💬 [GitHub Discussions](https://github.com/langgptai/promptver/discussions) - 讨论和提问
- 🐛 [GitHub Issues](https://github.com/langgptai/promptver/issues) - 报告问题
- 🌐 [LangGPT Community](https://github.com/langgptai/LangGPT) - 加入社区
- 📧 Email: *通过 GitHub Issues 联系*

---

## 📜 许可证

本规范采用 [Creative Commons Attribution 4.0 International License](LICENSE) 许可。

这意味着你可以自由地：
- ✅ 分享：复制和重新分发
- ✅ 改编：重新混合、转换和构建

只需：
- 📌 署名：给予适当的署名

---

## 🙏 致谢

**PromptVer** 由 [LangGPT Community](https://github.com/langgptai/LangGPT) 提出和维护。

特别感谢：
- [Semantic Versioning](https://semver.org) 提供的灵感
- LangGPT 社区成员的支持
- 所有早期采纳者和贡献者

---

## ⭐ Star History

如果 PromptVer 对你有帮助，欢迎 Star！

[![Star History Chart](https://api.star-history.com/svg?repos=langgptai/promptver&type=Date)](https://star-history.com/#langgptai/promptver&Date)

---

<div align="center">

**[⬆ 回到顶部](#promptver)**

Made with ❤️ by [LangGPT Community](https://github.com/langgptai)

[网站](https://github.com/langgptai/promptver) · 
[文档](spec/1.0.0/spec.md) · 
[示例](examples/) · 
[社区](https://github.com/langgptai/LangGPT)

</div>