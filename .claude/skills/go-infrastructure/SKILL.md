---
name: go-infrastructure
description: Use when writing Go code for infrastructure tools, Kubernetes operators,
  Prometheus exporters, or CLI tools. Applies Go conventions and patterns specific
  to this project.
allowed-tools: Read, Grep, Glob, Bash(go:*), Bash(golangci-lint:*)
---

# Go Infrastructure Tool Patterns

## Project Structure
All Go tools in this project follow this layout:
```
tool-name/
├── cmd/            # Main entrypoints (one per binary)
├── internal/       # Private packages — prefer internal/ over pkg/
├── Dockerfile      # Multi-stage build: builder → distroless/static
├── Makefile        # Targets: build, test, lint, docker-build, release
└── .goreleaser.yml # Release configuration
```

## Coding Standards
- Error wrapping: `fmt.Errorf("context: %w", err)` — always wrap with context
- Structured logging: `slog` (Go 1.21+) — no fmt.Println in production code
- Context propagation: every function that does I/O takes `context.Context` as first arg
- Table-driven tests: `[]struct{ name, input, want }` pattern for all unit tests
- Interfaces for external deps: K8s client, HTTP, libvirt — enables testing without real infra

## Kubernetes Client Pattern
```go
import (
    "k8s.io/client-go/kubernetes"
    "k8s.io/client-go/tools/clientcmd"
)

func newK8sClient() (*kubernetes.Clientset, error) {
    config, err := clientcmd.BuildConfigFromFlags("",
        clientcmd.RecommendedHomeFile)
    if err != nil {
        return nil, fmt.Errorf("building kubeconfig: %w", err)
    }
    return kubernetes.NewForConfig(config)
}
```

## Prometheus Exporter Pattern
```go
import "github.com/prometheus/client_golang/prometheus"

var vmCount = prometheus.NewGaugeVec(prometheus.GaugeOpts{
    Namespace: "kvm",
    Name:      "vm_count",
    Help:      "Number of VMs by state.",
}, []string{"state"})

func init() { prometheus.MustRegister(vmCount) }
```

## CLI Pattern (cobra)
```go
var rootCmd = &cobra.Command{
    Use:   "platform",
    Short: "Platform engineering operations CLI",
}

func Execute() error { return rootCmd.Execute() }
```

## Dockerfile (multi-stage, distroless)
```dockerfile
FROM golang:1.22 AS builder
WORKDIR /app
COPY go.mod go.sum ./
RUN go mod download
COPY . .
RUN CGO_ENABLED=0 go build -o /bin/tool ./cmd/tool

FROM gcr.io/distroless/static-debian12
COPY --from=builder /bin/tool /tool
ENTRYPOINT ["/tool"]
```

## Before Committing
Always run in this order:
1. `go fmt ./...`
2. `go vet ./...`
3. `golangci-lint run ./...`
4. `go test -race ./...`
