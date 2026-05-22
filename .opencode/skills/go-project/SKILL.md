---
name: go-project
description: Use when working with Go projects. Provides Go project structure guidelines, common commands, best practices, code templates, and directory organization for monolithic and microservice architectures.
---

# Go Project Skill

Comprehensive guide for Go project development, including project structure, best practices, and common workflows.

## Project Structure

### Monolithic Service

```
project/
├── cmd/                    # Main application entry points
│   └── app/
│       └── main.go         # Startup file, keep minimal
│
├── internal/               # Private code (cannot be imported externally)
│   ├── handler/            # HTTP/gRPC handlers
│   ├── service/            # Business logic layer
│   ├── repository/         # Data access layer (GORM implementation)
│   ├── model/              # Domain models (GORM models)
│   └── middleware/          # Middleware
│
├── pkg/                    # Public libraries (can be imported externally)
│   ├── config/             # Configuration loading
│   ├── logger/             # Logging wrapper
│   ├── errors/             # Unified error handling
│   ├── database/           # Database connections (GORM init)
│   │   ├── gorm.go         # GORM initialization
│   │   ├── migrate.go      # Auto migration
│   │   └── transaction.go  # Transaction helper
│   ├── redis/              # Redis client
│   ├── auth/               # Authentication tools (JWT, etc.)
│   ├── utils/              # Utility functions
│   └── models/             # Public data models
│
├── migrations/             # Database migration files (optional)
│   ├── 001_create_users.up.sql
│   ├── 001_create_users.down.sql
│   └── ...
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
├── go.mod                  # Go module definition
├── go.sum                  # Dependency checksum
├── Makefile                # Build commands
├── .gitignore              # Git ignore rules
└── README.md               # Project documentation
```

### Microservice Architecture

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
│   └── openapi/            # OpenAPI definitions
│
├── configs/                # Configuration files
│   ├── gateway.yaml
│   ├── user.yaml
│   ├── order.yaml
│   └── payment.yaml
│
├── go.mod
├── go.sum
├── Makefile
└── README.md
```

## Common Commands

### Module Management

```bash
# Initialize module
go mod init github.com/username/project

# Add dependency
go get github.com/some/package

# Update dependency
go get -u github.com/some/package

# Tidy dependencies (remove unused)
go mod tidy

# Download dependencies
go mod download

# Show dependency graph
go mod graph

# Verify dependencies
go mod verify
```

### Building

```bash
# Build current package
go build

# Build specific package
go build ./cmd/app

# Build with output
go build -o bin/app ./cmd/app

# Cross-compile for Linux
GOOS=linux GOARCH=amd64 go build -o bin/app-linux ./cmd/app

# Build all packages
go build ./...

# Build with race detector
go build -race ./cmd/app
```

### Testing

```bash
# Run all tests
go test ./...

# Run tests with verbose output
go test -v ./...

# Run tests with coverage
go test -cover ./...

# Generate coverage report
go test -coverprofile=coverage.out ./...
go tool cover -html=coverage.out

# Run specific test
go test -run TestFunctionName ./path/to/package

# Run tests with race detector
go test -race ./...

# Run benchmarks
go test -bench=. ./...

# Run tests multiple times
go test -count=5 ./...
```

### Code Quality

```bash
# Format code
gofmt -w .

# Check formatting
gofmt -l .

# Run go vet
go vet ./...

# Run linter (install golangci-lint first)
golangci-lint run

# Run staticcheck
staticcheck ./...
```

### Running

```bash
# Run main package
go run ./cmd/app

# Run with arguments
go run ./cmd/app --flag value

# Run with environment variables
PORT=8080 go run ./cmd/app
```

## Code Templates

### Main Entry Point (cmd/app/main.go)

```go
package main

import (
    "context"
    "log"
    "os"
    "os/signal"
    "syscall"

    "github.com/username/project/internal/app"
    "github.com/username/project/pkg/config"
)

func main() {
    // Load configuration
    cfg, err := config.Load()
    if err != nil {
        log.Fatalf("Failed to load config: %v", err)
    }

    // Create application
    application, err := app.New(cfg)
    if err != nil {
        log.Fatalf("Failed to create app: %v", err)
    }

    // Handle graceful shutdown
    ctx, cancel := context.WithCancel(context.Background())
    defer cancel()

    go func() {
        sigCh := make(chan os.Signal, 1)
        signal.Notify(sigCh, syscall.SIGINT, syscall.SIGTERM)
        <-sigCh
        cancel()
    }()

    // Run application
    if err := application.Run(ctx); err != nil {
        log.Fatalf("Application error: %v", err)
    }
}
```

### HTTP Handler (internal/handler/user.go)

```go
package handler

import (
    "encoding/json"
    "net/http"

    "github.com/username/project/internal/service"
    "github.com/username/project/pkg/errors"
)

type UserHandler struct {
    userService *service.UserService
}

func NewUserHandler(userService *service.UserService) *UserHandler {
    return &UserHandler{userService: userService}
}

func (h *UserHandler) GetUser(w http.ResponseWriter, r *http.Request) {
    id := r.URL.Query().Get("id")
    if id == "" {
        http.Error(w, "missing id parameter", http.StatusBadRequest)
        return
    }

    user, err := h.userService.GetUser(r.Context(), id)
    if err != nil {
        if errors.IsNotFound(err) {
            http.Error(w, "user not found", http.StatusNotFound)
            return
        }
        http.Error(w, "internal error", http.StatusInternalServerError)
        return
    }

    w.Header().Set("Content-Type", "application/json")
    json.NewEncoder(w).Encode(user)
}
```

### Service Layer (internal/service/user.go)

```go
package service

import (
    "context"

    "github.com/username/project/internal/model"
    "github.com/username/project/internal/repository"
    "github.com/username/project/pkg/errors"
)

type UserService struct {
    userRepo repository.UserRepository
}

func NewUserService(userRepo repository.UserRepository) *UserService {
    return &UserService{userRepo: userRepo}
}

func (s *UserService) GetUser(ctx context.Context, id string) (*model.User, error) {
    user, err := s.userRepo.FindByID(ctx, id)
    if err != nil {
        return nil, errors.Wrap(err, "failed to get user")
    }
    if user == nil {
        return nil, errors.NewNotFound("user not found")
    }
    return user, nil
}

func (s *UserService) CreateUser(ctx context.Context, input *model.CreateUserInput) (*model.User, error) {
    // Validate input
    if err := input.Validate(); err != nil {
        return nil, errors.NewValidation(err.Error())
    }

    // Check if user exists
    existing, err := s.userRepo.FindByEmail(ctx, input.Email)
    if err != nil {
        return nil, errors.Wrap(err, "failed to check existing user")
    }
    if existing != nil {
        return nil, errors.NewConflict("user already exists")
    }

    // Create user
    user := &model.User{
        Name:  input.Name,
        Email: input.Email,
    }

    if err := s.userRepo.Create(ctx, user); err != nil {
        return nil, errors.Wrap(err, "failed to create user")
    }

    return user, nil
}
```

### Repository Layer (internal/repository/user_repository.go)

```go
package repository

import (
    "context"

    "github.com/username/project/internal/model"
)

type UserRepository interface {
    FindByID(ctx context.Context, id uint) (*model.User, error)
    FindByEmail(ctx context.Context, email string) (*model.User, error)
    FindAll(ctx context.Context, offset, limit int) ([]*model.User, int64, error)
    Create(ctx context.Context, user *model.User) error
    Update(ctx context.Context, user *model.User) error
    Delete(ctx context.Context, id uint) error
}
```

### GORM Repository Implementation (internal/repository/user_repo_gorm.go)

```go
package repository

import (
    "context"

    "github.com/username/project/internal/model"
    "gorm.io/gorm"
)

type userGormRepo struct {
    db *gorm.DB
}

func NewUserGormRepo(db *gorm.DB) UserRepository {
    return &userGormRepo{db: db}
}

func (r *userGormRepo) FindByID(ctx context.Context, id uint) (*model.User, error) {
    var user model.User
    if err := r.db.WithContext(ctx).First(&user, id).Error; err != nil {
        if err == gorm.ErrRecordNotFound {
            return nil, nil
        }
        return nil, err
    }
    return &user, nil
}

func (r *userGormRepo) FindByEmail(ctx context.Context, email string) (*model.User, error) {
    var user model.User
    if err := r.db.WithContext(ctx).Where("email = ?", email).First(&user).Error; err != nil {
        if err == gorm.ErrRecordNotFound {
            return nil, nil
        }
        return nil, err
    }
    return &user, nil
}

func (r *userGormRepo) FindAll(ctx context.Context, offset, limit int) ([]*model.User, int64, error) {
    var users []*model.User
    var total int64

    if err := r.db.WithContext(ctx).Model(&model.User{}).Count(&total).Error; err != nil {
        return nil, 0, err
    }

    if err := r.db.WithContext(ctx).Offset(offset).Limit(limit).Find(&users).Error; err != nil {
        return nil, 0, err
    }

    return users, total, nil
}

func (r *userGormRepo) Create(ctx context.Context, user *model.User) error {
    return r.db.WithContext(ctx).Create(user).Error
}

func (r *userGormRepo) Update(ctx context.Context, user *model.User) error {
    return r.db.WithContext(ctx).Save(user).Error
}

func (r *userGormRepo) Delete(ctx context.Context, id uint) error {
    return r.db.WithContext(ctx).Delete(&model.User{}, id).Error
}
```

### Domain Model (internal/model/user.go)

```go
package model

import (
    "time"

    "gorm.io/gorm"
)

// BaseModel contains common fields for all models
type BaseModel struct {
    ID        uint           `json:"id" gorm:"primaryKey"`
    CreatedAt time.Time      `json:"created_at"`
    UpdatedAt time.Time      `json:"updated_at"`
    DeletedAt gorm.DeletedAt `json:"-" gorm:"index"`
}

type User struct {
    BaseModel
    Name     string `json:"name" gorm:"size:100;not null"`
    Email    string `json:"email" gorm:"size:255;uniqueIndex;not null"`
    Password string `json:"-" gorm:"size:255;not null"`
    Age      int    `json:"age" gorm:"default:0"`
    Status   int    `json:"status" gorm:"default:1"` // 1: active, 0: inactive
}

// TableName specifies the table name for User model
func (User) TableName() string {
    return "users"
}

type CreateUserInput struct {
    Name     string `json:"name" binding:"required"`
    Email    string `json:"email" binding:"required,email"`
    Password string `json:"password" binding:"required,min=6"`
    Age      int    `json:"age"`
}

type UpdateUserInput struct {
    Name   string `json:"name"`
    Email  string `json:"email"`
    Age    int    `json:"age"`
    Status int    `json:"status"`
}
```

### Middleware (internal/middleware/logging.go)

```go
package middleware

import (
    "log"
    "net/http"
    "time"
)

func Logging(next http.Handler) http.Handler {
    return http.HandlerFunc(func(w http.ResponseWriter, r *http.Request) {
        start := time.Now()

        // Wrap response writer to capture status code
        wrapped := &responseWriter{ResponseWriter: w, statusCode: http.StatusOK}

        next.ServeHTTP(wrapped, r)

        log.Printf(
            "%s %s %d %s",
            r.Method,
            r.URL.Path,
            wrapped.statusCode,
            time.Since(start),
        )
    })
}

type responseWriter struct {
    http.ResponseWriter
    statusCode int
}

func (rw *responseWriter) WriteHeader(code int) {
    rw.statusCode = code
    rw.ResponseWriter.WriteHeader(code)
}
```

### Error Handling (pkg/errors/errors.go)

```go
package errors

import (
    "fmt"
    "net/http"
)

type AppError struct {
    Code    int    `json:"code"`
    Message string `json:"message"`
    Err     error  `json:"-"`
}

func (e *AppError) Error() string {
    if e.Err != nil {
        return fmt.Sprintf("%s: %v", e.Message, e.Err)
    }
    return e.Message
}

func NewNotFound(message string) *AppError {
    return &AppError{
        Code:    http.StatusNotFound,
        Message: message,
    }
}

func NewValidation(message string) *AppError {
    return &AppError{
        Code:    http.StatusBadRequest,
        Message: message,
    }
}

func NewConflict(message string) *AppError {
    return &AppError{
        Code:    http.StatusConflict,
        Message: message,
    }
}

func Wrap(err error, message string) error {
    return fmt.Errorf("%s: %w", message, err)
}

func IsNotFound(err error) bool {
    if appErr, ok := err.(*AppError); ok {
        return appErr.Code == http.StatusNotFound
    }
    return false
}
```

### Configuration (pkg/config/config.go)

```go
package config

import (
    "os"

    "gopkg.in/yaml.v3"
)

type Config struct {
    Server   ServerConfig   `yaml:"server"`
    Database DatabaseConfig `yaml:"database"`
    Redis    RedisConfig    `yaml:"redis"`
}

type ServerConfig struct {
    Port         int    `yaml:"port"`
    ReadTimeout  int    `yaml:"read_timeout"`
    WriteTimeout int    `yaml:"write_timeout"`
}

type DatabaseConfig struct {
    Host     string `yaml:"host"`
    Port     int    `yaml:"port"`
    User     string `yaml:"user"`
    Password string `yaml:"password"`
    DBName   string `yaml:"dbname"`
}

type RedisConfig struct {
    Host     string `yaml:"host"`
    Port     int    `yaml:"port"`
    Password string `yaml:"password"`
}

func Load() (*Config, error) {
    configPath := os.Getenv("CONFIG_PATH")
    if configPath == "" {
        configPath = "configs/config.yaml"
    }

    data, err := os.ReadFile(configPath)
    if err != nil {
        return nil, err
    }

    cfg := &Config{}
    if err := yaml.Unmarshal(data, cfg); err != nil {
        return nil, err
    }

    return cfg, nil
}
```

### GORM Database Connection (pkg/database/gorm.go)

```go
package database

import (
    "fmt"
    "log"
    "time"

    "github.com/username/project/pkg/config"
    "gorm.io/driver/mysql"
    "gorm.io/driver/postgres"
    "gorm.io/gorm"
    "gorm.io/gorm/logger"
)

func NewGormDB(cfg config.DatabaseConfig) (*gorm.DB, error) {
    var dialector gorm.Dialector

    switch cfg.Driver {
    case "mysql":
        dsn := fmt.Sprintf("%s:%s@tcp(%s:%d)/%s?charset=utf8mb4&parseTime=True&loc=Local",
            cfg.User, cfg.Password, cfg.Host, cfg.Port, cfg.DBName)
        dialector = mysql.Open(dsn)
    case "postgres":
        dsn := fmt.Sprintf("host=%s user=%s password=%s dbname=%d port=%d sslmode=disable TimeZone=Asia/Shanghai",
            cfg.Host, cfg.User, cfg.Password, cfg.DBName, cfg.Port)
        dialector = postgres.Open(dsn)
    default:
        return nil, fmt.Errorf("unsupported database driver: %s", cfg.Driver)
    }

    db, err := gorm.Open(dialector, &gorm.Config{
        Logger: logger.Default.LogMode(logger.Info),
        NowFunc: func() time.Time {
            return time.Now().Local()
        },
    })
    if err != nil {
        return nil, fmt.Errorf("failed to connect database: %w", err)
    }

    sqlDB, err := db.DB()
    if err != nil {
        return nil, fmt.Errorf("failed to get underlying sql.DB: %w", err)
    }

    // Connection pool settings
    sqlDB.SetMaxIdleConns(10)
    sqlDB.SetMaxOpenConns(100)
    sqlDB.SetConnMaxLifetime(time.Hour)

    log.Println("Database connected successfully")
    return db, nil
}
```

### GORM Auto Migration (pkg/database/migrate.go)

```go
package database

import (
    "log"

    "gorm.io/gorm"
)

func AutoMigrate(db *gorm.DB, models ...interface{}) error {
    for _, model := range models {
        if err := db.AutoMigrate(model); err != nil {
            return err
        }
        log.Printf("Auto migrated: %T", model)
    }
    return nil
}
```

### GORM Transaction Helper (pkg/database/transaction.go)

```go
package database

import (
    "context"

    "gorm.io/gorm"
)

func WithTransaction(ctx context.Context, db *gorm.DB, fn func(tx *gorm.DB) error) error {
    return db.WithContext(ctx).Transaction(fn)
}

func WithTransactionCtx(ctx context.Context, db *gorm.DB, fn func(ctx context.Context, tx *gorm.DB) error) error {
    return db.WithContext(ctx).Transaction(func(tx *gorm.DB) error {
        return fn(ctx, tx)
    })
}
```

### SQL Migration File Example (migrations/001_create_users.up.sql)

```sql
CREATE TABLE IF NOT EXISTS users (
    id BIGINT UNSIGNED NOT NULL AUTO_INCREMENT,
    created_at DATETIME(3) NULL,
    updated_at DATETIME(3) NULL,
    deleted_at DATETIME(3) NULL,
    name VARCHAR(100) NOT NULL,
    email VARCHAR(255) NOT NULL,
    password VARCHAR(255) NOT NULL,
    age INT DEFAULT 0,
    status INT DEFAULT 1,
    PRIMARY KEY (id),
    INDEX idx_users_deleted_at (deleted_at),
    UNIQUE INDEX idx_users_email (email)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_unicode_ci;
```

### SQL Migration Rollback (migrations/001_create_users.down.sql)

```sql
DROP TABLE IF EXISTS users;
```

### Makefile Template

```makefile
.PHONY: build run test lint clean

# Variables
APP_NAME := myapp
BUILD_DIR := ./bin
GO := go

# Build
build:
	$(GO) build -o $(BUILD_DIR)/$(APP_NAME) ./cmd/app

# Run
run:
	$(GO) run ./cmd/app

# Test
test:
	$(GO) test -v ./...

# Test with coverage
test-cover:
	$(GO) test -coverprofile=coverage.out ./...
	$(GO) tool cover -html=coverage.out

# Lint
lint:
	golangci-lint run

# Format
fmt:
	gofmt -w .

# Vet
vet:
	$(GO) vet ./...

# Clean
clean:
	rm -rf $(BUILD_DIR)

# Tidy dependencies
tidy:
	$(GO) mod tidy

# Cross-compile for Linux
build-linux:
	GOOS=linux GOARCH=amd64 $(GO) build -o $(BUILD_DIR)/$(APP_NAME)-linux ./cmd/app

# Docker build
docker-build:
	docker build -t $(APP_NAME) .

# Docker run
docker-run:
	docker run -p 8080:8080 $(APP_NAME)

# Help
help:
	@echo "Available targets:"
	@echo "  build        - Build the application"
	@echo "  run          - Run the application"
	@echo "  test         - Run tests"
	@echo "  test-cover   - Run tests with coverage"
	@echo "  lint         - Run linter"
	@echo "  fmt          - Format code"
	@echo "  vet          - Run go vet"
	@echo "  clean        - Clean build artifacts"
	@echo "  tidy         - Tidy dependencies"
	@echo "  build-linux  - Cross-compile for Linux"
	@echo "  docker-build - Build Docker image"
	@echo "  docker-run   - Run Docker container"
```

### Dockerfile Template

```dockerfile
# Build stage
FROM golang:1.21-alpine AS builder

WORKDIR /app

# Copy go mod files
COPY go.mod go.sum ./
RUN go mod download

# Copy source code
COPY . .

# Build
RUN CGO_ENABLED=0 GOOS=linux go build -o /app/bin/main ./cmd/app

# Final stage
FROM alpine:latest

RUN apk --no-cache add ca-certificates

WORKDIR /app

# Copy binary from builder
COPY --from=builder /app/bin/main .

# Expose port
EXPOSE 8080

# Run
CMD ["./main"]
```

### docker-compose.yml Template

```yaml
version: '3.8'

services:
  app:
    build: .
    ports:
      - "8080:8080"
    environment:
      - CONFIG_PATH=/app/configs/config.yaml
    depends_on:
      - postgres
      - redis
    volumes:
      - ./configs:/app/configs

  postgres:
    image: postgres:15-alpine
    environment:
      - POSTGRES_USER=postgres
      - POSTGRES_PASSWORD=postgres
      - POSTGRES_DB=mydb
    ports:
      - "5432:5432"
    volumes:
      - postgres_data:/var/lib/postgresql/data

  redis:
    image: redis:7-alpine
    ports:
      - "6379:6379"

volumes:
  postgres_data:
```

## Naming Conventions

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

## Best Practices

1. **Keep cmd minimal**: Only initialization and startup, business logic in internal

2. **internal protects private code**: Use Go compiler enforcement

3. **Use pkg carefully**: Only truly reusable code

4. **Organize by domain**: In microservice architecture, organize by service boundaries

5. **Dependency injection**: Use wire, fx, etc.

6. **Externalize configuration**: Sensitive info in env vars, config files in configs

7. **Separate generated code**: Don't manually edit ent, protobuf, graphql generated code

8. **Error handling**: Use custom error types, wrap errors with context

9. **Logging**: Use structured logging (zap, zerolog)

10. **Testing**: Write unit tests, integration tests, and use testdata directory

## Common Patterns

### Dependency Injection

```go
// cmd/app/main.go
func main() {
    // Initialize dependencies
    db := database.New(cfg.Database)
    userRepo := repository.NewUserRepository(db)
    userService := service.NewUserService(userRepo)
    userHandler := handler.NewUserHandler(userService)

    // Setup router
    mux := http.NewServeMux()
    mux.HandleFunc("/users", userHandler.GetUser)

    // Start server
    server := &http.Server{
        Addr:    fmt.Sprintf(":%d", cfg.Server.Port),
        Handler: mux,
    }
    server.ListenAndServe()
}
```

### Graceful Shutdown

```go
ctx, cancel := context.WithCancel(context.Background())
defer cancel()

go func() {
    sigCh := make(chan os.Signal, 1)
    signal.Notify(sigCh, syscall.SIGINT, syscall.SIGTERM)
    <-sigCh
    cancel()
}()

if err := server.Shutdown(ctx); err != nil {
    log.Printf("Shutdown error: %v", err)
}
```

### Middleware Chain

```go
func NewRouter() http.Handler {
    mux := http.NewServeMux()

    // Setup routes
    mux.HandleFunc("/api/users", userHandler.List)
    mux.HandleFunc("/api/users/{id}", userHandler.Get)

    // Apply middleware
    return middleware.Chain(
        mux,
        middleware.Logging,
        middleware.Recovery,
        middleware.CORS,
        middleware.Auth,
    )
}
```

## Quick Start

```bash
# Create project structure
mkdir -p cmd/app internal/{handler,service,repository,model} pkg/{config,logger,errors} api configs scripts build docs

# Initialize module
go mod init github.com/username/project

# Create main.go
cat > cmd/app/main.go << 'EOF'
package main

import "fmt"

func main() {
    fmt.Println("Starting application...")
}
EOF

# Run
go run ./cmd/app
```

## References

- [Go Official Documentation](https://go.dev/doc/)
- [Effective Go](https://go.dev/doc/effective_go)
- [Go Project Layout](https://github.com/golang-standards/project-layout)
- [Go Code Review Comments](https://go.dev/wiki/CodeReviewComments)