---
description: Run tests and linting for a Go package or the entire module
allowed-tools: Bash(go:*), Bash(golangci-lint:*)
---

Run the full test and lint suite for $ARGUMENTS (default: ./... for the whole module).

Steps:
1. `go fmt $ARGUMENTS` — format check
2. `go vet $ARGUMENTS` — static analysis
3. `golangci-lint run $ARGUMENTS` — full linter suite including gosec
4. `go test -v -race -coverprofile=coverage.out $ARGUMENTS` — tests with race detector
5. `go tool cover -func=coverage.out | tail -1` — print total coverage

Report:
- Any lint violations with file:line references
- Failed tests with full output
- Coverage percentage; flag if below 70%
- Suggested fixes for any failures
