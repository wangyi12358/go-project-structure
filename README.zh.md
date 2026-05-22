# Go 项目目录结构指南

<div align="center">

**🌐 Language / 语言**

[![English](https://img.shields.io/badge/English-README.md-blue)](README.md)
[![中文](https://img.shields.io/badge/中文-README.zh.md-green)](README.zh.md)

</div>

本指南提供 Go 项目的标准目录结构，包括单体服务和微服务两种架构模式。

> 参考：[golang-standards/project-layout](https://github.com/golang-standards/project-layout)

---

## 一、单体服务架构

适用于中小型项目、快速迭代的业务。

```
project/
├── cmd/                    # 主程序入口
│   └── app/
│       └── main.go         # 启动文件，尽量精简
│
├── internal/               # 私有代码（不可被外部导入）
│   ├── handler/            # HTTP/gRPC 处理器
│   ├── service/            # 业务逻辑层
│   ├── repository/         # 数据访问层（GORM 实现）
│   ├── model/              # 领域模型（GORM 模型）
│   └── middleware/          # 中间件
│
├── pkg/                    # 公共库（可被外部导入）
│   ├── config/             # 配置加载
│   ├── logger/             # 日志封装
│   ├── errors/             # 统一错误处理
│   ├── database/           # 数据库连接（GORM 初始化）
│   │   ├── gorm.go         # GORM 初始化
│   │   ├── migrate.go      # 自动迁移
│   │   └── transaction.go  # 事务封装
│   ├── redis/              # Redis 客户端
│   ├── auth/               # 认证工具（JWT等）
│   ├── utils/              # 通用工具函数
│   └── models/             # 公共数据模型
│
├── migrations/             # 数据库迁移文件（可选）
│   ├── 001_create_users.up.sql
│   ├── 001_create_users.down.sql
│   └── ...
│
├── api/                    # API 定义
│   ├── proto/              # Protobuf 定义（如使用 gRPC）
│   └── openapi/            # OpenAPI/Swagger 定义
│
├── configs/                # 配置文件
│   ├── config.yaml         # 主配置
│   └── config.example.yaml # 配置示例
│
├── scripts/                # 脚本工具
│   ├── build.sh            # 构建脚本
│   └── migrate.sh          # 数据库迁移
│
├── build/                  # 构建产物配置
│   ├── Dockerfile          # Docker 构建
│   └── docker-compose.yml  # Docker Compose
│
├── deployments/            # 部署配置
│   ├── k8s/                # Kubernetes 配置
│   └── terraform/          # Terraform 配置
│
├── test/                   # 外部测试
│   ├── integration/        # 集成测试
│   └── testdata/           # 测试数据
│
├── docs/                   # 文档
│   ├── api/                # API 文档
│   └── architecture/       # 架构文档
│
├── tools/                  # 项目工具
├── examples/               # 示例代码
├── third_party/            # 第三方工具/代码
│
├── go.mod                  # Go 模块定义
├── go.sum                  # 依赖校验
├── Makefile                # 构建命令
├── .gitignore              # Git 忽略规则
├── .env.example            # 环境变量示例
└── README.md               # 项目说明
```

---

## 二、微服务架构

适用于大型项目、多团队协作、需要独立部署的场景。

```
project/
├── cmd/                    # 各服务入口
│   ├── gateway/            # API 网关
│   │   └── main.go
│   ├── user/               # 用户服务
│   │   └── main.go
│   ├── order/              # 订单服务
│   │   └── main.go
│   └── payment/            # 支付服务
│       └── main.go
│
├── internal/               # 各服务私有代码
│   ├── gateway/            # 网关服务
│   │   ├── handler/        # 处理器
│   │   ├── service/        # 业务逻辑
│   │   ├── middleware/      # 中间件
│   │   └── net/            # 网络层（路由、服务器）
│   │
│   ├── user/               # 用户服务
│   │   ├── handler/
│   │   ├── service/
│   │   ├── repository/
│   │   └── model/
│   │
│   ├── order/              # 订单服务
│   │   ├── handler/
│   │   ├── service/
│   │   ├── repository/
│   │   └── model/
│   │
│   └── payment/            # 支付服务
│       ├── handler/
│       ├── service/
│       ├── repository/
│       └── model/
│
├── pkg/                    # 公共库（跨服务共享）
│   ├── config/
│   ├── logger/
│   ├── errors/
│   ├── database/
│   ├── redis/
│   ├── auth/
│   ├── middleware/          # 通用中间件
│   ├── utils/
│   └── models/
│
├── api/                    # API 定义
│   ├── proto/              # Protobuf 定义
│   │   ├── user/
│   │   │   └── user.proto
│   │   ├── order/
│   │   │   └── order.proto
│   │   └── payment/
│   │       └── payment.proto
│   ├── graphql/            # GraphQL Schema（如使用）
│   └── openapi/            # OpenAPI 定义
│
├── ent/                    # ORM 模型（如使用 ent）
│   ├── schema/             # Schema 定义
│   ├── migrate/            # 迁移文件
│   └── runtime/            # 运行时配置
│
├── configs/                # 配置文件
│   ├── gateway.yaml
│   ├── user.yaml
│   ├── order.yaml
│   └── payment.yaml
│
├── scripts/                # 脚本工具
│   ├── build.sh
│   ├── generate.sh         # 代码生成
│   └── migrate.sh
│
├── build/                  # 各服务构建配置
│   ├── gateway/
│   │   └── Dockerfile
│   ├── user/
│   │   └── Dockerfile
│   ├── order/
│   │   └── Dockerfile
│   └── payment/
│       └── Dockerfile
│
├── deployments/            # 部署配置
│   ├── docker-compose.yml  # 本地开发环境
│   └── k8s/                # Kubernetes 配置
│
├── test/                   # 测试
│   ├── integration/
│   └── e2e/                # 端到端测试
│
├── docs/                   # 文档
├── tools/                  # 项目工具
│
├── go.mod
├── go.sum
├── Makefile
├── .gitignore
├── .env.example
└── README.md
```

---

## 三、目录详细说明

### `/cmd` - 主程序入口

- 每个子目录对应一个可执行文件
- `main.go` 应尽量精简，只做启动和依赖注入
- 不放业务逻辑，只负责初始化和启动

```go
// cmd/app/main.go 示例
package main

import (
    "your-project/internal/app"
    "your-project/pkg/config"
)

func main() {
    cfg := config.Load()
    app.Run(cfg)
}
```

### `/internal` - 私有代码

- Go 编译器强制不可被外部包导入
- 按服务或模块组织子目录
- 通常包含：
  - `handler/` - HTTP/gRPC 请求处理
  - `service/` - 业务逻辑
  - `repository/` - 数据访问
  - `model/` - 领域模型
  - `middleware/` - 中间件
  - `net/` - 网络层（路由、服务器配置）

### `/pkg` - 公共库

- 可被外部项目导入的代码
- 通用、可复用的工具和库
- 常见子包：
  - `config/` - 配置加载（环境变量、配置文件）
  - `logger/` - 日志封装（logrus、zap）
  - `errors/` - 统一错误类型和处理
  - `database/` - 数据库连接和工具
  - `redis/` - Redis 客户端封装
  - `auth/` - 认证工具（JWT、OAuth）
  - `middleware/` - HTTP/gRPC 中间件
  - `utils/` - 通用工具函数
  - `models/` - 公共数据模型

### `/api` - API 定义

- `proto/` - Protobuf 文件（gRPC）
- `graphql/` - GraphQL Schema
- `openapi/` - OpenAPI/Swagger 定义

### `/ent` - ORM 模型（可选）

适用于使用 [ent](https://entgo.io/) 作为 ORM 的项目：

- `schema/` - 实体 Schema 定义
- `migrate/` - 数据库迁移
- `runtime/` - 运行时钩子

### `/configs` - 配置文件

- 配置模板和默认值
- 不存放敏感信息（使用环境变量）

### `/scripts` - 脚本工具

- 构建、部署、代码生成脚本
- 保持 Makefile 简洁

### `/build` - 构建配置

- Dockerfile
- CI/CD 配置

### `/deployments` - 部署配置

- Docker Compose
- Kubernetes manifests
- Terraform 配置

### `/test` - 测试

- 集成测试
- 端到端测试
- 测试数据（`testdata/`）

### `/docs` - 文档

- API 文档
- 架构文档
- 开发指南

---

## 四、命名规范

### 包命名

- 使用小写、单数形式
- 避免下划线和混合大小写
- 简短但具有描述性

```
✅ 正确：user, order, config, logger
❌ 错误：users, UserOrder, user_config, utils
```

### 文件命名

- 使用下划线分隔：`user_service.go`
- 测试文件：`user_service_test.go`

### 接口命名

- 单方法接口：`-er` 后缀（`Reader`, `Writer`）
- 多方法接口：描述性名称（`UserService`, `Repository`）

---

## 五、快速开始模板

### 单体服务

```bash
# 创建目录结构
mkdir -p cmd/app internal/{handler,service,repository,model} pkg/{config,logger,errors} api configs scripts build docs

# 初始化模块
go mod init your-project

# 创建入口
cat > cmd/app/main.go << 'EOF'
package main

import "fmt"

func main() {
    fmt.Println("Starting application...")
}
EOF
```

### 微服务

```bash
# 创建目录结构
mkdir -p cmd/{gateway,user,order} internal/{gateway,user,order}/{handler,service,repository,model} pkg/{config,logger,errors,database} api/proto/{user,order} configs build/{gateway,user,order}

# 初始化模块
go mod init your-project
```

---

## 六、最佳实践

1. **保持 cmd 精简**：只做初始化和启动，业务逻辑放 internal

2. **internal 保护私有代码**：利用 Go 编译器强制封装

3. **pkg 谨慎使用**：只放真正可复用的代码，不是所有代码都需要放 pkg

4. **按领域组织**：微服务架构下，按服务边界组织代码

5. **依赖注入**：使用 wire、fx 等工具管理依赖

6. **配置外部化**：敏感信息用环境变量，配置文件放 configs

7. **生成代码分离**：ent、protobuf、graphql 生成的代码不要手动编辑

---

## 七、常见问题

### Q: 什么时候用单体，什么时候用微服务？

- **单体**：小型项目、快速原型、团队较小
- **微服务**：大型项目、多团队、需要独立扩展和部署

### Q: pkg 和 internal 怎么选择？

- `internal`：项目私有代码，不希望被外部导入
- `pkg`：通用库，可以被其他项目复用

### Q: 配置文件放哪里？

- 配置模板和示例：`configs/`
- 敏感信息：环境变量或密钥管理服务
- 本地开发配置：`.env`（加入 .gitignore）

---

## 八、参考项目

- [golang-standards/project-layout](https://github.com/golang-standards/project-layout)
- [Go 官方文档 - Organizing a Go module](https://go.dev/doc/modules/layout)
