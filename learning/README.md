# DeepSeek Harness Learning Plan

English | [中文](README.zh.md)

This directory manages the learning path from running the project to developing an independent plugin. Spend one or two hours each day and complete the six stages in order; every stage provides reading entry points, hands-on tasks, and completion criteria.

## How to use this directory

1. Run `git rev-parse --short HEAD` before each study period and record the current commit in your notes. The project is in developer preview, so the commit separates current source behavior from older notes.
2. Complete the stages in order without reading every directory under `packages/` in advance.
3. Update [progress.md](progress.md) as you complete milestones.
4. Use [notes/template.md](notes/template.md) to record each source trace.
5. Keep practice code in the ignored `tmp/` directory whenever possible so product code stays unchanged.

## Learning path

1. [Environment setup and running the project](01-environment-and-run.md): install dependencies, build, start the Web UI, and export the effective configuration.
2. [Cordis foundations](02-cordis-foundation.md): learn plugins, services, events, dependency injection, and lifecycle management.
3. [Profiles and plugin composition](03-profile-and-composition.md): understand how the CLI, profiles, bundles, and configuration layers start the application.
4. [The Agent turn flow](04-agent-turn-flow.md): trace user input, model calls, tool execution, and the Session Log.
5. [Capability roles and plugin practice](05-capability-and-plugin.md): implement a tool plugin and learn the Definition, Provider, and Consumer roles.
6. [Testing, debugging, and continued study](06-testing-and-debugging.md): choose checks that match a change and establish a repeatable source-reading method.

## Completion criteria

After completing this plan, you should be able to:

- Locate a runtime plugin and its source from `--dump-config` output.
- Explain the steps, model requests, tool calls, and durable events in one turn.
- Decide whether new behavior should register a service, listen to an event, provide a tool, or extend an existing capability.
- Implement a local plugin with configuration, cleanup behavior, and tests.
- Choose focused tests, typechecking, snapshots, or build verification for a local change.
