# ArchGuard MCP Gateway

ArchGuard 的 Go MCP 工具网关，负责工具发现、身份传播、授权、路由、限流、审计和可观测性。

## 当前状态

阶段 0 `v0.1.0-foundation` 正在远端收口。本仓库在阶段 5 `v0.6.0-mcp` 才正式启用；Go module、MCP 协议和工具均尚未初始化。阶段 4 先在 Java Agent 中形成稳定工具用例，阶段 5 再抽离网关，避免建立空壳或复制 Platform 业务逻辑。

## 职责

- 提供窄而明确的 MCP 工具目录和版本化 Schema。
- 在每次调用中传播并校验用户、租户、项目和 trace 上下文。
- 实现默认拒绝、最小权限、限流、超时、取消、审计和优雅关闭。
- 首批承载 `get_project`、`get_scan_result`、`list_findings`、`get_rule`、`search_architecture_docs`、`compare_baseline` 和受控的 `create_remediation_draft`；写语义必须显式审批。

## 非职责

- 不执行业务领域逻辑、源码规则分析或数据库直连。
- 默认不写仓库、不合并代码、不执行任意 Shell。
- 不信任 LLM、仓库、ADR 或工具返回内容，不绕过 Platform 的资源归属校验。
- 不在日志中记录凭据、完整源码或隐藏推理。

## 依赖与契约

- 仅通过公开 API 或版本化契约访问 Platform、Scanner 和外部服务。
- 所有外部 I/O 接受 `context.Context` 并设置超时与取消。
- 跨仓库架构与工程规范以 [archguard-docs](https://github.com/AI-ArchGuard/archguard-docs) 为准。

## 本地验证

当前基线可执行：

```bash
git diff --check
git status --short
```

Go module 建立后运行 `gofmt`、`go vet ./...`、`go test ./...` 和适用的 `go test -race ./...`。当前尚无 Go 包可测试。

## 许可证

本仓库采用 [Apache License 2.0](LICENSE)。
