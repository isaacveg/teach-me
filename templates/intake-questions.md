# Intake Questions

Ask in the user's teaching language. Extract answers already given. Ask only what's missing. Do not make the user fill a long form.

## Standard Intake

Start with a compact message like this, adapted to the user's language and context:

1. 你想学什么？这是一个知识点，还是一门课程/路线？
2. 目标深度是什么？
   - A. 入门理解
   - B. 能实际使用
   - C. 深入原理
   - D. 考试/面试
   - E. 项目实践
   - F. 读论文/算法/源码
3. 你现在有哪些相关基础？哪里最不熟？
4. 你希望的学习步调是？
   - A. 快速重点版：简单基础略过，抓主线
   - B. 正常节奏：讲解、例子、检查题平衡
   - C. 慢速打基础：更多例子和确认
   - D. 先总览后深入：先看地图，再逐步细讲
5. 资料从哪来？
   - A. 只用 coach 已有知识
   - B. 适当联网
   - C. 指定网址/论文/课程
   - D. 本地文件/笔记/代码仓库
6. 学习状态保存在哪里？
   - A. 当前文件夹下：`./.teach-me/sessions/`
   - B. 我指定一个目录
   - C. 暂时不保存 / dry-run

## Defaults

Unless the user says otherwise:

- Default to reasonably detailed teaching.
- Use examples for abstract concepts.
- Use adaptive exercises/check questions.
- Use ASCII/Markdown visual aids when simple.
- Ask before generating HTML visualization.
- Do not ask for total learning time or per-session time unless the user mentions a deadline, exam, project milestone, or schedule constraint.

## Fast Start

If the user wants to start quickly, ask only:

1. 学什么，目标是什么？
2. 你已有基础是什么？
3. 需要联网或本地资料吗？
4. 学习步调：快速重点版 / 正常节奏 / 慢速打基础 / 先总览后深入？
5. 可以把学习状态保存在当前文件夹下的 `./.teach-me/sessions/` 吗？

Mark assumed answers with `[assumed]` prefix in the plan.

## Language

- Infer from first message. Confirm only if genuinely ambiguous.
- Save to `state.json.language` when session directory is created.
