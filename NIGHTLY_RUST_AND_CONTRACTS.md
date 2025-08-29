# Using nightly Rust to compile WebAssembly smart contracts

## Problem

There is a smart contract that have to be compiled to WebAssembly
but using a specific Rust version, let's say `1.81.0`.

## Solution

Find the release date of requested Rust version, i.e.: `1.81.0`

```shell
$ rustc +1.81.0 --version
```

Output:

```text
rustc 1.81.0 (eeb90cda1 2024-09-04)
```

Install a nightly Rust version from this date or earlier if not exists:

```shell
$ rustup install nightly-2024-09-04
```

Install `wasm32-unknown-unknown` target for specific nightly version:

```shell
$ rustup target add wasm32-unknown-unknown --toolchain nightly-2024-09-04-x86_64-unknown-linux-gnu
```
