---
name: subagent-builder
description: Create, repair, delete, and validate OpenClaw sub-agents or specialized assistants with an execution-first workflow. Use when the user asks to create or 落地 a new agent, fix an agent that exists but does not reply, delete an obsolete agent, troubleshoot agentId / allowAgents / workspace / model / provider / auth-profiles / tool-permission issues, or standardize the workflow for reusable agents. Also use when the task mentions 子智能体、智能体创建、删除智能体、落地智能体、agentId、独立工作区、专用助手、spawn 被拒绝、不能回复、无权限、No API key found、provider 对不上、默认模型、上下文错乱.
---

# Subagent Builder

Use an execution-first workflow. Treat `created`, `callable`, and `usable` as different states.

## Core rules

- Ask for user confirmation before any creation, repair, config change, restart, or write action.
- Validate one layer at a time.
- Prefer the smallest change that proves the next layer works.
- Do not declare success because a directory, config entry, or agentId exists.
- If the failure is clearly at allowlist / provider / auth / model level, stop editing persona files and switch to config diagnosis.
- Always report using the standard result format.

## Fixed execution order

Unless the user explicitly asks otherwise, use this order:

**registration → workspace → identity → model → auth → tools/permissions → minimum reply test**

## Quick routing

Use this routing before doing deeper work:

- `agentId is not allowed for sessions_spawn`
  - route to registration
- call accepted but `(no output)`
  - check model/auth first, then identity
- `No API key found for provider ...`
  - route to auth
- provider/model missing or stale default
  - route to model
- files exist but agent cannot be called
  - route to registration, not identity

For detailed routing, read `references/semi-automatic-playbook.md`.

## Deletion workflow

When deleting an obsolete landed agent:

1. Clarify the exact target display name and `agentId`.
2. Locate the runtime registration, workspace path, agentDir, and any caller allowlist references.
3. Backup current config before deletion.
4. Remove the file layer first: workspace and agent directory.
5. Remove the registration layer: `agents.list` entry and any `allowAgents` references.
6. If `config.patch` leaves array-based residue, immediately switch to `config.get` + full `config.apply`.
7. Verify deletion on three layers:
   - file layer
   - config layer
   - runtime list layer via `openclaw agents list`
8. Report whether it is partially deleted or fully removed.

Deletion is only considered complete when the agent is gone from:
- disk
- config
- runtime agent listing

## Creation workflow

When building a new landed agent:

1. Clarify target name, `agentId`, purpose, workspace, and persistence requirements.
2. Land the workspace.
3. Write the minimum identity files.
4. Register the agent and verify allowlist status.
5. Bind or verify model.
6. Verify auth.
7. Run the minimum reply test.
8. Report whether it is created only, callable, or usable.

Use `references/bootstrap-template.md` for the minimum file set.

## Repair workflow

When repairing an existing agent:

1. Capture the exact symptom or error text.
2. Route the symptom to the most likely layer.
3. Make one focused fix.
4. Re-test immediately.
5. Only move to another layer if the new result requires it.

Use `references/failure-patterns.md` and `references/case-library.md` when the problem resembles a known path.

## Acceptance standard

A sub-agent is only considered usable when all are true:

1. It has a unique and correct `agentId`
2. It has the correct and effective workspace
3. It has a clear working identity
4. It has a callable model
5. It has usable auth for that model when required
6. It has the necessary tools and permissions
7. It can return stable, normal, role-consistent text output

If any item is missing, report it as partially complete, not done.

## References

- Read `references/semi-automatic-playbook.md` first when the user wants actual creation/repair work, not just explanation.
- Read `references/config-paths.md` when you need a fast path-oriented lookup of what config to inspect first.
- Read `references/checklist.md` for the final acceptance checklist.
- Read `references/failure-patterns.md` for common failure diagnosis.
- Read `references/bootstrap-template.md` for minimum creation templates.
- Read `references/execution-playbook.md` for the stronger execution decision tree.
- Read `references/case-library.md` when a new problem resembles a past real-world creation/repair path.

## Reporting format

Always report using this structure:

- target `agentId`:
- purpose:
- registration status:
- workspace status:
- identity status:
- model status:
- auth status:
- tools/permissions status:
- minimum reply test:
- final conclusion: usable / partially complete / not usable
- next step:
