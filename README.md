# lmdb-rs

Idiomatic and safe APIs for interacting with the
[Symas Lightning Memory-Mapped Database (LMDB)](http://symas.com/mdb/).

This repo is a fork of [mozilla/lmdb-rs](https://github.com/mozilla/lmdb-rs)
which is a fork of [danburkert/lmdb-rs](https://github.com/danburkert/lmdb-rs).

## Why does it exist?

The sole reason why this crate exists is because it fixes a memory issue in LMDB
where LMDB was keeping a lot of pages and not freeing them. So we decided to fork it
and add a new `MDB_ALWAYSFREEPAGES` flag to the `MDB_Env` struct. This flag now forces
LMDB to always free single pages instead of keeping them in a list for future reuse.

## Vendoring

By default, if LMDB is installed on the system, this crate will attempt to make use of the system-available LMDB.

To force installation from source, build this crate with the `vendored` feature.

## Building from Source

### Using the system LMDB if available

```bash
git clone --recursive git@github.com:meilisearch/lmdb-rs.git
cd lmdb-rs
cargo build
```

### Always vendoring

```bash
git clone --recursive git@github.com:meilisearch/lmdb-rs.git
cd lmdb-rs
cargo build --features vendored
```

## Publishing to crates.io

To publish the lmdb-rkv-sys crate to crates.io:

```bash
git clone --recursive git@github.com:meilisearch/lmdb-rs.git
cd lmdb-rs/lmdb-sys
# Update the version string in lmdb-sys/Cargo.toml and lmdb-sys/src/lib.rs.
cargo publish
git tag lmdb-rkv-sys-$VERSION # where $VERSION is the updated version string
git push git@github.com:meilisearch/lmdb-rs.git --tags
```

To publish the lmdb-rkv crate to crates.io:

```bash
git clone --recursive git@github.com:meilisearch/lmdb-rs.git
cd lmdb-rs
# Update the version string in Cargo.toml and src/lib.rs and temporarily change
# the lmdb-rkv-sys dependency in Cargo.toml to the latest version on crates.io.
cargo publish
git tag $VERSION # where $VERSION is the updated version string
git push git@github.com:meilisearch/lmdb-rs.git --tags
# Change the lmdb-rkv-sys dependency in Cargo.toml back to a path dependency
# on the ./lmdb-sys directory.
```

## Features

* [x] lmdb-sys.
* [x] Cursors.
* [x] Zero-copy put API.
* [x] Nested transactions.
* [x] Database statistics.
