# Case Library

## Purpose

Use these cases as pattern memory. They are not meant to be copied blindly. Read them when a new agent or broken agent looks similar.

This file is for analogy and judgment. For exact execution order, defer to `semi-automatic-playbook.md` and `config-paths.md`.

---

## Case 1. `mahe-knowledge-base` (creation validation)

### Task type
- create a real reusable knowledge-focused assistant

### Early false impression
- it looked created
- files and identity existed
- it seemed close to done

### Actual problem
- creation success was confused with usability success
- the agent needed to be verified as a truly callable and responding agent

### What mattered
1. verify the agent can actually be invoked
2. verify the model is callable
3. verify it can return normal text output
4. use a minimal reply test instead of trusting the setup work

### Useful test
- ask who it is
- ask for one-sentence responsibility

### Reusable lesson
A sub-agent is not usable just because its folder and configuration exist. It must pass a real reply test.

---

## Case 2. `mahe-knowledge-base` (deletion cleanup)

### Task type
- delete an obsolete landed agent completely

### Initial action
- workspace and agent directory were removed
- `allowAgents` was updated
- config restart was triggered

### False impression after the first pass
- runtime sessions disappeared
- it looked mostly deleted
- but `openclaw status` still showed traces and `openclaw agents list` still listed the agent

### Actual problem
- deleting files is not the same as deleting registration
- `config.patch` did not fully remove the old item from the array-based `agents.list`

### What finally worked
1. verify with `openclaw agents list`, not status summary alone
2. search the config file directly for the old `agentId`
3. if the old item is still present, do not assume cache
4. re-run `config.get`
5. generate a full config without the target agent
6. use `config.apply` instead of another patch
7. re-verify file layer, config layer, and runtime list layer

### Reusable lesson
For deletion of landed agents, the safe standard is:
- remove files
- remove registration
- remove allowlist references
- verify with `openclaw agents list`
- if array residue remains after `config.patch`, switch to full `config.apply`

Deletion is only complete when the agent is gone from disk, config, and runtime listing.

---

## Case 3. `weather-assistant`

### Task type
- create a landed weather assistant, not just a temporary test agent

### Initial state
- workspace files were created
- identity-related files existed
- the agent name and role were defined

### Failure chain

#### Stage 1. Spawn forbidden
Symptom:
- `agentId is not allowed for sessions_spawn`

Root cause:
- the agent was not in the caller's allowed sub-agent list

Fix:
- add `weather-assistant` into the relevant allowlist
- ensure it is present in the configured agent list
- reload config

Lesson:
Do not treat on-disk creation as registration.

#### Stage 2. Spawn accepted, but no output
Symptom:
- invocation succeeded
- result returned no visible output

Initial hypothesis:
- identity files might be too weak

Useful action:
- strengthen `IDENTITY.md`
- add explicit `AGENT.md`

Lesson:
When output is empty, identity is worth checking, but only after model/auth failures are ruled out.

#### Stage 3. Auth/provider failure surfaced
Symptom:
- `No API key found for provider ...`

Root cause:
- the selected provider/auth path for that agent was not viable

Fix direction:
- identify the actual provider being used
- do not keep patching persona files
- move to model/auth diagnosis

Lesson:
A provider/auth error is not a prompt problem.

#### Stage 4. Default model drift discovered
Symptom:
- the agent kept hitting an unhealthy provider/model path

Root cause:
- `agents.defaults.model.primary` pointed to an old or stale provider chain

Fix:
- bind the agent explicitly to a known-good model
- then repair the default model to prevent future agents from inheriting the same broken path

Lesson:
If a new agent only works after explicit model binding, check the default model immediately.

#### Stage 5. Final success
What finally made it usable:
- registration fixed
- identity strengthened
- auth issue diagnosed
- model explicitly bound to a known-good path
- minimum reply test passed

### Final reusable lesson
When creating a landed agent, the real path is:
- file landing
- registration
- allowlist
- identity
- model
- auth
- reply test

Skipping any of these creates a false sense of completion.

---

## Cross-case lessons

### 1. `Created` is not `usable`
A workspace, config block, or identity file only proves partial progress.

### 2. The minimum reply test is mandatory
Ask for:
- self-introduction
- one-sentence role summary
- one domain-specific prompt

### 3. Route by symptom, not by intuition
- allowlist error → registration layer
- no API key → auth layer
- stale provider/default drift → model layer
- vague output without config errors → identity layer

### 4. Fix one layer, then retest
Do not stack multiple speculative fixes before testing.

### 5. If future agents repeat the same issue, fix the default path
Do not keep repairing agents one by one if the real problem is the shared default model or shared registration policy.
