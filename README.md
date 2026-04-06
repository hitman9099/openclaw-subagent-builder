# openclaw-subagent-builder

`subagent-builder` 是一个面向 OpenClaw 的 AgentSkill。

它的目标是用更偏执行、可复用的方式，帮助用户创建、修复、删除和校验 OpenClaw 子智能体。

## 这个 Skill 能做什么
- 从零创建新的子智能体
- 修复“已存在但不会回复”的子智能体
- 干净地删除废弃子智能体
- 排查常见的 agent 配置问题
- 将子智能体生命周期管理流程标准化、可复用化

## 仓库主要内容
- `SKILL.md`：Skill 定义与触发说明
- `references/`：相关参考资料、检查清单、经验模式与执行手册

## 适用场景
- 用户想新建一个专用 OpenClaw 子智能体
- 某个子智能体已经存在，但无法正常回复
- agent 注册、allowlist、workspace、model、provider 等配置存在问题
- 希望把子智能体的创建、修复、删除流程沉淀成标准工作流

## 这个仓库的用途
这个仓库用于独立发布 `subagent-builder` skill，方便单独维护、版本化管理、复用与分享，而不依赖主工作区整体结构。

## 说明
当前仓库主要包含 skill 本体与相关参考资料，不包含单独的演示应用或独立运行时。
