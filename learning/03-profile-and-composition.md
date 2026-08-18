# 3. Profile 与插件组合

本阶段跟踪 `pnpm dsh web` 如何从命令行进入插件树，不深入具体业务实现。

## 3.1 阅读启动链路

按顺序阅读：

1. [CLI 入口](../apps/cli/src/bin.ts)
2. [命令参数解析](../apps/cli/src/args.ts)
3. [Profile 启动](../apps/cli/src/profile-boot.ts)
4. [Profile 组装](../packages/boot/app-boot/src/profile.ts)
5. [基础 Bundle 配置](../packages/bundle/base/cordis.patch.yml)
6. [Web Bundle 配置](../packages/bundle/web-app/cordis.patch.yml)

阅读时只回答一个问题：当前文件接收什么输入，并把控制权交给谁。

## 3.2 理解组合层次

```text
Profile
  -> 按顺序声明 Bundle
  -> Bundle 提供 cordis.patch.yml
  -> Profile 用户层和 --patch 继续覆盖
  -> Loader 挂载最终配置中的插件
```

配置层替换目标行的完整 `config`，不是对对象字段做隐式深度合并。运行时插件树以 `--dump-config` 输出为准。

## 3.3 追踪一个配置项

重新生成配置：

```bash
pnpm dsh --profile web --dump-config > /tmp/dsh-web-config.yml
```

从输出中选择一个插件，完成以下追踪：

1. 找到它来自哪个 Bundle 或用户层。
2. 找到 package 的 `README.md`。
3. 找到 `src/index.ts` 或配置中指定的入口。
4. 记录它声明的 `inject`。
5. 记录它提供的服务、事件监听器或工具。

可以使用：

```bash
rg -n "插件名或配置 id" apps packages
rg -n "inject|ctx\.plugin|ctx\.effect|ctx\.on" packages/目标目录
```

## 验收标准

- 能解释 Profile、Bundle、Patch 和 Plugin 的关系。
- 能从最终配置定位一个插件的来源和入口。
- 能判断插件处于等待依赖、已挂载或加载失败状态时应该检查哪里。
