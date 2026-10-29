# 多语言编码规范 Skills

面向 TypeScript、Go、C++、JavaScript、Python 的中文编码规范，可供支持 Agent Skills 的编码助手在实现、修改和代码审查时使用。

规则参考 Google、Uber、Airbnb、腾讯、百度等公开资料，并结合语言特性重新整理。每份 Skill 都区分安全与正确性要求、默认风格和可选建议，按目标项目的工具链、现有约定和公开契约适配。

## 语言与入口

| 语言 | Skill | 规则数 | 来源与取舍 |
|---|---|---:|---|
| TypeScript | [typescript-expert-zh](skills/typescript-expert-zh/SKILL.md) | 27 | [说明](skills/typescript-expert-zh/references/sources.md) |
| Go | [golang-expert-zh](skills/golang-expert-zh/SKILL.md) | 26 | [说明](skills/golang-expert-zh/references/sources.md) |
| C++ | [cpp-expert-zh](skills/cpp-expert-zh/SKILL.md) | 28 | [说明](skills/cpp-expert-zh/references/sources.md) |
| JavaScript | [javascript-expert-zh](skills/javascript-expert-zh/SKILL.md) | 27 | [说明](skills/javascript-expert-zh/references/sources.md) |
| Python | [python-expert-zh](skills/python-expert-zh/SKILL.md) | 30 | [说明](skills/python-expert-zh/references/sources.md) |

共 5 个 Skill、138 条编号规则，当前版本均为 `1.0.0`。

## 使用

1. 将需要的语言目录完整复制到目标项目的 `.agents/skills/`，保留其中的 `SKILL.md`、`references/` 和 `agents/`。已有同名 Skill 时，先比较内容再决定如何合并。
2. 确认编码助手支持该目录，并按其机制重新加载 Skill。
3. 在任务中引用对应名称，例如在 Codex 中：

```text
使用 $golang-expert-zh，审查当前改动的错误处理、并发退出和接口兼容性。
```

每个 Skill 可以独立分发，无需同时安装其他语言 Skill。其他支持 Agent Skills 的工具，按其约定放置目录并调用。

## 文件结构

```text
skills/<language>-expert-zh/
├── SKILL.md                    # 适用范围、项目基线与按需加载入口
├── agents/openai.yaml          # Codex 展示信息
└── references/
    ├── standards.md            # 带编号的编码规则
    ├── examples.md             # 场景与易错写法示例
    ├── verification.md         # 验证方法与审查要点
    └── sources.md              # 一手资料、规则取舍与署名说明
```

## 适配原则

- 先确认语言版本、运行时、编译选项及项目检查入口，再应用规则。
- 保留既有业务行为与协议契约；纯风格差异不构成扩大重构范围的理由。
- 只按当前任务加载相关材料，不要求每次联网或读取全部参考文件。
- 检查结果按实际执行范围报告，静态类型、Mock 和格式检查各有边界。

公开来源及取舍记录在各语言的 `references/sources.md`。这些内容是重新组织的实践规则与示例，不代表相关公司的官方规范全文或内部现行制度；引用上游内容时仍须遵守其许可要求。
