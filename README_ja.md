<div align="center">

# PromptVer

**プロンプトのセマンティックバージョニング**

[![License](https://img.shields.io/badge/License-CC%20BY%204.0-blue.svg)](LICENSE)
[![Version](https://img.shields.io/badge/PromptVer-1.0.0-green.svg)](spec/1.0.0/spec_ja.md)

[English](README_en.md) | 日本語 | [简体中文](README.md)

</div>

## 🎯 PromptVerとは？

PromptVerは、プロンプト専用の**セマンティックバージョニング仕様**です：
- ✅ アップグレードが既存機能を破壊するか
- ✅ 新機能が追加されたか
- ✅ 安全にアップグレード・ロールバックする方法

## 📖 バージョン形式

```
MAJOR.MINOR.PATCH
例: 1.2.3

MAJOR: 破壊的変更（出力形式、コア機能の変更）
MINOR: 後方互換性のある新機能
PATCH: 後方互換性のある修正と最適化
```

## 🚀 クイックスタート

### プロンプトにバージョンを追加

```markdown
# Role: Customer Service

## Profile
- Version: 1.0.0
- PromptVer: 1.0.0
- Description: プロフェッショナルなカスタマーサービス
```

### 変更履歴を維持

```markdown
## [1.1.0] - 2025-10-06
### Added
- 返金処理シナリオを追加

## [1.0.0] - 2025-10-01
### Added
- 初回リリース
```

## 📚 ドキュメント

- [完全仕様](spec/1.0.0/spec_ja.md)
- [クイックスタート](docs/quickstart.md)
- [サンプル](examples/basic/customer-service/)

## 🤝 貢献

貢献歓迎！[CONTRIBUTING.md](CONTRIBUTING.md)をご覧ください

## 📜 ライセンス

[CC BY 4.0](LICENSE)でライセンスされています

---

<div align="center">

[LangGPTコミュニティ](https://github.com/langgptai/LangGPT)によって提案・維持

</div>