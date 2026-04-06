# Execution Playbook

## Purpose

This file turns the skill into a more execution-oriented workflow. Use it when the task is not just to explain the process, but to actually create or repair an agent.

Compared with `semi-automatic-playbook.md`, this file is the higher-level process view. Use `semi-automatic-playbook.md` for symptom-driven routing first, then use this file to keep the overall workflow disciplined.

## Master rule

Do not guess across layers. Route the problem to the most likely layer, fix one layer, then retest.

## A. New agent creation playbook

### Step A1. Clarify target
Confirm:
- display name
- `agentId`
- purpose
- target workspace
- whether this must be a persistent landed agent
- whether the result should be written into workspace docs or memory

### Step A2. Land the workspace
Create or verify:
- workspace directory
- optional memory directory
- minimal bootstrap files

At minimum, create:
- `IDENTITY.md`
- `AGENT.md`

### Step A3. Register the agent
Check:
- the `agentId` is not duplicated
- the agent is registered in config when needed
- the caller has permission to spawn or invoke it

### Step A4. Bind a known-good model
Do not rely on a questionable default if the environment has known model drift.

Preferred tactic:
- bind the new agent to a model already proven to work in this environment

### Step A5. Verify auth viability
Check:
- whether this runtime needs agent-local auth
- whether the selected provider has valid auth for that agent context

### Step A6. Run minimum reply test
Use:
- self-introduction
- role summary
- one domain-specific prompt

### Step A7. Report outcome
Classify the outcome as exactly one of:
- created only
- callable but not usable
- usable

## B. Existing agent repair playbook

### Step B1. Capture the exact symptom
Never paraphrase away important details. Keep the literal error text if possible.

### Step B2. Route by symptom

#### Symptom: spawn forbidden / allowlist error
Route to:
- registration layer

#### Symptom: no output
Route to:
1. hidden model/auth error check
2. identity layer only if model/auth is not the cause

#### Symptom: `No API key found`
Route to:
- auth layer

#### Symptom: provider/model missing or stale default
Route to:
- model layer

#### Symptom: files exist but agent cannot be invoked
Route to:
- registration layer

### Step B3. Make one focused repair
Examples:
- add the agent to allowlist
- bind the agent to a known-good model
- add the missing auth source
- strengthen identity files

### Step B4. Re-test immediately
Do not stack three repairs before testing.

### Step B5. Update the diagnosis based on the new result
If the new result changes layer, switch layers deliberately.

## C. Stop conditions

Immediately stop identity/prompt editing and switch to config diagnosis if you see:
- allowlist errors
- provider missing
- model missing
- `No API key found`
- default model pointing to stale provider

Immediately stop auth editing and switch to registration diagnosis if you see:
- spawn forbidden
- agent not registered

## D. Strong defaults

When the environment is messy, these defaults are safer:
- prefer a model already proven to work in another agent in the same environment
- prefer explicit model binding over relying on a broken default chain
- prefer minimum identity files over over-designed persona docs
- prefer one repair + one test over broad rewrites

## E. Output discipline

When reporting to the user:
- say what layer failed
- say what passed
- say what is still unknown
- say whether the agent is merely created, callable, or truly usable

Do not collapse these into a single vague success sentence.
