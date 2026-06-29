# teach-me

Interactive learning coach skill for Pi/Agent. Guides a learner through intake, resource collection, study planning, concise teaching, exercises, progress tracking, and visualizations.

## Usage

```text
/skill:teach-me 我想学习 JavaScript Promise
/skill:teach-me 继续学习 JavaScript Promise
/skill:teach-me 查看学习进度
```

## Installation

```text
~/.pi/agent/skills/teach-me/
```

Pi discovers any directory containing `SKILL.md`.

As a Pi package, it can also be installed from git:

```bash
pi install git:github.com/isaacveg/teach-me
```

## Repository

```text
package.json                  # Pi package manifest
SKILL.md                      # entrypoint, core rules, storage conventions
references/workflow.md        # detailed 6-step workflow
templates/
├── intake-questions.md       # diagnostic questions
└── plan-template.md          # lesson plan structure (includes resources + log)
```

## Runtime Data

Three top-level entries per learning session, stored in the user's project:

```text
.pi/learning-sessions/<topic-slug>/
├── state.json    # minimal teaching state for quick resume
├── plan.md       # lesson plan, progress, resources, and learning log
└── materials/    # generated visuals and collected files
```

No separate resources file, no separate logs file, no index registry. `plan.md` is the single main human-readable artifact; `state.json` exists only for quick resume, and `materials/` stores supporting files.
