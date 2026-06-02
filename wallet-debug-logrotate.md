# Wallet Debug Log Rotation

Rotates `debug.log` files for any cryptocurrency wallet installed under any user's home directory.

## Config File

`wallet-debug-logrotate.conf`

- Matches `/home/*/.*/debug.log` — covers any hidden-directory wallet for any user
- Rotates when log hits **100 MB**
- Keeps **3** compressed archives
- Uses **copytruncate** — truncates in place, no daemon signal required

## Deploy

```bash
sudo cp wallet-debug-logrotate.conf /etc/logrotate.d/wallet-debug
```

No cron job needed. Logrotate runs daily automatically via `/etc/cron.daily/logrotate` or `logrotate.timer` on systemd systems and will pick up any file placed in `/etc/logrotate.d/`.

## Usage

| Command | Description |
|---|---|
| `sudo logrotate -f /etc/logrotate.d/wallet-debug` | Force rotate now (ignores size threshold) |
| `sudo logrotate -d /etc/logrotate.d/wallet-debug` | Dry run — shows what would happen, no changes |
| `sudo logrotate /etc/logrotate.d/wallet-debug` | Normal run — only rotates if 100 MB threshold is met |

## Archived Log Naming

Rotated logs are named with a timestamp suffix, e.g.:

```
debug.log-20260602-143000.gz
```

## Notes

- Adding a new wallet or user requires no config changes — the wildcard handles it automatically
- `delaycompress` means the most recently rotated log is left uncompressed for one cycle, then compressed on the next run
- `missingok` and `notifempty` ensure the config works safely on servers where no wallets are present
