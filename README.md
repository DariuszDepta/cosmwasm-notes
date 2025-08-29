# Notes on [CosmWasm](https://github.com/CosmWasm)

> [!IMPORTANT]  
> The content in this repository represents my own opinions and understanding.
> It does not represent the views of any employer, company, or organization I am associated with.
> 
> **All information is provided “as is” without any warranty or responsibility**. 

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

- [Risk register](./RISK_REGISTER.md)
- [Security policy](./SECURITY_POLICY.md)
- [Using nightly Rust to compile contracts](./NIGHTLY_RUST_AND_CONTRACTS.md)
