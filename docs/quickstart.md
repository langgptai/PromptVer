# 快速开始

## 5 分钟上手 PromptVer

### 1️⃣ 在 Prompt 中添加版本

```markdown
# Role: Customer Service Assistant

## Profile
- Version: 1.0.0
- PromptVer: 1.0.0
- Description: 专业客服助手
```

### 2️⃣ 维护变更日志

创建 `CHANGELOG.md`:

```markdown
# Changelog

## [1.0.0] - 2025-10-06
### Added
- 初始版本发布
- 客户咨询响应功能
```

### 3️⃣ 版本递增规则

```
修复错误    → 1.0.0 → 1.0.1 (PATCH)
添加新功能  → 1.0.1 → 1.1.0 (MINOR)
破坏性变更  → 1.1.0 → 2.0.0 (MAJOR)
```

### 4️⃣ 什么是破坏性变更？

- 改变输出格式（JSON → 文本）
- 删除功能
- 修改核心约束（中文 → 英文）

## 完整示例

查看 [examples/basic/customer-service](../examples/basic/customer-service/)

