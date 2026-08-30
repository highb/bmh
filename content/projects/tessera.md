+++
title = "Tessera"
date = "2026-07-04"
description = "Reciprocal museum coverage optimizer — pure Rust, client-side, weighted set-cover"
+++

[Tessera](https://github.com/highb/museum-membership-finder) answers: **given
where I live and the institutions I want access to, what is the cheapest legal
combination of museum memberships that covers them?**

Models the U.S. reciprocal-network landscape (NARM, ASTC, AHS, ROAM, MARP, ACM,
AZA, Time Travelers), encodes per-network/per-institution exclusion rules as
Datalog ([Ascent](https://github.com/s-arash/ascent)), and solves
membership-selection as weighted set-cover.

All-[Rust](https://rust-lang.org) stack, only generated JavaScript:

- [Leptos](https://leptos.dev) [WASM](https://webassembly.org) SPA for the web UI
- [Ascent](https://github.com/s-arash/ascent) [Datalog](https://en.wikipedia.org/wiki/Datalog) for rule encoding
- Pure-Rust optimizer, zero C/C++ FFI
- Local-first: user's address never leaves the browser

Data is curated JSON with per-row provenance, shipped as [rkyv](https://rkyv.org) zero-copy
archives. Dual-licensed [MIT](https://opensource.org/licenses/MIT) /
[Apache 2.0](https://opensource.org/licenses/Apache-2.0).

Written with Claude / oh-my-pi running open models.