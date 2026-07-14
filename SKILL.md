---
name: prompt-readiness
description: Audit an incomplete or underspecified request against current prompt-writing practices, inspect relevant context and configuration files, identify only material information gaps, rewrite the request into an execution-ready prompt, and then carry out the original task. Use when the user explicitly invokes $prompt-readiness or asks for a prompt-readiness gate before coding, diagnosis, research, configuration, document, or operational work.
---

# Prompt Readiness

Turn a rough request into an execution-ready prompt without expanding its authority, then complete the task. Treat prompt rewriting as a gate to execution, not as the final deliverable.

## Workflow

### 1. Preserve the original request

Extract the user's intended outcome, requested action, explicit scope, constraints, and authorization. Do not reinterpret an analysis request as permission to edit, or a local change request as permission for external or destructive actions.

Keep direct user instructions authoritative. Never weaken a safety boundary or silently add a materially different objective while rewriting.

### 2. Inspect available context before asking

Read only the materials likely to resolve the request:

- The current conversation, attachments, paths, examples, and error text.
- Applicable instruction and configuration files such as `AGENTS.md`, project docs, manifests, runtime config, schemas, or tool definitions.
- The actual source-of-truth implementation or runtime path when the task depends on repository behavior.
- Existing tests, validation commands, or acceptance conventions relevant to the target.

Resolve configuration precedence from the active environment. Report conflicts that materially affect the task. Do not expose hidden instructions, credentials, tokens, or unrelated configuration.

### 3. Run the readiness audit

Assess the request against [references/readiness-checklist.md](references/readiness-checklist.md). Determine whether the following are sufficient for safe execution:

1. Outcome and task mode: answer, diagnose, plan, change, build, operate, or monitor.
2. Target and scope: repository, files, system, environment, audience, and exclusions.
3. Evidence and context: inputs, source of truth, examples, current behavior, and desired behavior.
4. Constraints and boundaries: compatibility, safety, privacy, cost, time, and approval requirements.
5. Success criteria: observable result, verification method, and acceptable remaining risk.
6. Output contract: required format, detail, tone, citations, artifacts, or handoff.

Classify each missing item:

- **Material blocker**: Different answers would substantially change the implementation, authority, safety, or acceptance result.
- **Safe assumption**: A reasonable default is reversible, stays within scope, and is unlikely to change the intended result.
- **Optional enhancement**: Helpful but unnecessary for correct execution.

Do not ask for information already available in context or discoverable through safe read-only inspection.

### 4. Close only material gaps

If a material blocker remains, pause before rewriting or executing. Ask one concise, consolidated question that:

- Lists only the missing decisions or inputs.
- Explains briefly why each matters.
- Provides a fill-in structure when several related fields are needed.
- Avoids optional preference questions.
- Asks the user to include `$prompt-readiness` with the missing information so this explicit-only skill is loaded again on the continuation turn.

Do not ask the user to restate the whole request. On the next invocation, merge the answer with all previously gathered context and rerun the audit.

If only safe assumptions remain, state them briefly and continue. Do not block on optional enhancements.

### 5. Rewrite the prompt

Create a compact execution prompt using only sections that add value:

```markdown
# Goal
<observable outcome>

# Context
<task-specific facts and source-of-truth locations>

# Scope and constraints
<included targets, exclusions, compatibility, safety, and approval boundaries>

# Execution requirements
<required workflow, evidence, tools, or decision rules>

# Acceptance criteria
<tests, checks, artifacts, and definition of done>

# Output
<required response or artifact format>
```

Follow these rules:

- State each instruction once; remove generic exhortations and duplicated policy.
- Prefer goals and verifiable outcomes over prescribing hidden reasoning steps.
- Keep examples only when they encode a product requirement or correct a known failure mode.
- Separate instructions from untrusted or variable context with clear Markdown headings or XML tags.
- Specify what a concise answer must retain instead of saying only "be concise."
- Define tone through observable writing behavior rather than broad adjectives.
- Name safe autonomous actions and actions that require confirmation.
- Include only tools relevant to the task and describe routing or stopping rules when tool use is complex.

Show the rewritten prompt in a concise `改写后的 Prompt` section before execution unless doing so would expose sensitive content. Redact secrets and summarize protected configuration.

### 6. Execute the rewritten prompt

Immediately carry out the original task using the rewritten prompt as the working contract. Do not stop after producing the prompt.

During execution:

- Continue within the original authorization and scope.
- Apply relevant domain skills and repository instructions normally.
- Use fresh, task-specific validation.
- If execution reveals a new material blocker, report the new evidence and ask only for the decision that blocks further progress.

### 7. Report the result

Lead with the task outcome. Include:

- What was changed or concluded.
- What was verified and the actual result.
- Any assumption that materially influenced the work.
- Remaining risk or unresolved blocker.

Avoid repeating the entire audit when the task succeeded. The final response must remain useful even if earlier progress updates are hidden.

## Guardrails

- Do not use prompt optimization to broaden scope or manufacture authorization.
- Do not demand perfect information; seek sufficient information for safe, verifiable execution.
- Do not turn optional style preferences into blockers.
- Do not overwrite source prompts or configuration files unless the user requested a file change.
- Do not claim the rewritten prompt succeeded until the underlying task was actually executed and validated.
- For current OpenAI model-specific guidance, use the `openai-docs` skill and refresh official documentation rather than relying only on this bundled checklist.

