# 1. 环境准备与运行项目

本阶段的目标是完成本地构建、打开 Web UI，并能查看应用实际加载的插件配置。

## 1.1 确认环境

仓库要求 Node.js `^22.19.0 || >=24.0.0`，并通过 `package.json` 固定 pnpm 版本。完整要求见[开发指南](../docs/development.zh.md#设置教程)。

```bash
cd /home/gitroot/deepseek-harness
node --version
pnpm --version
git --version
```

如果 pnpm 无法通过 Corepack 使用，执行：

```bash
corepack enable
```

## 1.2 安装并验证

```bash
pnpm install
pnpm run typecheck
pnpm run build
```

验收结果是三个命令都以退出码 `0` 完成。不要只依据终端最后几行判断成功。

## 1.3 查看真实插件配置

```bash
pnpm dsh --profile web --dump-config > /tmp/dsh-web-config.yml
less /tmp/dsh-web-config.yml
```

在文件中选择一个带 `id` 和 `name` 的配置项，找到它对应的 package。后续学习始终以这个组合结果为运行时事实。

## 1.4 启动 Web

```bash
pnpm dsh web
```

浏览器访问 `http://127.0.0.1:3080`。Web 服务可以在没有模型密钥时启动，但真实模型对话需要在仓库根目录的 `.env` 中设置 `DEEPSEEK_API_KEY`；不要提交 `.env` 或密钥。

## 1.5 同步官方代码

当前仓库使用个人仓库作为 `origin`，官方仓库作为 `upstream`。需要更新个人 `master` 时使用：

```bash
git fetch upstream
git switch master
git merge --ff-only upstream/master
git push origin master
```

如果快进失败，先检查双方提交关系，不要使用强制推送覆盖个人仓库。

## 验收标准

- Web UI 可以打开。
- 能说明 `web` 是一个 Profile，而不是单个 Web 进程插件。
- 能导出并查阅最终插件配置。
- 能区分个人远端 `origin` 和官方远端 `upstream`。
