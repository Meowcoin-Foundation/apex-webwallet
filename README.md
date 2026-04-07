# Apex Wallet — Meowcoin

A lightweight, self-contained web wallet for Meowcoin (MEWC). Single static file — open `index.html` in any browser and go.

---

## What is Apex Wallet?

Apex Wallet is the recommended wallet for **Meowcoin Core Apex (v30.2.0+)**. It derives addresses using the BIP44 path `m/44'/175'/0'/0` (coin type 175), which is the standard path for Meowcoin post-Apex.

If you were using the legacy Meowcoin Core wallet or Electrum, your funds are at a different derivation path or addressed differently. See the migration notes below.

---

## Difference from the legacy wallet

| | Apex Wallet | Legacy Wallet |
|---|---|---|
| Address format | P2PKH, version byte `0x32` (starts with **M**) | Same — addresses look identical |
| Derivation path | `m/44'/175'/0'/0` | Varies — often `m/44'/175'/0'/0` but Electrum uses its own scheme |
| Seed phrase type | BIP39 (standard 12 or 24 words) | Electrum uses its own non-BIP39 format |
| Offline capable | Yes — open the file locally | No — requires a running node |

---

## Security model

- **Keys never leave your browser.** All cryptographic operations happen locally in JavaScript. No seed phrases, private keys, or derived keys are ever transmitted over the network.
- **Nothing is stored.** No `localStorage`, `sessionStorage`, `IndexedDB`, or cookies are used. Your session ends when you close the tab or click Lock.
- **Direct server connection.** Balance and transaction data is fetched directly from your configured electrs server. No proxy, no tracking.
- **Open source.** Every line of logic is in `index.html`. Read it.
- **Signed releases.** Every release is GPG-signed. Verify before use (see below).

---

## How to use

1. Download `index.html` from the [Releases](../../releases) page.
2. Verify the file (see below).
3. Open `index.html` in any modern browser (Chrome, Firefox, Safari, Edge).
4. Enter your seed phrase or private key and open your wallet.

No installation, no npm, no server required.

---

## Electrum seed phrases are not compatible

Electrum uses its own non-BIP39 seed phrase format. **You cannot use your Electrum seed words in Apex Wallet.**

To access your funds from Electrum in Apex Wallet:

1. In Electrum: go to **Wallet → Private Keys → Export**
2. Export the private key for your address
3. In Apex Wallet: use the **Private Key** tab and paste the WIF key

---

## Verifying a release

Every release includes `SHA256SUMS` and `SHA256SUMS.asc`. Verify them before using:

```sh
# Download the release files
curl -LO https://github.com/meowcoin-foundation/apex-webwallet/releases/latest/download/index.html
curl -LO https://github.com/meowcoin-foundation/apex-webwallet/releases/latest/download/SHA256SUMS

# Verify the checksum matches the file
sha256sum -c SHA256SUMS
```

---

## Changing the electrs server

The default server is `https://electrs.mewccrypto.com`. To use a different server:

1. On the login screen, find the **electrs server** field.
2. Enter the URL of your server (e.g., `https://your-node.example.com`).
3. The wallet connects to that server for all balance, UTXO, and broadcast operations.

To run your own electrs server, see the [electrs documentation](https://github.com/romanz/electrs).

---

## Accessing different address indices

The default derivation path is `m/44'/175'/0'/0`. The last number is the address index.

To access a different address at the same account:

- Change `m/44'/175'/0'/0` → `m/44'/175'/0'/1` for address index 1
- Change `m/44'/175'/0'/0` → `m/44'/175'/0'/2` for address index 2
- etc.

Each index produces a different address, all derived from the same seed phrase.

---

## What this wallet does NOT support

- Electrum seed phrases (use private key import instead)
- Asset transactions (MEWC token layer)
- SegWit / Bech32 addresses
- Multi-address gap scanning
- Hardware wallets
- Testnet
- PWA / offline mode after first load

---

## Dependencies (all via CDN)

| Library | Version | Purpose |
|---|---|---|
| `@scure/bip39` | 1.2.0 | BIP39 mnemonic generation and validation |
| `@scure/bip32` | 1.3.3 | BIP32 HD key derivation |
| `@noble/secp256k1` | 1.7.1 | secp256k1 elliptic curve operations |
| `@noble/hashes` | 1.3.3 | SHA256, RIPEMD160, HMAC |
| `qrcode` | 1.5.3 | QR code generation for receive screen |

All dependencies are loaded from `cdn.jsdelivr.net` with exact version pins. The `qrcode` script tag includes a SHA384 SRI integrity hash.
