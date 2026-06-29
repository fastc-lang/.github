# fastc-lang

We build [fastC](https://github.com/fastc-lang/fastc): a small systems language for a world where most code is written by an agent and reviewed by a human.

fastC compiles to readable C11. It refuses to run anything at build time. Capabilities (`fs.read`, `net.connect`) are typed function arguments, so a function with no capability argument cannot do I/O. Public APIs carry `@requires` and `@ensures` contracts that the compiler discharges through a three tier pipeline: syntactic, Z3, and runtime.

## What is here

| Repo | What it is |
| --- | --- |
| [fastc](https://github.com/fastc-lang/fastc) | The compiler. Rust workspace, emits C11, 337+ tests. |
| [fastc-core-cli](https://github.com/fastc-lang/fastc-core-cli) | Argv access and flag parsing. |
| [fastc-core-log](https://github.com/fastc-lang/fastc-core-log) | Structured leveled logging. |
| [fastc-core-json](https://github.com/fastc-lang/fastc-core-json) | JSON encoder and integer field decoder. |
| [fastc-core-toml](https://github.com/fastc-lang/fastc-core-toml) | Read only flat table TOML parser. |
| [fastc-core-http](https://github.com/fastc-lang/fastc-core-http) | HTTP/1.1 client, gated on `CapNetConnect`. |
| [fastc-core-uuid](https://github.com/fastc-lang/fastc-core-uuid) | UUID v4 generation and parsing (RFC 4122). |
| [fastc-core-base64](https://github.com/fastc-lang/fastc-core-base64) | Base64 encoding and decoding (RFC 4648). |
| [fastc-core-time](https://github.com/fastc-lang/fastc-core-time) | Wall clock time helpers. |
| [fastc-core-crypto-primitives](https://github.com/fastc-lang/fastc-core-crypto-primitives) | SHA-256, HMAC-SHA-256, constant time compare, secure random. |
| [fastc-core-regex](https://github.com/fastc-lang/fastc-core-regex) | Thompson NFA regex engine, no backreferences, linear time. |
| [fastc-core-sqlite](https://github.com/fastc-lang/fastc-core-sqlite) | SQLite bindings via FFI to system libsqlite3. |

The `fastc-core` set is the standard library preview. Each module ships a `v0.1.0` release. The code also lives inside the compiler prelude today; the standalone repos become installable via `fastc add` when the v1.1 vendor flow lands.

## Why a new language

C is everywhere and unsafe. Rust is safe and slow to compile. Zig is close but runs build scripts. Go ships a 2 MB hello world. None of them make capability based I/O a first class type, and none forbid executable build scripts at the language level.

fastC picks four properties and holds them:

1. Capabilities are typed function arguments, not ambient authority.
2. Contracts on public APIs are mandatory, lowered to runtime asserts and SMT proven where possible.
3. The package system has no executable build steps and no central registry. Dependencies are vendored and content hashed.
4. Compile time budget is enforced in CI, not aspirational.

A hello world binary is 53 KB stripped. Cross compile to eight targets with one flag via `zig cc`. First compile success against four open weight LLMs is 12 out of 12 on T1 once the shipped cheatsheet is faithful.

## Try it

```bash
git clone https://github.com/fastc-lang/fastc.git
cd fastc
cargo install --path crates/fastc

fastc new my_project
cd my_project
fastc run
```

## Docs

- [Why fastC](https://docs.fastc-lang.com/why/) with the rubric, benchmarks, and FAQs.
- [Getting started](https://docs.fastc-lang.com/getting-started/).
- [Language guide](https://docs.fastc-lang.com/language/).
- [CLI reference](https://docs.fastc-lang.com/cli/).

## Status

v1.0 feature complete. Stages 0.1 through 2.1 shipped. The fastc-core launch set is in preview. Work continues on the vendor consumption flow and the standalone library split.

## License

MIT.