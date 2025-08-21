# Development process

## Currently developed sources

The current development sources of the Rust crates are kept on the main branch.
This applies to both single-crate and multi-crate repositories like `cosmwasm` repository.

On the main branch (which serves as the development branch),
all dependencies between the developed crates should be defined using relative paths,
as shown below (example taken from `cosmwasm-std` crate):

```toml
[dependencies]
cosmwasm-core = { path = "../core" }
cosmwasm-derive = { path = "../derive" }
cw-schema = { path = "../cw-schema" }
```

This convention is practical because:
- all dependencies remain up to date, regardless of which crate is changed during development,
- `cargo` prevents publishing a crate that has a path dependency,
- code coverage can be measured across all crates, regardless of their position in the dependency tree.

The versions of crates under development on the `main` branch should use the planned release version number
with a **`-dev`** suffix. If such a crate is accidentally published, the suffix makes it easy to identify
and immediately **yank** on [crates.io](https://crates.io).

Changes to the source code on the `main` branch must be made through **pull requests** based on separate branches.

When the code on the `main` branch is ready for release, create a separate branch named `release/`
followed by the major and minor version numbers from the version triple (e.g., `release/3.0`).
If such a branch already exists and the current change is only a bug fix, use the existing branch instead.

The released version of creates must not contain `-dev` suffix.

On the release branch, all dependencies between crates should be changed to version numbers, like this:

```toml
[dependencies]
cosmwasm-core = { version = "3.0.2" }
cosmwasm-derive = { version = "3.0.2" }
cw-schema = { version = "3.0.2" }
```

Then all crates can be published to [crates.io](https://crates.io) in the correct order,
as determined by the dependency tree (from leaves to root).

This convention is convenient, because now:
- all dependencies are fixed to released version,
- `cargo` allows publishing all crates,
- code coverage can be measured independently for each crate.
