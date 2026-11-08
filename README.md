# VaultMark Public Ledger

Repository: `kinglabs-ai/vaultmark-ledger`

Property of Kingship Holdings Inc. | KingLabs AI

The VaultMark Public Ledger is the public, append-only record of VaultMark certificates. Once a day, VaultMark publishes the Merkle root of every certificate issued that UTC day. A Merkle root is one hash built from all of the day's certificate hashes, so changing, adding, removing or reordering any one certificate changes the root. Because these commits are public and timestamped by GitHub, nobody, including Kingship, can quietly rewrite a past day.

## Where the roots are

Branch **`ledger`**, one file per day: `roots/YYYY/YYYY-MM-DD.json`

```json
{"certCount":42,"date":"2026-11-07","merkleRoot":"<64 hex>","v":1}
```

- The `ledger` branch is protected: no force pushes, no deletion. Its history cannot be rewritten.
- `main` holds this documentation and takes changes only through pull requests.
- A day with no certificates is still published, with `certCount` 0 and the root of the empty list.

## How to check a certificate

1. Ask VaultMark's public endpoint `GET /verify/<sha256>` for the certificate's hash, timestamp, signature and chain position.
2. Get the day's certificate hashes from the certificate owner, in chain order.
3. Recompute the root with `dailyRoot()` from `@kinglabs-ai/stamp-core` (RFC 6962 Merkle tree: leaf = SHA-256(0x00 || hash), node = SHA-256(0x01 || left || right)).
4. Compare it with the file for that day on the `ledger` branch of the VaultMark Public Ledger.

## Signing keys

Certificates are signed with Ed25519. Public keys are published under `keys/<public_key_id>.pem`, where `public_key_id` is the SHA-256 of the key's SPKI DER encoding.
