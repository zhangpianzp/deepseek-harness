# 4. Agent Turn 主链路

本阶段跟踪一次用户输入如何变成模型请求、工具调用和可重放的 Session Log。先读[架构说明](../docs/architecture.zh.md)，再沿一个具体场景阅读源码。

## 4.1 掌握事件顺序

```text
turn/start
  -> claim input
  -> agent/pre-step
  -> step/start
  -> assemble system prompt and tool schemas
  -> derive model messages from the session log
  -> agent/request
  -> llm/stream
  -> assistant/chunk and assistant/message
  -> tool/call
  -> tools/pre-execute
  -> tools/execute
  -> tools/post-execute
  -> tool/result
  -> step/end
  -> turn/end
```

`turn/*`、`step/*`、用户消息、助手消息和工具事件属于持久 Session Event；`agent/*` 和工具执行阶段还包含只在进程内存在的协作事件。

## 4.2 按职责阅读源码

1. [Session 事件类型](../packages/core/session/src/types.ts)：确认持久事件的数据字段。
2. [Session 实现](../packages/core/session/src/index.ts)：关注 `append()` 和 `deriveMessages()`。
3. [Agent 服务](../packages/core/agent/src/index.ts)：确认对外创建、发送和生命周期接口。
4. [Agent Loop](../packages/core/agent-loop/src/agent.ts)：关注 turn、step、请求和结果的顺序。
5. [工具运行时](../packages/core/tools/src/index.ts)：关注注册、校验和执行阶段。
6. [LLM 服务](../packages/llm/llm/src/index.ts)：关注 adapter 注册、`prepareCall()` 和流式调用。

不要从头逐行阅读 `agent.ts`。先搜索事件或方法名，再向上寻找调用者、向下寻找产生的事件。

```bash
rg -n "turn/start|step/start|agent/request" packages/core
rg -n "deriveMessages|prepareCall" packages/core packages/llm
rg -n "tools/pre-execute|tools/execute|tools/post-execute" packages/core/tools
```

## 4.3 完成一次纸面追踪

选择“用户要求执行一条 Bash 命令”作为场景，在笔记中记录：

1. 用户内容进入哪个 inbox。
2. 哪些事件形成 turn 和 step。
3. 系统提示词和工具 Schema 在哪里组装。
4. LLM adapter 在哪里选定。
5. 模型工具调用在哪里进入工具运行时。
6. Bash 返回值如何写入 Session Log。
7. 下一次 step 如何重新派生模型消息。

## 验收标准

- 能区分一个 turn 和一个 step。
- 能区分持久 Session Event 与进程内 Agent Event。
- 能说明“模型可见内容必须能从 Session Log 重建”的影响。
- 给定 `tool/result`，能够向前找到调用来源并向后说明它如何进入后续模型请求。
