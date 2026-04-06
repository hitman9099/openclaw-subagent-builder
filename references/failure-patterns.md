# Failure Patterns

## 1. `agentId is not allowed for sessions_spawn`

### Symptom
- The agent exists as files or config
- Spawn is rejected immediately
- Error mentions `not allowed` or `allowAgents`

### Root cause
- The target `agentId` is not in the caller's allowed sub-agent list
- The agent may also be missing from `agents.list`

### Standard fix
1. Check whether the target agent is registered
2. Check whether the caller allows that `agentId`
3. Add the agent to the allowlist if appropriate
4. Restart / reload config if required
5. Re-run the minimum reply test

## 2. Spawn accepted, but result is `(no output)`

### Symptom
- Call is accepted
- No visible text is returned
- No obvious error appears in the user-facing result

### Root cause candidates
- Identity layer is too weak or missing
- Model layer is unhealthy
- Auth layer failed but was not surfaced clearly in the first pass

### Standard fix
1. Check for explicit runtime/auth errors first
2. If there is no model/auth error, inspect `IDENTITY.md` and `AGENT.md`
3. Add a minimum role definition if missing
4. Re-run the minimum reply test
5. If still empty, move to model/auth diagnosis

## 3. `No API key found for provider ...`

### Symptom
- Error explicitly mentions missing API key or provider auth

### Root cause
- The selected provider has no usable credentials for this agent
- The agent-local auth store is missing or mismatched
- The selected model/provider is not aligned with available auth

### Standard fix
1. Identify the actual provider the agent is using
2. Check whether that provider is intended
3. Check agent-local auth configuration
4. Prefer binding the agent to a known-good model/provider
5. Only then consider copying auth files if that matches the runtime design

## 4. Default model points to a stale provider

### Symptom
- New agents keep hitting an unexpected provider
- Config references a provider that is missing from current `models.providers`
- Agents fail in confusing ways during first run

### Root cause
- `agents.defaults.model.primary` still points to a legacy provider/model chain

### Standard fix
1. Compare `agents.defaults.model.primary` with current `models.providers`
2. If the provider is gone, either restore it intentionally or change the default model
3. Prefer changing the default to a currently known-good model
4. Restart / reload config
5. Re-test with a new or repaired agent

## 5. Agent exists on disk, but is not really usable

### Symptom
- Workspace exists
- Files exist
- Agent still cannot be called or cannot answer

### Root cause
- Only file-layer creation happened
- Registration, model, auth, or test layer was never completed

### Standard fix
Do not treat file creation as success. Continue through:
- registration
- workspace
- identity
- model
- auth
- tools/permissions
- minimum reply test
