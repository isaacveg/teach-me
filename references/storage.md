# Storage Rules

Use a minimal file-based storage system. Do not use SQL or any complex database.

## Root Location

All session data should live under the current project:

```text
.pi/learning-sessions/
```

Maintain an index:

```text
.pi/learning-sessions/index.md
```

Each topic/course gets one directory:

```text
.pi/learning-sessions/<topic-slug>/
```

Use a simple slug: lowercase if possible, hyphen-separated, ASCII when reasonable. If the topic is non-ASCII, either transliterate or use a short safe slug.

## Files

```text
state.json      # machine-readable state
plan.md         # human-readable plan/progress
resources.md    # web/local/model resources and directions
logs.md         # learning journal
visuals/        # generated self-contained HTML visualizations
```

## state.json

`state.json` is the source of truth for machine-readable state.

Important fields:

- `version`
- `skill`
- `title`
- `slug`
- `createdAt`
- `updatedAt`
- `phase`
- `teachingPreferences.language`
- `teachingPreferences`
- `learner`
- `resources`
- `current`
- `progress`
- `artifacts`
- `resumeSummary`

Read `schemas/state.schema.json` before creating or heavily modifying this file.

## plan.md

`plan.md` is the source of truth for human-readable progress display.

Use only:

- `[ ]` not started
- `[-]` in progress
- `[x]` completed or already mastered

Add notes below items when useful.

Keep `plan.md` consistent with `state.json.progress`.

## resources.md

Record the source direction, not every detail.

Include:

- Web searches and query themes.
- URLs and titles.
- Local files/folders read.
- User-provided materials.
- Model-knowledge-only if no external materials are used.
- Which learning points each resource supports.

## logs.md

Append, do not overwrite unless fixing a mistake.

Each log entry should include:

- Date/time if available.
- What the user learned.
- What the user asked for.
- Teaching preference changes.
- Exercises attempted.
- Wrong answers and retained questions.
- Mastery assessment.
- Next recommended step.

## visuals/

When generating HTML visualizations:

- Save under `visuals/`.
- Use standalone HTML with inline CSS/JS.
- Avoid external dependencies by default.
- Name files with an order prefix, e.g. `001-promise-state.html`.
- Record the file in `state.json.artifacts.visuals` and mention it in `logs.md` or `plan.md` when relevant.

## Update Timing

Update files immediately when:

- A plan is created or revised.
- A learning item changes status.
- The user changes teaching preferences.
- An exercise is answered incorrectly or reveals a misconception.
- A visualization or resource file is created.
- The user asks to summarize or stop.

Before responding to a request that depends on persisted progress, read the files first.
