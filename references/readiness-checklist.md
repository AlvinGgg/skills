# Prompt Readiness Checklist

Use this checklist to judge sufficiency, not to force every prompt into the same template. Load only the portions relevant to the task.

## Readiness dimensions

### Goal and task mode

- Is the desired outcome observable?
- Is the request asking for analysis, diagnosis, planning, editing, implementation, operation, or monitoring?
- Is the user asking for a recommendation or authorizing an action?

### Context and source of truth

- Are the relevant repository, files, environment, inputs, and current behavior known?
- Can missing facts be obtained through safe read-only inspection?
- Is the real configuration or runtime path known rather than inferred from names?
- Are supporting documents or retrieved data clearly delimited from instructions?

### Scope and authority

- What is included and explicitly excluded?
- Which local, reversible actions may proceed autonomously?
- Which external, destructive, costly, privacy-sensitive, or scope-expanding actions require confirmation?
- Could an apparently simple rewrite alter a compatibility or stored-data contract?

### Acceptance and evidence

- What result defines success?
- Which tests, builds, renders, HTTP checks, runtime probes, or evidence are required?
- What should happen if full validation is unavailable?
- What risks or caveats must remain in the final answer?

### Output contract

- What format or artifact is required?
- Which facts, evidence, caveats, decisions, and next actions must survive a concise response?
- What details may be omitted first?
- Is tone described through specific writing choices?

## Prompt-writing principles

Apply these principles from OpenAI's GPT-5.6 and prompt-engineering guidance:

- Favor lean prompts. Remove repeated instructions and irrelevant examples or tools one group at a time, then rerun representative evaluations.
- State domain context, hard constraints, approval boundaries, success criteria, and the condition that should trigger a question.
- Organize reusable developer instructions around identity, instructions, examples, and context. Use only the sections the task needs.
- Use Markdown hierarchy and XML delimiters to separate instructions, examples, and supporting data.
- Keep stable reusable prefixes before variable request context when prompt caching matters.
- Use few-shot examples only to demonstrate a required pattern or correct a measured gap; make examples diverse enough to cover relevant boundaries.
- Control default detail with API settings such as `text.verbosity` when available, and use the prompt for task-specific content requirements.
- Define autonomy compactly: inspection-only requests do not authorize changes; change requests may permit in-scope local edits and non-destructive validation; external or destructive actions require confirmation.
- For difficult or pro-mode tasks, keep the prompt outcome-focused. Do not add instructions to "think harder" or reveal hidden reasoning.
- Evaluate prompt changes on representative tasks using correctness, completeness, required evidence, latency, token use, and cost.

## Gap decision test

Treat a gap as material only if at least one is true:

1. Different answers would lead to substantially different artifacts or conclusions.
2. Proceeding could exceed the user's authority or cause an external, destructive, costly, or privacy-sensitive effect.
3. Success cannot be verified without the missing value.
4. The available sources conflict and the user must choose the intended behavior.

Otherwise, prefer a stated safe assumption or discover the information through read-only inspection.

## Example audit

Rough request:

```text
修复登录问题，改完帮我上线。
```

Audit:

- Goal: partially known; the observed login failure is missing.
- Target: repository and deployment environment are missing.
- Evidence: error, reproduction, logs, and desired behavior are missing.
- Authority: code changes appear authorized; production deployment is high-impact and the target is unclear.
- Acceptance: no test or runtime success condition is given.

One consolidated question:

```text
请提供登录问题的复现/错误信息、目标仓库与环境，以及“上线”指测试环境还是真实生产发布；这些信息分别决定根因定位范围、修改目标和是否需要生产变更授权。若已有验收方式，也请一并给出。
```

After the answer, rewrite the prompt with the confirmed goal, source paths, permitted actions, deployment boundary, checks, and final reporting format, then perform the work.

## Official sources

- GPT-5.6 model guidance: https://developers.openai.com/api/docs/guides/latest-model
- Prompt engineering: https://developers.openai.com/api/docs/guides/prompt-engineering

