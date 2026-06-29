# Plan Template

Use this structure in `plan.md`. Translate all user-facing text to the session teaching language. The template below uses Chinese as an example — adapt to the actual teaching language.

```md
# <title>

> 目标：<one sentence — what the learner wants to achieve>
> 语言：<language> | 步调：<pace> | 进度：<completed>/<total> | 更新：<date>
> 保存目录：<session-directory>

## 资料

- <type>: <title> — <URL or path> — <brief note on what it covers>
- 模型知识 — <topics covered from model knowledge>
- 生成材料 — <materials/path> — <only if generated or user-provided material exists>

## <Chapter 1>

- [x] <item-id> <title> — <note: why completed / already known>
- [-] <item-id> <title> — <note: current progress, doubts>
- [ ] <item-id> <title>
  目标：<what the learner should be able to do>

## <Chapter 2>

- [ ] <item-id> <title>
  目标：<learning objective>

---

## 学习记录

### <date>

- 学习了：<items covered>
- 掌握情况：<per-item assessment>
- 练习：<what was asked, how user answered, result>
- 疑问：<unresolved questions>
- 下次：<next item to teach>
```

## Rules

- Only one `[-]` item at a time once learning starts.
- Each item note should be one line when possible: mastery, doubt, exercise result, skip reason, etc.
- The "资料" section lists all collected resources inline — no separate resources file.
- The "学习记录" section is appended after each session — no separate logs file.
- Generated/downloaded/supporting materials go in `materials/` and are referenced from item notes or the 资料 section.
- HTML visualizations must be generated only after explicit user confirmation/request. Keep them minimal and instructional.
- After any progress change, update `plan.md`. This IS the human-readable record.
