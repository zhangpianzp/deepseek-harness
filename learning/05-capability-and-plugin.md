# 5. 能力分层与插件实践

本阶段先用 Bash 理解内置能力的三个角色，再开发一个本地工具插件。完整教程见[三角色能力设计](../docs/user/develop/practice/index.zh.md)。

## 5.1 阅读 Bash 能力

先读 [Shell 能力总览](../packages/shell/README.zh.md)，然后按角色阅读：

1. `packages/shell/shell/`：Service Definition，拥有请求、返回值和服务接口。
2. `packages/shell/bash-local/`：Service Provider，执行本地 Bash。
3. `packages/shell/tool-bash/`：Consumer，把 Shell 服务暴露为模型工具。

Definition 与 Provider、Consumer 可以分别演进；Provider 和 Consumer 只依赖 Definition，不直接依赖彼此。简单工具不需要为了形式而拆成三个 package。

## 5.2 编写本地工具插件

从[第一个 Harness 插件](../docs/user/develop/basic/index.zh.md)和[工具教程](../docs/user/develop/basic/tool.zh.md)开始。练习放在：

```text
tmp/learning-plugin/
  cordis.yml
  src/greet-tool.ts
```

通过 Web Profile 的额外 Patch 加载：

```bash
pnpm dsh web --patch ./tmp/learning-plugin/cordis.yml
```

按顺序增加以下行为：

1. 注册接收 `name` 参数的 `greet` 工具。
2. 增加可校验的问候语配置。
3. 为配置错误和参数错误提供明确诊断。
4. 监听 `tools/result` 并观察规范化结果。
5. 验证插件卸载后工具注册被撤销。

模型调用工具需要 API Key。没有密钥时，可以沿 Cordis 教程第七章直接调用 `ctx.tools.execute()`，验证真实工具执行管道。

## 5.3 设计检查

实现新功能前回答：

- 这是一次直接服务调用，还是需要事件扩展点？
- 状态是否需要持久化到 Session Log？
- 能力是否真的需要可替换 Provider？
- 每个注册行为如何在插件卸载时撤销？
- 错误能否在配置加载或最早可判定的位置失败？
- 工具返回值是否是结构化的规范值，而不是要求调用者解析文本？

## 验收标准

- 本地 `greet` 工具能通过真实工具运行时执行。
- 配置、参数和返回值均经过明确校验。
- 插件卸载后没有残留注册。
- 能解释 Bash 三个角色之间的依赖方向。
