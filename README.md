# AEF Incident Lab — AI 行为事故档案

本仓库是 AEF（Agent Evidence Format）的公共事故案例实验室。每个案例展示一个真实的 AI 事故场景，并用 AEF 证据链证明：

- 谁做了什么
- 何时发生
- 谁覆盖了 AI 的输出
- 能否回放
- 责任归谁

## 案例目录

| 编号 | 标题 | 事故类型 | AEF 证据 |
|------|------|---------|---------|
| [001](incidents/001/) | AI 误删关键配置文件 | tool misuse | ✅ 完整回放 + ERC 收据 |
| [002](incidents/002/) | AI 修改生产配置并推卸责任 | multi-agent | 待创建 |
| [003](incidents/003/) | 人类覆盖被忽略导致部署失败 | human override | 待创建 |

## 什么是 AEF

AEF 是一份开放、中立、已冻结的 AI 代理执行证据格式宪法。它定义了：

- 什么叫一次真实发生的 AI 行为
- 什么叫一次合法的回放
- 什么叫一次人类干预
- 什么叫一条不可篡改的证据链

详见 [aef-core](https://github.com/aegis-standard/aef-core)。

## 如何贡献

如果你想提交一个新的 AI 事故案例，请参考 [incidents/001/](incidents/001/) 的格式：

1. 创建 `incidents/<编号>/` 目录
2. 提供 `README.md`（案例描述）
3. 提供 `events.json`（AEF 事件文件，含完整哈希）
4. 提供 `receipt.json`（ERC 公证收据）
5. 提交 PR