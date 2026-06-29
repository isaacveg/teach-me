# teach-me

Interactive learning coach skill. Guides a learner through intake, resource collection, study planning, structured teaching, exercises, progress tracking, and user-confirmed visualizations.

## Usage

```text
/skill:teach-me 我想学习 JavaScript Promise
/skill:teach-me 继续学习 JavaScript Promise
/skill:teach-me 查看学习进度
```

If your agent does not support `/skill:` commands, ask it to load or follow `SKILL.md`.

## Installation

Use whichever skill-loading mechanism your agent supports. This repository follows the common Agent Skills layout:

```text
teach-me/
├── SKILL.md
├── references/
└── templates/
```

For agents that read repository instructions, `AGENTS.md` points them to the skill entrypoint.

## Repository

```text
package.json                  # package metadata and compatibility manifest
AGENTS.md                     # generic agent entrypoint for agents that read repo instructions
SKILL.md                      # skill entrypoint and core rules
references/workflow.md        # detailed workflow
templates/
├── intake-questions.md       # diagnostic questions
└── plan-template.md          # lesson plan structure (includes resources + log)
```

## Runtime Data

For a new learning session, the coach asks where to save state. If the current folder is accepted, the default is:

```text
.teach-me/sessions/<topic-slug>/
├── state.json    # minimal teaching state for quick resume
├── plan.md       # lesson plan, progress, resources, and learning log
└── materials/    # generated visuals and collected files
```

If the user specifies another folder, sessions are stored there instead. No separate resources file, no separate logs file, no index registry. `plan.md` is the single main human-readable artifact; `state.json` exists only for quick resume, and `materials/` stores supporting files.
