# High-Quality CosmWasm

> Rules for high-quality CosmWasm development.

## Maintenence scope

| Repository      | Component |
|-----------------|-----------|
| [cosmwasm]      |           |
| [cw-plus]       |           |
| [cw-multi-test] |           |

[cosmwasm]: https://github.com/CosmWasm/cosmwasm
[cw-plus]: https://github.com/CosmWasm/cw-plus
[cw-multi-test]: https://github.com/CosmWasm/cw-multi-test

## Security policy

Each repository should include a file named **SECURITY.md** placed in the root directory.

The contents of this file should be as follows:

```text
# Security Policy

This repository is part of the **CosmWasm** stack.
Please see the [Advisories] for its security policy.

[Advisories]: https://github.com/CosmWasm/advisories/blob/main/SECURITY.md
```

Related issues: [\#1](https://github.com/DariuszDepta/hqcw/issues/1)

## Unified CHANGELOG

Changelog SHOULD NOT contain `Unreleased` section.
Only released functionalities should be mentioned in CHANGELOG.md.

## License

Each repository should have a **LICENSE** and **NOTICE** files.
At the end of the README file should be the following text: 

```text
## License

Licensed under [Apache License, Version 2.0](https://www.apache.org/licenses/LICENSE-2.0)
(see [LICENSE][apache-url] and [NOTICE][notice-url]).

Any contribution intentionally submitted for inclusion in this crate by you,
shall be licensed as above, without any additional terms or conditions.
```

where `[apache-url]` and `[notice-url]` should be defined as following:

```text
[apache-url]: LICENSE
[notice-url]: NOTICE
```
