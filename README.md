# teach-me

`teach-me` is a Pi/Agent skill for interactive learning. It guides a learner through intake, resource collection, study planning, concise teaching, exercises, progress tracking, visualizations, and learning logs.

During learning, the agent may call itself `coach`, but the skill name is always `teach-me`.

## Usage

```text
/skill:teach-me 我想学习 JavaScript Promise
/skill:teach-me 继续学习 JavaScript Promise
/skill:teach-me 查看学习进度
```

## Installation for Pi

Clone or copy this repository into a Pi skill discovery location, for example:

```text
~/.pi/agent/skills/teach-me/
```

Pi discovers a skill directory that contains `SKILL.md`. For local development/testing from this repository, start Pi with this skill path explicitly if needed:

```bash
pi --skill .
```

## Repository Structure

```text
SKILL.md                         # skill entrypoint and core rules
references/workflow.md           # detailed workflow
references/storage.md            # minimal persistence rules
schemas/state.schema.json        # state file schema
templates/intake-questions.md    # intake question template
templates/plan-template.md       # plan template
templates/log-template.md        # learning log template
```

## Runtime Data

This repository is the skill source code. User learning sessions should be stored in the consuming project, not in this repository:

```text
.pi/learning-sessions/<topic-slug>/
```

Runtime files include `state.json`, `plan.md`, `resources.md`, `logs.md`, and optional `visuals/*.html`.
