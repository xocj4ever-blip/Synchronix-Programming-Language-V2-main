# SYNCHRONIX v2.0.0 — Self-Hosted Native Programming Language

**Synchronix v2** is a standalone, self-hosted, native programming language implemented in **100% pure `.synchronix` source files** (zero C, C++, Rust, Python, Go, PowerShell, or Makefile files), achieving full 1-to-1 architectural and feature parity with Synchronix v1 across all 5 architectural domains.

Influenced by **Swift** (ARC, safety, modern design) and **Kotlin** (tracing GC, M:N concurrency, functional ergonomics), with integrated **$\Phi$ (Phi) Formal Logic** verification and **$\Lambda$ (Lambda) Functional Logic** closures.

## Strict 1-to-1 Architecture Mapping (V1 $\rightarrow$ V2 Pure `.synchronix`)

| Diagram Phase | Subsystem | Pure Synchronix Source |
|---|---|---|
| **Portable Layer** | Cross-Platform ABI, Memory/Pointer, C-String & OS Helpers + Definitions | `include/portable_asm.synchronix`, `include/sx_defs.synchronix` |
| **Build System** | Native Build, Clean & Cross-Target Verification (`all`/`runtime`/`compiler`/`verify`/`clean`/`help`) | `build.synchronix` |
| **Phase 1: Frontend** | Lexer / Tokenizer (`.synchronix`) & Re-entrant Context/Utilities | `src/frontend/lexer.synchronix`, `src/frontend/utils.synchronix` |
| **Phase 1: Frontend** | Parser (AST) | `src/frontend/parser.synchronix` |
| **Phase 1: Frontend** | Semantic Analyzer, Type Inference, Generics, Scope, $\Phi$ Formal Verification, $\Lambda$ Capture Analysis | `src/frontend/sema.synchronix` |
| **Phase 2: Middle-End** | IR Generator $\rightarrow$ High-Level IR $\rightarrow$ SUMIR | `src/middle/ir_gen.synchronix` |
| **Phase 2: Middle-End** | Static Analysis & Memory Safety (Ownership/Borrow Checker, ARC Insertion, GC Root Tracking, Lifetime Analyzer) | `src/middle/mem_safety.synchronix` |
| **Phase 2: Middle-End** | $\Phi$ & $\Lambda$ Optimization Passes (Proof Elimination, ARC Peephole) | `src/middle/phi_lambda_opt.synchronix` |
| **Phase 2: Middle-End** | Lowering (Constant Folding, Inlining, Fixed-Point DCE) | `src/middle/lowering.synchronix` |
| **Phase 3: Backend** | Target-Specific Optimization (Linear-Scan Register Allocation & Instruction Scheduling) | `src/backend/target_opt.synchronix` |
| **Phase 3: Backend** | LLVM IR / Custom `x86_64` & `ARM64` Code Generator | `src/backend/codegen.synchronix` |
| **Phase 3: Backend** | Assembler & Linker (`ld`/`lld`) + External Libs (`.a`/`.so`/`.lib`) | `src/backend/emitter.synchronix` |
| **Foundation: Runtime** | Unified Memory (ARC Engine, Tracing Mark-and-Sweep GC, Arena Manager) | `src/runtime/memory_rt.synchronix` |
| **Foundation: Runtime** | M:N Async Scheduler & Fibers/Tasks | `src/runtime/async_rt.synchronix` |
| **Foundation: Runtime** | $\Phi$ Formal Logic & $\Lambda$ Closure Runtime | `src/runtime/phi_lambda_rt.synchronix` |
| **Foundation: Stdlib** | Synchronix Standard Library (`std`) | `src/runtime/stdlib.synchronix` |
| **Ecosystem Tooling** | Cargo Package Manager & Build System (`new`/`build`/`run`/`test`) | `src/tooling/cargo_pkg.synchronix` |
| **Ecosystem Tooling** | Formatter (`synchronix fmt`) & Linter (`synchronix lint`) | `src/tooling/fmt_lint.synchronix` |
| **Ecosystem Tooling** | Synchronix LSP (JSON-RPC 2.0 Language Server) | `src/tooling/lsp.synchronix` |
| **Ecosystem Tooling** | Debugger (LLDB/GDB `-g`) & Profiler (`synchronix profile`) | `src/tooling/debug_prof.synchronix` |
| **CLI Entry** | Unified `synchronix` CLI Driver (full `argc`/`argv` support + `--self-test`) | `src/main.synchronix` |

## Build & Quick Start

```sh
# Rebuild the self-hosted V2 compiler & cross-verify all units (using sx_build.exe)
./build/sx_build.exe              # Build compiler + cross-verify all .synchronix units
./build/sx_build.exe all          # Build compiler only
./build/sx_build.exe clean        # Clean generated build artifacts
./build/sx_build.exe help         # Show all targets & CC/AR/SX/CFLAGS/LDFLAGS overrides

# Compile & run the full architecture showcase
./build/synchronix.exe profile examples/showcase.synchronix
./build/synchronix.exe run examples/showcase.synchronix

# Verify negative static safety checks (Ownership & Phi Formal Logic)
./build/synchronix.exe compile examples/move_fail.synchronix
./build/synchronix.exe compile examples/phi_fail.synchronix

# Cross-compile to ARM64 or emit LLVM IR
./build/synchronix.exe compile examples/showcase.synchronix --target arm64 -o build/showcase_arm64
./build/synchronix.exe compile examples/showcase.synchronix --emit-llvm -o build/showcase_llvm

# Run built-in V2 self-hosted runtime & pipeline self-test
./build/synchronix.exe --self-test

# Ecosystem tooling
./build/synchronix.exe new my_app
./build/synchronix.exe fmt examples/showcase.synchronix
./build/synchronix.exe lint examples/showcase.synchronix
./build/synchronix.exe lsp
```

## License

Synchronix is licensed under the [Apache License, Version 2.0](./LICENSE). End-user terms are provided in [EULA.md](./EULA.md).
