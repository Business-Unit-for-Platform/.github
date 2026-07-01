# 工程平台 / Platform

公司内部通用工程底座、RuoYi/Yudao、代码生成、clone-bot 与交付模板。

> 打开本组织时，先看这条主线：**Clone-Bot 负责生成/更新平台底座，codegen-bot 负责把业务 SQL/模型生成业务模块。**

## Repository Map

| Repository | Visibility | Purpose |
|---|---:|---|
| [`Clone-ruoyi-vue-pro-Bot`](https://github.com/Business-Unit-for-Platform/Clone-ruoyi-vue-pro-Bot) | public | 后端/全栈 RuoYi-Vue-Pro / Yudao 底座克隆、重建和结构化改造工具。 |
| [`Clone-ruoyi-ui-admin-vue3-Bot`](https://github.com/Business-Unit-for-Platform/Clone-ruoyi-ui-admin-vue3-Bot) | public | `yudao-ui-admin-vue3` 管理后台前端底座克隆工具。 |
| [`Clone-yudao-mall-uniapp-Bot`](https://github.com/Business-Unit-for-Platform/Clone-yudao-mall-uniapp-Bot) | public | `yudao-mall-uniapp` 移动端/商城前端底座克隆工具。 |
| [`codegen-bot`](https://github.com/Business-Unit-for-Platform/codegen-bot) | public | 基于业务 SQL / 数据模型生成后端模块与 Vue admin 页面，默认应走 branch + PR。 |
| [`future-vue-pro`](https://github.com/Business-Unit-for-Platform/future-vue-pro) | public | 从 Gitee `ruoyi-vue-pro` 同步/改造出的后端平台底座，采用 future layout。 |
| [`future-ui-admin-vue3`](https://github.com/Business-Unit-for-Platform/future-ui-admin-vue3) | public | 从 Gitee `yudao-ui-admin-vue3` 同步/改造出的管理后台前端底座；部分业务早期新增页面可能暂存在这里。 |
| [`future-mall-uniapp`](https://github.com/Business-Unit-for-Platform/future-mall-uniapp) | public | 从 Gitee `yudao-mall-uniapp` 同步/改造出的移动端/商城前端底座。 |
| [`ruoyi-vue-pro`](https://github.com/Business-Unit-for-Platform/ruoyi-vue-pro) | public | RuoYi/Yudao 后端 upstream 同步基线。 |
| [`yudao-ui-admin-vue3`](https://github.com/Business-Unit-for-Platform/yudao-ui-admin-vue3) | public | Yudao Vue3 管理后台 upstream 同步基线。 |
| [`ruoyi-vue-pro-mysql`](https://github.com/Business-Unit-for-Platform/ruoyi-vue-pro-mysql) | private | RuoYi/Yudao MySQL SQL dump / module SQL 资料。 |
| [`.github`](https://github.com/Business-Unit-for-Platform/.github) | public | 组织 profile、仓库地图和平台工具链入口文档。 |

## Platform Toolchain: Clone-Bot then codegen-bot

### One-line rule

```text
requirements / data model / SQL
  -> Clone-Bot 初始化或更新 RuoYi/Yudao 平台底座
  -> codegen-bot 生成业务模块
  -> 生成代码以 branch + PR 进入业务仓库
  -> Review / Test / Merge
  -> 独立部署仓库发布
```

### Why this order

| Step | Tool | Role | Analogy |
|---|---|---|---|
| 1 | `Clone-ruoyi-vue-pro-Bot` / frontend clone bots | 初始化、同步或重建平台底座 | 建房子 / 搭框架 |
| 2 | `codegen-bot` | 根据业务 SQL/数据模型生成业务模块 | 装修房间 / 生成业务功能 |
| 3 | PR review | 审查权限、API、数据边界、测试 | 验房 |
| 4 | deploy repo | 发布构建产物 | 交付入住 |

Clone-Bot 和 codegen-bot **不要过早合并**：

- Clone-Bot 的 `rebuild_from_upstream` 模式可能删除/重建目标底座仓库，用于重新跟上最新 upstream/template。
- codegen-bot 面向长期业务仓库时，应默认保留历史，通过 branch + PR 合入。
- 两者生命周期、风险和权限边界不同，先保持独立工具仓库。

## Operating Modes

### Mode A: Build or refresh base repo

用于：新业务第一次建立 RuoYi/Yudao 底座，或明确要从最新 upstream/template 重建底座。

```text
Clone-ruoyi-vue-pro-Bot
  input: target_org / backend_repo / branch / layout_mode / mode
  output: future-vue-pro-style backend repo
```

注意：

- `rebuild_from_upstream` 可以是破坏性模式，只用于底座/可重建仓库。
- 对长期业务仓库，不要默认删除重建。
- frontend clone bots 负责 UI admin / uniapp 等前端底座。

### Mode B: Generate business module into an existing repo

用于：业务需求已经有 SQL/数据模型，需要生成后端模块和管理后台页面。

```text
codegen-bot
  input: business SQL / module name / table prefix / target backend repo / target frontend repo / layout_mode
  output: generated backend modules + frontend pages + manifest/report
```

规则：

- 长期业务仓库默认：branch + PR。
- 直接推 main 只适合 disposable demo 或用户明确要求的可重建底座。
- 业务 SQL 最好来自业务 `requirements` 或业务代码仓库，不要永久复制进工具仓库。

## Layouts and Generated Shape

### future-vue-pro base shape

```text
apps/future-server
platform/future-dependencies
platform/future-framework
modules/core/{system,infra}
modules/biz/{crm,erp,mall}
modules/extend/{ai,bpm,iot,member,mp,pay,report}
modules/custom/<module>
```

### codegen-bot split output

`codegen-bot` 支持 api/biz split：

```text
modules/custom/<module>/future-module-<module>-api
modules/custom/<module>/future-module-<module>-biz
src/api/<module>
src/views/<module>
```

核心规则：

- Java package 中 `api`、`enums` 进入 api module。
- controller、service、dal、convert、job、listener、framework 等进入 biz module。
- biz module 依赖 api module。
- server 只依赖 biz module。
- 生成结果必须带 `generated/manifest.json` 和 `generated/codegen-report.md` 或等价文档。

## What to Record Per Generation

每次 Clone/codegen 都应记录：

```text
source repo / branch / commit
generator repo / commit
SQL file list
business table list
module names and table prefixes
target org/repo/branch
layout mode
output file list
created PR link
human/agent review checklist
```

## Review Checklist for Generated Code

- 是否暴露了不该公开的 admin 写接口？
- 用户、租户、数据权限边界是否明确？
- POM 聚合、模块依赖、server dependency 是否正确？
- 前端路由、菜单、权限码是否需要人工配置？
- README/docs 是否说明 generated origin 和下一步审查？
- 应用仓库是否只负责构建/测试/打包，部署是否仍在独立 deploy repo？

## Governance

- 具体业务代码、部署和数据资产留在对应 BU。
- 业务需求源头放在对应 BU 的 `requirements` 仓库；Platform 只沉淀通用底座和生成工具规则。
- 跨 BU 的战略、组合、制度和决策进入 `President-Office`。
- 平台底座、clone-bot、codegen-bot、通用模板进入 `Business-Unit-for-Platform`。
- 详细平台方法论仍可在 `President-Office/ai-project-operating-system` 沉淀，但 Platform 组织必须保留可直接阅读的操作入口，避免只打开 Platform 看不懂调用顺序。
- 生产部署默认按多机模式设计，数据库/缓存不以 `127.0.0.1` 作为生产默认。

## Related Methodology

- `President-Office/ai-project-operating-system`: AI 项目操作系统、Spec、ADR、Harness、平台化方法论。
- `Business-Unit-for-Platform/codegen-bot`: 代码生成实现和 workflow。
- `Business-Unit-for-Platform/Clone-ruoyi-vue-pro-Bot`: RuoYi/Yudao 后端底座克隆/重建工具。

---

_Last updated: 2026-07-01_
