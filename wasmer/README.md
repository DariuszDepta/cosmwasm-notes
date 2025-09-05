# Wasmer in CosmWasm


## Asked ChatGPT 

**Question:**

In what extent is Wasmer Singlepass compiler different from Cranelift and LLVM solutions
used internally in Wasmerio Edge offering?

**Answer:**

Good question — Wasmer uses different backends (compilers) to turn WebAssembly (Wasm)
into native machine code, and each has different trade-offs.
Here’s a breakdown of Singlepass vs. Cranelift vs. LLVM in the context of Wasmer and Wasmer Edge:

### Wasmer Singlepass

- **Design goal**: Predictable, safe execution with extremely fast compilation speed.
- **How it works**: `Singlepass` means it goes through the Wasm bytecode once, generating machine code without deep optimizations.
  Each Wasm instruction maps pretty directly to a sequence of native instructions.
- **Pros**:
  - Very low compilation latency → ideal for short-lived functions (e.g. serverless cold starts).
  - Deterministic stack spilling → strong guarantees around memory safety and security.
  - Lower memory usage compared to Cranelift/LLVM.

Cons:

Slower runtime execution (code runs ~2–3× slower than Cranelift or LLVM).

Very limited optimizations.

Best for:

Fast startup / untrusted sandbox environments (IoT, blockchain smart contracts, edge cases with tight resource constraints).

2. Cranelift

Design goal: Balance between compilation speed and execution performance.

How it works:

JIT compiler designed specifically for WebAssembly (now part of the Bytecode Alliance).

Generates relatively optimized code, but compiles faster than LLVM.

Pros:

Much faster runtime than Singlepass (optimized register allocation, inlining, some vectorization).

Still relatively fast to compile, though slower than Singlepass.

Cons:

Not as fast or optimized as LLVM for long-running workloads.

Best for:

Serverless and edge workloads where you want better performance than Singlepass but still reasonable startup time.

3. LLVM

Design goal: Maximum runtime performance with deep optimizations.

How it works:

Uses LLVM as a backend, applying advanced compiler passes (vectorization, loop unrolling, interprocedural analysis, etc.).

Pros:

Fastest runtime execution (often close to native C performance).

Huge ecosystem of optimizations.

Cons:

Slowest compilation time (can be 10–50× slower than Singlepass).

Higher memory usage.

Best for:

Long-lived services and compute-heavy workloads where compilation overhead is amortized.

In Wasmer Edge (Wasmer’s managed compute platform):

Singlepass: used when you need near-instant cold starts and strict safety.

Cranelift: good middle ground, default in many cases.

LLVM: reserved for workloads where execution speed matters much more than startup latency.

✅ Summary:

Singlepass = fastest to compile, slowest to run.

Cranelift = balanced (good trade-off).

LLVM = slowest to compile, fastest to run.


Assuming I compile WASM using Wasmer LLVM, is it possible to store the compiled code somewhere outside the memory? Can you find any working example if so?
ChatGPT powiedział:

Absolutely—yes, when using Wasmer with the LLVM compiler, it is possible to persist the compiled WebAssembly code outside of in-memory execution. The mechanism varies by language, but the functionality exists across Wasmer’s supported platforms. Below are working examples and details to guide you:

Go Example (wasmer-go)

With wasmer-go, you can compile a module, serialize it to bytes, and save it to disk—even when using LLVM:

config := wasmer.NewConfig().UseLLVMCompiler().UseNativeEngine()
engine := wasmer.NewEngineWithConfig(config)
store := wasmer.NewStore(engine)
module, _ := wasmer.NewModule(store, wasmBytes)

// Serialize the module (includes compiled code)
serialized, _ := module.Serialize()

// Persist to disk
ioutil.WriteFile("module_cache.bin", serialized, 0644)

// Later, reload it
data, _ := ioutil.ReadFile("module_cache.bin")
module2, _ := wasmer.DeserializeModule(store, data)
instance, _ := wasmer.NewInstance(module2, wasmer.NewImportObject())


The important pieces:

.UseLLVMCompiler() ensures the LLVM backend.

.UseNativeEngine() enables the Native engine, which stores compiled code in a shared library format (.so, .dll, .dylib).

Serialize() captures the compiled code, and DeserializeModule() allows reloading it later 
Wasmer
.

Rust Example (wasmer-rust)

In Rust, Wasmer supports AOT and serialized artifacts too:

use wasmer::{Store, Module};

let mut store = Store::default();
let module = Module::new(&store, wasm_bytes)?;
// Optionally, configure the engine to use LLVM if needed.

// Serialize compiled module to bytes
let serialized: Vec<u8> = module.serialize()?;

// Save `serialized` to a file, later load using:
let loaded_module = unsafe { Module::deserialize(&store, &serialized)? };
// Instantiate as usual


Here:

Module::new compiles the module (using whichever backend is configured—LLVM if set).

serialize() captures the compiled output.

deserialize() reloads it, enabling reuse without recompilation 
wasmerio.github.io
.

Additional Context: Caching Strategies

Beyond simple serialization, more advanced caching techniques exist:

In HPC contexts, tools built on Wasmer store LLVM-generated shared objects on disk, keyed by a hash of the original Wasm module. This ensures that recompilation only happens if the module changes 
ar5iv
.

In Wasmer versions 4.2 and above, zero-copy deserialization dramatically speeds up module loading: load times can be cut by nearly half with this method 
Reddit
.

Summary Table
Feature	Supported?	How to Use It
Persist LLVM-compiled code	Yes	Use Native engine + Serialize() / DeserializeModule() in Go; serialize() / deserialize() in Rust
Store compiled code on disk	Yes	Write serialized bytes to file
Zero-copy deserialization	Yes	Automatic in Wasmer 4.2+ for faster load times
Shared-object caching	Yes (advanced)	Use hash-based filesystem storage in specialized setups
Final Thoughts

Yes, you can store compiled Wasm modules (even those compiled via LLVM) outside memory, for reuse across executions.

Working examples:

Go: Use .UseLLVMCompiler() + .UseNativeEngine() and serialize to and from disk 
Wasmer
.

Rust: Call serialize() and deserialize() on Module, with the appropriate compiler configured 
wasmerio.github.io
.

Advanced performance:

Zero-copy deserialization in Wasmer 4.2+ can improve load times significantly 
Reddit
.

Filesystem caching based on hash and shared-object artifacts is used effectively in HPC and edge scenarios 
ar5iv
.

Let me know if you’d like code samples for other languages (like C/C++ or Python), or deeper insights into caching strategies and file formats!

ChatGPT może popełniać błędy. Sprawdź ważne informacje. Zobacz Preferencje dotyczące plików cookie.


Is it the same possible using wasmtime?
ChatGPT powiedział:

Great follow-up 👌 — Wasmtime (like Wasmer) also lets you persist compiled Wasm code outside of memory and reload it later, but the mechanism is a bit different.

🔑 Key difference vs Wasmer

In Wasmer, you call serialize() / deserialize() on a Module and optionally use the Native engine to emit .so/.dll/.dylib.

In Wasmtime, you use the Module::serialize / Module::deserialize API.

Wasmtime serializes the compiled code into a binary blob (engine-independent format).

You can then save this blob to disk and later load it back without recompiling the original Wasm.

🦀 Rust Example (Wasmtime)
use wasmtime::{Engine, Module};

fn main() -> anyhow::Result<()> {
    // Use default engine (Cranelift by default; can configure LLVM)
    let engine = Engine::default();

    // Compile module
    let wasm = std::fs::read("example.wasm")?;
    let module = Module::new(&engine, &wasm)?;

    // Serialize compiled module to bytes
    let serialized = module.serialize()?;
    std::fs::write("cached_module.cwasm", serialized)?;

    // Later: reload without recompilation
    let data = std::fs::read("cached_module.cwasm")?;
    let module2 = unsafe { Module::deserialize(&engine, &data)? };

    println!("Reloaded module successfully");
    Ok(())
}

⚙️ Configuring Compiler Backend

By default, Wasmtime uses Cranelift.
But you can enable LLVM if you compile Wasmtime with the wasmtime-llvm feature enabled. Then the above serialization works the same way — the compiled code (LLVM-generated) is stored in the serialized blob.

✅ Summary

Yes, Wasmtime also supports persisting compiled Wasm outside memory.

API: Module::serialize() → write to disk → Module::deserialize() later.

Works with both Cranelift and LLVM backends (depending on how Wasmtime is built).

The format is portable across runs of the same engine, but not guaranteed to be portable across different Wasmtime versions.

👉 Do you want me to show you a working minimal example where the same .cwasm file is generated once, and then loaded multiple times without recompilation (with timing benchmarks)? That way you can see how much startup time it saves compared to recompiling.

---
There are several WASM engines like wasmer, wasmtime and wazero. Do you know any other doing the same?
ChatGPT powiedział:

You're absolutely right that Wasmer, Wasmtime, and Wazero are among the most prominent WebAssembly (WASM) engines—but there's a vibrant ecosystem beyond those. Here's a curated list of other notable WASM runtimes, interpreters, and execution environments you might find interesting:

Standalone WebAssembly Runtimes & Interpreters

From the GitHub "Awesome WebAssembly runtimes" list:

Wasm3 – a super-lightweight and portable interpreter ideal for embedded and resource-constrained environments 
GitHub
+1
.

WasmEdge (formerly SSVM) – a high-performance, extensible runtime for edge computing, cloud native, smart contracts, IoT, and more 
GitHub
arXiv
.

WAMR (WebAssembly Micro Runtime) – tailored for embedded systems and IoT with a tiny footprint; supports interpreter, JIT, and AOT modes 
GitHub
arXiv
.

Fizzy – a fast, deterministic interpreter written in C++ 
GitHub
gitplanet.com
.

Fuzm (if including experimental runtimes): GraalWasm – embedded in GraalVM; allows interpretation and compilation using Java technologies 
GitHub
+1
.

Other standouts from the same list include:

aWsm – an ahead-of-time (AoT) WASM compiler/runtime, focusing on speed and simplicity 
GitHub
.

Wasmi – a lightweight interpreter designed for embedded use 
GitHub
.

WasmVM – a standalone WASM process virtual machine 
GitHub
.

WAVM – the WebAssembly Virtual Machine, historically one of the fastest, though its maintenance has slowed 
GitHub
Frank DENIS random thoughts.
.

Lunatic – an Erlang-inspired runtime for WebAssembly, using Rust/AssemblyScript 
GitHub
+1
.

Extism – a framework for building apps with WASM across many languages (Rust, Go, .NET, Python, etc.) 
GitHub
+1
.

Other Noteworthy or Specialized Runtimes

Lucet – an edge-focused AOT-only WASM runtime from the Bytecode Alliance, previously used for Fastly's Compute@Edge 
KodeKloud Notes
LogRocket Blog
.

Chicory – a JVM-based (Java) WASM runtime 
GitHub
.

The Wizard Research Engine (Virgil) – experimental research WASM engine 
GitHub
+1
.

Beyond runtime types, there are also frameworks and tooling built around WASM:

WasmCloud – actor-based platform for deploying WASM microservices with secure capabilities 
KodeKloud Notes
.

Spin – application framework (from Fermyon) for building and running WASM microservices using Wasmtime 
CNCF
Reddit
.

WasmEx – fast, secure WASM + WASI runtime for Elixir 
CNCF
.

Timecraft – runtime with sandboxing, task orchestration, and time-travel debugging for WASM 
CNCF
.

Ambient – WASM + Rust + WebGPU runtime for high-performance 3D applications, games 
CNCF
.

Extism (again, here noted for its plugin architecture) 
CNCF
.

WASI X (WASIX) – a superset of the WASI system interface providing extended capabilities 
CNCF
.

SpiderLightning, Homestar – other emerging runtimes and components in the WASM ecosystem 
CNCF
.

At-a-Glance Reference Table
Runtime / Framework	Primary Focus / Strength
Wasm3	Lightweight interpreter for embedded use
WasmEdge (SSVM)	Cloud-native, edge, serverless deployment
WAMR	Micro-runtime for embedded/IoT (JIT/AOT support)
Fizzy	Deterministic C++ interpreter
GraalWasm	Java/GraalVM integration
aWsm	Simplicity and AoT compilation
Wasmi	Minimal interpreter for embedded systems
WasmVM	Standalone WASM process VM
WAVM	Historically high-performance VM (now less active)
Lunatic	Erlang-inspired runtime with actor model
Extism	Plugin framework across many languages
Lucet	Fast AOT for edge computing
WasmCloud	Actor-based microservices platform
Spin	Dev-friendly WASM microservices deployment
WasmEx	Elixir-focused runtime
Timecraft	Time-travel + sandboxed WASM execution
Ambient	High-performance game/3D engine
WASIX / SpiderLightning / Homestar	Emerging experimental systems
Insights from the Community

On Reddit, one user highlights the growing support across ecosystems:

"Without browsers, you need to use a WebAssembly runtime... there are also WebAssembly runtimes that only support WebAssembly, and offer additional features to run: wasmtime, wasmer, wasmedge, wasm3, iwasm, etc." 
Reddit

Another discusses Fermyon’s tooling:

"At Fermyon... we are building Spin... applications that run in Spin and Fermyon Cloud consist of WebAssembly components... executed in a runtime built on top of Wasmtime…" 
Reddit

TL;DR Summary

Beyond Wasmer, Wasmtime, and Wazero, the WASM ecosystem includes:

Lightweight interpreters: Wasm3, Wasmi, Fizzy, aWsm

Embedded/micro runtimes: WAMR, WasmEdge, WAVM

Edge/AoT-specialized: Lucet

Java integration: GraalWasm, Chicory

Actor/microservice platforms: WasmCloud, Spin, Ambient

Language/plugin frameworks: Extism, WASIX, etc.

If you have a particular use case in mind—say, embedded deployment, serverless apps, sandboxed execution, edge computing, or secure enclaves—I’d be happy to help narrow down the best fit or dive deeper into one of these options!

---

Am I right thinking that the WASM binary in all these tools is somehow compiled to the machine code specific for the architecture it is compiled on?
ChatGPT powiedział:

You’re exactly on the right track 👍

But the answer depends a bit on what execution mode the WASM engine supports. Most modern WASM engines don’t just “interpret” the bytecode; they typically compile (at least partially) to native machine code for the host CPU. Let me break it down:

🏗️ Execution Strategies in WASM Engines

Interpreter

The WASM bytecode is read and executed instruction by instruction (like a virtual machine).

Examples: Wasm3, Wasmi, Fizzy.

Super portable, but slower than native execution.

JIT (Just-In-Time compilation)

The WASM module is compiled into native machine code at runtime, optimized for the current CPU architecture.

This is what Wasmtime, Wasmer (JIT mode), and WAMR (JIT) often do.

Gives good performance, but introduces some runtime overhead.

AOT (Ahead-Of-Time compilation)

The WASM binary is compiled into a native binary before execution.

Examples: Lucet, Wasmer (AOT mode), WasmEdge (AOT mode).

You get very close to native speed, and you can ship precompiled binaries for a specific architecture.

🔧 Architecture-Specific Code Generation

Yes — when JIT or AOT is used, the WASM instructions are translated into machine code for the host CPU (x86-64, ARM64, RISC-V, etc.).

The compiled code is therefore architecture-dependent.

The original .wasm file itself is architecture-agnostic: the same .wasm module can run on ARM, x86, or RISC-V — the engine handles translation.

📊 Example

Take Wasmtime:

Input: module.wasm (portable, platform-independent bytecode).

At runtime:

If using JIT, Wasmtime generates x86-64 instructions if running on a PC, or ARM64 instructions if on a Raspberry Pi.

If using AOT, you can precompile with wasmtime compile and ship an ELF or Mach-O binary that runs natively.

✅ So your intuition is correct:

The WASM binary itself is portable.

The engine makes it machine-specific by compiling (JIT/AOT) to the host CPU architecture.

In pure interpreters, the WASM binary stays in bytecode form, but those are slower and mostly used for constrained environments.

---

Nice, so when the WASM portable binary in the AoT mode is precompiled, how these tools resuse the machine code binary and how it is executed?
ChatGPT powiedział:

Great follow-up — you’ve spotted an important detail 👍

When a WASM engine does Ahead-of-Time (AoT) compilation, it has to bridge two worlds:

The WASM module (portable, platform-neutral).

The native machine code (architecture-specific, but still sandboxed and controlled by the engine).

Let’s unpack how the machine code is produced, reused, and executed.

🔄 1. AoT Compilation Step

The engine (e.g. Wasmer AOT, Lucet, WasmEdge AOT, Wasmtime compile) translates the .wasm bytecode into a native object file or shared library (ELF on Linux, Mach-O on macOS, DLL on Windows).

It embeds:

The translated machine code for each WASM function.

Some metadata: imports/exports, memory/table layouts, sandboxing checks, relocation info.

This compiled artifact is sometimes called a “native bundle” or “precompiled cache”.

📦 2. Storage & Reuse

The AoT output can be saved to disk (like .so, .dylib, .dll, or custom cache files).

Later runs of the same module can load this precompiled binary instead of re-compiling the .wasm again.

Example:

wasmtime compile foo.wasm -o foo.cwasm → produces a cached, precompiled module.

At runtime: wasmtime run foo.cwasm → executes directly with no JIT cost.

This is similar to how Java creates .class files and caches them in a JIT/AOT hybrid JVM.

⚡ 3. Execution Phase

When the precompiled artifact is loaded:

The runtime still manages it. It doesn’t just hand execution to the OS like a normal binary, because:

WASM requires sandboxing (no arbitrary syscalls, controlled memory access).

Imports/exports must still be resolved (host functions, WASI syscalls, etc.).

Memory and table objects must be allocated fresh for each instance.

The runtime maps the compiled machine code into memory (like loading a .so file).

Function pointers in the WASM module now point directly to native instructions.

The runtime enforces checks (bounds checking, type safety, sandbox rules) while dispatching calls.

So even though you get near-native speed, execution still happens inside the WASM runtime sandbox.

🎯 Analogy

Think of AoT-compiled WASM like this:

.wasm = portable recipe card.

AoT compiler = translates recipe into a ready-made meal kit for a specific kitchen (CPU).

At runtime, the engine = the chef who unpacks the kit, sets up the safe kitchen space (sandbox), and serves the dish.

🔑 Takeaway

AoT compilation produces a host-architecture native binary, but it’s wrapped with metadata so the WASM runtime can manage and sandbox it.

These precompiled artifacts can be cached and reused across multiple runs, saving startup cost.

They still run under the control of the WASM runtime (not like a fully independent .exe).
