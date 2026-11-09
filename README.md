<div align="center">

<img src="assets/rayquaza.png" alt="Rayquaza 8-bit" width="128" style="image-rendering: pixelated;" />

# Rayqo OS

**A minimalist bare-metal operating system kernel targeting `x86_64`, written in Go and Assembly.**

[![Go](https://img.shields.io/badge/Language-Go%20%2B%20Assembly-00ADD8?style=for-the-badge&logo=go&logoColor=white)](https://golang.org)
[![Architecture](https://img.shields.io/badge/Arch-x86__64-222222?style=for-the-badge&logo=intel&logoColor=white)](#)
[![Emulator](https://img.shields.io/badge/Emulator-QEMU-FF9900?style=for-the-badge&logo=qemu&logoColor=white)](#)
[![Conventional Commits](https://img.shields.io/badge/Conventional%20Commits-1.0.0-yellow.svg?style=for-the-badge&logo=git&logoColor=white)](https://conventionalcommits.org)
[![License: MIT](https://img.shields.io/badge/License-MIT-success.svg?style=for-the-badge)](LICENSE)

<br/>

[GopherOS](https://github.com/achilleasa/gopher-os) · [Writing an OS in Rust](https://os.phil-opp.com/) · [Operating Systems: Three Easy Pieces (OSTEP)](https://pages.cs.wisc.edu/~remzi/OSTEP/)

</div>

---

## 🗺️ Roadmap

### Phase 0: Bootstrap & Early Initialization
- [ ] Multiboot2 compliant header & boot stub (`nasm` / `gas`)
- [ ] 32-bit Protected Mode to 64-bit Long Mode transition
- [ ] Early stack initialization and jump into Go entry point (`_start` / `kernel_main`)
- [ ] Stripped-down Go runtime harness (`//go:nosplit`, zero stdlib dependencies)

### Phase 1: Hardware I/O & Memory Subsystem
- [ ] Serial console driver (UART 16550) for QEMU debugging
- [ ] VGA Text Mode / Linear Framebuffer buffer management
- [ ] Physical Memory Manager (PMM): bitmap / buddy frame allocator
- [ ] Virtual Memory: 4-level paging setup (PML4, PDPT, PD, PT) with identity mapping & kernel heap

### Phase 2: Interrupts, CPU Tables & Basic Drivers
- [ ] Global Descriptor Table (GDT) & Task State Segment (TSS)
- [ ] Interrupt Descriptor Table (IDT) & CPU exception handlers
- [ ] PIC remapping & APIC initialization
- [ ] Programmable Interval Timer (PIT) & PS/2 keyboard driver

### Phase 3: Multitasking & System Calls
- [ ] Kernel thread abstraction & context switching assembly stub
- [ ] Cooperative -> Preemptive scheduler
- [ ] Ring 3 (User Mode) switch & basic syscall dispatcher

---

### 1. Git Discipline & History
- **Linear History**: Keep branches rebased; do not create merge bubbles on `main`.
- **Atomic Commits**: Each commit represents a single logical unit of work that compiles and passes checks.
- **WIP & Fixup Workflow**: Never push uncurated WIP commits. Use fixup commits:
- **Commit Format**: Follow [Conventional Commits](https://www.conventionalcommits.org/):
  ```text
  <type>(<scope>): <short imperative summary>

  [optional body: explain why, context, and hardware constraints]
  ```
  *Types*: `feat`, `fix`, `refactor`, `docs`, `chore`, `perf`, `build`.

### Go Constraints
- **Zero Allocations in Early Boot**: The Go GC and standard heap do not exist until the PMM and Paging are active.
- **No Stdlib**: Rely solely on custom memory-mapped I/O and low-level primitives.
- **Explicit Inlining & Stack Bounds**: Annotate critical boot paths with `//go:nosplit` to prevent stack check traps.

---

## Environment & Toolchain

| Component | Tool / Target |
| :--- | :--- |
| **Architecture** | `x86_64` |
| **Emulator** | `qemu-system-x86_64` |
| **Assembler** | `nasm` |
| **Compiler** | `go` (bare-metal configuration) |
| **Debugging** | `gdb` via QEMU stub (`-s -S`) |

---

## License

This project is licensed under the [MIT License](LICENSE).
