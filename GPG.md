# GPG

## Generate a new GPG key

```bash
gpg --full-generate-key
```

## Export all public keys

```bash
gpg --export --armor > public-keys.asc
```

## Export all private keys

```bash
gpg --export-secret-keys --armor > private-keys.asc
```

## List all keys

```bash
gpg --list-keys
```

## List all keys with subkey fingerprints

```bash
gpg --list-keys --with-subkey-fingerprint
```

## Subkey

A GPG key, often referred to as a private key, can have associated subkeys. Subkeys are beneficial as they help keep the primary key safe. For instance, you can create multiple subkeys, each for a different purpose, and use them to sign, encrypt, or authenticate messages.

```bash
$ gpg -k --with-subkey-fingerprint
/home/user/.gnupg/pubring.kbx
----------------------------------
pub   ed25519 2024-04-28 [SC] [expires: 2026-04-28]
      28D3DC46315EF15EC3FB0DB64FE2CF412F192CDD
uid           [ultimate] Chanwoong Kim <me@chanwoong.kim>
sub   cv25519 2024-04-28 [E] [expires: 2026-04-28]
      64C8EDE87A62FA45E28ABBC9DEDFDD5BE94C46B7
```

- SC: Sign and Certify
- S: Sign
- C: Certify
- E: Encryption
- A: Authentication
