# Codex Tactical Squad

[English](README.md) | [Installation](INSTALL.md) | [安装说明](INSTALL.zh-CN.md)

Codex Tactical Squad 是一个面向 Codex 的闭环开发调度技能。

它部分受到 Anthropic 关于长时间应用开发 harness 设计文章的启发：
<https://www.anthropic.com/engineering/harness-design-long-running-apps>

它不是泛化的“多代理协作”。

它是一个把三种原本容易互相污染的权力明确拆开的编码系统：

- 决策权
- 执行权
- 否决权

在这套架构里，这三种权力通过三个独立的战术位置技能来表达：

- `$codex-tactical-planner`：定义边界、拆解任务、控制节奏、做最终决策
- `$codex-tactical-executor`：在批准范围内产生变更
- `$codex-tactical-reviewer`：独立审查并行使门禁权

这套系统只有在以下三条约束同时成立时才有效：

- 权力分离：规划、执行、审查不能塌缩到同一个主体
- 单一调度中心：所有结果都回流给 `Planner` 做下一步决策
- 审查具备门禁属性：`Reviewer` 不是建议来源，而是通过与否的关卡

**由 `Planner` 驱动、由 `Executor` 执行、由 `Reviewer` 门禁的闭环开发系统。**

## 这个技能做什么

`codex-tactical-squad` 负责告诉主线程何时使用三个战术位置技能，并把一个编码任务转成受控闭环：

1. task intake
2. planner contract
3. executor implementation
4. reviewer gate
5. planner accept、rework 或 abort

它主要用于降低两类常见失败模式：

- 过早实现
- 自我宽恕式审查

## 背景

如果你是从 Anthropic 那篇 harness 设计文章找到这个项目的，可以把两者这样对应理解：

- planner -> 边界设定与决策权
- executor -> 受控实现
- reviewer -> 独立门禁

Codex Tactical Squad 把那种通用 harness 思路，落成了一个可复用的 Codex 调度 skill 加三个位置 skill，并补上了显式子代理启动、结构化交接和审查门禁推进。

## 它如何工作

当技能启用时，它要求显式使用独立的 planner、executor、reviewer 位置技能。
主线程伪装成三种角色，不算合格的 tactical squad 执行。
调度器同时也是唯一的运行时注入端口：由它决定给每个战术位置子代理注入哪些模板和哪些任务级约束。

它也不假设三种角色在每一轮都必须全部重建。

- `$codex-tactical-planner`：优先在同一会话内持续复用
- `$codex-tactical-executor`：优先在同一任务簇内复用
- `$codex-tactical-reviewer`：优先每一轮审查都使用新的 reviewer

这样可以同时保留边界与执行连续性，并尽量维持审查独立性。
在实际运行中，调度器应先检查是否已有可复用的 planner 或 executor 存活，只有在不适合复用时才新建。

技能会在每一轮使用结构化产物：

- task intake
- round contract
- implementation report
- review report
- planner decision
- replan request

每个战术位置都必须先提交自己要求的结构化产物，调度器才能推进到下一个步骤。

## 战术位置技能

- `$codex-tactical-planner`
- `$codex-tactical-executor`
- `$codex-tactical-reviewer`

## 安装

见 [INSTALL.zh-CN.md](INSTALL.zh-CN.md)。

## 使用

显式调用这个技能：

```text
使用 $codex-tactical-squad 处理这个编码任务：
目标：
约束：
完成标准：
相关文件：
```

示例：

```text
使用 $codex-tactical-squad 处理这个编码任务：
目标：为训练流程增加暂停和恢复控制
约束：不要重构无关 UI 模块
完成标准：暂停和恢复行为正确，现有测试不回归
相关文件：training/runtime.py, views/workbench_window.py
```

## 内置参考文件

- `references/task_intake_template.md`
- `references/round_contract_template.md`
- `references/implementation_report_template.md`
- `references/review_report_template.md`
- `references/planner_decision_template.md`
- `references/replan_request_template.md`
- `references/orchestrator_checklist.md`
- `references/spawn_protocol.md`
- `references/agent_registry_template.md`
- `references/prompt_assembly.md`
- `references/submission_contract.md`

## 绕过条件

只有在以下情况时才应绕过完整闭环：

- 任务不是编码任务
- 任务过于简单，完整闭环会纯粹增加开销
- 子代理工具或显式启动子代理的能力不可用

绕过时，应明确说明原因。
