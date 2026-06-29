# 工程平台 / Platform

公司内部通用工程底座、RuoYi/Yudao、代码生成、clone-bot 与交付模板。

## Repository Map

| Repository | Visibility | Purpose |
|---|---:|---|
| [`Clone-ruoyi-ui-admin-vue3-Bot`](https://github.com/Business-Unit-for-Platform/Clone-ruoyi-ui-admin-vue3-Bot) | public | 克隆ruoyi-ui-admin-vue3 |
| [`Clone-ruoyi-vue-pro-Bot`](https://github.com/Business-Unit-for-Platform/Clone-ruoyi-vue-pro-Bot) | public | 利用github actions克隆新项目 |
| [`Clone-yudao-mall-uniapp-Bot`](https://github.com/Business-Unit-for-Platform/Clone-yudao-mall-uniapp-Bot) | public | 克隆yudao-mall-uniapp |
| [`codegen-bot`](https://github.com/Business-Unit-for-Platform/codegen-bot) | public | 代码生成 |
| [`future-mall-uniapp`](https://github.com/Business-Unit-for-Platform/future-mall-uniapp) | public | Init from Gitee yudao-mall-uniapp |
| [`future-ui-admin-vue3`](https://github.com/Business-Unit-for-Platform/future-ui-admin-vue3) | public | Init from Gitee yudao-ui-admin-vue3 |
| [`future-vue-pro`](https://github.com/Business-Unit-for-Platform/future-vue-pro) | public | Init from Gitee ruoyi-vue-pro master-jdk17 |
| [`ruoyi-vue-pro`](https://github.com/Business-Unit-for-Platform/ruoyi-vue-pro) | public | Backend repo synced from Gitee branch master-jdk17 + codegen |
| [`yudao-ui-admin-vue3`](https://github.com/Business-Unit-for-Platform/yudao-ui-admin-vue3) | public | Frontend repo synced from Gitee branch master + codegen |

## Governance

- 具体业务代码、部署和数据资产留在对应 BU。
- 跨 BU 的战略、组合、制度和决策进入 `President-Office`。
- 平台底座和代码生成资产进入 `Business-Unit-for-Platform`。
- 生产部署默认按多机模式设计，数据库/缓存不以 `127.0.0.1` 作为生产默认。

## Naming note

- 组织名当前可用。

---

_Last updated: 2026-06-29_
