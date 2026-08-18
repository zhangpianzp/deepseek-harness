# 6. 测试、调试与持续学习

本阶段建立局部验证和源码阅读习惯。完整规则见[测试策略](../docs/testing.zh.md)。

## 6.1 选择匹配变更范围的检查

```bash
# 运行一个测试文件
pnpm exec vitest run packages/目标目录/tests/目标文件.spec.ts

# 检查类型
pnpm run typecheck

# 运行普通单元测试
pnpm run test

# 验证模型、协议或用户可见输出
pnpm run test:snapshot

# 验证文档
pnpm run doc-sync

# 验证构建产物
pnpm run build
```

不要默认执行整个仓库的所有检查。先运行覆盖修改行为的聚焦测试；只有构建产物、跨 package 类型或最终组合受到影响时，才增加对应检查。

## 6.2 学习测试中的架构

阅读 package 时采用以下顺序：

1. Package README：确认职责、配置、扩展点和限制。
2. `tests/`：确认外部可观察行为和失败条件。
3. `src/index.ts`：找到公开服务或插件入口。
4. 其他 `src/`：只阅读当前场景调用到的实现。
5. Agent Note：只在需要理解设计取舍时阅读。

重点观察：

- 插件挂载和卸载。
- Effect 是否撤销注册。
- 事件顺序和 Waterfall 是否调用 `next()`。
- 取消、超时和错误恢复。
- 配置错误是否尽早失败。
- 测试是否通过真实 Loader 或产品入口验证组合。

## 6.3 日常源码追踪方法

每次只追一个问题，例如“Bash 结果如何进入下一次模型请求”。使用[笔记模板](notes/template.md)记录入口、依赖、事件、状态、清理和测试。

```bash
rg -n "目标事件名|目标方法名" packages apps examples
git log --oneline --all -- 目标路径
git blame -L 起始行,结束行 目标文件
```

`git log` 和 `git blame` 用于定位相关设计记录，不替代当前代码和文档。

## 6.4 后续专题

掌握主链路后，根据兴趣选择一个专题，不要同时展开：

- 文件与沙箱：`packages/fs/`、`packages/sandbox/`。
- 子 Agent 与后台任务：`packages/subagent/`、`packages/jobs/`。
- 工作流：`packages/workflow/`。
- Web UI：`packages/host/`、`packages/client/`、`apps/web/`。
- 协议与 SDK：`packages/acp/`、`packages/sdk/`、`python/`。

项目处于开发者预览阶段，源码和配置可能发生不兼容变化。每篇学习笔记记录对应提交，更新代码后先比较架构文档和配置输出，再修订旧笔记。

## 验收标准

- 能为局部改动选择最小且充分的验证命令。
- 能通过测试快速识别 package 的行为和失败模式。
- 能使用一次具体场景完成跨 package 调用追踪。
- 能选择一个专题并独立建立源码地图。
