# 当前项目简介

基于 `danny-avila/LibreChat` 进行公司内部定制改造的聊天工作室项目。

当前项目采用“外层项目管理工作区 + 内层源码目录”的结构：外层用于管理项目说明、发布记录、参考资料、脚本和版本信息；上游源码完整放在 `src/libre-chat/` 内，尽量保持 LibreChat 原项目结构不被打散。

# 上游同步记录

- 上游项目：`danny-avila/LibreChat`
- 上游仓库：`https://github.com/danny-avila/LibreChat`
- 上游本地参考路径：`/Users/hzk/Documents/Github-git/0_AI_聊天编程工具/LibreChat - 增强版ChatGPT克隆 - 37.6K`
- 上游已同步到：`v0.8.6` / `268f095c1` / `🔒 feat: Add On-Behalf-Of (OBO) token exchange support for MCP Servers (#13429)`
- 同步方式：按上游新增 commit 逐条手动检查和改写，保留本项目自定义改造；不 cherry-pick、不 merge、不整包覆盖。
- 记录口径：`上游已同步到` 表示当前内层源码已经同步到的上游位置；后续继续同步时，必须先更新上游本地参考仓库，再逐条检查上游新增 commit，处理完成后再更新此记录。

# 目录结构

- `src/libre-chat/` — LibreChat 上游源码和公司内部定制源码
- `0_Doc/` — 项目文档
- `0_Reference/` — 参考资料
- `0_Backup/` — 备份文件
- `0_Release/` — 发布产物
- `1_Script/` — 辅助脚本
- `assets/` — 静态资源
- `CHANGELOG.md` — 当前项目更新记录
- `VERSION` — 当前项目版本号

# 开发入口

开发、安装依赖、运行脚本、测试和构建时，进入内层源码目录：

```bash
cd src/libre-chat
```

常用命令：

```bash
npm run smart-reinstall
npm run backend
npm run frontend:dev
npm run build:data-provider
```

# Git 服务器

- 平台：Gitea（https://git.libre.cn）
- 用户名：TonyHZK
- API 格式：Gitea v1（`https://git.libre.cn/api/v1/...`）
- 认证方式：`Authorization: token <token>`
- Token：存放在 `local.env`，不提交到 Git

# 习惯

- 当前外层目录用于管理完整项目；内层源码目录默认不是独立 Git 仓库。
- 后续如需重新初始化 Git，默认在外层目录初始化。
- 开发命令通常在 `src/libre-chat/` 内执行。
- Git 状态、提交、发布产物和项目级说明按外层工作区处理。
- 同步上游前，先更新上游本地参考仓库，再处理当前项目。
- 每次同步上游后，及时更新本文件的“上游同步记录”。
- 每次修改版本号、发布产物或用户可见内容时，同步更新 `VERSION`、`README.md` 和 `CHANGELOG.md`。
- 用户使用语音输入，英文单词可能识别不准确，需要结合上下文理解。

# 必读文档

- `src/libre-chat/CLAUDE.md` — LibreChat 上游项目说明和源码工作规范
