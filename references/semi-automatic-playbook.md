# Semi-Automatic Playbook

## Purpose

Use this playbook when the task is not just to reason about sub-agent problems, but to execute a repeatable diagnosis path with minimal improvisation.

This file is intentionally action-oriented:
- symptom
- first checks
- likely conclusion
- next action

## Rule of use

Do not run all checks blindly. Start from the symptom, follow the prescribed first checks, then branch.

---

## 1. Symptom: `agentId is not allowed for sessions_spawn`

### First checks
1. Check which agents are currently allowed
2. Inspect `agents.list`
3. Inspect the caller agent's `subagents.allowAgents`

### Likely conclusion
- target agent is not registered
- target agent is registered but not allowed for the caller

### Next action
- register the agent if missing
- add the `agentId` into the correct allowlist
- restart / reload config if required
- re-run a minimum reply test

---

## 2. Symptom: agent exists on disk, but cannot be called

### First checks
1. Confirm workspace path exists
2. Check `agents.list`
3. Check allowlist status

### Likely conclusion
- only file-layer creation happened
- config registration did not happen

### Next action
- do not edit prompt files first
- complete registration layer
- then test invocation again

---

## 3. Symptom: call is accepted, but result is `(no output)`

### First checks
1. Check whether a hidden model/auth error exists in the runtime result
2. Check the selected model for the agent
3. Check whether identity files are too weak or missing

### Likely conclusion
- model/auth failed silently in the first user-facing view
- or identity layer is too weak to produce stable output

### Next action
- if model/auth error appears, switch to model/auth branch immediately
- otherwise strengthen `IDENTITY.md` and `AGENT.md`
- re-run the same minimum reply test

---

## 4. Symptom: `No API key found for provider ...`

### First checks
1. Identify which model/provider the agent is actually using
2. Check whether that provider is intended
3. Check whether the agent has usable auth for that provider

### Likely conclusion
- auth layer failure
- provider/model selection does not match available credentials

### Next action
- prefer binding the agent to a known-good model/provider
- only copy or rebuild auth if the runtime design requires agent-local auth
- re-test immediately after changing model or auth

---

## 5. Symptom: new agents keep hitting a weird or broken provider

### First checks
1. Inspect `agents.defaults.model.primary`
2. Compare it against current `models.providers`
3. Compare with another agent already known to work

### Likely conclusion
- default model points to stale provider
- environment has model drift / historical residue

### Next action
- set `agents.defaults.model.primary` to a currently known-good model
- or intentionally restore the missing provider if that is the desired long-term state
- restart / reload config
- test a new or repaired agent again

---

## 6. Symptom: agent starts only after explicit model binding

### First checks
1. Inspect `agents.defaults.model.primary`
2. Inspect the agent's explicit `model` field
3. Compare which one actually worked

### Likely conclusion
- default model path is unhealthy
- explicit binding is bypassing a broken default chain

### Next action
- fix the default model if future agents should inherit it
- keep explicit binding on the repaired agent

---

## 7. Symptom: output is generic, vague, or role-inconsistent

### First checks
1. Read `IDENTITY.md`
2. Read `AGENT.md`
3. Check whether the role, tone, and constraints are explicit

### Likely conclusion
- identity layer is too weak
- the agent knows it is an assistant, but not what kind of assistant

### Next action
- add minimum role definition
- add one domain-specific behavior rule
- re-run the same prompt

---

## 8. Symptom: agent can reply, but not perform its intended job

### First checks
1. Check required tools for the role
2. Check tool profile / deny rules / runtime restrictions
3. Check whether the failure is really permissions, not model/auth

### Likely conclusion
- tools/permissions layer failure

### Next action
- grant only the minimum viable tools for the role
- re-test with a task-specific prompt

---

## 9. Minimum creation script (mental version)

When creating a new landed agent, use this exact order:

1. clarify target
2. create workspace
3. write `IDENTITY.md`
4. write `AGENT.md`
5. register the agent
6. verify allowlist
7. bind or verify model
8. verify auth
9. run minimum reply test
10. report status in standard format

---

## 10. Minimum repair script (mental version)

When repairing a broken agent, use this exact order:

1. capture literal symptom or error
2. match the symptom to this playbook
3. perform the prescribed first checks
4. identify the layer
5. make one focused fix
6. re-run the same test
7. only then move to the next layer if required

---

## 11. Strong stop conditions

Stop editing identity/prompt files and switch to config diagnosis immediately if you see:
- allowlist errors
- provider missing
- model missing
- `No API key found`
- stale default model/provider mismatch

Stop editing auth files and switch to registration diagnosis immediately if you see:
- spawn forbidden
- unregistered `agentId`

---

## 12. Reporting discipline

Always end with:
- what layer failed
- what layers passed
- what test was run
- whether the agent is `created only`, `callable`, or `usable`

Never collapse these into a vague sentence like `已经好了`.
