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

## 如何安装这个 Skill
你可以用以下任一方式安装：

### 方式一：直接克隆仓库后放入 skills 目录
将仓库放到你的 OpenClaw skills 目录下，例如：

```bash
git clone https://github.com/hitman9099/openclaw-subagent-builder.git
```

然后把其中的 skill 目录内容按你的 OpenClaw skills 组织方式放置。

### 方式二：手动复制 skill 内容
如果你已经有自己的 skills 工作区，也可以直接复制：
- `SKILL.md`
- `references/`

到一个新的 `subagent-builder/` 目录中使用。

## 如何使用这个 Skill
安装后，当你在 OpenClaw 中处理以下类型任务时，这个 skill 应该会被触发或被主动使用：

- 创建一个新的子智能体
- 修复一个不会回复的子智能体
- 删除一个废弃子智能体
- 检查 agentId、workspace、模型、provider、allowlist、配置注册等问题
- 把子智能体的工作流沉淀成标准流程

### 典型用法示例
- 帮我创建一个新的子智能体
- 这个子智能体为什么不会回复
- 帮我删除这个废弃的子智能体
- 检查一下这个 agent 的配置问题
- 把子智能体创建流程标准化

## 这个仓库的用途
这个仓库用于独立发布 `subagent-builder` skill，方便单独维护、版本化管理、复用与分享，而不依赖主工作区整体结构。

## 适合公开展示的仓库描述文案
如果你想在 GitHub 仓库设置里填写描述（Description），推荐使用：

```text
An OpenClaw AgentSkill for creating, repairing, deleting, and validating sub-agents with a reusable execution-first workflow.
```

## 适合公开展示的仓库标签建议
如果你想在 GitHub 仓库设置里填写 Topics，推荐使用：

- openclaw
- agentskill
- subagent
- ai-agent
- automation
- workflow

## 说明
当前仓库主要包含 skill 本体与相关参考资料，不包含单独的演示应用或独立运行时。
