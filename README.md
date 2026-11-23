# Obsid

Obsid is a local-first desktop vault for passwords, notes, and files. Your vault is encrypted on your device and works without an account or cloud service.

> Obsid is open source and under active development. Review the security notes below before storing sensitive information.

## Features

- Store and search passwords, with a password generator, strength indicator, and quick copy.
- Keep encrypted notes with categories and tags.
- Store files encrypted on disk.
- Lock the vault manually, after an inactivity timeout, or when the display turns off or the computer suspends.
- Create and restore local backups with integrity checks.
- Review a local activity log for vault events.
- Unlock the desktop vault from another device on your local network with a temporary QR-code session.
- Check for new releases in the app and open the release page to download an update. Updates are not installed automatically.

## Security overview

- The master password is processed with Argon2id.
- Vault items are encrypted with XChaCha20-Poly1305 using per-item nonces.
- The vault key is kept in memory while the vault is unlocked and cleared when it locks.
- A recovery key is generated when the vault is created. It is shown once, so store it somewhere safe and separate from this device.
- Backups contain encrypted vault data. Keep them private and store them securely.
- Remote unlock starts only when requested, uses a temporary HTTPS session on the local network, and stops after a successful unlock, expiration, failed attempts, or cancellation. The session uses a self-signed certificate; only use it on a network you trust.

This overview is not a security audit or a guarantee that the software is free of vulnerabilities. Please report suspected security issues privately to the maintainers rather than posting exploit details publicly.

## Requirements

- Windows 10 or later
- Node.js and pnpm
- Rust stable and the platform prerequisites for building Tauri applications

See the [Tauri v2 prerequisites](https://v2.tauri.app/start/prerequisites/) for setup details.

## Development

Clone the repository, install dependencies, and start the desktop app:

```bash
git clone https://github.com/readlinkdev/obsid.git
cd obsid
pnpm install
pnpm tauri dev
```

Build the frontend:

```bash
pnpm build
```

Build the desktop application and installers:

```bash
pnpm tauri build
```

Build artifacts are written under `src-tauri/target/release/bundle/`.

## Tests

Run the Rust test suite:

```bash
cd src-tauri
cargo test
```

The tests cover vault creation and recovery, encryption at rest, backup and restore behavior, audit records, remote-unlock payload encryption, and legacy data migration.

## Data and backups

Obsid stores its data in the operating system's application data directory. Backups are created locally and can be restored from **Settings → Local backup**. Restoring a backup locks the vault after replacing its data. Keep a separate copy of your recovery key; backups do not replace it.

## Contributing

Contributions are welcome. Before opening a pull request:

1. Open an issue for larger changes so the approach can be discussed.
2. Keep changes focused and include tests for behavior changes where practical.
3. Run `pnpm build` and `cargo test` for the areas you changed.
4. Describe user-visible changes and any security implications in the pull request.

Please do not include real passwords, recovery keys, vault databases, or personal backup files in issues or pull requests.
