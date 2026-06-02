# 当前项目简介

基于 `danny-avila/LibreChat` 进行公司内部定制改造的聊天工作室项目。

当前项目采用“外层项目管理工作区 + 内层源码目录”的结构：外层用于管理项目说明、发布记录、参考资料、脚本和版本信息；上游源码完整放在 `src/libre-chat/` 内，尽量保持 LibreChat 原项目结构不被打散。

# 技术栈

- npm workspaces + Turborepo
- 后端：Node.js、Express、Mongoose、Passport、Jest
- 前端：React 18、TypeScript、Vite、React Router、React Query、Tailwind CSS
- 共享包：`packages/api`、`packages/data-schemas`、`packages/data-provider`、`packages/client`
- 主数据库：MongoDB
- 搜索：Meilisearch
- RAG：RAG API + PostgreSQL/pgvector
- 可选缓存和会话存储：Redis

# 上游同步记录

- 上游项目：`danny-avila/LibreChat`
- 上游仓库：`https://github.com/danny-avila/LibreChat`
- 上游本地参考路径：`/Users/hzk/Documents/Github-git/0_AI_聊天编程工具/LibreChat - 增强版ChatGPT克隆 - 37.6K`
- 上游已同步到：`v0.8.6` / `268f095c1` / `🔒 feat: Add On-Behalf-Of (OBO) token exchange support for MCP Servers (#13429)`
- 同步方式：按上游新增 commit 逐条手动检查和改写，保留本项目自定义改造；不 cherry-pick、不 merge、不整包覆盖。
- 记录口径：`上游已同步到` 表示当前内层源码已经同步到的上游位置；后续继续同步时，必须先更新上游本地参考仓库，再逐条检查上游新增 commit，处理完成后再更新此记录。

# 目录结构

```plaintext
libre-chat/
├── .claude -> /Users/hzk/Documents/HZK-git/HZK-Daily/.claude
├── .delete/                         # 本地备份目录，不提交
├── 0_Doc/                           # 项目文档
├── 0_Reference/                     # 参考资料
├── 0_Backup/                        # 备份文件
├── 0_Release/                       # 发布产物
├── 1_Script/                        # 辅助脚本
├── assets/                          # 静态资源
├── CLAUDE.md                        # 当前项目说明与开发规范入口
├── CHANGELOG.md                     # 当前项目更新记录
├── README.md
├── VERSION                          # 当前项目版本号
└── src/
    └── libre-chat/       # LibreChat 上游源码和公司内部定制源码
        ├── api/                     # Express 后端服务
        ├── client/                  # React + Vite 前端应用
        ├── packages/                # 共享包和 TypeScript 后端包
        │   ├── api/                 # 新后端 TypeScript 代码
        │   ├── client/              # 前端共享工具
        │   ├── data-provider/       # 前后端共享 API 类型、接口和数据服务
        │   └── data-schemas/        # 数据库模型和 schema
        ├── e2e/                     # Playwright 端到端测试
        ├── config/                  # 项目脚本和维护脚本
        ├── docker-compose.yml       # Docker 调试和部署服务
        ├── .env.example             # 环境变量示例
        └── package.json
```

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

# 本地开发调试

- 后端默认地址：`http://localhost:3080/`
- 前端开发服务默认地址：`http://localhost:3090/`
- `npm run backend` 启动后端服务。
- `npm run backend:dev` 使用 nodemon 启动后端开发监听。
- `npm run frontend:dev` 启动前端 Vite 开发服务，需要后端同时运行。
- 修改 `packages/data-provider`、`packages/data-schemas`、`packages/api` 等共享包后，按影响范围运行对应 build；最常用的是 `npm run build:data-provider`。
- 本地环境变量以内层源码目录的 `.env` 为准，可从 `.env.example` 复制后按需修改。

## 本地数据服务

- 主业务数据库是 MongoDB，默认连接 `MONGO_URI=mongodb://127.0.0.1:27017/LibreChat`。
- Docker Compose 中 MongoDB 服务名为 `mongodb`，容器内连接 `mongodb://mongodb:27017/LibreChat`。
- Meilisearch 用于搜索，默认端口 `7700`，通过 `MEILI_HOST` 和 `MEILI_MASTER_KEY` 配置。
- RAG 依赖 `rag_api` 和 PostgreSQL/pgvector，Docker Compose 中 PostgreSQL 服务名为 `vectordb`。
- Redis 是可选项，启用时配置 `USE_REDIS=true` 和 `REDIS_URI`。
- 本地全量调试可优先用 Docker Compose 跑 MongoDB、Meilisearch、PostgreSQL/pgvector、RAG API，再用源码命令跑前后端，避免每次重新构建应用镜像。

## 登录和认证

- 默认支持邮箱密码登录和注册，配置项为 `ALLOW_EMAIL_LOGIN`、`ALLOW_REGISTRATION`、`ALLOW_UNVERIFIED_EMAIL_LOGIN`。
- 支持 LDAP 登录；配置 `LDAP_URL` 和 `LDAP_USER_SEARCH_BASE` 后，登录路由会改走 LDAP 认证。
- 支持 Google、GitHub、Discord、Facebook、Apple 社交登录，按对应 `*_CLIENT_ID` 和 `*_CLIENT_SECRET` 是否存在决定是否启用。
- 支持 OpenID Connect、SAML 和 2FA。
- LibreChat 本地认证使用短期 JWT + refresh token。`SESSION_EXPIRY` 控制短期 token 有效期，`REFRESH_TOKEN_EXPIRY` 控制 refresh token 有效期。
- OpenID 可通过 `OPENID_REUSE_TOKENS` 复用身份提供方 token，并将 OpenID token 保存在服务端 session 中，避免 cookie 过大。

# Git 服务器

- 平台：Gitea（https://git.libre.cn）
- 用户名：TonyHZK
- API 格式：Gitea v1（`https://git.libre.cn/api/v1/...`）
- 认证方式：`Authorization: token <token>`
- Token：存放在 `local.env`，不提交到 Git

# 工作习惯

- 当前外层目录用于管理完整项目；内层源码目录默认不是独立 Git 仓库。
- 后续如需重新初始化 Git，默认在外层目录初始化。
- 开发命令通常在 `src/libre-chat/` 内执行。
- Git 状态、提交、发布产物和项目级说明按外层工作区处理。
- 如果命令需要在外层根目录执行，文件路径要写 `src/libre-chat/...`；如果命令已经在 `src/libre-chat/` 内执行，文件路径按 LibreChat 原项目根目录书写。
- 同步上游前，先更新上游本地参考仓库，再处理当前项目。
- 每次同步上游后，及时更新本文件的“上游同步记录”。
- 每次修改版本号、发布产物或用户可见内容时，同步更新 `VERSION`、`README.md` 和 `CHANGELOG.md`。
- 用户使用语音输入，英文单词可能识别不准确，需要结合上下文理解。

# LibreChat 开发规范

- 所有新的后端 TypeScript 代码优先放在 `src/libre-chat/packages/api/`。
- `src/libre-chat/api/` 保持为薄 JS 后端入口，尽量只做路由、控制器和对 TypeScript 包的调用。
- 数据库模型和 schema 放在 `src/libre-chat/packages/data-schemas/`。
- 前后端共享 API 类型、接口地址和 data service 放在 `src/libre-chat/packages/data-provider/`。
- 前端用户可见文案必须使用本地化 key，英文 key 在 `src/libre-chat/client/src/locales/en/translation.json` 中维护。
- 前端 API 调用走 React Query，查询 key 和 mutation key 统一维护在 `packages/data-provider/src/keys.ts`。
- 测试框架为 Jest；MongoDB 相关测试优先使用 `mongodb-memory-server`。
- 修改后按影响范围运行最小验证，不要直接跑完整测试套件。

# 必读文档

- `src/libre-chat/CLAUDE.md` — LibreChat 上游项目说明和源码工作规范
- `README.md`
- `CHANGELOG.md`
