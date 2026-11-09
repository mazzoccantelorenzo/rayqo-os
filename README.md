# Rayqo OS

Rayqo OS is a operating system kernel targeting written in Go and Assembly.

Inspired by [GopherOS](https://github.com/achilleasa/gopher-os), [Writing an OS in Rust](https://os.phil-opp.com/), and *Operating Systems: Three Easy Pieces* (OSTEP).

---

## Roadmap

### Phase 0: Bootstrap & Early Initialization
- [ ] Multiboot2 compliant header & boot stub (NASM / GAS)
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

## Engineering Standards

### 1. Git Discipline & History
- **Linear History**: Keep branches rebased; do not create merge bubbles on `main`.
- **Atomic Commits**: Each commit represents a single logical unit of work that compiles and passes checks.
- **WIP & Fixup Workflow**: Never push uncurated WIP commits. Use fixup commits
- **Commit Format**: Follow [Conventional Commits](https://www.conventionalcommits.org/):
  ```text
  <type>(<scope>): <short imperative summary>

  [optional body: explain why, context, and hardware constraints]
  ```
  Types: `feat`, `fix`, `refactor`, `docs`, `chore`, `perf`, `build`.

### 2. Bare-Metal Go Constraints
- **Zero Allocations in Early Boot**: The Go GC and standard heap do not exist until the PMM and Paging are active.
- **No Stdlib**: Rely solely on custom memory-mapped I/O and low-level primitives.
- **Explicit Inlining & Stack Bounds**: Annotate critical boot paths with `//go:nosplit` to prevent stack check traps.

---

## Environment & Toolchain

- **Target Architecture**: `x86_64`
- **Emulator**: `qemu-system-x86_64`
- **Assembler**: `nasm`
- **Compiler**: `go` (configured for bare-metal target)
- **Debugging**: `gdb` via QEMU stub (`-s -S`)
