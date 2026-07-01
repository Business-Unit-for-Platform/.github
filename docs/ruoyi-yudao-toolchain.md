# RuoYi/Yudao Clone-Bot and codegen-bot Toolchain

This document explains the detailed call order for Platform organization users.

## Call Order

```text
Business requirements / SQL / data model
  -> Clone-Bot prepares the platform base repo
  -> codegen-bot generates business modules
  -> generated code lands in a branch/PR
  -> review permissions, APIs, data boundaries, tests
  -> merge into the business repository
  -> deploy through an independent deploy repository
```

## Tool Responsibilities

### Clone-Bot

Clone-Bot creates or refreshes platform base repositories.

Relevant repositories:

- `Clone-ruoyi-vue-pro-Bot`
- `Clone-ruoyi-ui-admin-vue3-Bot`
- `Clone-yudao-mall-uniapp-Bot`

Use it when:

- a new business needs a RuoYi/Yudao base;
- the base should be rebuilt from latest upstream/template;
- the target is a generated/base repo where destructive rebuild is explicitly allowed.

Do not use destructive rebuild for long-lived business repos unless explicitly approved.

### codegen-bot

`codegen-bot` generates business modules from SQL/data models.

Use it when:

- the business tables and module boundaries are known;
- backend api/biz modules and admin frontend pages should be generated;
- code should enter the target business repo through branch + PR.

## Modes

| Mode | Use case | Default safety |
|---|---|---|
| `rebuild_from_upstream` | Rebuild disposable/generated base from latest upstream | Can be destructive only when explicitly selected |
| `update_existing_repo_with_pr` | Generate or update module in long-lived repo | Preserve history and use branch + PR |
| `backend-only` | Admin frontend repo is missing or not ready | Generate backend only and record skipped frontend |

## Required Generation Evidence

Each run should produce or update:

- `generated/manifest.json`
- `generated/codegen-report.md`
- PR link or target branch name
- SQL source path and commit
- generator commit
- target repo and layout mode
- review checklist

## Source-of-Truth Split

- Business requirements: target BU `requirements` repo.
- Toolchain and generated layout: this Platform organization.
- Broader AIPOS methodology: `President-Office/ai-project-operating-system`.
- Time priority only: `President-Office/project-time-management`.
