# clone-lin-rust-bitcoin-feerate

**Class: EXPERIMENTAL.** This is not a wallet, not rust-bitcoin, and not Bitcoin Core.

LIN scalar kernel of [`FeeRate::mul_by_weight`](https://github.com/rust-bitcoin/rust-bitcoin/blob/1cbf4bd62ea99c558c6d8ef794edbd984a2a4684/units/src/fee_rate/mod.rs) plus `Amount::MAX` (`21_000_000 * 100_000_000`).

- Upstream: [rust-bitcoin/rust-bitcoin](https://github.com/rust-bitcoin/rust-bitcoin) commit `1cbf4bd62ea99c558c6d8ef794edbd984a2a4684` (CC0-1.0)
- Cross-oracle: Bitcoin Core v27.1 `CFeeRate::GetFee` (integer ceil stand-in; Core itself uses `std::ceil(double)`)
- Results and proofs live in [kbelludoo/lin-open](https://github.com/kbelludoo/lin-open): `src/lin_rust_bitcoin_feerate.lin`, `test/prove_rust_bitcoin_feerate_external.py`, `docs/RUST_BITCOIN_FEERATE_CLONE.rulel`

## What this clone contains

| File | Role |
|---|---|
| `lin_rust_bitcoin_feerate.lin` | LIN kernel (Compiler 0 / C11 host) |
| `rust_bitcoin_feerate_scalar.c` | gcc oracle (`unsigned __int128`) |
| `rust_bitcoin_feerate_scalar.rs` | no_std scalar fixture for `lin_from_rust` v2 |

## Honest limits

- i64 domain: products that wrap fail-closed. rust-bitcoin uses `u128`.
- rust-bitcoin and Core **diverge** when `wu % 4 != 0` (weight units vs integer vbytes). Example: `381 wu * 864 sat/kwu` = **330** sat in rust-bitcoin; Core 95 vB = **329**, 96 vB = **332**.
- No PSBT, no script, no network.

## Reproduce (from lin-open)

```bash
make -C transpile/c c0
python3 test/prove_rust_bitcoin_feerate_external.py
```
