# chem-hazmat-class-checker

> **状态：RESERVED（占位 · 开放认领）** — 本仓已按平台协议规范建好行业接入四件套骨架，
> 等待具备本域资质的运营方认领并填充真实规则。

把「这个化学品属哪类危化品、标识和存储到不到位」拆成可核验的属性，让 AI 只做提示，不下安全结论。

## 这个域管什么

危险化学品分类、GHS 标识与重大危险源核验

危险化学品的分类、标识、储存、运输各有强制要求，重大危险源还需专门辨识。AI 能做的是把目录归属与配置要求摆清楚，判定权在鉴定机构与监管部门。

## 域标识

| 项 | 值 |
|---|---|
| 域 ID | `chem`（全局唯一，一经分配不复用） |
| 域名称 | 化工 · 危险化学品分类与 GHS |
| Profile 版本 | `domain/1.0` |
| 当前状态 | `RESERVED` |
| 占位时间 | 2026-09-12 |

## 属性清单

| 属性键 | 类型 | 说明 |
|---|---|---|
| `chem.hazmat_class` | enum | 危险化学品种类，以现行目录与鉴定结论为准 · 取值 explosive/flammable_gas/flammable_liquid/toxic/corrosive/oxidizer/radioactive/other/not_hazmat |
| `chem.ghs_labeling` | enum | GHS 标签与安全数据表配备情况 · 取值 compliant/incomplete/missing/not_applicable |
| `chem.sds_available` | boolean | 是否可提供现行版安全技术说明书 |
| `chem.storage_compliance` | enum | 储存条件与禁配物质隔离情况 · 取值 compliant/segregation_required/violation/unknown |
| `chem.major_hazard_source` | enum | 是否构成重大危险源，须按标准辨识 · 取值 yes/no/unidentified |

## 本域红线（不可逾越，机器可读）

1. 危险化学品分类以现行目录与鉴定结论为准，AI 不得自行判定物质是否属于危化品
2. 不得输出「可安全储存/运输/使用」的放行类表述，安全条件须由具备资质机构评估
3. 不得建议混储、超量储存或简化安全设施

> 红线在 `gate-map.json` 中均有对应阻断规则。平台校验器会检查「每条红线都有规则覆盖」，
> 缺失即校验失败——**制度与系统不允许不同步**。

## 行业接入四件套

| 文件 | 作用 |
|---|---|
| `domain.manifest.json` | 本域声明：属性清单、签发方要求、有效期、红线 |
| `gate-map.json` | 本域「什么动作要多少摩擦」：silent / warn / confirm / block / require-owner |
| `privacy.json` | 本域隐私声明：默认关闭、最小必要、可撤回、可删除 |
| `checker` | 本域核验器（MCP 工具，**只出示核验，不下判定**） |

## 核心原则

**平台只当擂台，不当货架。** 本域的核验器只回答「这条声明是否可核验、缺什么要件」，
不回答「这件事是否合规、该不该做」。判定权在本域的资质方、监管方与人。

**隐私是准入条件，不是整改事项。** 缺失 `privacy.json` 或任一必填字段不符，
符合性校验直接失败——不是警告，是拒绝接入。

## 参考依据

- 危险化学品安全管理条例
- GB 30000 化学品分类和标签规范（GHS）
- GB 18218 危险化学品重大危险源辨识
- 危险化学品目录

> 上列依据仅用于说明本域属性的来源与口径，不构成法律意见。具体适用以现行有效文本与主管部门解释为准。

## 认领方式

本域面向具备相应资质的机构开放。认领后请：

1. Fork 本仓，填注 `operator` 与 `checker_endpoint`
2. 按本域现行有效规则校准属性取值与红线表述
3. 跑平台侧校验器自测（五项判据全过方可提交）
4. 提 PR，附资质证明与规则依据

## 许可与署名

代码与配置按 MIT 许可使用。文档的知识版权归 SynomosAI 所有。

© 2026 SynomosAI. All rights reserved.
