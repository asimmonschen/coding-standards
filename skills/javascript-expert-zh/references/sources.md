# 来源与取舍

整理日期：2026-10-29。版本：1.0.0。公开网页与仓库链接为本次查阅入口，不代表固定提交快照或企业现行内部制度。

以下是公开的一手资料；正文是针对跨项目使用重新组织的中文规则与原创示例，不是任何公司的官方规范全文。国内安全指南与国外风格指南承担不同作用，不能把安全建议误称为某公司的完整语言风格标准。

| 来源 | 本 Skill 参考的范围 | 对应规则组 |
|---|---|---|
| [Airbnb JavaScript Style Guide](https://github.com/airbnb/javascript) | 变量、数组、函数、命名与可读性 | JS-STYLE、JS-VALUE、JS-FUNC |
| [Google TS/JS Style Guide](https://google.github.io/styleguide/tsguide.html) | 模块边界、语言特性与代码清晰性；只采用适用于 JS 的部分 | JS-MOD、JS-FUNC |
| [百度 FEX JavaScript 编码规范](https://github.com/fex-team/styleguide/blob/master/javascript.md) | 中文命名/组织思路、明确区分规则力度 | JS-STYLE、JS-FUNC |
| [腾讯 AlloyTeam ESLint Config](https://github.com/AlloyTeam/eslint-config-alloy) | 逻辑检查与格式化的分工 | JS-STYLE、验证工具选择 |
| [腾讯 JavaScript 安全指南](https://github.com/Tencent/secguide/blob/main/JavaScript%E5%AE%89%E5%85%A8%E6%8C%87%E5%8D%97.md) | DOM、输入、跨源与敏感信息风险 | JS-SEC |

## 冲突处理与本地综合

- 百度文档含较早期 JavaScript 示例；采用其可读性和组织原则，不复制历史 `var`、IIFE 或旧浏览器配置当新项目默认值。
- Airbnb、Google 与国内团队在缩进、导出及某些语法上有不同偏好；既有 formatter 与宿主契约优先，不同时安装多套完整规则强行求交集。
- ESM 仅是符合宿主条件的新项目默认，CJS 不是天然缺陷。本文不强制 default export、类、for 循环或某个框架的全局禁令。
- Promise 生命周期、幂等、数字协议、资源限制与检查分层是本地综合工程规则，不声称某份风格手册逐字规定了这些内容。

## 更新与署名

不把旧资料中的具体依赖版本、加密参数、废弃 API 或历史工具配置照搬到项目；采用前核对目标工具链及官方文档。本文未附带上游手册、配置包或代码文件，不宣称获得其商标或版权授权。若后续直接复制或改编上游文字、配置、代码，应核对其许可证并保留要求的署名和许可文件；腾讯 secguide 标注为 CC BY 4.0，保留来源与改写说明。

规则语义变化时更新版本并记录原因；已有编号不改作另一种含义。纯风格取舍与本地综合要求不应宣传为“所有大公司共同强制”。本 Skill 的项目适配和交付方式是本地综合设计。
