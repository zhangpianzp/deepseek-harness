# 2. Cordis 基础

本阶段使用无模型密钥的官方教程掌握 Harness 的插件运行机制。先完成教程，再阅读 Harness 的复杂 package。

## 2.1 建立概念模型

阅读 [Cordis Primer](../docs/cordis-primer.zh.md)，重点理解以下概念：

- Plugin：功能和生命周期的基本单位。
- Context：插件通过 `ctx.<key>` 获取服务。
- Service：向其他插件提供可调用能力。
- `inject`：声明插件启动所需的服务。
- Event：插件之间观察、包装或顺序协作的扩展点。
- Effect：插件卸载时能够自动撤销的注册行为。

Waterfall 监听器调用 `next()` 才会把处理交给后续监听器；直接返回会终止后续链路。

## 2.2 完成七章教程

从 [Cordis 教程](../docs/cordis-tutorial/index.zh.md) 开始，练习放在已被 Git 忽略的目录中：

```bash
mkdir -p tmp/cordis-tutorial
cd tmp/cordis-tutorial
```

按顺序完成：

1. 第一个插件。
2. 生命周期与 Effect。
3. Service 与依赖注入。
4. 类型化事件和 Waterfall。
5. 配置校验。
6. 插件组合与热加载。
7. 接入 Harness 工具系统。

每章示例通过同一个入口运行：

```bash
node --import tsx ../../vendor/cordis/bin.js
```

第七章会注册 `greet` 工具，通过真实 `ctx.tools` 执行，并监听 `tools/result`，整个过程不调用模型。

## 2.3 记录一个插件

使用[学习笔记模板](notes/template.md)记录教程中的 `greet` 插件，至少写明：

- 插件依赖的 `ctx` key。
- 插件注册的工具或事件。
- 调用从哪里进入。
- 插件卸载时撤销什么。
- 错误会在哪一层出现。

## 验收标准

- 能独立写出一个函数形式的 Cordis 插件。
- 能解释 `inject` 为什么不是普通的导入声明。
- 能区分普通事件、Waterfall 和直接服务调用。
- 能证明插件卸载后注册行为被撤销。
