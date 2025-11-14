# kvstore-rs

`kvstore-rs` is a minimal key--value storage engine implemented in Rust.
It's not intended to be a production datastore; it exists as a compact,
readable exploration of storage-engine internals --- log-structured
writes, on-disk persistence, indexing, and compaction --- written with
an emphasis on clarity and correctness.

The project reflects how a simple KV engine works under the hood without
pulling in external frameworks or abstractions.

------------------------------------------------------------------------

## Overview

`kvstore-rs` stores entries in an append-only log and maintains an
in-memory index mapping keys to their latest log offsets. This design
provides fast sequential writes and straightforward crash recovery. A
lightweight compaction step reclaims space by rewriting only active
keys.

The codebase is intentionally small so the core ideas are easy to
follow.

------------------------------------------------------------------------

## Features

-   Persistent, file-backed storage\
-   Append-only write log\
-   In-memory index for O(1) lookups\
-   Basic log compaction to reclaim stale segments\
-   Straightforward, dependency-light implementation\
-   Small CLI wrapper for manual inspection and testing

------------------------------------------------------------------------

## Getting Started

### Clone the repository

``` bash
git clone https://github.com/zachdrone/kvstore.git
cd kvstore
```

### Build

``` bash
cargo build
```

### Run Tests

``` bash
cargo test
```

------------------------------------------------------------------------

## Usage

Example using the CLI tool included in the project:

Set a key:

``` bash
kvstore set mykey "hello world"
```

Get a key:

``` bash
kvstore get mykey
```

Delete a key:

``` bash
kvstore delete mykey
```

(If your build outputs the binary elsewhere, adjust the path
accordingly.)


------------------------------------------------------------------------

## Concepts Demonstrated

### Log-Structured Storage

Writes append to a log, keeping operations simple and making writes very
fast. The system reconstructs state by replaying recent operations.

### In-Memory Index

A hash map tracks the current file offset of each key, providing
constant-time lookups without scanning the log.

### Compaction

Old values accumulate in the log when keys are overwritten. The store
includes a basic compaction pass that rewrites only the active keys to a
fresh log.
