# merkle-lite

## Credits

This project is a fork from the [merkle-lite](https://github.com/keithnoguchi/merkle-lite) crate by [@keithnoguchi](https://github.com/keithnoguchi), with modifications to align with behavior in Solidity-based Merkle tree libraries. Modifications were made by [@dralves](https://github.com/dralves).

[![CI](https://github.com/keithnoguchi/merkle-lite/actions/workflows/ci.yml/badge.svg)](https://github.com/keithnoguchi/merkle-lite/actions)
[![License](https://img.shields.io/badge/license-Apache--2.0_OR_MIT-blue.svg)](https://github.com/keithnoguchi/merkle-lite)
[![Cargo](https://img.shields.io/crates/v/merkle-lite.svg)](https://crates.io/crates/merkle-lite)
[![Documentation](https://docs.rs/merkle-lite/badge.svg)](https://docs.rs/merkle-lite)

A simple, fast, and composable binary [Merkle tree and proof]
for [Rust Crypto] hash functions.

## Examples

It's super simple to compose `MerkleTree` from the ordered array
of hashes and verify the proof of inclusion with `MerkleProof`:

```
use merkle_lite::MerkleTree;
use rand_core::RngCore;

// Composes MerkleTree from the 50,000 random hashes.
let tree: MerkleTree<sha3::Sha3_256> = std::iter::repeat([0u8; 32])
    .map(|mut leaf| {
        rand_core::OsRng.fill_bytes(&mut leaf);
        leaf
    })
    .take(50_000)
    .collect();

// Verifies the proof of inclusion for the arbitrary leaves.
let tree_leaves = tree.get_leaves();
let leaf_indices = [12, 0, 1, 1201, 13_903, 980];
let leaf_hashes: Vec<_> = leaf_indices
    .iter()
    .map(|index| (*index, tree_leaves[*index]))
    .collect();
assert_eq!(
    tree.proof(&leaf_indices)
        .expect("proof")
        .verify(&leaf_hashes)
        .expect("verify")
        .as_ref(),
    tree.root().expect("root"),
);
```

## License

Licensed under either of

- Apache License, Version 2.0 ([LICENSE-APACHE](LICENSE-APACHE) or http://www.apache.org/licenses/LICENSE-2.0)
- MIT license ([LICENSE-MIT](LICENSE-MIT) or http://opensource.org/licenses/MIT)

at your option.

#### Contribution

Unless you explicitly state otherwise, any contribution intentionally submitted
for inclusion in the work by you, as defined in the Apache-2.0 license, shall be
dual licensed as above, without any additional terms or conditions.

[merkle tree and proof]: https://en.wikipedia.org/wiki/Merkle_tree
[rust crypto]: https://github.com/RustCrypto
