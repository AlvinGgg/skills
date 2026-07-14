# Prompt Readiness

[中文](README.md) | [Apache-2.0](LICENSE)

A standalone Skill for Codex and other agent environments. Before acting, it audits, completes, and rewrites an underspecified request into a safe, verifiable execution prompt, then completes the original task.

## Start in 30 seconds

Install with the Skills CLI:

```bash
npx skills add https://github.com/AlvinGgg/prompt-readiness-skill --skill prompt-readiness
```

Or clone it directly into Codex's Skill directory:

```bash
git clone https://github.com/AlvinGgg/prompt-readiness-skill.git ~/.codex/skills/prompt-readiness
```

After installation, verify that `SKILL.md`, `agents/`, and `references/` exist.

## Ask an AI Agent to install it

Send the following prompt to an AI agent with shell access:

> Install the `prompt-readiness` Skill for me.
>
> 1. Ensure that `~/.codex/skills/` exists.
> 2. Clone `https://github.com/AlvinGgg/prompt-readiness-skill.git` into `~/.codex/skills/prompt-readiness`.
> 3. Verify that the directory contains `SKILL.md`, `agents/openai.yaml`, and `references/readiness-checklist.md`.
> 4. Report the installation result and tell me that I can invoke it explicitly with `$prompt-readiness` at the start of a task.

## Usage

Invoke it explicitly at the start of a task:

```text
$prompt-readiness

Diagnose the failed production deployment of payment-service. Do not modify the cluster.
```

This Skill supports explicit invocation only. If it needs material information, include `$prompt-readiness` again with the requested details in your next message.

## What it does

1. Preserve the original goal, scope, and authorization boundary.
2. Inspect the relevant conversation, files, configuration, and source code instead of asking for known information again.
3. Separate true blockers from safe defaults and optional preferences.
4. Ask one consolidated question only for gaps that change implementation, authority, safety, or acceptance.
5. Rewrite the request around the goal, context, constraints, execution requirements, acceptance criteria, and output.
6. Execute and validate the original task immediately rather than stopping after prompt rewriting.

## Good fits

- Confirming the conditions for safe execution before coding, diagnosis, configuration, research, or operations work.
- Turning a request into an execution prompt with explicit scope, constraints, and acceptance criteria.
- Checking repository context, configuration precedence, or safety boundaries before acting.

## Not a fit

For a simple task with a complete goal, context, and acceptance criteria, executing directly is usually more efficient. This Skill also does not replace project guidance or domain Skills; execution must still follow applicable engineering, operations, and security instructions.

## Layout

```text
.
├── SKILL.md
├── agents/openai.yaml
└── references/readiness-checklist.md
```

## Safety and privacy

The Skill reads only task-relevant material and must not expose credentials, tokens, or unrelated hidden configuration. It preserves the existing authorization boundary and redacts or summarizes sensitive information when rewriting a prompt.

## License

This project is licensed under the [Apache License 2.0](LICENSE).
