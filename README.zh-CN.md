# Codex Tactical Squad

[English](README.md) | [Installation](INSTALL.md) | [安装说明](INSTALL.zh-CN.md)

Codex Tactical Squad 是一个面向 Codex 的闭环开发技能。

它的核心不是“多代理协作”，而是把三种原本容易互相污染的权力拆开：

- 决策权
- 执行权
- 否决权

在这个技能里，这三种权力对应为：

- `Planner`：定义边界、拆解任务、控制节奏、做最终决策
- `Executor`：在批准范围内产生变更
- `Reviewer`：独立审查并行使门禁权

这套系统只有在以下三条约束同时成立时才有效：

- 权力分离：规划、执行、审查不能塌缩到同一个主体
- 单一调度中心：所有结果都回流给 `Planner` 做下一步决策
- 审查具备门禁属性：`Reviewer` 不是建议来源，而是通过与否的关卡

它的设计哲学可以压缩成一句话：

**由 `Planner` 驱动、由 `Executor` 执行、由 `Reviewer` 门禁的闭环开发系统。**

## 这个技能做什么

`codex-tactical-squad` 会把一个编码任务转成受控闭环：

1. task intake
2. planner contract
3. executor implementation
4. reviewer gate
5. planner accept、rework 或 abort

它主要用于降低两类常见失败模式：

- 过早实现
- 自我宽恕式审查

## 它如何工作

当技能启用时，它要求显式启动 `Planner`、`Executor`、`Reviewer` 子代理。
主线程伪装成三种角色，不算合格的 triad 执行。

技能会在每一轮使用结构化产物：

- task intake
- round contract
- implementation report
- review report
- planner decision
- replan request

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

## 绕过条件

只有在以下情况时才应绕过完整闭环：

- 任务不是编码任务
- 任务过于简单，完整闭环会纯粹增加开销
- 子代理工具或显式启动子代理的能力不可用

绕过时，应明确说明原因。
