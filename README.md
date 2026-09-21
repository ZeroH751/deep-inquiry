# deep-inquiry

opencode 的深度提问模式 Skill（Deep Inquiry Mode）。

当你的消息以 `>>> ` 前缀开头时，AI 会进入深度提问模式：一次只问一个选择题，像访谈一样穷追不舍地询问计划的每一个细节，走遍设计树的每一个分支，直到信心度 ≥ 90% 后才给出最终方案。


## 特性

- **一次只问一个问题**：通过选项弹窗呈现，提供 2-4 个具体选项，可直接选择或自定义回答
- **穷尽设计树**：按依赖关系逐分支解决决策，不遗漏任何细节
- **自主探索**：能通过代码库或文件查明的问题，AI 会自己探索而不是问你
- **决策总结**：给出方案前，先总结所有已确认的决策

## 安装

将 `SKILL.md` 复制到 opencode 的 skills 目录。

**全局安装（所有项目可用）**

```powershell
# Windows
New-Item -ItemType Directory -Force "$env:USERPROFILE\.config\opencode\skills\deep-inquiry"
Copy-Item SKILL.md "$env:USERPROFILE\.config\opencode\skills\deep-inquiry\SKILL.md"
```

```bash
# macOS / Linux
mkdir -p ~/.config/opencode/skills/deep-inquiry
cp SKILL.md ~/.config/opencode/skills/deep-inquiry/SKILL.md
```

**项目级安装（仅当前项目可用）**

```bash
mkdir -p .opencode/skills/deep-inquiry
cp SKILL.md .opencode/skills/deep-inquiry/SKILL.md
```

安装后**重启 opencode** 生效。

## 使用

在消息前加 `>>> ` 前缀即可触发：

```
>>> 帮我设计一个用户登录系统
```

AI 会开始逐个提问，例如：

> 问题：这个登录系统主要面向哪类用户？
>
> - 内部员工（内网使用）
> - 外部客户（公网使用）
> - 两者都有

回答后，AI 会简短确认并继续追问下一个问题，直到完全理解你的需求，最后给出决策总结和最终方案。

## 工作原理

1. **分析问题，构建设计树** — 识别模糊点、缺失信息和隐含假设
2. **排序分支，解决依赖** — 先解决前置决策，再解决依赖它们的后续决策
3. **逐个提问** — 每轮只问一个问题，通过 `question` 工具提供 2-4 个具体选项
4. **确认并追问** — 用 1-2 句话确认回答后立即提出下一个问题
5. **评估信心度** — 评估目标、约束、边界、偏好及设计树覆盖度
6. **给出方案** — 信心度 ≥ 90% 且分支全部解决时，先总结决策，再给出最终方案

## 可选增强

在 `AGENTS.md` 中加入以下内容，可让 `>>> ` 前缀的触发更可靠：

```markdown
## 深度提问模式

当我的消息以 `>>> `（三个大于号加空格）开头时，必须先加载 `deep-inquiry` skill，并严格遵循其流程。
```
