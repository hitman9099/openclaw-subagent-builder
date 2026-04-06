# openclaw-subagent-builder

`subagent-builder` is an AgentSkill for OpenClaw.

It is designed to help create, repair, delete, and validate OpenClaw sub-agents with a more execution-first workflow.

## What it helps with
- Create a new sub-agent from scratch
- Repair an existing sub-agent that cannot reply
- Remove an obsolete sub-agent cleanly
- Diagnose common agent setup problems
- Standardize the workflow for reusable agent creation and maintenance

## Main skill files
- `SKILL.md` — skill definition and trigger description
- `references/` — supporting references, checklists, patterns, and playbooks

## Typical scenarios
- A user wants to create a dedicated OpenClaw sub-agent
- A sub-agent exists but does not respond
- Agent registration / allowlist / workspace / model / provider settings are misconfigured
- A team wants a repeatable workflow for sub-agent lifecycle management

## Repository purpose
This repository publishes the standalone `subagent-builder` skill so it can be reviewed, versioned, and reused independently from the main workspace.

## Notes
This repository currently contains the skill content only. It does not include a separate demo app or standalone runtime.
