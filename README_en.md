<div align="center">

# PromptVer

**Semantic Versioning for Prompts**

[![License](https://img.shields.io/badge/License-CC%20BY%204.0-blue.svg)](LICENSE)
[![Version](https://img.shields.io/badge/PromptVer-1.0.0-green.svg)](spec/1.0.0/spec_en.md)

English | [日本語](README_ja.md) | [简体中文](README.md)

</div>

## 🎯 What is PromptVer?

PromptVer is a **semantic versioning specification** designed for Prompts, helping teams understand:
- ✅ Whether an upgrade breaks existing functionality
- ✅ What new features are added
- ✅ How to safely upgrade and rollback

## 📖 Version Format

```
MAJOR.MINOR.PATCH
Example: 1.2.3

MAJOR: Breaking changes (output format, core functionality)
MINOR: Backward-compatible new features
PATCH: Backward-compatible fixes and optimizations
```

## 🚀 Quick Start

### Add version to Prompt

```markdown
# Role: Customer Service

## Profile
- Version: 1.0.0
- PromptVer: 1.0.0
- Description: Professional customer service assistant
```

### Maintain changelog

```markdown
## [1.1.0] - 2025-10-06
### Added
- Added refund handling scenario

## [1.0.0] - 2025-10-01
### Added
- Initial release
```

## 📚 Documentation

- [Full Specification](spec/1.0.0/spec_en.md)
- [Quick Start](docs/quickstart.md)
- [Examples](examples/basic/customer-service/)

## 🤝 Contributing

Contributions welcome! See [CONTRIBUTING.md](CONTRIBUTING.md)

## 📜 License

Licensed under [CC BY 4.0](LICENSE)

---

<div align="center">

Proposed and maintained by [LangGPT Community](https://github.com/langgptai/LangGPT)

</div>