---
name: feature
description: 优云新功能开发完整闭环工作流
color: purple
---

你现在要严格按照以下 5 个阶段顺序执行，不能跳跃、不能合并阶段。每个阶段完成后，必须等待用户确认「继续」或提出修改意见后再进入下一阶段。

阶段 1：需求分析
─────────────────
→ 调用 agent: requirement-analyst
使用用户输入的需求描述 + 当前代码库上下文，生成完整的需求分析报告。
输出格式：保存为 docs/feature-xxx/01-需求分析.md，并展示主要内容。
完成后询问：需求分析是否OK？需要修改？输入「继续」进入下一阶段。

阶段 2：任务规划
─────────────────
→ 调用 agent: task-planner
参考需求分析 + 知识库中的架构/接口规范，生成详细的任务拆分规划。
输出：保存为 docs/feature-xxx/02-任务规划.md
包含：技术选型、文件清单、模块职责、实现步骤 checklist、潜在风险
询问用户确认。

阶段 3：代码生成
─────────────────
→ 调用 agent: code-generator
按照任务规划逐文件/逐模块生成代码。
支持增量生成：一次只实现 1-3 个文件，用户确认后继续。
每生成一批文件后，自动 diff 并询问是否应用。

阶段 4：代码审查
─────────────────
→ 调用 agent: code-reviewer
对所有新增/修改代码进行全面审查。
生成审查报告 → 保存为 docs/feature-xxx/03-代码审查报告.md
如果有问题，建议修复 → 返回阶段 3 迭代

阶段 5：单元测试
─────────────────
→ 调用 agent: unit-test-writer
为新增/修改代码编写单元测试。
生成测试文件 + 测试报告 → 保存为 docs/feature-xxx/04-单元测试报告.md
最后总结整个 feature 的完成度，并建议下一步（git commit / pr 等）

知识库引用规则：
- 每次规划/生成前，必须先 grep / read 知识库中的「架构」「接口」相关 md
- 路径约定：knowledge-base/architecture.md 和 knowledge-base/interfaces.md

开始时先问用户：请描述你要开发的新功能（越详细越好），以及 feature 的 slug/name（用于创建文件夹，例如 user-authentication）。