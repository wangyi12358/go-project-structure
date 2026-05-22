# Go Project Directory Structure Guide

<div align="center">

**🌐 Language / 语言**

[![English](https://img.shields.io/badge/English-README.md-blue)](README.md)
[![中文](https://img.shields.io/badge/中文-README.zh.md-green)](README.zh.md)

</div>

This guide provides standard directory structures for Go projects, including both monolithic service and microservice architecture patterns.

> Reference: [golang-standards/project-layout](https://github.com/golang-standards/project-layout)

---

## 1. Monolithic Service Architecture

Suitable for small to medium-sized projects and rapidly iterating business logic.

```
project/
├── cmd/                    # Main application entry points
│   └── app/
│       └── main.go         # Startup file, keep it minimal
│
├── internal/               # Private code (cannot be imported externally)
│   ├── handler/            # HTTP/gRPC handlers
│   ├── service/            # Business logic layer
│   ├── repository/         # Data access layer
│   ├── model/              # Domain models
│   └── middleware/          # Middleware
│
├── pkg/                    # Public libraries (can be imported externally)
│   ├── config/             # Configuration loading
│   ├── logger/             # Logging wrapper
│   ├── errors/             # Unified error handling
│   ├── database/           # Database connections
│   ├── redis/              # Redis client
│   ├── auth/               # Authentication tools (JWT, etc.)
│   ├── utils/              # Utility functions
│   └── models/             # Public data models
│
├── api/                    # API definitions
│   ├── proto/              # Protobuf definitions (for gRPC)
│   └── openapi/            # OpenAPI/Swagger definitions
│
├── configs/                # Configuration files
│   ├── config.yaml         # Main configuration
│   └── config.example.yaml # Configuration example
│
├── scripts/                # Scripts and tools
│   ├── build.sh            # Build script
│   └── migrate.sh          # Database migration
│
├── build/                  # Build artifacts configuration
│   ├── Dockerfile          # Docker build
│   └── docker-compose.yml  # Docker Compose
│
├── deployments/            # Deployment configuration
│   ├── k8s/                # Kubernetes configuration
│   └── terraform/          # Terraform configuration
│
├── test/                   # External tests
│   ├── integration/        # Integration tests
│   └── testdata/           # Test data
│
├── docs/                   # Documentation
│   ├── api/                # API documentation
│   └── architecture/       # Architecture documentation
│
├── tools/                  # Project tools
├── examples/               # Example code
├── third_party/            # Third-party tools/code
│
├── go.mod                  # Go module definition
├── go.sum                  # Dependency checksum
├── Makefile                # Build commands
├── .gitignore              # Git ignore rules
├── .env.example            # Environment variables example
└── README.md               # Project documentation
```

---

## 2. Microservice Architecture

Suitable for large projects, multi-team collaboration, and scenarios requiring independent deployment.

```
project/
├── cmd/                    # Service entry points
│   ├── gateway/            # API Gateway
│   │   └── main.go
│   ├── user/               # User service
│   │   └── main.go
│   ├── order/              # Order service
│   │   └── main.go
│   └── payment/            # Payment service
│       └── main.go
│
├── internal/               # Private code for each service
│   ├── gateway/            # Gateway service
│   │   ├── handler/        # Handlers
│   │   ├── service/        # Business logic
│   │   ├── middleware/      # Middleware
│   │   └── net/            # Network layer (routing, server)
│   │
│   ├── user/               # User service
│   │   ├── handler/
│   │   ├── service/
│   │   ├── repository/
│   │   └── model/
│   │
│   ├── order/              # Order service
│   │   ├── handler/
│   │   ├── service/
│   │   ├── repository/
│   │   └── model/
│   │
│   └── payment/            # Payment service
│       ├── handler/
│       ├── service/
│       ├── repository/
│       └── model/
│
├── pkg/                    # Public libraries (shared across services)
│   ├── config/
│   ├── logger/
│   ├── errors/
│   ├── database/
│   ├── redis/
│   ├── auth/
│   ├── middleware/          # Common middleware
│   ├── utils/
│   └── models/
│
├── api/                    # API definitions
│   ├── proto/              # Protobuf definitions
│   │   ├── user/
│   │   │   └── user.proto
│   │   ├── order/
│   │   │   └── order.proto
│   │   └── payment/
│   │       └── payment.proto
│   ├── graphql/            # GraphQL Schema (if used)
│   └── openapi/            # OpenAPI definitions
│
├── ent/                    # ORM models (if using ent)
│   ├── schema/             # Schema definitions
│   ├── migrate/            # Migration files
│   └── runtime/            # Runtime configuration
│
├── configs/                # Configuration files
│   ├── gateway.yaml
│   ├── user.yaml
│   ├── order.yaml
│   └── payment.yaml
│
├── scripts/                # Scripts and tools
│   ├── build.sh
│   ├── generate.sh         # Code generation
│   └── migrate.sh
│
├── build/                  # Build configuration for each service
│   ├── gateway/
│   │   └── Dockerfile
│   ├── user/
│   │   └── Dockerfile
│   ├── order/
│   │   └── Dockerfile
│   └── payment/
│       └── Dockerfile
│
├── deployments/            # Deployment configuration
│   ├── docker-compose.yml  # Local development environment
│   └── k8s/                # Kubernetes configuration
│
├── test/                   # Tests
│   ├── integration/
│   └── e2e/                # End-to-end tests
│
├── docs/                   # Documentation
├── tools/                  # Project tools
│
├── go.mod
├── go.sum
├── Makefile
├── .gitignore
├── .env.example
└── README.md
```

---

## 3. Detailed Directory Descriptions

### `/cmd` - Main Application Entry Points

- Each subdirectory corresponds to an executable file
- `main.go` should be minimal, only handling startup and dependency injection
- No business logic, only responsible for initialization and startup

```go
// cmd/app/main.go example
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

### `/internal` - Private Code

- Go compiler enforces that it cannot be imported by external packages
- Organized by service or module
- Typically contains:
  - `handler/` - HTTP/gRPC request handling
  - `service/` - Business logic
  - `repository/` - Data access
  - `model/` - Domain models
  - `middleware/` - Middleware
  - `net/` - Network layer (routing, server configuration)

### `/pkg` - Public Libraries

- Code that can be imported by external projects
- Generic, reusable tools and libraries
- Common subpackages:
  - `config/` - Configuration loading (environment variables, config files)
  - `logger/` - Logging wrapper (logrus, zap)
  - `errors/` - Unified error types and handling
  - `database/` - Database connections and tools
  - `redis/` - Redis client wrapper
  - `auth/` - Authentication tools (JWT, OAuth)
  - `middleware/` - HTTP/gRPC middleware
  - `utils/` - Utility functions
  - `models/` - Public data models

### `/api` - API Definitions

- `proto/` - Protobuf files (gRPC)
- `graphql/` - GraphQL Schema
- `openapi/` - OpenAPI/Swagger definitions

### `/ent` - ORM Models (Optional)

For projects using [ent](https://entgo.io/) as ORM:

- `schema/` - Entity Schema definitions
- `migrate/` - Database migrations
- `runtime/` - Runtime hooks

### `/configs` - Configuration Files

- Configuration templates and defaults
- Do not store sensitive information (use environment variables)

### `/scripts` - Scripts and Tools

- Build, deployment, code generation scripts
- Keep Makefile concise

### `/build` - Build Configuration

- Dockerfile
- CI/CD configuration

### `/deployments` - Deployment Configuration

- Docker Compose
- Kubernetes manifests
- Terraform configuration

### `/test` - Tests

- Integration tests
- End-to-end tests
- Test data (`testdata/`)

### `/docs` - Documentation

- API documentation
- Architecture documentation
- Development guides

---

## 4. Naming Conventions

### Package Naming

- Use lowercase, singular form
- Avoid underscores and mixed case
- Short but descriptive

```
✅ Correct: user, order, config, logger
❌ Wrong: users, UserOrder, user_config, utils
```

### File Naming

- Use underscores: `user_service.go`
- Test files: `user_service_test.go`

### Interface Naming

- Single method: `-er` suffix (`Reader`, `Writer`)
- Multiple methods: Descriptive names (`UserService`, `Repository`)

---

## 5. Quick Start Templates

### Monolithic Service

```bash
# Create directory structure
mkdir -p cmd/app internal/{handler,service,repository,model} pkg/{config,logger,errors} api configs scripts build docs

# Initialize module
go mod init your-project

# Create entry point
cat > cmd/app/main.go << 'EOF'
package main

import "fmt"

func main() {
    fmt.Println("Starting application...")
}
EOF
```

### Microservice

```bash
# Create directory structure
mkdir -p cmd/{gateway,user,order} internal/{gateway,user,order}/{handler,service,repository,model} pkg/{config,logger,errors,database} api/proto/{user,order} configs build/{gateway,user,order}

# Initialize module
go mod init your-project
```

---

## 6. Best Practices

1. **Keep cmd minimal**: Only initialization and startup, business logic in internal

2. **internal protects private code**: Use Go compiler enforcement

3. **Use pkg carefully**: Only truly reusable code, not all code needs to be in pkg

4. **Organize by domain**: In microservice architecture, organize by service boundaries

5. **Dependency injection**: Use tools like wire, fx to manage dependencies

6. **Externalize configuration**: Sensitive information in environment variables, config files in configs

7. **Separate generated code**: Don't manually edit code generated by ent, protobuf, graphql

---

## 7. Frequently Asked Questions

### Q: When to use monolithic vs microservice?

- **Monolithic**: Small projects, rapid prototypes, smaller teams
- **Microservice**: Large projects, multiple teams, need independent scaling and deployment

### Q: How to choose between pkg and internal?

- `internal`: Project private code, don't want external imports
- `pkg`: Generic libraries, can be reused by other projects

### Q: Where to put configuration files?

- Configuration templates and examples: `configs/`
- Sensitive information: Environment variables or secret management services
- Local development config: `.env` (add to .gitignore)

---

## 8. Reference Projects

- [golang-standards/project-layout](https://github.com/golang-standards/project-layout)
- [Go Official Documentation - Organizing a Go module](https://go.dev/doc/modules/layout)