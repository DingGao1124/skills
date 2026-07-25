# Agent Skills

[English](./README.md)

这里收录了两个用于代码仓库重构与应用迁移的 Agent Skills。

## Skills

### `refactor-codebase`

先判断重构属于哪个层级，再加载对应工作流：

- 代码质量重构
- 业务逻辑重构
- 系统架构重构
- 技术栈迁移

适合“重构整个项目”、整理业务规则、调整模块边界、规划技术迁移等任务。它会根据改动影响选择不同的分析、验证、兼容和回滚要求。

[查看 SKILL.md](./refactor-codebase/SKILL.md)

### `migrate-application-to-web`

用于把桌面端、本地优先、CLI 或其他拥有本地系统权限的应用迁移到浏览器架构，尤其适合 Vite/React 前端配合后端 API 的场景。

主要覆盖：

- 将文件系统、进程、凭据和系统调用迁移到后端
- 设计 API Contract、服务端存储和统一错误格式
- 区分复用、迁移、重设计、替换和删除的功能
- 按纵向业务切片逐步迁移
- 处理数据兼容、模型 Provider 替换、切换和回滚

[查看 SKILL.md](./migrate-application-to-web/SKILL.md)

## 安装

仓库上传后，将 `<owner/repo>` 替换为实际的 GitHub 仓库：

```bash
# 查看仓库中的 Skills
npx skills add <owner/repo> --list

# 安装全部 Skills
npx skills add <owner/repo> --all

# 只安装其中一个
npx skills add <owner/repo> --skill refactor-codebase
npx skills add <owner/repo> --skill migrate-application-to-web
```

## 使用示例

```text
使用 $refactor-codebase 分析这个仓库属于哪一种重构，第一轮只输出分析和计划。
```

```text
使用 $migrate-application-to-web 规划把这个桌面应用迁移到 Vite/React + 后端 API。
```

## 目录

```text
skills/
├── README.md
├── README.zh.md
└── skills
    ├── skills-lock.toml
    ├── refactor-codebase/
    │   ├── SKILL.md
    │   ├── agents/
    │   └── references/
    └── migrate-application-to-web/
        ├── SKILL.md
        ├── agents/
        └── references/
```

`skills-lock.toml` 是这个 Skill 包的简要清单，记录名称、用途、入口、资源文件和内容校验值。
