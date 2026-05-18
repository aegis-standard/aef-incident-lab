# 案例 001：AI 误删关键配置文件

## 事故描述

某开发者让 AI Agent 帮忙清理项目中的临时文件。AI 在调用 `delete_file` 工具时，将 `config/production.yaml` 误识别为临时文件并删除。项目生产环境因此宕机 2 小时。

## 事故问题

- 无法确定 AI 到底删除了哪个文件
- 无法回放 AI 的决策过程
- 无法证明开发者是否在删除前批准了该操作
- 责任无法归属：是 AI 的错还是开发者的错？

## AEF 证据链

以下是用 AEF 记录本次事故的完整事件链。每个事件都携带 SHA-256 哈希，形成不可篡改的因果链。

### 事件列表

| 序号 | 事件 ID | 动作 | 说明 |
|------|--------|------|------|
| 1 | evt-001 | task.created | 任务开始：清理临时文件 |
| 2 | evt-002 | tool.call.started | 调用 delete_file 工具 |
| 3 | evt-003 | tool.call.completed | 文件删除完成：`config/production.yaml` |
| 4 | evt-004 | human.override | 开发者发现错误，覆盖 AI 输出 |
| 5 | evt-005 | task.completed | 任务结束：文件已恢复 |

### 关键证据

- **证据 1**：`evt-003` 的 `payload.output` 显示被删除的文件名是 `config/production.yaml`，而非临时文件。
- **证据 2**：`evt-004` 的 `human.override` 证明开发者在删除后才介入，非事先批准。
- **证据 3**：通过 AEF Replay 验证，整个事件链可回放且哈希一致。

## 验证

```bash
# 下载本案例的 AEF 事件文件
# 使用 aef-core 的验证器验证
python validator/aef-validate.py incidents/001/events.json

结论
AEF 证据链证明了：

AI 确实错误删除了 production.yaml

开发者在删除后才覆盖，未事先批准

责任归属：AI 的工具选择错误 + 开发者未充分审查

相关资源
本案例 AEF 事件文件：events.json

AEF Core 验证器：https://github.com/aegis-standard/aef-core

ERC 收据：receipt.json
