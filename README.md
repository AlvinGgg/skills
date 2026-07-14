# Prompt Readiness

[English](README.en.md) | [Apache-2.0](LICENSE)

一个面向 Codex 与其他 Agent 环境的独立 Skill：在真正执行前，将模糊或信息不全的请求审计、补齐并改写为安全、可验证的执行 Prompt，然后完成原任务。

## 30 秒开始

使用 Skills CLI 安装：

```bash
npx skills add https://github.com/AlvinGgg/skills --skill prompt-readiness
```

也可以直接克隆到 Codex 的 Skill 目录：

```bash
git clone https://github.com/AlvinGgg/skills.git ~/.codex/skills/prompt-readiness
```

安装完成后，应能看到 `SKILL.md`、`agents/` 与 `references/`。

## 使用方式

在任务开头显式调用：

```text
$prompt-readiness

检查 payment-service 的生产部署失败原因；只做诊断，不修改集群。
```

该 Skill 仅支持显式调用。若需要补充关键信息，请在下一条消息中再次带上 `$prompt-readiness`。

## 它会做什么

1. 保留原始目标、范围和授权边界。
2. 检查当前对话、相关文件、配置与源代码，避免要求重复已知信息。
3. 区分真正阻塞执行的问题、可安全采用的默认假设和可选偏好。
4. 仅对会影响实现、权限、安全或验收结果的缺口提出一个汇总问题。
5. 将请求改写为含目标、上下文、约束、执行要求、验收标准和输出要求的 Prompt。
6. 立即执行并验证原始任务，而不是止步于 Prompt 改写。

## 适合的场景

- 编码、排障、配置变更、研究或运维任务开始前，需要先确认可执行条件。
- 需要将需求转成有明确范围、约束和验收标准的执行 Prompt。
- 需要在行动前检查仓库上下文、配置优先级或安全边界。

## 不适合的场景

对于目标、上下文和验收标准已经完整的简单任务，直接执行通常更高效。该 Skill 也不替代项目规范或领域 Skill；执行阶段仍须遵循适用的工程、运维与安全指令。

## 目录结构

```text
.
├── SKILL.md
├── agents/openai.yaml
└── references/readiness-checklist.md
```

## 安全与隐私

Skill 只读取与任务有关的材料；不得展示凭据、令牌或无关的隐藏配置。改写 Prompt 时会保留既有授权边界，并对敏感信息进行脱敏或概述。

## 许可证

本项目采用 [Apache License 2.0](LICENSE)。
