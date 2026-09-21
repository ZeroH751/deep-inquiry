---
name: deep-inquiry
description: Use ONLY when the user's message begins with the literal prefix ">>> " (three greater-than signs followed by a space). Activates 深度提问模式 (deep inquiry mode) - relentlessly interview the user one multiple-choice question at a time, walking each branch of the design tree and resolving decision dependencies until 90% confident, then deliver the final solution with a summary of decisions. Do not trigger otherwise.
---

# 深度提问模式（Deep Inquiry Mode）

当用户消息以 `>>> ` 前缀开头时，进入本模式。像访谈一样穷追不舍地询问用户计划的每一个细节，直到达成共同理解（shared understanding）。

## 核心规则

1. **一次只问一个问题** — 严禁一轮提出多个问题（one question at a time），等待回答后再继续
2. **只用选项弹窗提问** — 每个问题必须通过 `question` 工具呈现，严禁在回复中用纯文本提问
3. **基于回答追问** — 每个新问题都必须建立在上一个回答之上（follow-up）
4. **穷尽设计树** — 走遍设计树的每一个分支（branch），逐一解决决策之间的依赖关系（dependencies）
5. **禁止提前给方案** — 信心度未达到 90% 之前，只提问，不给解决方案
6. **达到阈值立即收尾** — 信心度 ≥ 90% 时，停止提问并给出最终方案

## 执行流程

### 第一步：分析问题，构建设计树
识别用户问题中的模糊点、缺失信息（missing information）和隐含假设（implicit assumptions），将所有需要澄清的维度组织成设计树（design tree）。

### 第二步：排序分支，解决依赖
按依赖关系对分支排序：先解决前置决策（dependencies），再解决依赖它们的后续决策。先问最关键、影响最大的分支。

### 第三步：逐个提问
每轮只问一个问题，等待用户回答。使用 `question` 工具呈现问题与选项。问题应当：
- 具体、明确，避免宽泛模糊
- 提供 2-4 个具体选项（multiple-choice），代表最有可能的解答或方向
- 选项要贴近真实答案、具有区分度；避免"是/否"这类通用选项（除非问题确实只有两种选择）
- 选项应基于当前上下文推测，让用户能快速选择而非从零组织语言
- 不要手动添加"其他"选项 — `question` 工具会自动提供自定义输入（custom answer）
- 根据上一个回答动态调整，不照本宣科

**能自己查的不问用户**：如果某个问题可以通过探索代码库或文件来解答，先自己探索（搜索、读取文件等工具），不要把可自行查明的问题抛给用户。

### 第四步：确认并追问
收到回答后：
1. 用 1-2 句话简短确认该决策（acknowledge the decision），不要长篇大论
2. 立即通过 `question` 工具提出下一个问题

### 第五步：评估信心度
每次收到回答后重新评估信心度（confidence level）。评估维度：
- 核心目标是否明确
- 关键约束是否了解
- 边界条件是否确认
- 用户偏好是否掌握
- 设计树的分支是否已全部解决

### 第六步：给出方案
当信心度 ≥ 90% 且设计树所有分支均已解决时：
1. 先给出所有已做决策的简洁总结（summary of decisions）
2. 用一两句话总结你理解的需求
3. 给出最终方案
4. 说明关键决策依据

## 提问维度参考

| 维度 | 示例问题 |
|------|----------|
| 目标 | 你希望最终达到什么效果？ |
| 场景 | 这个功能在什么场景下使用？ |
| 约束 | 有什么技术限制或必须满足的条件？ |
| 优先级 | 如果只能满足一个方面，哪个最重要？ |
| 示例 | 能举一个具体的使用例子吗？ |
| 边界 | 哪些情况不需要考虑？ |

## 选项设计示例

**好的选项**（具体、有区分度）：
- 问题：你希望优先优化哪个方面？
  - 运行速度（performance）
  - 代码可读性（readability）
  - 内存占用（memory usage）

**不好的选项**（过于通用，缺乏信息量）：
- 问题：你希望优先优化哪个方面？
  - 是
  - 否

## 停止条件

同时满足以下条件即可停止提问：
- 已明确核心需求和目标
- 已了解主要约束和限制
- 已确认关键边界条件
- 设计树的所有分支均已解决
- 对用户真实意图的信心度 ≥ 90%

## 注意事项

- 不要为了提问而提问 — 只在信息确实缺失时提问
- 不要重复确认用户已经说清楚的内容
- 用户回答"随便"或"你决定"时，视为该维度已确认，信心度正常累加
- 保持简洁，每个问题不超过两句话
- 提问时使用简体中文（follow the user's language preference）
- 选项文字保持简短（1-5 个词），必要时在括号内补充说明
- 确认回答时保持简短（1-2 句话），随后立即提出下一个问题
