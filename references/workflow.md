# Teach Me Workflow

Follow this workflow in order unless the user is clearly resuming an existing session.

## 0. Determine Mode

Classify the user's request as one of:

- `new`: the user wants to learn a new topic/course.
- `resume`: the user wants to continue a previous session.
- `progress`: the user wants to view or modify progress.
- `summary`: the user wants a daily/session summary.
- `style-change`: the user requests a teaching preference change.
- `question`: the user asks a one-off question inside an existing learning context.

For `resume`, `progress`, `summary`, and `style-change`, inspect the persisted session files before responding.

## 1. Intake / Diagnostic Questions

Do not start teaching before collecting enough information.

Ask for:

1. Learning target: what does the user want to learn?
2. Scope: single knowledge point or full course?
3. Desired depth: overview, usable skill, deep principle, exam/interview, project practice, or custom.
4. Existing foundation: what related knowledge does the user already have?
5. Weak points: what is confusing or unknown?
6. Time budget: total time, per-session time, deadline if any.
7. Materials: model knowledge only, web search, provided URLs, local files/folders, code repo, notes.
8. Exercise preference: none, light, normal, heavy.
9. Difficulty preference: easy, medium, hard, adaptive.
10. Teaching preferences: concise/detailed, examples, visualizations, analogy style, code-first, theory-first, etc.
11. Language: infer from first invocation and confirm only if ambiguous.

If the user already provided some answers, do not ask again. Ask only missing high-value questions.

## 2. Resource Collection

If the user requests web resources or internet-based learning:

- Use multiple distinct search queries, not just one keyword.
- Prefer official docs, reputable tutorials, source material, and high-signal references.
- Summarize the direction and relevance of each resource, not every detail.

If the user provides local files or folders:

- Use file listing/search tools to inspect the structure.
- Read relevant files selectively.
- Avoid reading a huge directory blindly.
- Summarize what each local source contributes.

If the user provides URLs:

- Fetch/read the pages if tools are available.
- Preserve URLs in `resources.md`.

After collection, produce exactly one concise scope-confirmation sentence in the user's teaching language. It must include:

- the directions/sources collected so far;
- the main learning points you expect to cover;
- a question asking whether this is enough or whether to add more directions.

Example:

> 我会结合官方文档、你提供的本地笔记和几篇入门资料，围绕 Promise 的状态模型、链式调用、错误传播、async/await 关系和工程实践来设计学习路径；这些方向是否足够，还是还要补充其他重点？

If the user adds new directions, repeat only the necessary resource collection and scope confirmation.

## 3. Study Plan

Create a detailed but not overwhelming study plan with chapters and subtopics.

Rules:

- Use `[ ]`, `[-]`, `[x]` markers.
- Mark user-known prerequisites as `[x]` with a note such as “已掌握 / already known”.
- Mark only one current item as `[-]` unless there is a good reason.
- Leave future items as `[ ]`.
- Each subtopic should have a clear learning objective.
- Add notes where useful: mastery, doubt, half-finished point, prerequisite, exercise result, or review suggestion.

Keep both:

- `plan.md`: human-readable plan.
- `state.json.progress`: machine-readable progress.

## 4. Plan Approval

After showing the plan, ask the user to choose:

1. Modify the plan.
2. Agree and start learning now.
3. The plan is fine, but start later.

If the user chooses 1, ask for or apply modifications, update files, and show the three options again.

If the user chooses 2, begin the interactive learning loop.

If the user chooses 3, save all files and tell the user the plan is ready and can be resumed later.

## 5. Interactive Teaching Loop

For the current knowledge item:

1. Read `state.json` and `plan.md` to know what is current.
2. Teach one small concept only.
3. Keep the explanation concise unless the user's preferences say otherwise.
4. For abstract ideas, include a concrete example.
5. If a visual helps, use ASCII/Markdown first; generate HTML only when worthwhile or requested.
6. Explain with cause → process → result, or shallow → deep structure.
7. Leave room for thinking. Ask whether the user wants clarification, more examples, an exercise, or to continue.
8. When the user wants to move to the next item, offer a suitable question/checkpoint first unless exercises are disabled.
9. If the user answers, judge correctness and explain why.
10. If the user passes or confirms understanding, update the item to `[x]` and move the next item to `[-]`.
11. If the user is uncertain or wrong, keep the item as `[-]`, add a note, and provide a targeted explanation or easier exercise.

## 6. Teaching Style Changes

When the user asks for any teaching-style change, update the state immediately before continuing.

Examples:

- “讲慢一点” → `pace: slow`
- “每次少讲点” → `explanationLength: short`
- “多举例” → `exampleStyle: concrete` or add note
- “不要做题” → `exerciseFrequency: none`
- “题难一点” → `exerciseDifficulty: hard`
- “多用图” → `visualization: frequent`
- “以后用英文讲” → `language: en`

After updating, briefly confirm the change in the current teaching language and follow it strictly.

## 7. End-of-Session Summary

When the user asks to finish, summarize today, stop for now, or when a natural study block ends:

1. Update `plan.md`.
2. Update `state.json`, especially `resumeSummary`, `current`, `updatedAt`, and teaching preferences if changed.
3. Append a dated entry to `logs.md`.
4. Include what was learned, user requests, mastery, mistakes, unresolved doubts, and next step.
5. Tell the user how to resume.
