# Subagent Builder Checklist

## Pre-action confirmation

Confirm with the user before doing any of the following:
- create a new sub-agent
- repair an existing sub-agent
- edit config
- write files into workspace
- persist rules into memory

## Creation / repair checklist

### A. Goal definition
- [ ] New build or repair is clear
- [ ] Name is clear
- [ ] `agentId` is clear
- [ ] Intended responsibility is clear
- [ ] Workspace requirement is clear
- [ ] Tool/permission requirement is clear

### B. Registration
- [ ] `agentId` is unique
- [ ] `agentId` is consistently referenced
- [ ] No duplicate, old, or misspelled id remains
- [ ] Agent is registered where required
- [ ] Caller allowlist includes this agent when needed

### C. Workspace
- [ ] Workspace path exists
- [ ] Workspace path matches the target agent
- [ ] Runtime actually uses the expected path
- [ ] No fallback to default workspace is happening

### D. Identity
- [ ] `IDENTITY.md` or equivalent role file exists
- [ ] `AGENT.md` or equivalent explicit instruction file exists
- [ ] Name / role / tone are clear enough for stable output

### E. Model
- [ ] Model or alias is valid
- [ ] Provider exists in current config
- [ ] Model is callable
- [ ] Call path actually reaches that model
- [ ] Default model does not point to a stale provider

### F. Auth
- [ ] Required auth profile or credential source exists
- [ ] Selected provider has usable auth for this agent
- [ ] No `No API key found`-type failure remains

### G. Tools / permissions
- [ ] Required tools are available
- [ ] Permission scope matches the role
- [ ] The agent is not blocked from acting

### H. Minimum viable test
- [ ] `你是谁？` returns normal text
- [ ] `请用一句话说明你的职责。` returns role-consistent text
- [ ] A domain-specific prompt returns useful output
- [ ] No empty reply or obvious context confusion appears

## Final acceptance

Mark as fully usable only if all are true:
- [ ] unique `agentId`
- [ ] correct effective workspace
- [ ] clear working identity
- [ ] callable model
- [ ] usable auth for the selected model
- [ ] necessary tools and permissions
- [ ] stable normal text output

## Suggested completion wording

Use wording like:

- `已完成注册与工作区校验，模型可调用，权限满足最小职责要求，并通过了最小回复测试。`
- `当前已达到可用标准。`

If not fully done, say exactly what failed, for example:

- `已完成 agentId 与工作区修正，但模型层仍未通过，暂不能判定为可用。`
- `已能启动，但尚未通过最小回复测试，因此不能视为完成。`
