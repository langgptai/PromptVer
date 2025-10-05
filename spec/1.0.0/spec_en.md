# Prompt Semantic Versioning Specification (PromptVer) 1.0.0

> Proposed by LangGPT Community | Created by Yun Zhong Jiang Shu (云中江树)

## Summary

Given a version number **MAJOR.MINOR.PATCH**, increment the:

- **MAJOR** version when you make incompatible changes to the behavioral contract
- **MINOR** version when you add functionality in a backward compatible manner
- **PATCH** version when you make backward compatible optimizations or fixes

Additional labels for **model identifier** and **build metadata** can be appended.

---

## Introduction

As AI applications become widespread, Prompts have become critical production assets. However, the lack of unified versioning standards leads to:

- **Integration Risk**: Uncertainty about whether Prompt upgrades will break existing functionality
- **Collaboration Difficulty**: Team members struggle to understand version differences
- **Maintenance Chaos**: Unable to track Prompt evolution history and compatibility

**PromptVer** aims to solve these problems by providing a clear, predictable versioning specification.

PromptVer is an integral part of the LangGPT ecosystem, perfectly complementing LangGPT's structured design philosophy:
- **LangGPT**: How to write good Prompts (structured design)
- **PromptVer**: How to manage Prompt versions (semantic versioning)

---

## Core Concepts

### 1. Behavioral Contract

The "public API" of a Prompt is its **behavioral contract**, including:

- **Output Format**: Text structure, JSON schema, Markdown format, etc.
- **Functional Scope**: Committed task types and capability boundaries
- **Core Constraints**: Key limitations like language, tone, length, safety rules
- **Expected Behavior**: Response patterns in standard scenarios

### 2. Backward Compatibility

Version B is considered backward compatible with version A if:

- All valid inputs from version A produce **semantically equivalent** outputs in version B
- Version B's output format contains all required fields from version A
- Version B does not violate any core constraints declared in version A

---

## PromptVer Specification

### Rule 1: Behavioral Contract Declaration

Prompts using PromptVer **MUST** declare their behavioral contract. The contract can be defined through:

- Structured metadata file (recommended)
- Header comments in the Prompt file
- Separate documentation

**Example metadata:**

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
    language: "en"
    tone: "professional"
    max_length: 500
```

### Rule 2: Version Number Format

Normal version numbers **MUST** take the form `MAJOR.MINOR.PATCH` where:

- All three numbers are non-negative integers
- Must not contain leading zeros
- Each element must increment numerically

Example: `1.0.0` → `1.1.0` → `1.1.1` → `2.0.0`

### Rule 3: Initial Development Phase

- Major version zero (`0.y.z`) indicates initial development
- Anything may change at any time during this phase
- The behavioral contract should not be considered stable

### Rule 4: Stable Version Release

Version `1.0.0` defines the stable behavioral contract. Release conditions:

- Prompt is being used in production
- Behavioral contract is clearly defined
- Team commits to maintaining backward compatibility

### Rule 5: PATCH Version Increment

Increment PATCH version (`x.y.Z`) when making **backward compatible optimizations or fixes**:

- ✅ Fix typos or grammatical errors
- ✅ Improve instruction wording for clarity (without changing behavior)
- ✅ Adjust example data (without changing functionality)
- ✅ Performance optimization (e.g., reduce token usage)

**Example:**
```
v1.2.0 → v1.2.1
Change: Fixed spelling of "您的" to "你的" (unified wording)
```

### Rule 6: MINOR Version Increment

Increment MINOR version (`x.Y.z`) when **adding functionality in a backward compatible manner**:

- ✅ Add new use cases or functional modules
- ✅ Extend output format (add optional fields)
- ✅ Add new examples or edge case handling
- ✅ Mark a feature as deprecated (but still retain it)

MINOR version increment **MUST** reset PATCH version to 0.

**Example:**
```
v1.2.3 → v1.3.0
Change: Added refund request handling scenario
Contract: Maintains existing customer service functionality
```

### Rule 7: MAJOR Version Increment

Increment MAJOR version (`X.y.z`) when making **backward incompatible changes**:

- ⚠️ Change output format structure
- ⚠️ Modify core functionality definition or role setting
- ⚠️ Remove existing functionality or fields
- ⚠️ Modify core constraints (e.g., change language, tone)

MAJOR version increment **MUST** reset MINOR and PATCH to 0.

**Example:**
```
v1.3.2 → v2.0.0
Change: Output changed from plain text to JSON format
Impact: All callers need to modify parsing logic
```

### Rule 8: Model Identifier (Optional)

A model identifier can be appended after the version number using `@model-name`:

```
v1.2.3@gpt-4-turbo
v1.2.3@claude-sonnet-4
v2.0.0@gemini-pro
```

**Purpose:**
- Indicate the model the Prompt was tested and optimized for
- Warn about cross-model compatibility issues

### Rule 9: Pre-release Versions

Pre-release versions are denoted by appending `-` and identifiers after PATCH:

```
v1.0.0-alpha        # Internal testing
v1.0.0-alpha.1      # First round of fixes
v1.0.0-beta         # Public testing
v1.0.0-beta.2       # Second round of optimization
v1.0.0-rc.1         # Release candidate
v1.0.0              # Official release
```

**Rules:**
- Identifiers may only contain `[0-9A-Za-z-]`
- Pre-release versions have lower precedence than normal versions
- Example: `1.0.0-alpha < 1.0.0`

### Rule 10: Build Metadata

Build metadata is appended using `+` to denote non-functional information:

```
v1.2.3+20251005              # Build date
v1.2.3+customer-service      # Functional identifier
v1.2.3-beta+exp.sha.a1b2c3   # Experimental version + Git hash
```

**Note:** Build metadata does not affect version precedence.

---

## Best Practices

### When to Increment MAJOR Version?

✅ **Should increment:**
- Change required fields in JSON schema
- Change output language from Chinese to English
- Remove existing functional modules

❌ **Should not increment:**
- Add new optional JSON fields
- Internal implementation optimization (user-imperceptible)
- Update example data

### Rapid Iteration Strategy

Experiment rapidly in the `0.x.x` phase:

```
v0.1.0  Initial version
v0.2.0  Daily rapid iterations
v0.3.0  No compatibility guarantee
...
v1.0.0  Release after stabilization
```

### Deprecation Process

Standard process for marking features as deprecated:

```
v1.5.0  Add new feature X, mark old feature Y as deprecated
v1.6.0  Continue to retain feature Y, warn again
v2.0.0  Remove feature Y
```

---

## Version Validation Regex

Complete PromptVer format (with optional model identifier):

```regex
^(0|[1-9]\d*)\.(0|[1-9]\d*)\.(0|[1-9]\d*)(?:-((?:0|[1-9]\d*|\d*[a-zA-Z-][0-9a-zA-Z-]*)(?:\.(?:0|[1-9]\d*|\d*[a-zA-Z-][0-9a-zA-Z-]*))*))?(?:\+([0-9a-zA-Z-]+(?:\.[0-9a-zA-Z-]+)*))?(?:@([a-z0-9-]+))?$
```

**Matching examples:**
- `1.2.3` ✅
- `1.2.3-alpha` ✅
- `1.2.3+20251005` ✅
- `1.2.3@gpt-4` ✅
- `1.2.3-beta+build@claude` ✅
- `v1.2.3` ❌ (no v prefix)

---

## Adopting PromptVer

If your project adopts this specification, please add a badge in your README:

```markdown
[![PromptVer](https://img.shields.io/badge/PromptVer-1.0.0-blue.svg)](https://github.com/langgptai/promptver)
```

And note in the Prompt file header:

```
# Customer Service Prompt
# Version: 1.2.3
# PromptVer: 1.0.0
# License: MIT
```

---

## Summary

PromptVer provides a clear, predictable Prompt versioning specification that helps teams:

- ✅ Clearly understand compatibility relationships between versions
- ✅ Safely upgrade and rollback Prompts
- ✅ Effectively collaborate and transfer knowledge
- ✅ Reduce integration risks

**Start using PromptVer today to make your Prompt version management more professional!**

---

## License

This specification is licensed under CC BY 4.0.

**Specification Version:** PromptVer 1.0.0  
**Release Date:** 2025-10-05  
**Maintainer:** LangGPT Community