# Intake Questions Template

Ask in the user's teaching language. Do not ask all questions if the user already answered some of them. Prefer a compact numbered list.

## Compact Intake

1. 你想学什么？这是一个具体知识点，还是一门完整课程？
2. 你希望学到什么程度：入门了解、能使用、深入原理、考试/面试、项目实践，还是其他？
3. 你现在有哪些相关基础？哪些地方最不熟？
4. 你预计投入多少时间：总时间、每次学习时长、是否有截止时间？
5. 学习资料来源是什么：只用我已有知识、联网搜索、指定网页、本地文件夹/笔记/代码仓库？
6. 你希望怎样学：每次简短一点还是详细一点？要不要例子、图示、练习题？题目难度如何？

## If User Wants Fast Start

If the user wants to start quickly, ask only the minimum:

1. 学习主题是什么？
2. 目标深度是什么？
3. 你已有基础是什么？
4. 是否需要使用网页或本地资料？

Then create a provisional plan and mark assumptions clearly.

## Language Handling

- Infer the teaching language from the user's first invocation.
- If ambiguous, ask: “你希望我后续用哪种语言教学？” / “Which language should I use for teaching?”
- Save the chosen/inferred language in `state.json` when a session is created.
