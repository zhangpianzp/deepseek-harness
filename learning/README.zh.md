# DeepSeek Harness 学习计划

[English](README.md) | 中文

本目录用于管理从运行项目到独立开发插件的学习过程。建议每天投入一到两个小时，按顺序完成六个阶段；每个阶段都包含阅读入口、动手任务和验收标准。

## 使用方法

1. 在开始学习前执行 `git rev-parse --short HEAD`，把当前提交记录到学习笔记中。项目处于开发者预览阶段，记录提交可以避免后续源码变化与旧笔记混淆。
2. 按阶段完成任务，不要求提前通读所有 `packages/`。
3. 在 [progress.md](progress.md) 更新进度。
4. 使用 [notes/template.md](notes/template.md) 记录每次源码追踪结果。
5. 练习代码优先放在仓库已忽略的 `tmp/` 目录，避免污染产品代码。

## 学习路径

1. [环境准备与运行项目](01-environment-and-run.md)：完成依赖安装、构建、Web 启动和真实配置导出。
2. [Cordis 基础](02-cordis-foundation.md)：掌握插件、服务、事件、依赖注入和生命周期。
3. [Profile 与插件组合](03-profile-and-composition.md)：理解 CLI、Profile、Bundle 和配置层的启动关系。
4. [Agent Turn 主链路](04-agent-turn-flow.md)：跟踪用户消息、模型调用、工具执行和 Session Log。
5. [能力分层与插件实践](05-capability-and-plugin.md)：编写工具插件并理解 Definition、Provider、Consumer 三个角色。
6. [测试、调试与持续学习](06-testing-and-debugging.md)：选择匹配变更范围的验证方式并建立日常阅读方法。

## 学习完成标准

完成本计划后，应当能够：

- 从 `--dump-config` 输出定位一个运行时插件及其源码。
- 解释一次 turn 中的 step、模型请求、工具调用和持久事件。
- 判断新功能应注册服务、监听事件、提供工具，还是扩展已有能力。
- 编写一个带配置、清理行为和测试的本地插件。
- 为一次局部修改选择聚焦测试、类型检查、快照或构建验证。
