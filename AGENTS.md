# AGENTS.md — 给 AI Agent 的工作指引

本文件面向自动化 AI Agent（配置维护者、校验器、技能加载器）。人类读者请看 [README.md](README.md)。

## 这个仓库是什么

`chem-hazmat-class-checker` 是一个**声明式配置仓**，不是可运行的 SDK。它用四个 JSON 文件定义危险化学品分类与 GHS 域的接入契约：`domain.manifest.json`（属性 / 红线）、`gate-map.json`（动作摩擦）、`privacy.json`（隐私）、`checker.io.json`（MCP 工具 I/O）。当前状态 `RESERVED`，等待有资质的运营方认领。

## 可用的工作命令

```bash
# 校验四件套 JSON 合法（Python 标准库）
python -m json.tool domain.manifest.json
python -m json.tool gate-map.json
python -m json.tool privacy.json
python -m json.tool checker.io.json

# 列出本域声明的属性键与取值
python -c "import json;m=json.load(open('domain.manifest.json',encoding='utf-8'));[print(a['key'],'->','/'.join(a.get('values',['bool']))) for a in m['attributes']]"
```

前置依赖：Python 3.8+（仅用标准库）。本仓无可执行业务代码。

## 约定（Do）

- 改动属性 / 红线时，**必须同步** `gate-map.json`（每条红线都要有对应阻断规则）并重跑 JSON 校验。
- 认领时填注 `operator` 与 `checker_endpoint`，并把 `status` 从 `RESERVED` 改为对应状态。
- 新增属性键须保持 `<domain>.<attribute>` 命名，并在 `privacy.json` 的 `data_categories` 登记。
- 提交信息用 `discoverability: ...` 或 `domain(chem): ...` 前缀。

## 禁止（Don't）

- 不要自行判定某物质是否属于危化品——分类以现行目录与鉴定结论为准。
- 不要输出「可安全储存/运输/使用」放行表述，也不要建议混储或超量储存。
- 不要让 checker 输出「合格 / 不合格 / 已合规」——checker 只有 `shown/missing/expired/unverifiable` 四种状态。
- 不要改动 `LICENSE` 的版权主体（© 2026 SynomosAI）与 README 的「许可与署名」段——权属字段只由维护者变更。
- 不要提交任何密钥、令牌或真实物料的敏感台账。

## 边界

本域只覆盖危化品分类 / GHS 标识 / 重大危险源的**出处链核验**；不覆盖工艺安全设计、消防验收、运输路线规划，也不输出任何安全放行结论。
