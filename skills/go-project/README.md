# Go Project Skill for opencode

A comprehensive skill for Go project development, providing project structure guidelines, common commands, best practices, and code templates.

## Installation

### Using npx (Recommended)

```bash
npx @opencode-ai/skill-go-project
```

Or add to your project:

```bash
npx skills add @opencode-ai/skill-go-project
```

### Manual Installation

1. Download the `SKILL.md` file
2. Place it in your skills directory:
   - Project: `.opencode/skills/go-project/SKILL.md`
   - Global: `~/.config/opencode/skills/go-project/SKILL.md`

## What's Included

### Project Structure Templates

- **Monolithic Service**: Standard Go project layout
- **Microservice Architecture**: Multi-service project layout

### Code Templates

- Main entry point (`cmd/app/main.go`)
- HTTP handlers (`internal/handler/`)
- Service layer (`internal/service/`)
- Repository pattern (`internal/repository/`)
- Domain models (`internal/model/`)
- Middleware (`internal/middleware/`)
- Error handling (`pkg/errors/`)
- Configuration (`pkg/config/`)

### Configuration Templates

- `Makefile` - Build commands
- `Dockerfile` - Container configuration
- `docker-compose.yml` - Local development environment

### Common Commands

- Module management
- Building and running
- Testing and coverage
- Code quality and linting

### Best Practices

- Package organization
- Naming conventions
- Error handling patterns
- Dependency injection
- Graceful shutdown

## Usage

Once installed, the skill is automatically available when working with Go projects. opencode will load the skill when it detects Go-related files or when you ask about Go project structure.

### Example Queries

- "How should I structure my Go project?"
- "Create a new Go project with best practices"
- "What's the recommended way to handle errors in Go?"
- "Show me a Go HTTP handler template"
- "How to set up dependency injection in Go?"

## Quick Start

```bash
# Create a new Go project
mkdir my-project && cd my-project

# Initialize Go module
go mod init github.com/username/my-project

# Create project structure
mkdir -p cmd/app internal/{handler,service,repository,model} pkg/{config,logger,errors} api configs

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

## Customization

You can customize the skill by editing the `SKILL.md` file:

1. Add your own code templates
2. Modify project structure
3. Add company-specific conventions
4. Include additional best practices

## Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

### Development

1. Fork the repository
2. Create your feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add some amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## Acknowledgments

- [Go Official Documentation](https://go.dev/doc/)
- [Effective Go](https://go.dev/doc/effective_go)
- [Go Project Layout](https://github.com/golang-standards/project-layout)

## Support

- [Issues](https://github.com/opencode-ai/skill-go-project/issues)
- [Documentation](https://opencode.ai)