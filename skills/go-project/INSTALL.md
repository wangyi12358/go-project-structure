# Go Project Skill - Installation Guide

## Quick Installation

### Using npx (Recommended)

```bash
# Install globally
npx @opencode-ai/skill-go-project

# Or add to project
npx skills add @opencode-ai/skill-go-project
```

### Manual Installation

```bash
# Clone the repository
git clone https://github.com/opencode-ai/skill-go-project.git

# Copy to skills directory
cp -r skill-go-project ~/.config/opencode/skills/go-project
```

## What's Included

This skill provides:

1. **Project Structure Templates**
   - Monolithic service layout
   - Microservice architecture layout

2. **Code Templates**
   - Main entry point
   - HTTP handlers
   - Service layer
   - Repository pattern
   - Domain models
   - Middleware
   - Error handling
   - Configuration

3. **Configuration Templates**
   - Makefile
   - Dockerfile
   - docker-compose.yml

4. **Common Commands**
   - Module management
   - Building and running
   - Testing and coverage
   - Code quality

5. **Best Practices**
   - Package organization
   - Naming conventions
   - Error handling patterns
   - Dependency injection

## Usage

Once installed, the skill is automatically available when working with Go projects. opencode will load the skill when:

- You ask about Go project structure
- You create a new Go project
- You need Go code templates
- You want best practices

### Example Queries

```
"How should I structure my Go project?"
"Create a new Go project with best practices"
"What's the recommended way to handle errors in Go?"
"Show me a Go HTTP handler template"
```

## Publishing

To publish your own version:

```bash
# Login to npm
npm login

# Publish
npm publish --access public
```

## Development

```bash
# Clone repository
git clone https://github.com/opencode-ai/skill-go-project.git

# Make changes
cd skill-go-project
# Edit SKILL.md

# Test locally
# Copy to ~/.config/opencode/skills/go-project/
```

## License

MIT License - see [LICENSE](LICENSE) file