# Teach Me Workflow

## 0. Determine Mode

| Mode | Trigger | Action |
|------|---------|--------|
| `new` | User wants to learn a new topic | Go to Step 1. Phase = `intake`. |
| `resume` | "继续学习", "resume", etc. | Read state.json + plan.md. Jump to current phase, enter teaching loop. |
| `progress` | "查看进度", "progress" | Read state.json + plan.md. Display progress. |
| `summary` | "总结", "今天学了什么" | Read plan.md. Summarize. Append log entry to plan.md. |
| `style-change` | "慢一点", "用英文", etc. | Read state.json. Update preferences. Confirm. |
| `question` | One-off question in existing context | Read state.json for language/preferences. Answer in teaching style. |

`phase` in state.json tracks workflow position. For `new`, it advances: `intake` → `planning` → `learning` → `done`. Other modes re-enter at the current phase without changing it.

## 1. Intake

Do not start teaching yet. Ask only missing high-value questions:

1. What to learn? (single point or full course?)
2. Desired depth? (overview, usable, deep principle, exam, project)
3. Existing foundation? Weak points?
4. Time budget? (total, per session, deadline)
5. Materials? (model knowledge only, web search, URLs, local files)
6. Learning style? (concise/detailed, examples, visuals, exercises, difficulty)

If user resists, use fast-start (4 questions, see `templates/intake-questions.md`). Mark assumptions.

After intake, create the session directory and write initial `state.json` with `phase: intake`, inferred `language`, and `updated`. If user chose model-knowledge-only, skip to Step 3.

## 2. Resource Collection

If user wants web resources:
- Multiple distinct search queries.
- Prefer official docs, reputable tutorials.
- Summarize direction and relevance.
- Save findings to plan.md resources section.

If user provides local files/URLs:
- Inspect selectively. Don't read huge directories blindly.
- Summarize contribution.

If web search is unavailable, fall back to model knowledge and tell the user.

After collection, produce one scope-confirmation sentence covering sources and learning points. Ask if scope is sufficient.

## 3. Study Plan

Create study plan in `plan.md` using `templates/plan-template.md`.

- Chapters and subtopics with clear learning objectives.
- Mark user-known items `[x]` with note.
- Only one item `[-]` at a time.
- Include resources section with all collected materials.
- Set `state.json.phase` to `planning`.

Show the plan. Ask user to: (1) modify, (2) start now, or (3) save for later.

If (2): set `phase: learning`, mark first item `[-]`, set `current` in state.json, enter teaching loop.
If (3): save and tell user how to resume.

## 4. Interactive Teaching

For the current item (`state.json.current`):

1. Read state.json and plan.md to confirm current position.
2. Teach one small concept (the current item).
3. Include a concrete example for abstract ideas.
4. Use ASCII/Markdown visuals first; generate HTML only when complexity warrants it. Save to `materials/`.
5. Leave room for thinking. Ask: clarify, more examples, exercise, or continue?
6. Before moving to next item, offer a check question (unless `exerciseFrequency: none`).
   - `easy`: recall/definition
   - `medium`: apply or compare
   - `hard`: analyze, debug, design
7. Judge the answer and explain.
8. If correct: mark item `[x]` in plan.md, mark next item `[-]`, update `state.json.current`.
9. If incorrect: keep `[-]`, add a note, re-explain or give an easier exercise.

After each status change, update plan.md. Update state.json when `current` changes, preferences change, important doubts/wrong answers appear, or the session may be interrupted. Refresh `updated` and `resume` on those meaningful changes.

If user asks a tangential question (question mode within an active session): answer it in teaching style. If related to a plan item, note it. Don't change progress unless user asks.

## 5. Teaching Style Changes

Update `state.json.preferences` immediately, confirm the change, follow it from the next turn.

| User says | Field update |
|-----------|-------------|
| "讲慢一点" | `pace: slow` |
| "多举例" | `explanationLength: long` (with examples) |
| "不要做题" | `exerciseFrequency: none` |
| "题难一点" | `exerciseDifficulty: hard` |
| "多用图" | `visualization: frequent` |
| "用英文讲" | `language: en` |

## 6. End of Session

When user finishes or a natural block ends:

1. Update plan.md: append a dated log entry at the bottom covering what was learned, exercises, doubts, next step.
2. Update state.json: set `resume` to a one-sentence summary (current item, open questions, next step), refresh `updated`, and set `phase` to `paused` (or `done` if completed).
3. Tell user how to resume: `/skill:teach-me 继续学习 <topic>`

## Graceful Degradation

- **No web search**: Fall back to model knowledge. Note in plan.md resources.
- **No file write**: Warn user. Operate from memory. Remind at end that progress will be lost.
- **No HTML generation**: Use ASCII diagrams and Markdown tables only.
