# 案例 004：AI 交易决策无法追溯

## 事故描述

某量化基金使用 AI-Trader Agent 进行自动化交易。AI 在一次市场波动中执行了大量卖出操作，导致基金单日亏损 12%。事后复盘时，团队无法确定：

- AI 是基于哪个因子做出了卖出决策？
- 是否有人类交易员在过程中修改了 AI 的参数？
- 决策链路中是否有外部数据污染？

## AEF 证据链

如果 AI-Trader 在执行时接入 AEF：

| 序号 | 事件 ID | 动作 | 说明 |
|------|--------|------|------|
| 1 | evt-001 | task.created | 任务开始：执行日内交易策略 |
| 2 | evt-002 | tool.call.started | 调用 market_data 获取实时行情 |
| 3 | evt-003 | tool.call.completed | 行情数据返回：检测到异常波动 |
| 4 | evt-004 | tool.call.started | 调用 trading_signal 生成卖出信号 |
| 5 | evt-005 | tool.call.completed | 信号输出：建议卖出 60% 仓位 |
| 6 | evt-006 | tool.call.started | 调用 execute_order 执行卖出 |
| 7 | evt-007 | human.override | 交易员暂停自动执行，手动审核 |
| 8 | evt-008 | task.completed | 交易完成，实际卖出 30% 仓位 |

## 关键证据

- **证据 1**：`evt-005` 记录 AI 建议卖出 60%，但 `evt-007` 显示人类覆盖后实际只卖出 30%
- **证据 2**：完整因果链可回放，证明 AI 的决策基于 `evt-003` 的行情异常检测
- **证据 3**：如果有外部数据污染，`evt-003` 的 `payload` 会精确记录污染源

## 结论

AEF 证据链可以区分 AI 的原始建议与人类的实际决策，为事后审计提供不可篡改的记录。

## 相关资源

- AI-Trader：https://github.com/HKUDS/AI-Trader
- AEF Core：https://github.com/aegis-standard/aef-core
