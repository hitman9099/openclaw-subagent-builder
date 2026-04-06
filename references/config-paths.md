# OpenClaw Config Paths Quick Reference

## Purpose

Use this file when creating or repairing sub-agents and you need to know which config path to inspect first.

This is not a full config manual. It is a task-oriented quick reference for the most relevant paths in real sub-agent work.

---

## 1. `agents.list`

### Why check it
This is the first core registration block for named agents.

### Check it when
- an agent exists on disk but cannot be called
- you are not sure whether the agent is really registered
- you are verifying name / `agentId` / workspace / explicit model binding

### Typical questions it answers
- does this `agentId` exist in config?
- what workspace does it point to?
- does it have an explicit `model`?
- does it have an `agentDir`?

### Typical conclusions
- agent missing here → file-layer only, not registered
- wrong workspace here → runtime likely landing in the wrong place
- explicit model here → agent may bypass broken defaults

---

## 2. `agents.defaults.model.primary`

### Why check it
This is the default model path that new or loosely configured agents may inherit.

### Check it when
- new agents keep failing in similar ways
- a new agent keeps hitting a strange provider/model path
- explicit model binding fixes the problem, but default behavior is still broken

### Typical questions it answers
- what model path do new agents inherit by default?
- does the default point to a provider that still exists?

### Typical conclusions
- stale provider here → future agents will keep inheriting broken behavior
- healthy default here → look elsewhere

---

## 3. `models.providers`

### Why check it
This is the source of available configured providers.

### Check it when
- a model/provider seems missing
- a default model points to something suspicious
- auth/provider failures appear

### Typical questions it answers
- does the referenced provider exist at all?
- which models are actually configured right now?

### Typical conclusions
- provider absent here but referenced elsewhere → config drift / stale reference
- provider present here → continue with model/auth checks

---

## 4. `agents.list[*].model`

### Why check it
This shows whether a specific agent has an explicit model binding.

### Check it when
- one agent works and another does not
- you want to force an agent onto a known-good model
- you suspect the agent is falling back to defaults

### Typical questions it answers
- is this agent explicitly pinned to a model?
- is it inheriting the default model chain?

### Typical conclusions
- explicit model here can explain why one agent works while another fails
- missing explicit model means default drift matters more

---

## 5. `agents.list[*].workspace`

### Why check it
This controls the intended runtime workspace for an agent.

### Check it when
- files were created, but behavior suggests the wrong workspace
- the agent appears to have amnesia or wrong local files
- you are landing a persistent agent

### Typical questions it answers
- where should this agent actually work?
- is it using the intended persistent directory?

### Typical conclusions
- wrong path here → workspace-layer failure
- correct path here but wrong runtime behavior → investigate fallback / runtime landing

---

## 6. `agents.list[*].agentDir`

### Why check it
This often determines the agent-local runtime directory for identity/auth and related files.

### Check it when
- auth looks agent-local
- runtime mentions an agentDir path in errors
- identity/auth behavior differs across agents

### Typical questions it answers
- which directory is treated as the agent's own runtime home?
- where should agent-local auth and identity files live?

### Typical conclusions
- auth copied to the wrong directory → auth-layer failure persists
- identity files in the wrong place → identity changes may not take effect

---

## 7. `agents.list[*].tools`

### Why check it
This shows whether the agent has a specific tool profile or restrictions.

### Check it when
- the agent can talk but cannot do its intended job
- you suspect a permissions/tool profile issue

### Typical questions it answers
- is this agent using a specific tool profile?
- is it different from defaults?

### Typical conclusions
- restrictive tool config here can explain role failure
- permissive config here means the problem may be elsewhere

---

## 8. `agents.defaults.workspace`

### Why check it
This is the fallback workspace path for agents without explicit workspace settings.

### Check it when
- the agent seems to fall back to the main workspace
- a new agent was created with incomplete config

### Typical questions it answers
- what workspace will an unpinned agent inherit?

### Typical conclusions
- missing explicit workspace + this path pointing to main workspace → hidden fallback risk

---

## 9. `agents.list[main].subagents.allowAgents`

### Why check it
This is one of the highest-value paths for spawn failures from the main session.

### Check it when
- `agentId is not allowed for sessions_spawn`
- the target agent is registered but still forbidden

### Typical questions it answers
- is the target `agentId` actually allowed for the main caller?

### Typical conclusions
- missing target here → registration is not enough; allowlist is blocking invocation

---

## 10. `auth.profiles`

### Why check it
This shows which auth profile types are configured globally.

### Check it when
- provider/auth issues appear
- you need to know what auth modes are even available

### Typical questions it answers
- does the system have configured auth profiles for the providers in use?
- is auth expected to be oauth or api_key based?

### Typical conclusions
- auth profile absent here → provider may not be viable globally
- auth profile present here → continue to agent-local auth path if required

---

## 11. Agent-local auth files

### Typical path pattern
- `<agentDir>/auth-profiles.json`

### Why check it
Some agent runs depend on local auth material relative to agentDir.

### Check it when
- runtime says `No API key found for provider ...`
- the error explicitly mentions an auth store path

### Typical questions it answers
- does the expected auth file exist in the directory the runtime is using?
- is the error pointing to a different agentDir than expected?

### Typical conclusions
- file missing here → auth-layer failure
- file present but wrong provider mapping → model/auth mismatch

---

## 12. `channels.feishu.groupPolicy`

### Why check it
Not a sub-agent registration path, but highly relevant to the risk of exposed powerful agents.

### Check it when
- you are reviewing whether the environment is safe for agents with strong tools
- a security audit warns about open group exposure

### Typical questions it answers
- are Feishu groups open or allowlisted?

### Typical conclusions
- `open` here + high-privilege agents/tools = elevated exposure risk

---

## 13. `tools.profile` and default tool exposure

### Why check it
This helps explain whether the environment is broadly permissive.

### Check it when
- security audit warns about runtime/filesystem exposure
- you are deciding whether agent failures are due to missing tools or too much exposure

### Typical questions it answers
- is the environment broadly full-access by default?

### Typical conclusions
- broad defaults mean new agents may inherit more power than intended
- restrictive defaults mean you may need explicit agent-level tool config

---

## Suggested inspection order by symptom

### Spawn forbidden
1. `agents.list`
2. `agents.list[main].subagents.allowAgents`

### No output
1. `agents.list[*].model`
2. `agents.defaults.model.primary`
3. `models.providers`
4. agent-local auth path
5. identity files

### No API key found
1. `agents.list[*].model`
2. `models.providers`
3. `auth.profiles`
4. `<agentDir>/auth-profiles.json`

### New agents keep inheriting broken behavior
1. `agents.defaults.model.primary`
2. `models.providers`
3. `agents.defaults.workspace`

### Agent acts from the wrong directory
1. `agents.list[*].workspace`
2. `agents.list[*].agentDir`
3. `agents.defaults.workspace`
