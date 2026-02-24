---
description: Scaffold a new Go infrastructure tool following project conventions
allowed-tools: Read, Write, Bash(go:*), Bash(git:*), Bash(mkdir:*)
---

Scaffold a new Go tool in `go-tools/$ARGUMENTS` following the project's Go conventions.

Read the Go infrastructure skill file first:
.claude/skills/go-infrastructure/SKILL.md

Then create:
1. `go-tools/$ARGUMENTS/cmd/$ARGUMENTS/main.go` — main entrypoint
2. `go-tools/$ARGUMENTS/internal/config/config.go` — configuration
3. `go-tools/$ARGUMENTS/Makefile` — build, test, lint, docker targets
4. `go-tools/$ARGUMENTS/Dockerfile` — multi-stage, distroless output
5. `go-tools/$ARGUMENTS/.goreleaser.yml` — release configuration
6. `go-tools/$ARGUMENTS/go.mod` — module initialization
7. `go-tools/$ARGUMENTS/README.md` — tool description and usage

After scaffolding:
cd go-tools/$ARGUMENTS
go mod tidy
go build ./...
go test ./...
golangci-lint run ./...

Report any compilation errors or lint warnings.
