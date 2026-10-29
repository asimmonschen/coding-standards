# 来源与取舍

整理日期：2026-10-29。版本：1.0.0。公开网页与仓库链接为本次查阅入口，不代表固定提交快照或企业现行内部制度。

以下是公开的一手资料；正文是针对跨项目使用重新组织的中文规则与原创示例，不是任何公司的官方规范全文。国内安全指南与国外风格指南承担不同作用，不能把安全建议误称为某公司的完整语言风格标准。

| 来源 | 本 Skill 参考的范围 | 对应规则组 |
|---|---|---|
| [Google Go Style](https://google.github.io/styleguide/go/) | 可读性、命名及规则与建议的区分 | GO-STYLE、GO-PKG |
| [Uber Go Style Guide](https://github.com/uber-go/guide/blob/master/style.md) | 接口、错误、初始化、共享状态与 goroutine 生命周期 | GO-TYPE、GO-ERR、GO-CONC |
| [腾讯 Go 安全指南](https://github.com/Tencent/secguide/blob/main/Go%E5%AE%89%E5%85%A8%E6%8C%87%E5%8D%97.md) | 输入、注入、文件、网络和敏感数据风险 | GO-SEC |
| [Effective Go](https://go.dev/doc/effective_go) 与 [Go 1.22 release notes](https://go.dev/doc/go1.22) | 语言惯用法与循环语义的版本约束 | GO-TYPE、GO-CONC |

## 冲突处理与本地综合

- Google 的 guide、decisions 与 best-practices 自身具有不同约束等级；此处不把建议全部变成硬门禁。
- 不因 Uber 使用某个日志、原子或测试库，就要求项目安装相同依赖；标准库与现有成熟方案优先。
- Effective Go 不是完整的现代工程手册；新语法、标准库与循环语义还需目标版本资料。不能只看本机编译器版本忽略 Module 的最低支持版本。
- context、事务幂等、验证范围和渐进适配是本地综合规则；保留现有协议与错误契约。

## 用户提供的结构参考

参考用户提供的 `E:/workspace-front/27w-ea-cover-4/game-api-27w/.agents/skills/golang-expert-zh/` 的入口、主题路由与项目适配结构。未搬入其游戏业务常量、仓库专属命令、规格审批门禁或上游文件；该路径仅作来源记录，运行本 Skill 不需要访问它。

## 更新与署名

不把旧资料中的具体依赖版本、加密参数、废弃 API 或历史工具配置照搬到项目；采用前核对目标工具链及官方文档。本文未附带上游手册、配置包或代码文件，不宣称获得其商标或版权授权。若后续直接复制或改编上游文字、配置、代码，应核对其许可证并保留要求的署名和许可文件；腾讯 secguide 标注为 CC BY 4.0，保留来源与改写说明。

规则语义变化时更新版本并记录原因；已有编号不改作另一种含义。纯风格取舍与本地综合要求不应宣传为“所有大公司共同强制”。本 Skill 的项目适配和交付方式是本地综合设计。
