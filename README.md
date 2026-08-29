# WhiteStarUML Analysis Diagrams Skill

这个 Codex skill 用于根据用例说明生成、修改和检查 UML sequence diagram 与 analysis class diagram，重点支持 WhiteStarUML 的可编辑 `.uml`（XPD）文件。

## 功能

- 从用例流程推导 actor、boundary、control、entity 的 sequence diagram。
- 根据 sequence diagram 推导 per-use-case class diagram，并保持方法签名、属性和关系一致。
- 支持抽象类与子类的多态建模。
- 生成和检查 WhiteStarUML 的 UMLActor、lifeline、message、class、association、dependency、realization 与 generalization 结构。
- 提供独立的 XPD 结构校验规则：GUID、引用、集合数量、关系端点、`#Views`、`#Connections` 和关联标签均可在没有现成 `.uml` 文件的情况下检查。
- 默认 sequence diagram 使用深红色线条与黑色文字；class diagram 的所有 class 框使用统一浅黄色。

## 文件说明

- `SKILL.md`：建模、命名、关系选择与一致性规则。
- `references/whitestar-xpd.md`：WhiteStarUML XPD 的结构规范和验证清单。
- `references/whitestar-xpd-structural-reference.uml`：不包含项目业务内容的通用结构示例，可在排查 XPD 层级或字段位置时参考。

## 安装

将整个 `uml-analysis-diagrams` 文件夹放到 Codex skills 目录：

```text
C:\Users\<用户名>\.codex\skills\uml-analysis-diagrams
```

之后可在 Codex 中请求生成、修改或检查 UML sequence/class diagram，或直接指出需要编辑 WhiteStarUML `.uml` 文件。

## 注意

通用 `.uml` 示例只用于理解文件结构。生成新文件时必须使用新的 GUID，并按照 XPD 规范建立模型、视图和反向引用；不要复制示例中的 GUID 或业务内容。
