# Teach Me Workflow

## 0. Determine Mode

| Mode | Trigger | Action |
|------|---------|--------|
| `new` | User wants to learn a new topic | Go to Step 1. Phase = `intake`. |
| `resume` | "继续学习", "resume", etc. | Locate a session from the current folder first, then read `state.json` + `plan.md`. Jump to current phase and enter teaching loop. |
| `progress` | "查看进度", "progress" | Locate a session, read `state.json` + `plan.md`, display progress. |
| `summary` | "总结", "今天学了什么" | Locate a session, read `plan.md`, summarize, append log entry to `plan.md`. |
| `style-change` | "慢一点", "用英文", etc. | Locate the active session if any, read `state.json`, update preferences, confirm. |
| `question` | One-off question in existing context | Read `state.json` if a session is active; answer in teaching style. |
| `dry-run` | User says they are testing/simulating/walking through the skill | Ask whether to write real files or run without persistence. If dry-run, do not create normal session files. |

`phase` in `state.json` tracks workflow position. For `new`, it advances: `intake` → `planning` → `learning` → `done`. Other modes re-enter at the current phase without changing it.

## Session Location Rules

Before creating a new real session, ask:

> 可以把学习状态保存在当前文件夹下吗？默认会使用 `./.teach-me/sessions/<slug>/`。你也可以指定其他目录。

If the user agrees, use:

```text
<current-working-folder>/.teach-me/sessions/<slug>/
```

If the user specifies another storage base, use:

```text
<user-specified-folder>/<slug>/
```

If the user specifies an existing session directory containing `state.json` and `plan.md`, use that directory directly.

For resume/progress/summary, locate sessions in this order:

1. If current folder contains `state.json` and `plan.md`, use current folder as the session directory.
2. Else look for session directories under `./.teach-me/sessions/`.
3. If none are found, ask the user to either open the folder containing the learning session or provide the session/storage directory path.
4. If multiple sessions are found, list them with title/updated/resume if available and ask which one to use.

Do not create a new session while handling a resume request unless the user explicitly asks to start over.

## Planning Wizard Display

During setup and planning, show a lightweight step marker so the user knows where they are:

```text
规划阶段 Step 1/4：摸底
规划阶段 Step 2/4：资料与范围确认
规划阶段 Step 3/4：生成学习计划
规划阶段 Step 4/4：确认是否开始
```

Rules:

- Show the step marker when entering each planning step.
- Do not store this step counter in `state.json` or `plan.md`.
- Keep transitions brief: explain what just happened and what happens next.

## 1. Intake

Show: `规划阶段 Step 1/4：摸底`.

Do not start teaching yet. Ask only missing high-value questions. Keep the first intake compact.

Collect:

1. Learning target and scope: what to learn; single point or full course?
2. Target depth: overview, usable skill, deep principle, exam/interview, project/practice, paper/algorithm reading, or custom.
3. Existing foundation and weak points.
4. Learning pace, using options:
   - `快速重点版` — skip obvious basics, focus on key ideas.
   - `正常节奏` — balanced explanation and examples.
   - `慢速打基础` — more examples and checks.
   - `先总览后深入` — roadmap first, then details.
5. Material source: model knowledge only, web search when useful, provided URLs, local files/folders, code repo, notes.
6. Storage location: ask whether current folder is okay for saving session files, or whether the user wants to specify another folder.

Do **not** ask for total learning time or per-session time by default. Ask about schedule only if the user mentions a deadline, exam, project milestone, or time constraint.

Default assumptions unless the user says otherwise:

- Explain with enough detail to be useful.
- Use concrete examples for abstract ideas.
- Use adaptive check questions/exercises.
- Use ASCII/Markdown visuals directly when simple.
- Ask before generating HTML visualizations.

If user resists, use fast-start questions from `templates/intake-questions.md`. Mark assumptions in `plan.md` with `[assumed]`.

After intake and storage consent, create the session directory and write initial `state.json` with `phase: intake`, inferred `language`, `updated`, and initial `preferences`. If user chose model-knowledge-only, skip to Step 3.

## 2. Resource Collection and Scope Confirmation

Show: `规划阶段 Step 2/4：资料与范围确认`.

If user wants web resources:

- Use multiple distinct search queries.
- Prefer official docs, reputable tutorials, papers, source docs, and high-signal references.
- Summarize direction and relevance, not every detail.
- Save findings to the `资料` section of `plan.md`.

If user provides local files/URLs:

- Inspect selectively. Do not read huge directories blindly.
- Summarize what each source contributes.
- Save paths/URLs and short notes to the `资料` section of `plan.md`.

If web search is unavailable, fall back to model knowledge and note that in `plan.md`.

After collection, produce one concise scope-confirmation sentence covering:

- sources/directions collected;
- main learning points to cover;
- whether the user wants to add anything.

If the user adds directions, repeat only the necessary collection and confirmation.

## 3. Study Plan

Show: `规划阶段 Step 3/4：生成学习计划`.

Create `plan.md` using `templates/plan-template.md`.

Rules:

- Chapters and subtopics with clear learning objectives.
- Mark user-known items `[x]` with a note.
- Only one item `[-]` at a time once learning starts.
- Include resources/materials section with all collected materials.
- Include notes where useful: mastery, doubt, exercise result, skip reason, or review suggestion.
- Set `state.json.phase` to `planning`.

Show the plan after creating it.

## 4. Plan Approval

Show: `规划阶段 Step 4/4：确认是否开始`.

Ask the user to choose:

1. Modify the plan.
2. Start learning now.
3. Save the plan and start later.

If (1): apply or ask for modifications, update files, then show the three options again.

If (2): set `phase: learning`, mark the first item `[-]`, set `current` in `state.json`, and enter the teaching loop.

If (3): save files, set `phase: paused`, update `resume`, and tell user how to resume.

## 5. Interactive Teaching

For the current item (`state.json.current`):

1. Read `state.json` and `plan.md` to confirm current position.
2. Teach one small concept, but make the explanation complete enough.
3. Default teaching shape:
   - why this concept exists;
   - core intuition;
   - concrete example;
   - connection to the user's learning goal;
   - ask whether to clarify, see another example, do a check, or continue.
4. Use ASCII/Markdown visuals directly when simple.
5. If HTML would help, ask first. Generate minimal HTML only after explicit user confirmation/request, then save to `materials/` and reference it in `plan.md`.
6. Before moving to the next item, offer a check question unless `exerciseFrequency: none`.
   - `easy`: recall/definition
   - `medium`: apply or compare
   - `hard`: analyze, debug, design
7. Judge the answer and explain.
8. If correct or the user confirms understanding: mark item `[x]` in `plan.md`, mark next item `[-]`, update `state.json.current`.
9. If incorrect or uncertain: keep `[-]`, add a note, re-explain with a better example or give an easier exercise.

After each status change, update `plan.md`. Update `state.json` when `current` changes, preferences change, important doubts/wrong answers appear, or the session may be interrupted. Refresh `updated` and `resume` on those meaningful changes.

If user asks a tangential question within an active session: answer in teaching style. If related to a plan item, note it. Do not change progress unless user asks or the answer clearly completes a planned item.

## 6. Teaching Style Changes

Update `state.json.preferences` immediately, confirm the change, and follow it from the next turn.

| User says | Field update |
|-----------|-------------|
| "讲慢一点" | `pace: slow` |
| "快一点 / 简单点" | `pace: fast` or `explanationDepth: concise` |
| "详细一点" | `explanationDepth: detailed` |
| "多举例" | `examples: more` |
| "不要做题" | `exerciseFrequency: none` |
| "题难一点" | `exerciseDifficulty: hard` |
| "多用图" | `visualization: frequent`, but still ask before HTML |
| "不要 HTML" | `htmlVisualization: never` |
| "用英文讲" | `language: en` |

## 7. End of Session

When user finishes or a natural block ends:

1. Update `plan.md`: append a dated log entry at the bottom covering what was learned, exercises, doubts, next step.
2. Update `state.json`: set `resume` to a one-sentence summary (current item, open questions, next step), refresh `updated`, and set `phase` to `paused` (or `done` if completed).
3. Tell user how to resume in natural language, for example: `继续学习 <topic>`.

## Graceful Degradation

- **No web search**: Fall back to model knowledge. Note in `plan.md` resources.
- **No file write**: Warn user. Operate from memory. Remind at end that progress will be lost.
- **No HTML generation**: Use ASCII diagrams and Markdown tables only.
