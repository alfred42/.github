# Agent 派单默认模板

这是 alfred42 账号的默认 Issue 模板仓库。没有自定义 Issue 模板或模板配置的公开、私有仓库可继承这些入口；有项目模板的仓库会使用项目自身配置，不会自动合并两套模板。

## 创建任务

在目标仓库打开 **Issues → New issue**，按已接入的节点选择入口：

| 模板 | 节点标签 | 执行器标签 |
|---|---|---|
| Mac mini / Codex | `node:macmini` | `executor:codex` |
| Mac mini / Hermes | `node:macmini` | `executor:hermes` |
| Mac mini / Antigravity | `node:macmini` | `executor:antigravity` |
| Linux42 / Hermes | `node:linux42` | `executor:hermes` |

1. 确认仓库已获节点授权且运行环境已准备好。继承模板、创建标签都不会自动授权节点访问仓库。
2. 填写目标、上下文、允许修改范围、编号验收项及精确测试命令，替换全部占位内容。
3. 保存 Issue，检查实际标签，确保只有一个 node 和一个 executor。模板不自动添加 `agent:ready`。
4. 全部修改保存后等 2 秒，再单独添加 `agent:ready`。领取后的输入被冻结；不要用运行中的正文编辑或评论更改已领取任务。
5. 查看结果和 Draft PR，按[人工验收评论](docs/acceptance-comment.md)核对实际代码、测试及具体 commit，再决定通过、返工或终止。验收评论不会自动触发任务、合并或关闭 Issue。

## 停止与预算

- 人工取消：给原 Issue 添加 `agent:cancel`，保持节点服务运行，等待确认任务已停止。取消有扫描及清理延迟；移除 Ready 或评论“停止”不会中断执行。
- 默认累计预算为 600 秒。正文末尾唯一的隐藏 `agent-runner` JSON 控制块提供机器参数；普通文字不会修改预算。调整时同步修改可见数字，并遵守节点上限。
- 同一 Issue 的重试不重置累计预算。缺少输入、验证环境不可用或必须扩大范围时，应返回 blocked 并说明所需信息。
- AC 验收项和语义停止条件由 agent 遵守并由人核验。当前 runner 不会根据勾选框自动验收、统计同因修复次数或停止任务。

## 返工

等待上一轮结束且结果交付完成，移除旧 `agent:ready`，将反馈整理到原 Issue。Codex 可把唯一控制块中的 `retry_mode` 改为 `revise` 修订原 PR，或用 `resume` 续做；Hermes / Antigravity 当前仅支持 `fresh`。保留原预算，保存后等 2 秒再单独添加 Ready。先前取消的任务须确认停止后才移除 `agent:cancel`。

## 接入其他仓库

- 本仓库 `.github/ISSUE_TEMPLATE/` 位于默认分支；更新一次即可供符合继承条件的同账号仓库使用。
- 模板使用的标签必须同时存在于本仓库和使用模板的目标仓库。状态标签为 `agent:ready`、`agent:cancel`、`agent:review`、`agent:blocked`；显示状态应结合结果评论判断。
- 若目标仓库已有自己的有效 Issue 模板或 `config.yml`，需由维护者决定保留项目模板并复制需要的入口，或迁移到账号默认配置。
- 本仓库公开，只存放通用规范。业务细节、代码和测试数据填写在对应项目的 Issue 中。

[GitHub 默认模板与继承规则](https://docs.github.com/en/communities/setting-up-your-project-for-healthy-contributions/creating-a-default-community-health-file)
