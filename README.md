# go-consul-kit

`go-consul-kit` 是一个面向 Go 项目的 Consul 客户端辅助库。当前仓库提供 `consul` 包，用 protobuf 配置描述 HashiCorp Consul 连接参数，并基于 `github.com/hashicorp/consul/api` 创建 Consul API client。

## 功能特性

- 使用 `consul.Config` 统一描述 Consul 地址、路径前缀、数据中心、ACL token、命名空间、分区、HTTP Basic Auth 和 TLS PEM 配置。
- 提供 `consul.NewClient` / `consul.NewConsulClient` 创建 `*api.Client`。
- 创建 client 后会通过写入 KV `ping=pong` 验证 Consul 连接和 KV 写权限。
- 使用 `config.proto` 维护配置结构，并通过 Makefile 目标生成 Go protobuf 代码。

## 安装

```bash
go get github.com/ikaiguang/go-consul-kit
```

```go
import consulpkg "github.com/ikaiguang/go-consul-kit/consul"
```

## 快速开始

```go
package main

import (
	"log"

	consulpkg "github.com/ikaiguang/go-consul-kit/consul"
)

func main() {
	client, err := consulpkg.NewClient(&consulpkg.Config{
		Scheme:  "http",
		Address: "127.0.0.1:8500",
	})
	if err != nil {
		log.Fatal(err)
	}

	_ = client
}
```

调用 `NewClient` 时，目标 Consul 必须可访问，并且当前凭据需要具备写入 KV `ping` 的权限。没有可用 Consul 环境时，示例会在连接验证阶段返回错误。

## 目录结构

```text
.
├── consul/        # Consul client 配置、构造逻辑和 protobuf 定义
├── docs/          # 仓库使用、维护和发布前检查文档
├── Makefile       # 初始化、生成、格式化和帮助命令入口
├── go.mod         # Go module 和依赖声明
└── README.md      # 项目入口文档
```

## 包说明

| 包 | 导入路径 | 说明 |
| --- | --- | --- |
| `consul` | `github.com/ikaiguang/go-consul-kit/consul` | 将 protobuf 配置转换为 HashiCorp Consul API client 配置，并创建 Consul client。 |

更多包级说明见 [consul/README.md](consul/README.md)。

## 常用命令

```bash
make help
make init
make protoc-config-protobuf
make format-protobuf
go test ./...
go vet ./...
```

说明：

- `make init` 会安装 protobuf、Kratos、Wire、mockgen、golangci-lint 等开发工具。
- `make protoc-config-protobuf` 会根据 `consul/config.proto` 生成 protobuf 相关文件。
- `make format-protobuf` 使用 `clang-format` 格式化仓库内 protobuf 文件，可通过 `CLANG_FORMAT=/path/to/clang-format` 覆盖命令。

## 文档

- [docs/README.md](docs/README.md)：文档索引和阅读路径。
- [consul/README.md](consul/README.md)：`consul` 包使用说明。

## 安全注意事项

- `Config.Token`、`AuthPassword`、`TlsKeyPem` 等字段可能包含敏感信息，不要写入日志、公开文档或提交真实生产值。
- `InsecureSkipVerify` 会跳过 TLS 证书校验，只应在明确风险的测试环境中使用。
- `NewClient` 会写入 Consul KV `ping=pong` 做连接验证，调用方需要确认目标环境允许该写操作。
- 发布公开仓库前，请检查 README、docs、示例配置和 prompt 中是否包含私有域名、账号、Token、密码、内网地址或公司专属流程。

## 贡献

提交前建议执行：

```bash
go test ./...
go vet ./...
git diff --check
```

修改 protobuf 后不要手改生成文件，应通过 Makefile 目标重新生成。

## License

本仓库已提供 [LICENSE](LICENSE)。发布前请确认许可证内容符合项目预期。
