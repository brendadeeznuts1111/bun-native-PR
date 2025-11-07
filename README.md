# Bun Native Pipeline (BPM)

Enterprise-grade, zero-dependency build pipeline compiled into a single binary using Bun with advanced production features.

## 🚀 Advanced Features

| Feature | Usage in Pipeline | Benefit |
|---------|-------------------|---------|
| **YAML import** | Direct `import config from "./bun.yaml"` | Zero parsing code, type-safe |
| **DisposableStack** | Auto-cleanup workers, sockets, processes | No leaks, DRY cleanup |
| **Stream helpers** | `.text()`, `.bytes()`, `.json()` | Cleaner code, no TextDecoder |
| **Zstd** | Compress deltas & binaries | 30-50% smaller artefacts |
| **Bun.secrets** | Store registry tokens in OS keychain | Secure, not in env vars |
| **CSRF** | Protect registry auth endpoints | Enterprise security |
| **CookieMap** | Session management for private registry | Stateful auth without libraries |
| **WASM streaming** | Stream bsdiff WASM module | Faster init, lower memory |

## Features

- **Thread Worker Pool**: Rapid hash deduplication with `NanoPool`
- **Streaming Pipeline**: Back-pressure control with ANSI stripping
- **Socket Health Monitoring**: Auto-reconnect with enhanced diagnostics
- **YAML-Driven Macros**: Compile-time configuration expansion
- **Self-Updating Binary**: Delta patch system with Zstd compression
- **Zero Dependencies**: Single binary deployment
- **Auto-Cleanup**: DisposableStack for resource management
- **Secure Storage**: OS keychain integration for tokens
- **Enterprise Security**: CSRF protection and session management
- **WASM Optimization**: Streaming WASM loading for performance

## Quick Start

```bash
# Install dependencies
bun install

# Build the binary
bun run build

# Run the pipeline
bun run start

# Run health check
bun run doctor

# Bootstrap new project
bun run bootstrap
```

## Architecture

### Core Components

- `src/runtime/worker-pool.ts` - Thread worker management with DisposableStack
- `src/runtime/stream-pipe.ts` - Streaming pipeline with Bun stream helpers
- `src/runtime/socket-health.ts` - Resilient socket connections
- `src/runtime/logger.ts` - ANSI-stripped logging with log levels
- `src/commands/self-update.ts` - Delta update with Zstd + WASM streaming
- `src/runtime/secrets.ts` - OS keychain integration
- `src/runtime/csrf.ts` - CSRF protection for enterprise security
- `src/runtime/session.ts` - CookieMap session management
- `src/runtime/wasm-stream.ts` - WASM streaming loader

### Macro System

- `macros/features.ts` - Feature flag compilation with direct YAML import
- `macros/targets.ts` - Build target configuration with cache invalidation
- `macros/defines.ts` - Compile-time definitions with content hashing

### Configuration

The `bun.yaml` file drives the entire build process:

```yaml
name: "@corp/bpm"
version: "3.0.0"
features:
  newParser: true
  telemetry: false
  selfUpdate: true
```

## Building

```bash
# Build for current platform
bun run build

# Build for all targets (defined in bun.yaml)
bun run build:all
```

## Usage

```typescript
import { NanoPool, createPipeline } from './src/runtime';
import { SecureSecrets } from './src/runtime/secrets';
import { CSRFProtection } from './src/runtime/csrf';

// Secure token storage
await SecureSecrets.setRegistryToken("your-token");
const token = await SecureSecrets.getRegistryToken();

// CSRF-protected requests
const options = await CSRFProtection.addCSRFHeaders();
const response = await fetch("https://registry.corp.com/api", options);

// Auto-cleanup with DisposableStack
using pool = new NanoPool();
const pipeline = createPipeline(tasks);
await pipeline.pipeTo(Bun.stdout);
// Auto-disposed when scope ends
```

## Environment Variables

```bash
BPM_LOG_LEVEL=trace    # Verbose logging
NO_COLOR=1             # Disable colors
BPM_TOKEN=xxx          # Registry token (or use Bun.secrets)
```

## Enterprise Features

- **Private Registry Integration**: Secure package management with CSRF protection
- **Binary Signing**: Cryptographic verification with embedded keys
- **SBOM Generation**: Software Bill of Materials
- **Delta Updates**: Efficient binary patches with Zstd compression
- **Health Monitoring**: Real-time system diagnostics
- **OS Keychain**: Secure token storage via Bun.secrets
- **Session Management**: Stateful authentication with CookieMap
- **WASM Streaming**: Optimized module loading for performance
- **Auto-Cleanup**: Resource management with DisposableStack
# RCM-Compliant-Wager-Factory
