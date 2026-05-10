# 文档索引

本目录收集 `go-consul-kit` 的使用、维护和发布前检查文档。首次使用建议先读根目录 README，再按需要进入包级文档或仓库指导文档。

## 快速入口

| 文档 | 用途 |
| --- | --- |
| [项目 README](../README.md) | 项目介绍、安装、快速开始、目录结构、常用命令和安全注意事项。 |
| [consul 包文档](../consul/README.md) | `consul` 包 API、配置字段、生成方式和使用注意事项。 |

## 阅读建议

- 只想使用库：阅读 [项目 README](../README.md) 和 [consul 包文档](../consul/README.md)。
- 需要修改 protobuf：阅读 [consul 包文档](../consul/README.md) 的“生成”章节，并使用 `make protoc-config-protobuf`。
- 准备公开发布：先执行发布前检查，确认许可证、敏感信息、测试结果和生成文件状态。

## 发布前检查

发布到公开仓库前，请检查：

- `README.md`、`docs/` 和示例代码中没有真实 Token、密码、内网地址或生产环境配置。
- `LICENSE` 内容符合项目预期。
- `go test ./...`、`go vet ./...` 和 `git diff --check` 的结果已记录。
- 修改 protobuf 后已重新生成对应文件。
