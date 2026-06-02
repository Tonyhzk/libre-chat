# libre-chat

基于 `danny-avila/LibreChat` 进行公司内部定制改造的聊天工作室项目。

项目采用“外层项目管理工作区 + 内层源码目录”结构。LibreChat 上游源码完整放在 `src/libre-chat/` 内，外层用于管理项目文档、版本、发布产物、参考资料和辅助脚本。

## 开发入口

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

## 上游同步

- 上游项目：`danny-avila/LibreChat`
- 上游本地参考路径：`/Users/hzk/Documents/Github-git/0_AI_聊天编程工具/LibreChat - 增强版ChatGPT克隆 - 37.6K`
- 当前已同步到：`v0.8.6` / `268f095c1`

详细维护规则见 `CLAUDE.md`。
